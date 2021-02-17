# NOTES - BUILT-IN CLASS-BASED VIEWS

1. Base Views
    - View
    - TemplateView
    - RedirectView
2. Generic display views
    - DetailView
    - ListView
3. Generic editing views
    - FormView
    - CreateView
    - UpdateView
    - DeleteView
4. Generic date views
    - ArchiveIndexView
    - YearArchiveView
    - MonthArchiveView
    - WeekArchiveView
    - DayArchiveView
    - TodayArchiveView
    - DateDetailView
5. Class-based views mixins
    - Simple Mixins
        - ContextMixin
        - TemplateResponseMixin
    - Single object mixins
        - SingleObjectMixin
        - SingleObjectTemplateResponseMixin
    - Multiple Object mixins
        - MultipleObjectMixin
        - MultipleObjectTemplateResponseMixin
    - Editing mixins
        - FormMixin
        - ModelFormMixin
        - ProcessFormView
        - DeletionMixin
    - Date-based mixins
        - YearMixin
        - MonthMixin
        - DayMixin
        - WeekMixin
        - DateMixin
        - BaseDateListView
        
Class-based views are deployed into a URL pattern using the `as_view()` class method.

### Base vs Generic views

Base class-based views can be thought of as parent views, which can be used by themselves or inherited from.
They may not provide all the capabilities required for projects, in which case there are Mixins which extend what 
base views can do.

Generic views are built off of those base views, and were developed as a shortcut for common usage patterns
such as displaying the details of an object.

They take certain common idioms and patterns found in view development and abstract them so that you can quickly write
common views of data without to repeat yourself.

Most generic views require the `queryset` key, which is a `Queryset` instance.

