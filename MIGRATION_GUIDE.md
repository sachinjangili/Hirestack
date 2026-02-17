# HireStack - Migration & Setup Guide

## ✅ Updates Completed

### 1. **Updated Job Model** (`portal/models.py`)
Added new fields to the Job model for better job posting functionality:
- `company_name` - Company name
- `location` - Job location(s)
- `salary_range` - Salary range (e.g., "10-15 LPA")
- `job_type` - Full-time, Part-time, Internship, Contract, Freelance
- `experience_level` - Entry, Mid, Senior, Executive
- `skills_required` - Required skills (comma-separated)
- `job_link` - Link to full job posting
- `apply_link` - External application link
- `featured` - Featured job status
- `is_active` - Job active status
- `updated_at` - Last update timestamp
- `application_count()` - Method to count applications

### 2. **Enhanced Post Job Form** (`jobportal/Templates/post_job.html`)
New form fields:
- Company Name (required)
- Job Type (Full-time, Part-time, Internship, Contract, Freelance)
- Experience Level (Entry, Mid, Senior, Executive)
- Location(s) - Comma-separated or "Remote"
- Salary Range - e.g., "10-15 LPA"
- Skills Required - Comma-separated
- Job Posting Link - URL to full posting
- Application Link - External application URL
- Featured Job checkbox

### 3. **Updated Dashboard** (`jobportal/Templates/dashboard.html`)
- Displays all job information including location, salary, experience level
- Shows job type badges
- Shows skills required
- Application count for each job
- Works for both recruiters and candidates
- Tracks applied jobs for candidates

### 4. **Enhanced Views** (`portal/views.py`)
Updated views to handle new fields:
- `post_job()` - Saves all new job fields
- `dashboard()` - Shows different dashboard based on user role
- `profile_view()` - Profile with stats
- Better context passing to templates

### 5. **Updated URLs** (`portal/urls.py`)
Added HTML view routes:
- `/` - Home page
- `/register/` - Registration
- `/login/` - Login
- `/logout/` - Logout
- `/dashboard/` - Dashboard
- `/post-job/` - Post job form
- `/profile/` - User profile
- `/apply/<job_id>/` - Apply for job

---

## 🚀 REQUIRED SETUP STEPS

### Step 1: Create Database Migrations

Run these commands in your terminal:

```bash
# Navigate to project directory
cd c:\Users\rajku_sc0x7ng\OneDrive\Desktop\jobportal\jobportal

# Activate virtual environment (if not already activated)
# On Windows:
..\myvenv\Scripts\activate

# Create migrations
python manage.py makemigrations

# Apply migrations
python manage.py migrate
```

### Step 2: Update Settings.py

Make sure these settings are in your `jobportal/settings.py`:

```python
# REST Framework Configuration
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    ),
}

# Login URL
LOGIN_URL = '/login/'
LOGIN_REDIRECT_URL = '/dashboard/'
LOGOUT_REDIRECT_URL = '/login/'

# Template Configuration
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'jobportal' / 'Templates'],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

# Media Files
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'

# Static Files
STATIC_URL = '/static/'
STATICFILES_DIRS = [BASE_DIR / 'static']
```

### Step 3: Update Main URLs (jobportal/urls.py)

Verify this configuration:

```python
from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static
from portal import views

urlpatterns = [
    path('', views.home_view, name='home'),
    path('admin/', admin.site.urls),
    path('api/', include('portal.urls')),
    path('login/', views.login_view, name='login'),
    path('logout/', views.logout_view, name='logout'),
    path('dashboard/', views.dashboard, name='dashboard'),
    path('profile/', views.profile_view, name='profile'),
    path('post-job/', views.post_job, name='post_job'),
    path('register/', views.register_view, name='register'),
    path('apply/<int:job_id>/', views.apply_job, name='apply_job'),
]

if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
    urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
```

---

## 🎯 FEATURE OVERVIEW

### For Recruiters:
1. ✅ Post jobs with detailed information
2. ✅ Set external application links
3. ✅ Mark jobs as featured
4. ✅ View application count
5. ✅ Manage active/inactive jobs

### For Candidates:
1. ✅ Browse all active jobs with filters
2. ✅ See job details: location, salary, experience level, skills
3. ✅ Apply directly or via external link
4. ✅ Track applied jobs
5. ✅ Save jobs for later

---

## 🔗 Important Links & Routes

| Route | Purpose | Who |
|-------|---------|-----|
| `/` | Home Page | All |
| `/register/` | User Registration | Anonymous |
| `/login/` | User Login | Anonymous |
| `/dashboard/` | Main Dashboard | Authenticated |
| `/post-job/` | Post New Job | Recruiters |
| `/profile/` | User Profile | Authenticated |
| `/apply/<id>/` | Apply for Job | Candidates |
| `/logout/` | Logout | Authenticated |

---

## 📋 API Endpoints

| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/api/register/` | POST | Register new user |
| `/api/login/` | POST | Get JWT token |
| `/api/post-job/` | POST | Post job (API) |
| `/api/jobs/` | GET | Get all jobs (API) |
| `/api/apply/<id>/` | POST | Apply for job (API) |

---

## ⚠️ Troubleshooting

### Issue: "No Module Named 'portal'"
**Solution:** Make sure `portal` is added to `INSTALLED_APPS` in settings.py:
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework',
    'rest_framework_simplejwt',
    'portal',
]
```

### Issue: Templates Not Found
**Solution:** Check `TEMPLATES` setting in settings.py and ensure template directory path is correct.

### Issue: Migration Errors
**Solution:** 
```bash
# Delete all migrations except __init__.py
python manage.py makemigrations --empty portal --name init
python manage.py migrate --fake-initial
```

---

## ✨ Next Steps

1. Run migrations: `python manage.py migrate`
2. Create superuser: `python manage.py createsuperuser`
3. Run development server: `python manage.py runserver`
4. Access admin: `http://127.0.0.1:8000/admin/`
5. Register test users and try the portal

---

## 📝 Database Models Summary

### Profile Model
- OneToOne with User
- role (candidate/recruiter)
- profile_summary

### Job Model (NEW FIELDS)
- title, company_name, description
- recruiter (FK to User)
- location, salary_range
- job_type, experience_level
- skills_required
- job_link, apply_link
- featured, is_active, is_approved
- created_at, updated_at

### Application Model
- job (FK to Job)
- candidate (FK to User)
- applied_at (timestamp)
- Unique constraint on (job, candidate)

### Education, Employment, Skills, Languages, etc.
- All linked to User via ForeignKey
- Support full profile building

---

## 🎓 Profile Fields Available

Candidates can add:
- Education (degree, institution, graduation year)
- Employment history (company, position, dates)
- Skills (with proficiency levels)
- Languages (with proficiency)
- Internships
- Projects (with links)
- Accomplishments
- Competitive Exams
- Academic Achievements
- Resume (file upload)

---

## 🔐 Security Notes

1. All views with `@login_required` are protected
2. Role checks ensure recruiters can only post jobs
3. Candidates can only apply for jobs (not edit)
4. Profile data is user-specific
5. Use HTTPS in production
6. Keep SECRET_KEY secure

---

## 📞 Support

If you encounter any issues:
1. Check the error message carefully
2. Verify all migrations are up to date
3. Clear browser cache (Ctrl+Shift+Delete)
4. Check Django logs in terminal
5. Use browser developer tools (F12) to check for JS errors

