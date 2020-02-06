![Concentric Sky](http://concentricsky.com/media/uploads/images/csky_logo.jpg)

# Django Basic Models

Django Basic Models is an open-source Django library developed by [Concentric Sky](http://concentricsky.com/). It provides abstract models that are commonly needed for Django projects.


### Table of Contents
- [Installation](#installation)
- [Getting Started](#getting-started)
- [License](#license)
- [About Concentric Sky](#about-concentric-sky)


## Installation

    pip install git+https://github.com/concentricsky/django-basic-models.git


## Getting Started

models.py

	from basic_models.models import CreatedEditedAt, CreatedEditedBy, IsActive, NameSlug, TitleBody

	class MyModel(CreatedEditedAt, CreatedEditedBy, IsActive, NameSlug, TitleBody):
		pass

admin.py

    from basic_models import actions
    from basic_models.admin import site

    class MyModelAdmin(actions.Clone):
        list_display = ('__str__', 'is_active')

    site.register(MyModel, MyModelAdmin)

### CreatedUpdatedAt

    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)


### CreatedUpdatedBy

    created_by = models.ForeignKey(AUTH_USER_MODEL, null=True, blank=True,
                                   related_name='+',
                                   on_delete=models.SET_NULL)
    updated_by = models.ForeignKey(AUTH_USER_MODEL, null=True, blank=True,
                                   related_name='+',
                                   on_delete=models.SET_NULL)

### IsActive

    is_active = models.BooleanField()

    MyModel.active_objects.all()

### NameSlug

    name = models.CharField(max_length=255)
    slug = models.SlugField()


### TitleBody

    title = models.CharField(max_length=255)
    body = models.TextField()


### OnlyOneActive(models.Model):

    def save(self, *args, **kwargs):
        super(OnlyOneActive, self).save(*args, **kwargs)
        # If we were made active, deactivate all other instances
        if self.is_active:
            self.__class__.objects.filter(is_active=True).exclude(pk=self.pk) \
                .update(is_active=False)

## License

This project is licensed under the [Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0). Details can be found in the LICENSE.md file.


## About Concentric Sky

_For nearly a decade, Concentric Sky has been building technology solutions that impact people everywhere. We work in the mobile, enterprise and web application spaces. Our team, based in Eugene Oregon, loves to solve complex problems. Concentric Sky believes in contributing back to our community and one of the ways we do that is by open sourcing our code on GitHub. Contact Concentric Sky at hello@concentricsky.com._
