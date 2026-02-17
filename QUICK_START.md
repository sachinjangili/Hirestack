# HireStack - Quick Start Guide

## 🚀 Get Started in 5 Minutes

### Prerequisites
- Python 3.8+
- pip
- Virtual environment (myvenv in your project)

---

## Step 1: Activate Virtual Environment

**On Windows (Command Prompt):**
```bash
myvenv\Scripts\activate
```

**On Windows (PowerShell):**
```bash
myvenv\Scripts\Activate.ps1
```

**On Mac/Linux:**
```bash
source myvenv/bin/activate
```

---

## Step 2: Navigate to Project

```bash
cd c:\Users\rajku_sc0x7ng\OneDrive\Desktop\jobportal\jobportal
```

---

## Step 3: Run Migrations

```bash
# Create migrations for database changes
python manage.py makemigrations

# Apply migrations
python manage.py migrate
```

### Expected Output:
```
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, portal, sessions
Running migrations:
  Applying portal.0002_job_new_fields... OK
```

---

## Step 4: Create Superuser (Admin)

```bash
python manage.py createsuperuser
```

**Follow prompts:**
- Username: `admin`
- Email: `admin@example.com`
- Password: (enter secure password)

---

## Step 5: Start Development Server

```bash
python manage.py runserver
```

### Output Should Show:
```
Starting development server at http://127.0.0.1:8000/
```

---

## 🌐 Access Your Portal

Open in browser:
- **Home**: http://127.0.0.1:8000/
- **Register**: http://127.0.0.1:8000/register/
- **Login**: http://127.0.0.1:8000/login/
- **Dashboard**: http://127.0.0.1:8000/dashboard/
- **Admin Panel**: http://127.0.0.1:8000/admin/

---

## 👥 Test the System

### 1. Create Test Users

**Recruiter:**
1. Go to `/register/`
2. Username: `recruiter1`
3. Email: `recruiter@test.com`
4. Password: `TestPass123`
5. Select: **Recruiter**
6. Click Register

**Candidate:**
1. Go to `/register/`
2. Username: `candidate1`
3. Email: `candidate@test.com`
4. Password: `TestPass123`
5. Select: **Candidate**
6. Click Register

---

### 2. Post a Job (as Recruiter)

1. Login as `recruiter1`
2. Click "Dashboard" → "Post Job"
3. Fill in the form:
   - **Job Title**: Senior Python Developer
   - **Company Name**: Tech Company Inc.
   - **Job Type**: Full-Time
   - **Experience Level**: Mid Level
   - **Location**: Bangalore, Delhi, Remote
   - **Salary Range**: 15-20 LPA
   - **Skills Required**: Python, Django, PostgreSQL, REST API
   - **Description**: Write a detailed job description here...
   - **Job Link** (optional): https://yourcompany.com/jobs/123
   - **Apply Link** (optional): https://yourcompany.com/apply/123
4. Click "Post Job"

---

### 3. Browse Jobs (as Candidate)

1. Login as `candidate1`
2. Go to Dashboard
3. See the job you just posted
4. See all details: location, salary, experience level, skills
5. Click the job title to visit the posting (if link provided)
6. Click "Apply Now" button

---

### 4. Check Applications

1. Login as `recruiter1`
2. Go to Dashboard
3. See your posted job
4. Application count shows how many applied

---

## 📊 View in Admin Panel

1. Go to http://127.0.0.1:8000/admin/
2. Login with superuser credentials
3. Browse:
   - Users
   - Profiles
   - Jobs
   - Applications
   - Skills
   - Education
   - Employment
   - And more...

---

## 🛠️ Helpful Commands

```bash
# Check Django version
python manage.py --version

# Run tests
python manage.py test

# Collect static files
python manage.py collectstatic

# Create another superuser
python manage.py createsuperuser

# Export database data
python manage.py dumpdata > backup.json

# Import database data
python manage.py loaddata backup.json

# Reset specific app (WARNING: Deletes all data)
python manage.py migrate portal zero

# Show all available URL patterns
python manage.py show_urls
```

---

## 🧹 Clear Browser Cache (Important First Time)

**Chrome/Edge:**
1. Press `Ctrl + Shift + Delete`
2. Select "All time"
3. Check all boxes
4. Click "Clear data"

**Firefox:**
1. Press `Ctrl + Shift + Delete`
2. Select "Everything"
3. Click "Clear Now"

---

## ⚠️ Common Issues & Fixes

### Issue: "No module named 'portal'"
```bash
# Make sure portal is in INSTALLED_APPS in settings.py
# Then restart server
```

### Issue: "Templates not found"
```bash
# Check if Templates folder path is correct in settings.py
# Restart server
```

### Issue: Database locked
```bash
python manage.py migrate --no-input
```

### Issue: Port 8000 already in use
```bash
python manage.py runserver 8001
```

### Issue: Static files not loading
```bash
python manage.py collectstatic --noinput
```

---

## 🎯 Feature Workflow

### Complete Recruiter Workflow:
```
Register → Login → Post Job → Add Links → Wait for Applications
         ↓
      Dashboard (View Posted Jobs + Application Count)
```

### Complete Candidate Workflow:
```
Register → Login → Browse Jobs → View Details → Apply
         ↓                      ↓
      Dashboard              External Link
    (View Applied Jobs)    (If Provided)
```

---

## 📱 Responsive Design

The portal is responsive and works on:
- ✅ Desktop (1920px+)
- ✅ Laptop (1366px+)
- ✅ Tablet (768px+)
- ✅ Mobile (320px+)

---

## 🔐 Login Test Accounts

After setup, use:

**Recruiter Test Account:**
- Username: recruiter1
- Password: TestPass123
- Role: Recruiter

**Candidate Test Account:**
- Username: candidate1
- Password: TestPass123
- Role: Candidate

---

## 📞 Troubleshooting Tips

1. **Always check the terminal** for error messages
2. **Clear browser cache** when templates don't update
3. **Restart server** after making changes
4. **Use Ctrl+C** to stop the server
5. **Re-activate** virtual environment if commands fail
6. **Check file paths** are correct
7. **Use browser dev tools** (F12) for frontend issues

---

## ✨ You're All Set!

Your HireStack is ready to use. Start with:
1. ✅ Create test accounts
2. ✅ Post a test job
3. ✅ Apply to that job
4. ✅ Check admin panel
5. ✅ Explore all features

**Happy Job Hunting! 🎉**

---

## 📚 Next Steps

If you want to extend the portal:
- Add email notifications
- Add job search/filtering API
- Add user profile completion percentage
- Add recommendation system
- Add saved jobs feature
- Add resume parsing
- Add video interviews
- Add interview scheduling
- Add skill endorsements
- Add company ratings

---

## 📞 Support Resources

- Django Docs: https://docs.djangoproject.com/
- Django REST Framework: https://www.django-rest-framework.org/
- Bootstrap 5: https://getbootstrap.com/docs/5.0/
- SQLite: https://www.sqlite.org/docs.html

