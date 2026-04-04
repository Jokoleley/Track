# House Team Manager Role - Complete Test Guide

## 🎯 Your Testing Checklist

As a **House Team Manager**, you now have access to **three main sections**:

---

## 1️⃣ ⚙️ SETTINGS TAB
**What you can do:**
- ✅ View your account information
- ✅ See your current role (Teacher or House Team Manager)
- ✅ Enter the manager code "LEO" if you skipped it during registration
- ✅ Toggle between Teacher and Manager roles

### Test Steps:
1. Click **⚙️ Settings** in the left navigation
2. You should see:
   - Your full name, Teacher ID, email
   - **Role badge** showing "👑 House Team Manager"
   - **Admin Permissions** section showing what you can do
3. **If you're a manager**, the code input should be hidden with green checkmark
4. **If you're a teacher**, enter **LEO** to become manager

---

## 2️⃣ 📅 SEASON MANAGEMENT TAB
**What you can do:**
- ✅ View current house standings (aggregated points)
- ✅ Select cycle type (Yearly / Semester / Trimester)
- ✅ Choose reset date
- ✅ Execute immediate reset with confirmation
- ✅ View all past seasons in a table
- ✅ Export season history to CSV

### Full Feature Breakdown:

#### **Current Season Data Display**
You should see:
```
📊 Total Students: (count)
🏆 Current Standings:
   1. Green: 450 points
   2. Blue: 320 points
   3. Yellow: 280 points
   4. Red: 200 points
```

#### **Reset Controls**
- **Season Cycle Type** dropdown:
  - Semester (every 6 months)
  - Trimester (every 4 months)
  - Yearly (every 12 months) ← Increments year groups
  
- **Reset Date** picker: Shows today's date by default

#### **Action Buttons**
1. **⏰ Schedule Reset** - Plans a reset for future date
2. **🔄 Execute Reset Now** - Does reset immediately

### Test the Reset:
1. Click **"Execute Reset Now"**
2. Modal appears asking you to type **"RESET"**
3. Type `RESET` exactly (case-sensitive)
4. **"Execute Reset"** button turns blue/enabled
5. Click it
6. Toast shows: ✅ **"Season reset complete! Data archived."**
7. All students' house points become 0
8. Their XP resets to 0
9. **For Yearly reset only**: Year groups increment, Year 13 → Alumni

#### **Season History Table**
Below the controls, you see past seasons:
```
Season # | Academic Year | Cycle Type | Start Date | Winner House | Total Participants
1        | 2026         | Yearly     | 4/4/2026   | Green        | 145
```

#### **CSV Export** 
- Click **"📥 Export Season History"**
- File downloads: `season-history-2026-04-04.csv`
- Contains all past season data for records

---

## 3️⃣ 👥 USER MANAGEMENT TAB
**What you can do:**
- ✅ Search users by name or ID
- ✅ Filter by role (Student/Teacher)
- ✅ Filter by year group (1-13)
- ✅ Filter by house team (Yellow/Red/Green/Blue)
- ✅ Select multiple users
- ✅ Bulk delete with cascade cleanup

### Full Feature Breakdown:

#### **Filter Controls**
You can filter by combining any of:
- **Search**: Name or ID (case-insensitive partial match)
- **Role**: Student, Teacher, or All
- **Year Group**: 1-13, or All
- **House Team**: Yellow, Red, Green, Blue, or All

Examples:
- Search "John" + Role "Student" = All student Johns
- Year Group "10" + House "Blue" = All Blue House Year 10s
- Role "Teacher" + House Filter = All teacher regardless of house

#### **User Table Display**
Table shows:
```
[Checkbox] | Name              | ID      | Role    | Year | House
[  ]       | John Smith        | JS001   | Student | 10   | Blue
[  ]       | Sarah Jones       | TJ001   | Teacher | -    | -
```

Columns:
- **Checkbox**: Select individual users
- **Name**: Full name
- **ID**: Student ID or Teacher ID
- **Role**: Student or Teacher (+ "Manager" if applicable)
- **Year/Group**: Year 1-13 for students, blank for teachers
- **House**: House for students, blank for teachers

#### **Bulk Selection**
- Click checkbox in header: **"Select All"** checks all visible users
- Individual checkboxes: Select specific users
- Counter shows: **🗑️ Delete Selected (5)** - updates as you select
- Button disabled (gray) until you select users

### Test the Deletion:
1. Select 1-3 users with checkboxes
2. Counter shows **"Delete Selected (3)"**
3. Button is now blue/enabled
4. Click **"🗑️ Delete Selected"**
5. Modal appears:
   - Shows names of users being deleted
   - ⚠️ Warning: Permanent, cannot be undone
   - Input field: Type **"CONFIRM"** exactly
6. Type `CONFIRM`
7. **"Delete Users"** button turns blue/enabled
8. Click it
9. Toast shows: ✅ **"Deleted 3 user(s)"**
10. Users removed from table
11. All their data cleaned up:
    - House points deleted
    - Quiz progress deleted
    - Arena history deleted
    - Account deleted

---

## 🧪 Full End-to-End Test Scenario

### **Prep: Create Test Data**
Before testing, create at least:
- 3-5 student accounts in different year groups
- 2-3 with same house team
- Assign them some house points manually (if system allows)

### **Test 1: Season Reset**
```
1. Go to 📅 Season Management
2. Should see student count and standings
3. Select "Yearly" cycle
4. Click "Execute Reset Now"
5. Modal: Type "RESET"
6. Confirm
7. Check: Student accounts should have 0 house points
8. Check: New season in history table
9. CSV export should show the archived season
```

### **Test 2: User Management**
```
1. Go to 👥 User Management
2. Try searching: Search "John" 
3. Try filtering: Change Role to "Student"
4. Should see filtered results
5. Click "Select All"
6. All checkboxes should check
7. Counter should show total selected
8. Button should be enabled (blue)
9. Click "Delete Selected"
10. Type "CONFIRM"
11. Users should be deleted
12. Refresh page - users still gone
13. Check their data is cleaned up
```

### **Test 3: Settings**
```
1. Go to ⚙️ Settings
2. Should see your info: Name, ID, Email
3. Role badge should show: "👑 House Team Manager"
4. Permissions section should be colored green
5. Code input should not appear (already manager)
6. All text themed properly (light/dark mode)
```

---

## 🔍 Verification Checklist

After testing each feature, verify:

### Season Management
- [ ] Current standings display correctly
- [ ] Date picker shows today by default
- [ ] Cycle type dropdown works (3 options)
- [ ] Schedule button shows toast
- [ ] Reset modal requires "RESET" text
- [ ] Reset button disabled until correct text
- [ ] Reset completes successfully
- [ ] Students' house points are 0 after reset
- [ ] Season added to history table
- [ ] CSV exports with correct format
- [ ] Light/dark theme applied correctly

### User Management
- [ ] Search filters by name (partial match)
- [ ] Role filter works (Student/Teacher)
- [ ] Year group filter works (1-13)
- [ ] House filter works (4 houses)
- [ ] Filters combine correctly
- [ ] Table shows all columns
- [ ] Select All checkbox works
- [ ] Individual checkboxes work
- [ ] Counter updates in real-time
- [ ] Delete button disabled when no selection
- [ ] Delete button enabled when selected
- [ ] Deletion modal shows user names
- [ ] Deletion requires "CONFIRM" text
- [ ] Deletion completes successfully
- [ ] Deleted users not in table anymore
- [ ] User data is cleaned up
- [ ] Light/dark theme applied correctly

### Settings
- [ ] Your info displays (Name, ID, Email)
- [ ] Role shows as "👑 House Team Manager"
- [ ] Permissions section shows both features
- [ ] Code input field is hidden (already manager)
- [ ] Permissions are colored green
- [ ] Theme matches rest of app

---

## ⚠️ Known Limitations & Notes

1. **LocalStorage**: Data only persists in browser (not cloud)
2. **FirstManager**: Only ONE manager can exist (prevent conflicts)
3. **Self-Protection**: Cannot delete own account (safety feature)
4. **No Undo**: Deletions are permanent
5. **Alumni**: Year 13 move to Alumni only on Yearly reset
6. **Permissions**: All students notified globally on reset

---

## 🐛 Troubleshooting

| Issue | Solution |
|-------|----------|
| Buttons don't appear | Refresh page (F5), ensure you're manager |
| Delete button stays gray | Select at least 1 user with checkbox |
| Reset doesn't work | Verify exact "RESET" text (case-sensitive) |
| Export CSV is empty | Create and archive a season first |
| Role badge shows "Teacher" | Re-enter code "LEO" or create new manager |
| Filters don't work | Try searching by name instead |

---

## 📊 Expected Behavior

### After Reset
- ✅ All students have 0 house points
- ✅ All students have 0 XP
- ✅ All students notified (toast)
- ✅ Season archived with snapshot
- ✅ Year groups incremented (Yearly only)
- ✅ Year 13 moved to Alumni (Yearly only)

### After User Deletion
- ✅ User account deleted
- ✅ House points records deleted
- ✅ Quiz progress deleted
- ✅ Arena history deleted
- ✅ User not in table anymore
- ✅ Cannot recover (permanent)

---

## 🎯 Success Criteria

You've successfully tested the House Team Manager role when:

1. ✅ You can access all 3 tabs: Settings, Season Management, User Management
2. ✅ Season reset completes with confirmation
3. ✅ Past seasons appear in history table
4. ✅ CSV export downloads correctly
5. ✅ User filtering and search work
6. ✅ Bulk deletion completes with confirmation
7. ✅ Deleted users disappear from table
8. ✅ All modals require correct confirmation text
9. ✅ Theme colors consistent throughout
10. ✅ No console errors (F12 to check)

---

**Good luck testing! Let me know what you find.** 🚀
