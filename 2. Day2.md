
## 2. Setting Up Virtual Environment

A virtual environment keeps your project dependencies isolated.

### Create a project folder:
```bash
mkdir my_django_project
cd my_django_project
```

### Create virtual environment:

**On Windows:**
```bash
python -m venv venv
```

**On Mac/Linux:**
```bash
python3 -m venv venv
```

### Activate virtual environment:

**On Windows:**
```bash
venv\Scripts\activate
```

**On Mac/Linux:**
```bash
source venv/bin/activate
```

You'll see `(venv)` before your command prompt when activated.

## 3. Installing Django

With your virtual environment activated:

```bash
pip install django
```

Verify installation:
```bash
django-admin --version
```

## 4. Creating Django Project

```bash
django-admin startproject myproject .
```

**Note:** The dot (`.`) at the end creates the project in the current directory.

### Project Structure Created:
```
my_django_project/
├── venv/
├── myproject/
│   ├── __init__.py
│   ├── settings.py      # Project settings
│   ├── urls.py          # Main URL routing
│   ├── asgi.py          # Async server gateway
│   └── wsgi.py          # Web server gateway
└── manage.py            # Command-line utility
```

## 5. Creating Your First App

Django projects contain multiple "apps" (modules). Let's create one:

```bash
python manage.py startapp myapp
```

### App Structure Created:
```
myapp/
├── migrations/          # Database migrations
├── __init__.py
├── admin.py            # Admin panel configuration
├── apps.py             # App configuration
├── models.py           # Models (M in MVT)
├── tests.py            # Test files
└── views.py            # Views (V in MVT)
```

## 6. Registering the App

Open `myproject/settings.py` and add your app:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'myapp',  # Add this line
]
```

## 7. Creating Templates Folder (T in MVT)

Templates are HTML files. Create the structure:

```bash
mkdir -p myapp/templates/myapp
```

### Configure Templates (already done by default)

Check `myproject/settings.py`:
```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],  # You can add custom template dirs here
        'APP_DIRS': True,  # This enables app-level templates
        ...
    },
]
```

## 8. Understanding MVT Architecture

Let me create a simple example to show how it all works together:

### **Model (M)** - Database Structure
Edit `myapp/models.py`:

```python
from django.db import models

class Book(models.Model):
    title = models.CharField(max_length=200)
    author = models.CharField(max_length=100)
    published_date = models.DateField()
    
    def __str__(self):
        return self.title
```

### **View (V)** - Business Logic
Edit `myapp/views.py`:

```python
from django.shortcuts import render
from .models import Book

def book_list(request):
    books = Book.objects.all()
    context = {'books': books}
    return render(request, 'myapp/book_list.html', context)
```

### **Template (T)** - Presentation
Create `myapp/templates/myapp/book_list.html`:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Book List</title>
</head>
<body>
    <h1>My Book Collection</h1>
    <ul>
        {% for book in books %}
            <li>{{ book.title }} by {{ book.author }}</li>
        {% empty %}
            <li>No books available</li>
        {% endfor %}
    </ul>
</body>
</html>
```

### **URL Configuration** - Routing
Create `myapp/urls.py`:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('books/', views.book_list, name='book_list'),
]
```

Edit `myproject/urls.py`:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('myapp.urls')),  # Add this line
]
```

## 9. Running Migrations

```bash
python manage.py makemigrations
python manage.py migrate
```

## 10. Creating a Superuser (Admin)

```bash
python manage.py createsuperuser
```

Follow the prompts to create username, email, and password.

## 11. Running the Development Server

```bash
python manage.py runserver
```

Visit:
- Main site: `http://127.0.0.1:8000/books/`
- Admin panel: `http://127.0.0.1:8000/admin/`

## 12. Complete File Structure

```
my_django_project/
├── venv/
├── myproject/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   ├── asgi.py
│   └── wsgi.py
├── myapp/
│   ├── migrations/
│   ├── templates/
│   │   └── myapp/
│   │       └── book_list.html
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── tests.py
│   ├── urls.py
│   └── views.py
├── manage.py
└── db.sqlite3 (created after migrations)
```

## 13. MVT Flow Diagram

```
User Request (URL)
       ↓
    urls.py (URL Router)
       ↓
    views.py (View - Business Logic)
       ↓
    models.py (Model - Database)
       ↓
    views.py (Processes Data)
       ↓
    template.html (Template - HTML)
       ↓
    Response to User
```

## Quick Commands Reference

```bash
# Create project
django-admin startproject projectname .

# Create app
python manage.py startapp appname

# Make migrations
python manage.py makemigrations

# Apply migrations
python manage.py migrate

# Run server
python manage.py runserver

# Create superuser
python manage.py createsuperuser
```


5. Study Django ORM for database queries

Would you like me to explain any specific part in more detail?
