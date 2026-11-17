# Week 04 – Django Templates and ORM

## ORM examples
```python
from myApp.models import Student, Module
students = Student.objects.all()
student = Student.objects.get(id=4)  # raises DoesNotExist if missing
matching = Student.objects.filter(firstName="Connie")
modules = student.modules.all()
```
`all()` returns all rows, `get()` expects one row and `filter()` returns a queryset.

## myApp/urls.py
```python
from django.urls import path
from . import views
app_name = "myApp"
urlpatterns = [path("", views.home, name="home"), path("<int:student_id>/", views.student_details, name="student_details")]
```
Include with `path("", include("myApp.urls"))` in the project URL file.

## myApp/views.py
```python
from django.shortcuts import render, get_object_or_404
from .models import Student
def home(request):
    students = Student.objects.all().order_by("lastName", "firstName")
    return render(request, "myApp/home.html", {"students": students})
def student_details(request, student_id):
    student = get_object_or_404(Student, pk=student_id)
    return render(request, "myApp/student_details.html", {"student": student})
```

## Templates
`myApp/templates/myApp/base.html`:
```html
<!doctype html><html lang="en"><head><meta charset="utf-8"><title>{% block title %}Student Directory{% endblock %}</title></head><body><header><h1>Student Directory</h1></header><nav><a href="{% url 'myApp:home' %}">Home</a></nav><main>{% block content %}{% endblock %}</main><footer>Web Programming tutorial</footer></body></html>
```
`home.html`:
```html
{% extends "myApp/base.html" %}{% block content %}<h2>Students</h2><ul>{% for student in students %}<li><a href="{% url 'myApp:student_details' student.id %}">{{ student.firstName }} {{ student.lastName }}</a></li>{% empty %}<li>No students found.</li>{% endfor %}</ul>{% endblock %}
```
`student_details.html`:
```html
{% extends "myApp/base.html" %}{% block content %}<h2>{{ student.firstName }} {{ student.lastName }}</h2><p>Student number: {{ student.id }}</p><h3>Modules</h3><ul>{% for module in student.modules.all %}<li>{{ module.name }}</li>{% empty %}<li>No modules enrolled.</li>{% endfor %}</ul><a href="{% url 'myApp:home' %}">Back</a>{% endblock %}
```
**Note:** The supplied worksheet's model field names should be used. If yours are named differently, update the example names consistently.
