# Week 05 – Django Static Files, Images, Bootstrap and Pagination

## Readable model names
Add `__str__` methods to Student and Module so admin pages show meaningful names rather than `Student object (id)`. Example: `def __str__(self): return f"{self.firstName} {self.lastName}"` for Student and `return self.name` for Module. Use your model's actual field names.

## Dynamic student images
Install Pillow (`python -m pip install Pillow`) and add `image = models.ImageField(upload_to="students/", blank=True, null=True)` to Student. Run migrations. In settings: `MEDIA_URL = "/media/"` and `MEDIA_ROOT = BASE_DIR / "media"`. During development add `urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)` after importing `settings` and `static` in project urls.py. In the template use `{% if student.image %}<img src="{{ student.image.url }}" alt="Photo of {{ student }}">{% endif %}`.

## Static CSS
Create `myApp/static/myApp/site.css`; load with `{% load static %}` and `<link rel="stylesheet" href="{% static 'myApp/site.css' %}">`. Example CSS: `body{font-family:Arial;background:#f3f5f8;color:#243247} main{max-width:950px;margin:25px auto;padding:20px;background:white}`. Static files are project assets; media files are user uploads.

## Bootstrap
The worksheet introduces Bootstrap. Include its official CSS in the base template and use classes such as `container`, `row`, `col-md-6`, `table` and `btn`. It is optional to the plain CSS example.

## Pagination
```python
from django.core.paginator import Paginator
def home(request):
    students = Student.objects.all().order_by("lastName", "firstName")
    page_obj = Paginator(students, 5).get_page(request.GET.get("page"))
    return render(request, "myApp/home.html", {"page_obj": page_obj})
```
Template: `{% for student in page_obj %}<p>{{ student }}</p>{% empty %}<p>No students.</p>{% endfor %}` then add links: `{% if page_obj.has_previous %}<a href="?page={{ page_obj.previous_page_number }}">Previous</a>{% endif %} Page {{ page_obj.number }} of {{ page_obj.paginator.num_pages }} {% if page_obj.has_next %}<a href="?page={{ page_obj.next_page_number }}">Next</a>{% endif %}`. This shows five records per page.
