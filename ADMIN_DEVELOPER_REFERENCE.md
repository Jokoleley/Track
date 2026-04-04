# Admin Modules - Developer Reference

## 📖 Code Organization

### CSS Classes (in alphabetical order)

```css
.admin-controls         - Grid layout for action buttons
.admin-danger           - Red danger button styling
.admin-filters          - Filter bar container
.admin-highlight        - Warning/caution box styling
.admin-nav-item         - Navbar button for admin sections
.admin-panel            - Main admin content container
.admin-section          - Section container (hidden by default)
.admin-table            - Table styling for user lists
.confirmation-input     - Special monospace input with validation
.season-history-export  - Flex container for export buttons
.season-info-box        - Info display box with accent border
```

### New Collections (Data Schema)

#### seasonHistory
```javascript
{
  _id: uuid(),
  academicYear: number,
  cycleType: 'Yearly' | 'Semester' | 'Trimester',
  startDate: ISO8601_string,
  endDate: ISO8601_string,
  housePointsSnapshot: { Yellow: 0, Red: 0, Green: 0, Blue: 0 },
  housePlacements: [{ rank: 1, house: 'Green', points: 450 }],
  winner: 'Green',
  totalParticipants: 145,
  archivedAt: ISO8601_string
}
```

#### adminActionLogs
```javascript
{
  _id: uuid(),
  adminId: teacher_id,
  action: 'season_reset' | 'bulk_delete_users' | 'assign_manager' | etc,
  details: { cycleType, count, results, etc },
  timestamp: ISO8601_string
}
```

#### Modified: students
```javascript
{
  // ... existing fields ...
  globalNotifications: [
    { id: uuid(), message: string, timestamp: ISO8601, read: boolean }
  ]
}
```

---

## 🔌 Core Functions

### Season Management

#### `createSeasonHistory(cycleType: string): object`
Archives current season data.

```javascript
const archive = createSeasonHistory('Yearly');
// Returns: seasonHistory object with snapshot data
```

**Process:**
1. Aggregates all student house points
2. Ranks houses by total points
3. Saves to seasonHistory collection
4. Returns archive object for reference

#### `executeSeasonReset(cycleType: string): object`
Executes full season reset.

```javascript
const archive = executeSeasonReset('Yearly');
// Steps:
// 1. Archives current season
// 2. Resets student.housePoints to 0
// 3. Resets student.xp to 0
// 4. For Yearly: increments yearGroup, moves Year 13 to Alumni
// 5. Sends global notification
// 6. Returns archive object
```

**Safety:** Requires "RESET" confirmation before execution.

#### `getSeasonHistoryList(): array`
Returns all seasons sorted by date (newest first).

```javascript
const histories = getSeasonHistoryList();
// Returns: Array of seasonHistory objects
```

#### `exportSeasonHistoryToCSV(): void`
Downloads CSV file with all season data.

```javascript
exportSeasonHistoryToCSV();
// Downloads: season-history-YYYY-MM-DD.csv
```

**CSV Format:**
```
Season #,Academic Year,Cycle Type,Start Date,Winner House,Total Participants
```

#### `showGlobalNotification(message: string): void`
Sends notification to all students.

```javascript
showGlobalNotification('🌟 A new Season has begun!');
// Adds to each student's globalNotifications array
// Also shows Toast to current user
```

---

### User Management

#### `getFilteredUsers(filters: object): object`
Applies filters to students and teachers.

```javascript
const result = getFilteredUsers({
  search: 'John',
  role: 'Student',
  yearGroup: '10',
  house: 'Blue'
});
// Returns: { students: [], teachers: [] }
```

**Filter Options:**
- `search` - String match in name or ID (case-insensitive)
- `role` - 'Student' | 'Teacher' | empty for all
- `yearGroup` - Number 1-13 (students only)
- `house` - 'Yellow' | 'Red' | 'Green' | 'Blue' (students only)

#### `deleteStudent(studentId: string, currentUserId: string): boolean`
Permanently deletes student and cleanup data.

```javascript
const success = deleteStudent(studentId, currentTeacherId);
// Cleanup includes:
// - HousePoints records
// - QuizProgress entries
// - ArenaHistory references
// - Student account
// Returns: true if successful, false if error
```

**Safety Check:** Returns false if studentId === currentUserId (self-protection)

#### `deleteTeacher(teacherId: string, currentUserId: string): boolean`
Permanently deletes teacher.

```javascript
const success = deleteTeacher(teacherId, currentTeacherId);
// Logs managed classes to console for reassignment
// Returns: true if successful, false if error
```

**Note:** No auto-reassignment - admin must handle manually

#### `bulkDeleteUsers(userIds: array, currentUserId: string, userType: string): object`
Batch delete operation.

```javascript
const results = bulkDeleteUsers(
  ['id1', 'id2', 'id3'],
  currentTeacherId,
  'student'
);
// Returns: { success: [ids], failed: [ids] }
```

---

### Role Management

#### `validateSignupCode(code: string): boolean`
Validates manager signup code.

```javascript
const isValid = validateSignupCode('LEO');
// Returns: true if code === 'LEO', false otherwise
```

**Current Code:** `LEO` (hardcoded in client)

#### `setupInitialHouseTeamManager(teacherId: string, code: string): object`
Assigns initial HouseTeamManager role.

```javascript
const result = setupInitialHouseTeamManager(teacherId, 'LEO');
// Returns: { success: boolean, message: string }
// Prevents duplicate managers
// Validates signup code
```

#### `canUserAccessAdminPanel(userId: string, userType: string): boolean`
Checks if user can access admin features.

```javascript
const hasAccess = canUserAccessAdminPanel(currentTeacherId, 'teacher');
// Returns: true only if teacher.isHouseTeamManager === true
```

#### `logAdminAction(adminId: string, action: string, details: object): void`
Logs all admin actions for audit trail.

```javascript
logAdminAction(currentTeacherId, 'execute_season_reset', {
  cycleType: 'Yearly',
  archiveId: 'abc123'
});
// Saves to adminActionLogs collection
// Logs to console: [Admin Action] {...}
```

**Common Actions:**
- `schedule_season_reset`
- `execute_season_reset`
- `bulk_delete_users`
- `assigned_house_team_manager`

---

## 🎨 UI Rendering Functions

### `renderSeasonManagementPanel(): void`
Renders the Season Management admin panel.

```javascript
renderSeasonManagementPanel();
// Populates: #tSectionSeasonManagement
// Shows: current standings, controls, history table
// Event listeners: Schedule/Execute buttons, Export CSV
```

**Prerequisites:**
- Must be HouseTeamManager
- `currentTeacherId` must be set

### `renderUserManagementPanel(): void`
Renders the User Management admin panel.

```javascript
renderUserManagementPanel();
// Populates: #tSectionUserManagement
// Shows: filters, user table, bulk selection
// Event listeners: Setup after render
```

### `renderUserManagementTable(students: array, teachers: array): void`
Populates user table rows.

```javascript
renderUserManagementTable(allStudents, allTeachers);
// Populates: #umTableBody with rows
// Each row: ID, name, role, year/group, house
// Includes checkboxes for selection
```

### `setupUserManagementHandlers(currentUserId: string): void`
Attaches event listeners to user management UI.

```javascript
setupUserManagementHandlers(currentTeacherId);
// Listeners for:
// - Select All checkbox
// - Individual row checkboxes
// - Delete button
// - Filter updates
```

---

## 📱 Modal Functions

### `showSeasonResetConfirmationModal(cycleType: string): void`
Shows confirmation modal for season reset.

```javascript
showSeasonResetConfirmationModal('Yearly');
// Modal content:
// - Description of what will be reset
// - Input for "RESET" confirmation
// - Cancel/Execute buttons
// - Only executes when correct text entered
```

**Features:**
- Real-time input validation
- Visual feedback on input (turns green)
- Button disabled until correct text
- Execute triggers: `executeSeasonReset()`

### `showUserDeletionConfirmationModal(selectedUsers: array, currentUserId: string): void`
Shows confirmation modal for user deletion.

```javascript
showUserDeletionConfirmationModal(
  [{ userId: 'id1', userType: 'student' }],
  currentTeacherId
);
// Modal content:
// - List of users to delete
// - Warnings about permanent deletion
// - Input for "CONFIRM" confirmation
// - Cancel/Delete buttons
```

---

## 🔄 Data Flow Examples

### Season Reset Flow
```
User clicks "Execute Reset Now"
  ↓
showSeasonResetConfirmationModal()
  ↓
User types "RESET" → input validation triggers
  ↓
Execute button becomes enabled
  ↓
User clicks "Execute Reset"
  ↓
executeSeasonReset(cycleType):
  1. createSeasonHistory() → saves archive
  2. Loop all students → reset points/xp
  3. If Yearly: increment yearGroup, Alumni assignment
  4. showGlobalNotification()
  ↓
logAdminAction() → audit trail
  ↓
renderSeasonManagementPanel() → refresh UI
```

### User Deletion Flow
```
User selects students → checkboxes
  ↓
Click "Delete Selected"
  ↓
showUserDeletionConfirmationModal()
  ↓
User types "CONFIRM" → input validation
  ↓
Delete button becomes enabled
  ↓
User clicks "Delete Users"
  ↓
bulkDeleteUsers():
  1. Loop each user ID
  2. For students: deleteStudent()
     - Cleanup HousePoints, QuizProgress, ArenaHistory
     - Delete from students collection
  3. For teachers: deleteTeacher()
     - Log managed classes
     - Delete from teachers collection
  ↓
logAdminAction() → audit trail
  ↓
renderUserManagementPanel() → refresh UI
```

---

## 🧪 Testing Guide

### Unit Test Examples

```javascript
// Test season archive creation
const archive = createSeasonHistory('Yearly');
console.assert(archive.winner === 'Green', 'Winner should be determined');
console.assert(Array.isArray(archive.housePlacements), 'Should have placements');

// Test filter functionality
const filtered = getFilteredUsers({ yearGroup: '10' });
console.assert(filtered.students.every(s => s.yearGroup === 10));

// Test code validation
console.assert(validateSignupCode('LEO') === true);
console.assert(validateSignupCode('INVALID') === false);

// Test self-protection
const result = deleteStudent(currentTeacherId, currentTeacherId);
console.assert(result === false, 'Cannot delete self');
```

### Integration Test Checklist

- [ ] Season reset with Yearly cycle
- [ ] Year 13 students moved to Alumni
- [ ] QuizProgress cleared/marked
- [ ] All students receive notification
- [ ] CSV exports with correct format
- [ ] User filters work in combination
- [ ] Bulk deletion cascades properly
- [ ] Manager only sees admin buttons
- [ ] Signup code validates correctly
- [ ] Light/Dark theme consistency

---

## 🔍 Debugging Tips

### Check Admin Access
```javascript
const teacher = sdkLoad('teachers', currentTeacherId);
console.log('Is Manager?', teacher?.isHouseTeamManager);
console.log('Can Access?', canUserAccessAdminPanel(currentTeacherId, 'teacher'));
```

### View Admin Action Log
```javascript
const logs = sdkLoadAll('adminActionLogs');
logs.forEach(log => console.log(log.action, log.details));
```

### Check Season History
```javascript
const seasons = getSeasonHistoryList();
console.log('Total seasons:', seasons.length);
seasons.forEach(s => console.log(s.winner, s.totalParticipants));
```

### Monitor Data Changes
```javascript
// Before reset
const before = sdkLoad('students', studentId);
console.log('Before reset - HP:', before.housePoints, 'XP:', before.xp);

// After reset
const after = executeSeasonReset('Yearly');
const after_data = sdkLoad('students', studentId);
console.log('After reset - HP:', after_data.housePoints, 'XP:', after_data.xp);
```

---

## 📚 Extension Points

### Add New Cycle Type
```javascript
// In executeSeasonReset():
if (cycleType === 'Weekly') {
  // Custom logic for weekly reset
  student.weeklyResets = (student.weeklyResets || 0) + 1;
}
```

### Add Soft Deletes
```javascript
// Modify deleteStudent():
student.deletedAt = new Date().toISOString();
student.status = 'deleted';
sdkSave('students', studentId, student);
// Instead of: sdkDelete('students', studentId);
```

### Add Email Notifications
```javascript
// In showGlobalNotification():
allStudents.forEach(student => {
  if (student.email && student.emailNotifications) {
    sendEmail(student.email, 'Season has begun!', message);
  }
});
```

### Add Undo Capability
```javascript
// After executeSeasonReset(), save undo snapshot
const undo = {
  _id: uuid(),
  action: 'season_reset',
  previousState: previousStudentData,
  timestamp: now,
  expiresAt: new Date(now + 86400000) // 24 hours
};
```

---

## 🚀 Performance Considerations

### Large User Deletions
For bulk deletes of 100+ users:
- Consider pagination
- Show progress indicator
- Batch operations (background task)
- Disable UI during operation

### Season History Export
For 1000+ seasons:
- Lazy load history
- Stream CSV writing
- Show downloadprogress

### Data Archiving
- Archive only changed data
- Compress old seasons
- Implement data retention policy

---

## 📞 Common Issues & Solutions

| Issue | Cause | Solution |
|-------|-------|----------|
| Admin buttons don't show | User not manager | Check `isHouseTeamManager` flag |
| Reset doesn't work | Input text wrong | Ensure "RESET" exactly (case-sensitive) |
| CSV is empty | No seasons exist | First create a reset |
| Deletion is slow | Many associated records | Normal - data integrity takes time |
| Modal won't close | JS error | Check browser console |

---

**Last Updated:** April 4, 2026  
**API Version:** 1.0
