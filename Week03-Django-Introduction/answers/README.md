# Week 03 – Introduction to Django

The TODO asks for a Django site modelling rugby teams/players and positions with a many-to-many relationship. This is the implementation.

## Setup
```bash
python -m venv venv
# Windows: .\venv\Scripts\activate
# macOS/Linux: source venv/bin/activate
python -m pip install django
django-admin startproject rugby_site .
python manage.py startapp teams
```
Add `teams` to `INSTALLED_APPS`.

## teams/models.py
```python
from django.db import models
class Position(models.Model):
    name = models.CharField(max_length=60, unique=True)
    description = models.TextField(blank=True)
    def __str__(self): return self.name

class Player(models.Model):
    first_name = models.CharField(max_length=60)
    last_name = models.CharField(max_length=60)
    team_name = models.CharField(max_length=100)
    shirt_number = models.PositiveSmallIntegerField()
    positions = models.ManyToManyField(Position, related_name="players")
    def __str__(self): return f"{self.first_name} {self.last_name} ({self.team_name})"
```

## teams/admin.py
```python
from django.contrib import admin
from .models import Player, Position
@admin.register(Position)
class PositionAdmin(admin.ModelAdmin): search_fields = ("name",)
@admin.register(Player)
class PlayerAdmin(admin.ModelAdmin):
    list_display = ("first_name", "last_name", "team_name", "shirt_number")
    list_filter = ("team_name",)
    filter_horizontal = ("positions",)
```
Run `python manage.py makemigrations`, `python manage.py migrate`, `python manage.py createsuperuser`, then `python manage.py runserver`. Use `/admin/` to add positions and players. Assign more than one position to a player if required. No real player dataset was supplied, so sample real-world records have not been invented.
