---
layout: post
title:  "Serve Django Static Files with WhiteNoise"
date:   2023-05-05 15:18:34 +0300
categories: jekyll update
---

Want to deploy a Django app and your css and js files aren't being rendered? Well, [WhiteNoise](https://whitenoise.readthedocs.io/en/stable/django.html) to the rescue.

**Note** - Assumes familiarity with Python and Django

## Procedure
In your virtual development environment, install `WhiteNoise` and `Brotli` (for compression).

```
pip install whitenoise brotli
```

In the main app settings file, add the `app`, `middleware` and configure `static` directories

### Loading static files into Templates

Loading static files in a template is a two-step process:

* add {% load static %} at the top of the template
* add the {% static %} template tag with the proper link
*#project/appName/templates/index.html*
```
{% load static %}
<link rel="stylesheet" type="text/css" href="{% static'css/example.css' %}" />
```

### Add the whitenoise app
*#project/settings.py*
```
INSTALLED_APPS = [
  'yourApp',
  'whitenoise.runserver_nostatic',
  'django.contrib.staticfiles',
  ...
]
```

### Add the whitenoise middleware
The Whitenoise middleware should come after Django's security middleware.

*#project/settings.py*
```
MIDDLEWARE = [
  'django.middleware.security.SecurityMiddleware',
  'whitenoise.middleware.WhiteNoiseMiddleware',
  ...
]
```

### Configure the Static files root
Depending on where you went to school, (chuckles), this might or might not work for you.

This is what worked for me

*#project/settings.py*
```
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'static/')
STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'
```

### Deployment

Django comes with the built-in `collecstatic` command that compiles all static files into a single directory, `STATIC_ROOT` suitable for deployment into production.

```
python manage.py collectstatic

python manage.py migrate

gunicorn projectName.wsgi:application
```
