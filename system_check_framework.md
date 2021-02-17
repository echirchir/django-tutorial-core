# NOTES - SYSTEM CHECK FRAMEWORK

The system check framework is a set of static checks for validating Django projects.

It detects common problems and provides hints for how to fix them. The framework is extensible so you
can easily add your own checks.

## API Reference

### CheckMessage

- The warnings and errors raised by system checks must be instances of CheckMessage. An instance 
encapsulates a single reportable error or warning. It also provides context and hints applicable
to the message, and a unique identifier that is used for filtering purposes.

### Builtin Tags

- `admin`
- `caches`
- `compatibility`
- `database`
- `models`
- `security`
- `signals`
- `staticfiles`
- `templates`
- `translation`
- `urls`

### Core system checks

- `Backwards Compatibility`
- `caches`
- `database`
    - `Mysql`
- `model fields`
- `File fields`
- `Related fields`
- `Models`
- `Security`
- `Signals`
- `Templates`
- `Translation`
- `URLs`

### Contrib app checks

- `Admin`
- `ModelAdmin`
- `InlineModelAdmin`
- `GenericInlineModelAdmin`
- `AdminSite`
- `Auth`
- `Contenttypes`
- `Postgres`
- `Sites`
- `Staticfiles`
