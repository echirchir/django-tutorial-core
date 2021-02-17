# NOTES - APPLICATIONS

``` class AppConfig```

- `name` - Full python path to the application - defines which application the configuration applies to. 
It must be set in the AppConfig subclasses. Must be unique across a Django project
- `label ` - Short name for the application e.g `admin`. It must be unique across a Django project and defaults to the last component
of `name`
- `verbose_name` - Human-readable name for the application e.g `Administration`. It defaults to `label.title`
- `path` - Filesystem path to the application directory e.g `usr/lib/pythonX.y/dist-packages/django/contrib/admin`.
In most cases, Django can automatically detect and set this value but you can also provide an explicit override as a class
attribute on your AppConfig subclass.

### Read-only attributes

- `module` - Root module for the application, e.g `<module django.contrib.admin` from `django/contrib/admin/__init__.py`>
- `models_module` - Module containing the models e.g `<module django.contrib.admin.models` from `django/contrib/admin/models.py`>
It may be None of the application does not contain models module. Note that the database related signals such as `pre_migrate, post_migrate`
are only emitted for applications that have a `models` module

### Methods

- `get_models()` - Returns an iterable of `Model` classes for this application. Requires an app registry to be fully populated
- `get_model(model_name, require_ready=True`  - Returns the `Model` with the given model name. model_name is case insensitive.
It raises `LookupError` if no such model exists
- `ready()` - Subclasses can override this method to perform initialization tasks such as registering signals. It is called as soon as the registry
is fully populated.

***Example***

```
from django.apps import AppConfig
from django.db.models.signals import pre_save

class EmojiConfig(AppConfig):
    def ready(self):
        # import model classes
        from .models import SpecialEmoji #or
        SpecialEmoji = self.get_model('SpecialEmoji')
        
        # registering signals with the model's string label
        pre_save.connect(receiver, sender='app_label.SpecialEmoji')
```

Although you can access model class as described above, avoid interacting with the database in your `ready()` implementation.
These methods include `save(), delete(), manager methods etc`. Raw SQL are also not recommended here.

### Note

In the usual initialization process, the `ready` method is only called once by Django.

### Namespace Packages as apps

Python packages without an __init__.py file are known as `namespace packages` and may be spread across multiple directories
at different locations on the `sys.path`.

Namespace packages may only be Django applications if one of the following is true:

1. The namespace package actually has only a single location (is not spread across more than one directory)
2. The `AppConfig` class used to configure the application has a path class attribute, which is the absolute directory path Django
will use as the single base path for the application.

If neither of these conditions are met, Django will raise `ImproperlyConfigured` exception

### Application Registry

## apps

The application registry provides the following public API. Methods that are not listed below are considered private and may change without
notice.

- `apps.ready` - boolean attribute that is set to True after the registry is populated and all `ApppConfig.ready()` methods are called.
- `apps.get_app_configs()` - returns an iterable of AppConfig instances
- `apps.get_app_config(app_label)` - returns an AppConfig for the application with the given label. Raises LookupError if not found.
- `apps.is_installed(app_name)` - checks whether an application with given name exists in the registry. The app name is the
full name of the app e.g `django.contrib.admin`. 
- `apps.get_model(app_label, model_name, require_ready=True)` - returns the model with the given app label and model name
As a shortcut, this method also accepts a single argument in the form `app_label.model_name.model_name` ase insensitive.


### Initialization Process

When Django starts, `django.setup()` is responsible for populating the application registry.

`setup()`
 - Loading the settings
 - Setting up logging
 - If set_prefix is True, setting the URL resolver script prefix to FORCE_SCRIPT_NAME if defined, or / otherwise
 - Initializing the application registry
 
 The function is called automatically:
 
    - When running an HTTP server via Django's WSGI support
    - When invoking a management command
    
 The application registry is initialized in three stages. At each stage, Django processes all applications in the order of
 `INSTALLED_APPS`.
 
 1. First, Django imports each item in `INSTALLED_APPS`
 2. Then Django attempts to import the `models` submodule of each application, if there is one.
 3. Finally Django runs the `ready()` method of each application configuration.
 
 
 ### Troubleshooting
 
   - `AppRegistryNotReady` - this happens when importing an application configuration or a models module triggers code that
   depends on the app registry.
   
   - `ImportError: cannot import name ...` - this happens if the import sequence ends up in a loop. Always minimize dependencies
   between your models modules and so as little work as possible at import time. Consider `lazy evaluation`.
   
   - `django.contrib.admin` - automatically performs autodiscovery of admin modules in installed applications. To prevent it, change your
   INSTALLED_APPS to contain `django.contrib.admin.apps.SimpleAdminConfig`
   
   