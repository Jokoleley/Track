# Admin Modules Quick Start Guide

## 🎯 Access Admin Features

### Step 1: Become a HouseTeamManager

**During Teacher Registration:**
1. Fill in all teacher details (Title, Name, ID, Email, Password)
2. In "Manager SignUp Code" field, enter: **LEO**
3. Click "Create Teacher Account"
4. You'll see: "🎉 You are now the House Team Manager!"

**OR Transfer from existing manager:**
- Current manager can assign role to another teacher
- Teachers can request the role

### Step 2: Access Admin Panels

Once logged in as HouseTeamManager:
1. Two new tab buttons appear in teacher dashboard:
   - 📅 **Season Management**
   - 👥 **User Management**

---

## 📅 Season Management - Quick Actions

### Reset a Season

1. Click **📅 Season Management** tab
2. View current house standings in the info box
3. Select cycle type:
   - **Yearly** - Increment year groups, move Year 13 to Alumni
   - **Semester** - 6-month cycle
   - **Trimester** - 4-month cycle
4. Pick reset date (default: today)
5. Click **🔄 Execute Reset Now** button
6. Modal appears - Type **RESET** to confirm
7. ✅ Season archived, points reset, students notified

### View Past Seasons

1. Scroll to "Season History" section
2. See table of all archived seasons
3. Click **📥 Export Season History (CSV)** to download

---

## 👥 User Management - Quick Actions

### Delete Users

1. Click **👥 User Management** tab
2. Use filters to find users:
   - Search by name or ID
   - Filter by Role (Student/Teacher)
   - Filter by Year Group
   - Filter by House Team
3. Select users:
   - Click individual checkboxes, OR
   - Click "Select All" checkbox to select all visible
4. Counter shows selected count
5. Click **🗑️ Delete Selected** button (red)
6. Modal appears - Type **CONFIRM** to confirm
7. ✅ Users deleted, data cleaned up

---

## ⚠️ Critical Safety Features

### Confirmation Guardrails

| Action | Confirmation Required | Consequence |
|--------|----------------------|------------|
| Season Reset | Type "RESET" | Resets ALL student points permanently |
| User Deletion | Type "CONFIRM" | Permanently deletes user accounts |

### Self-Protection

- ✅ **Cannot delete your own account** - Protection built in
- Each action is logged for audit trail

---

## 🔐 Manager Signup Code

**Code:** `LEO` (case-insensitive internally)

- Only works during teacher registration
- Creates first HouseTeamManager if none exists
- Prevents duplicate managers
- Invalid codes show: "Invalid manager code"

---

## 📊 Season Reset Details

### What Gets Reset

| Item | Action | Notes |
|------|--------|-------|
| House Points | Set to 0 | All students' points cleared |
| XP | Set to 0 | Affects avatar levels |
| Quiz Progress | Cleared | Optional per configuration |
| Year Groups | Increment by 1 | Only for Yearly cycles |
| Year 13 | Moved to Alumni | Only for Yearly cycles |

### What Gets Archived

- ✅ Current standings (house → points)
- ✅ Rankings (sorted by points)
- ✅ Winner house
- ✅ Participant count
- ✅ Timestamp of reset

---

## 🗑️ User Deletion Details

### Student Deletion Cleanup

When a student is deleted, the system removes:
- House points records
- Quiz progress entries
- Arena history (duels fought)
- Student account

### Teacher Deletion

When a teacher is deleted:
- Teacher account removed
- Managed classes logged to console
- Admin must reassign classes manually

### Safety Check

Before deletion, you see:
- User count being deleted
- List of affected user names
- Confirmation requirement

---

## 📈 Sample CSV Export Format

```csv
Season #,Academic Year,Cycle Type,Start Date,Winner House,Total Participants
1,2026,Yearly,4/4/2026,Green,145
2,2025,Trimester,12/15/2025,Blue,142
3,2025,Trimester,8/20/2025,Yellow,138
```

---

## 🎨 UI Elements

### Buttons

- 🔄 **Execute Reset Now** (Blue primary) - Immediate reset
- ⏰ **Schedule Reset** (Blue primary) - Plan future reset
- 🗑️ **Delete Selected** (Red danger) - Delete users
- 📥 **Export Season History** (Gray secondary) - Download CSV

### Inputs

- **Confirmation Input** - Special monospace style with visual feedback
  - Turns green when correct text entered
  - Disable state when empty/incorrect

### Colors (Theme-Aware)

- Backgrounds: `var(--surface)` / `var(--surface-2)`
- Text: `var(--text)` / `var(--text-2)`
- Accents: `var(--accent)` (orange)
- Danger: `var(--house-red)`
- Success: `var(--theme-accent-reward)` (green)

---

## 🐛 Troubleshooting

### Admin buttons don't appear
- ✅ Confirm you're logged in as HouseTeamManager
- ✅ Check browser console for errors
- ✅ Refresh page (F5)

### CSV export shows blank
- ✅ Ensure seasons have been reset before
- ✅ Check localStorage for seasonHistory collection
- ✅ File may download with default filename - check Downloads folder

### Deletion is slow
- ✅ Normal for large user counts (deletes all associated data)
- ✅ Monitor browser console for progress
- ✅ Don't close tab during operation

### Confirmation input won't enable
- ✅ Verify exact text (case-sensitive: "RESET" or "CONFIRM")
- ✅ No extra spaces allowed
- ✅ Try clearing and re-typing

---

## 📚 Related Documentation

- Full Implementation Guide: `ADMIN_MODULES_IMPLEMENTATION.md`
- Code Comments: Search for "ADMIN MODULE" in index.html
- Admin Action Logs: Check browser console after each action

---

**Last Updated:** April 4, 2026  
**Version:** 1.0 (Complete)
