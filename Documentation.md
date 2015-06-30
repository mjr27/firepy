# Introduction #
The project is in an early stage, so there might be errors. Please report them, thanks.

# Installation #

Install FirePy using easy\_install, or manually install from source.

```
$ sudo easy_install -U firepy
Searching for firepy
Reading http://pypi.python.org/simple/firepy/
Reading http://code.google.com/p/firepy/
Reading http://code.google.com/p/firepy/downloads/list
Best match: firepy 0.1.1
Downloading http://firepy.googlecode.com/files/firepy-0.1.1.tar.gz
Processing firepy-0.1.1.tar.gz
Running firepy-0.1.1/setup.py -q bdist_egg --dist-dir /tmp/easy_install-rTr-93/f      irepy-0.1.1/egg-dist-tmp-myei_A
Removing firepy 0.1 from easy-install.pth file
Adding firepy 0.1.1 to easy-install.pth file

Installed /usr/lib/python2.5/site-packages/firepy-0.1.1-py2.5.egg
Processing dependencies for firepy
Finished processing dependencies for firepy
```

Test if it's installed properly. No message should be shown when you enter the command below. If some error occurs like below, it means that install went wrong.

```
$ python -c 'import firepy'
Traceback (most recent call last):
  File "<string>", line 1, in <module>
ImportError: No module named firepy
```

# Integrating With Django #

Integration is simple and clean.

Add the FirePy middleware to Django settings.py like below. Note that the position where the middleware was installed is important. To use FirePy's logging working correctly in the middleware codes, you should put the FirePy middleware on the top.

```
MIDDLEWARE_CLASSES = (
    'firepy.django.middleware.FirePHPMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
)
```

Now use the standard python logging facility to enjoy FirePHP's console logging.

```
import logging

from django.contrib.auth.models import User
from django.http import HttpResponse

def a(a, b, c):
    raise ValueError("asexception!!")

def fire(request):
    logging.debug([1, {"asdf":(123, 2, "3")}, "3"])
    logging.debug("debug")
    logging.info("info")
    logging.warn("warn %d", 2)
    logging.error("error")
    logging.critical("critical")
    ab = User.objects.filter(username="serialx").all()
    for i in ab:
        logging.debug(i)
    ab = User.objects.all()
    for i in ab:
        logging.debug(i.groups.all())
    try:
        a(1, 2, "3")
    except:
        logging.exception("sdf!!!")

    response = HttpResponse("hello world!")
    return response
```

Then you'll get something like this:

![http://firepy.googlecode.com/files/firepy.png](http://firepy.googlecode.com/files/firepy.png)