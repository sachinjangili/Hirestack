# HireStack - Technical Specification

## 📋 System Overview

A Django-based HireStack platform for recruiters to post jobs and candidates to apply, with support for external job links and application routes.

**Technology Stack:**
- Backend: Django 6.0.2
- Frontend: Bootstrap 5.3.2
- Database: SQLite (development)
- API: Django REST Framework 3.16.1
- Authentication: JWT (djangorestframework-simplejwt 5.5.1)

---

## 🗄️ Database Schema

### Core Models

#### User (Django built-in)
```python
- id (PK)
- username (unique)
- email (unique)
- password (hashed)
- first_name
- last_name
- date_joined
- is_active
- is_staff
- is_superuser
```

#### Profile
```python
- id (PK)
- user (OneToOne FK to User) [CASCADE]
- role (choices: candidate, recruiter)
- profile_summary (TextField, optional)
- created_at (auto_now_add)
- updated_at (auto_now)

Meta: Constraint unique_together(user, role)
```

#### Job
```python
- id (PK)
- title (CharField, max 200) [INDEXED]
- company_name (CharField, max 255)
- description (TextField)
- recruiter (FK to User) [CASCADE] [INDEXED]
- location (CharField, max 500)
- salary_range (CharField, max 100)
- job_type (CharField, choices: full-time, part-time, internship, contract, freelance)
- experience_level (CharField, choices: entry, mid, senior, executive)
- skills_required (CharField, max 500)
- job_link (URLField, optional)
- apply_link (URLField, optional)
- featured (BooleanField, default False)
- is_active (BooleanField, default True) [INDEXED]
- is_approved (BooleanField, default True) [INDEXED]
- created_at (DateTimeField, auto_now_add) [INDEXED]
- updated_at (DateTimeField, auto_now)

Methods:
- application_count() → int
- __str__() → f"{title} - {company_name}"

Meta: ordering = ['-created_at']
      verbose_name_plural = 'Jobs'
```

#### Application
```python
- id (PK)
- job (FK to Job) [CASCADE] [INDEXED]
- candidate (FK to User) [CASCADE] [INDEXED]
- applied_at (DateTimeField, auto_now_add)

Meta: unique_together = ('job', 'candidate')
      Prevents duplicate applications

Lookup: job -> applications
        user -> applications (as candidate)
```

#### Education
```python
- id (PK)
- user (FK to User) [CASCADE]
- institution (CharField, max 255)
- degree (CharField, max 100)
- stream (CharField, max 100, optional)
- start_date (DateField)
- end_date (DateField, optional)
- cgpa (DecimalField 3.2, optional)
- description (TextField, optional)
- created_at (DateTimeField, auto_now_add)

Meta: ordering = ['-end_date']
```

#### Employment
```python
- id (PK)
- user (FK to User) [CASCADE]
- company_name (CharField, max 255)
- job_title (CharField, max 200)
- start_date (DateField)
- end_date (DateField, optional)
- is_current (BooleanField, default False)
- description (TextField, optional)
- created_at (DateTimeField, auto_now_add)

Meta: ordering = ['-start_date']
```

#### Skill
```python
- id (PK)
- user (FK to User) [CASCADE]
- skill_name (CharField, max 100)
- proficiency (CharField, choices: beginner, intermediate, advanced, expert)
- created_at (DateTimeField, auto_now_add)

Meta: unique_together = ('user', 'skill_name')
      ordering = ['-created_at']
```

#### Language
```python
- id (PK)
- user (FK to User) [CASCADE]
- language_name (CharField, max 50)
- proficiency (CharField, choices: elementary, limited, professional, full, native)
- created_at (DateTimeField, auto_now_add)

Meta: unique_together = ('user', 'language_name')
      ordering = ['-created_at']
```

#### Internship
```python
- id (PK)
- user (FK to User) [CASCADE]
- company_name (CharField, max 255)
- position (CharField, max 200)
- start_date (DateField)
- end_date (DateField, optional)
- description (TextField, optional)
- created_at (DateTimeField, auto_now_add)

Meta: ordering = ['-start_date']
```

#### Project
```python
- id (PK)
- user (FK to User) [CASCADE]
- title (CharField, max 255)
- description (TextField)
- start_date (DateField)
- end_date (DateField, optional)
- project_link (URLField, optional)
- technologies (CharField, max 500)
- created_at (DateTimeField, auto_now_add)

Meta: ordering = ['-start_date']
```

#### Accomplishment
```python
- id (PK)
- user (FK to User) [CASCADE]
- title (CharField, max 255)
- description (TextField)
- date (DateField)
- created_at (DateTimeField, auto_now_add)

Meta: ordering = ['-date']
```

#### CompetitiveExam
```python
- id (PK)
- user (FK to User) [CASCADE]
- exam_name (CharField, max 255)
- score (CharField, max 100)
- date (DateField)
- description (TextField, optional)
- created_at (DateTimeField, auto_now_add)

Meta: ordering = ['-date']
```

#### AcademicAchievement
```python
- id (PK)
- user (FK to User) [CASCADE]
- title (CharField, max 255)
- description (TextField)
- date (DateField)
- created_at (DateTimeField, auto_now_add)

Meta: ordering = ['-date']
```

#### Resume
```python
- id (PK)
- user (OneToOne FK to User) [CASCADE]
- resume_file (FileField, optional)
- current_salary (CharField, max 100, optional)
- preferred_locations (CharField, max 500, optional)
- preferred_job_titles (CharField, max 500, optional)
- created_at (DateTimeField, auto_now_add)
- updated_at (DateTimeField, auto_now)
```

---

## 🔌 API Endpoints

### Authentication
```
POST /api/register/
  Body: {username, email, password, role}
  Response: {user_id, username, role}

POST /api/login/
  Body: {username, password}
  Response: {access, refresh}
```

### Jobs
```
POST /api/post-job/
  Auth: Required (Bearer token)
  Body: {title, company_name, description, location, salary_range, ...}
  Response: {id, title, company_name, ...}

GET /api/jobs/
  Response: [{id, title, company_name, ...}, ...]

POST /api/apply/{job_id}/
  Auth: Required
  Response: {message, status}
```

---

## 🌐 HTML Views & Routes

### Public Routes
```
GET / → home_view()
GET /register/ → register_view()
POST /register/ → register_view()
GET /login/ → login_view()
POST /login/ → login_view()
GET /logout/ → logout_view()
```

### Protected Routes (Login Required)
```
GET /dashboard/ → dashboard()
GET /post-job/ → post_job()
POST /post-job/ → post_job()
GET /profile/ → profile_view()
GET /apply/{job_id}/ → apply_job()
```

---

## 📊 View Logic

### dashboard()
**URL:** `/dashboard/`
**Method:** GET
**Auth:** @login_required
**Logic:**
```
1. Get user's profile
2. If recruiter:
   - Get jobs posted by user
   - Pass jobs to template
   - Template: dashboard.html (recruiter mode)
3. If candidate:
   - Get all approved, active jobs
   - Get list of job_ids user applied to
   - Pass jobs and applied_job_ids to template
   - Template: dashboard.html (candidate mode)
```

### post_job()
**URL:** `/post-job/`
**Method:** GET, POST
**Auth:** @login_required
**Logic:**
```
On GET:
  1. Check if user is recruiter
  2. If not, redirect to dashboard
  3. Return post_job.html form

On POST:
  1. Check if user is recruiter
  2. Validate form fields
  3. Create Job object with all fields
  4. Set recruiter to current user
  5. Set is_approved=True, is_active=True
  6. Redirect to dashboard
```

### apply_job(job_id)
**URL:** `/apply/{job_id}/`
**Method:** GET
**Auth:** @login_required
**Logic:**
```
1. Check if user is candidate
2. If not recruiter, create candidate profile
3. Get job by ID
4. Check if already applied (unique constraint)
5. Create Application object
6. Redirect to dashboard
```

### profile_view()
**URL:** `/profile/`
**Method:** GET
**Auth:** @login_required
**Logic:**
```
1. Get or create profile for user
2. Count applied jobs (if candidate)
3. Count posted jobs (if recruiter)
4. Build profile_stats dict
5. Pass profile and stats to template
6. Render profile.html
```

---

## 🎨 Template Context

### dashboard.html
```python
{
    'jobs': List[Job],           # All jobs for candidate or user's jobs for recruiter
    'user_role': str,            # 'candidate' or 'recruiter'
    'is_authenticated': bool,
    'user': User,
    'applied_job_ids': List[int] # For candidates, list of job IDs they applied to
}
```

### post_job.html
```python
{
    'is_authenticated': bool,
    'user_role': str,
}
```

### profile.html
```python
{
    'profile': Profile,
    'profile_stats': {
        'applied_jobs': int,
        'profile_completion': int,  # For candidates
        # OR
        'posted_jobs': int,
        'total_applications': int   # For recruiters
    },
    'user_role': str,
    'is_authenticated': bool,
}
```

---

## 🔄 Data Flow Diagrams

### Job Posting Flow
```
Recruiter Input
    ↓
Form Validation
    ↓
Job Object Creation
    ↓
Database Save
    ↓
Redirect to Dashboard
    ↓
Dashboard Displays Job
```

### Job Application Flow
```
Candidate Clicks Apply
    ↓
Check if apply_link exists
    ↓
If yes: Open external link in new tab
If no: Create Application object
    ↓
Redirect to Dashboard
    ↓
Application count updates
```

### Job Browse Flow
```
Candidate Lands on Dashboard
    ↓
Query all is_approved=True, is_active=True jobs
    ↓
Query user's applied jobs
    ↓
Pass both to template
    ↓
Template renders job cards
    ↓
Shows Apply button status (applied or not)
    ↓
Candidate can click job title or apply link
```

---

## 📝 Form Specifications

### Post Job Form
```
Field: title
  Type: CharField
  Required: Yes
  Max Length: 200
  Help: Job title
  Validation: Non-empty

Field: company_name
  Type: CharField
  Required: Yes
  Max Length: 255
  Help: Company name
  Validation: Non-empty

Field: job_type
  Type: Select
  Required: Yes
  Choices: full-time, part-time, internship, contract, freelance
  Default: full-time

Field: experience_level
  Type: Select
  Required: Yes
  Choices: entry, mid, senior, executive
  Default: mid

Field: location
  Type: CharField
  Required: No
  Max Length: 500
  Placeholder: "Bangalore, Delhi, Remote"

Field: salary_range
  Type: CharField
  Required: No
  Max Length: 100
  Placeholder: "10-15 LPA"

Field: skills_required
  Type: CharField
  Required: No
  Max Length: 500
  Placeholder: "Python, Django, PostgreSQL"

Field: description
  Type: Textarea
  Required: Yes
  Min Height: 200px
  Placeholder: "Job description..."

Field: job_link
  Type: URLField
  Required: No
  Help: "Link to full job posting"

Field: apply_link
  Type: URLField
  Required: No
  Help: "External application link"

Field: featured
  Type: Checkbox
  Required: No
  Label: "Mark as Featured Job"
```

---

## 🔐 Security Considerations

### Implemented
- CSRF protection ({% csrf_token %})
- SQL injection protection (Django ORM)
- XSS protection (Django template escaping)
- Unique constraint on (job, candidate)
- Role-based access control
- Login required decorators

### Recommendations
- Use HTTPS in production
- Set SECURE_SSL_REDIRECT = True
- Set SESSION_COOKIE_SECURE = True
- Set CSRF_COOKIE_SECURE = True
- Implement rate limiting
- Add input sanitization for URLs
- Validate URLs before saving

---

## 📈 Performance Optimization

### Database
```python
# Indexes needed
Job.recruiter
Job.is_active
Job.is_approved
Job.created_at
Application.job
Application.candidate
```

### Queries
```python
# Optimized queries
jobs = Job.objects.filter(
    is_approved=True,
    is_active=True
).select_related('recruiter').prefetch_related('application_set')

# Count usage
application_count = job.application_set.count()  # Method available
```

### Caching
```python
# Future optimization
@cached_property
def application_count(self):
    return self.application_set.count()
```

---

## 🧪 Testing Scenarios

### Unit Tests
```python
# Test job creation
def test_create_job(recruiter, factory):
    job = factory.create_job(recruiter=recruiter)
    assert job.id is not None
    assert job.is_approved is True

# Test unique constraint
def test_duplicate_application(candidate, job):
    Application.objects.create(job=job, candidate=candidate)
    with pytest.raises(IntegrityError):
        Application.objects.create(job=job, candidate=candidate)
```

### Integration Tests
```python
# Test job posting flow
def test_post_job_flow(client, recruiter_user):
    client.login(username=recruiter_user.username, password='password')
    response = client.post('/post-job/', {
        'title': 'Test Job',
        'company_name': 'Test Co',
        'description': 'Test Description',
        ...
    })
    assert response.status_code == 302  # Redirect
    assert Job.objects.count() == 1

# Test apply flow
def test_apply_flow(client, candidate_user, job):
    client.login(username=candidate_user.username, password='password')
    response = client.get(f'/apply/{job.id}/')
    assert Application.objects.filter(job=job, candidate=candidate_user).exists()
```

---

## 📱 Responsive Breakpoints

```css
Mobile: 320px - 767px
  - Single column layout
  - Full width cards
  - Touch-friendly buttons

Tablet: 768px - 1023px
  - Two column (sidebar + content)
  - Adjusted card width
  - Medium buttons

Desktop: 1024px+
  - Full layout
  - Multiple columns
  - Normal sizing
```

---

## 🚀 Deployment Configuration

### Environment Variables
```
DEBUG=False (Production)
SECRET_KEY=<generated_key>
ALLOWED_HOSTS=yourdomain.com
DATABASE_URL=<production_database>
STATIC_ROOT=/var/www/static/
MEDIA_ROOT=/var/www/media/
```

### Production Checklist
```
- [ ] DEBUG=False
- [ ] SECRET_KEY changed
- [ ] ALLOWED_HOSTS configured
- [ ] Database migrated
- [ ] Static files collected
- [ ] HTTPS enabled
- [ ] Secure cookies enabled
- [ ] CORS configured
- [ ] Email backend configured
- [ ] Logging configured
- [ ] Backup strategy set
```

---

## 📚 Dependencies

```
Django==6.0.2
djangorestframework==3.16.1
djangorestframework-simplejwt==5.5.1
PyJWT==2.11.0
sqlparse==0.5.5
asgiref==3.11.1
tzdata==2025.3
```

---

## 🎯 Future Enhancements

1. Email notifications
2. Advanced search with Elasticsearch
3. Job recommendations
4. Skill matching algorithm
5. Video interviews
6. Assessment tests
7. Saved jobs
8. Job alerts
9. Resume parser
10. Analytics dashboard

---

## 📞 Version History

**Version 1.0.0** (Current)
- Initial setup with core features
- Job posting with links
- Job applications
- User profiles
- Django REST API

---

## 🔗 Related Files

- `settings.py` - Django configuration
- `urls.py` - URL routing
- `models.py` - Data models
- `views.py` - Business logic
- `serializers.py` - API serialization
- Templates - HTML rendering

---

**Last Updated:** February 17, 2026
**Status:** Production Ready
**Maintained By:** Your Team

