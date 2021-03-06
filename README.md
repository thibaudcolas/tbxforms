[![PyPI](https://img.shields.io/pypi/v/tbxforms.svg)](https://pypi.org/project/tbxforms/)
[![npm](https://img.shields.io/npm/v/tbxforms.svg)](https://www.npmjs.com/package/tbxforms) [![PyPI downloads](https://img.shields.io/pypi/dm/tbxforms.svg)](https://pypi.org/project/tbxforms/)
[![Build status](https://github.com/kbayliss/tbxforms/workflows/CI/badge.svg)](https://github.com/kbayliss/tbxforms/actions)
[![Coverage Status](https://coveralls.io/repos/github/kbayliss/tbxforms/badge.svg?branch=main)](https://coveralls.io/github/kbayliss/tbxforms?branch=main)
[![Total alerts](https://img.shields.io/lgtm/alerts/g/kbayliss/tbxforms.svg?logo=lgtm&logoWidth=18)](https://lgtm.com/projects/g/kbayliss/tbxforms/alerts/)

# Torchbox Forms

A Torchbox-flavoured template pack for [django-crispy-forms](https://github.com/django-crispy-forms/django-crispy-forms), adapted from [crispy-forms-gds](https://github.com/wildfish/crispy-forms-gds).

## Contents

-   [Torchbox Forms](#torchbox-forms)
    -   [Installation](#installation)
        -   [Install the Python package](#install-the-python-package)
        -   [Install the NPM package](#install-the-npm-package)
    -   [Usage](#usage)
        -   [Creating a Django form](#creating-a-django-form)
        -   [Creating a Wagtail form](#creating-a-wagtail-form)
            -   [Add a `helper` property to the Wagtail form](#add-a-helper-property-to-the-wagtail-form)
            -   [Instruct a Wagtail Page model to use the newly created form](#instruct-a-wagtail-page-model-to-use-the-newly-created-form)
        -   [Render a form](#render-a-form)
        -   [Conditionally-required fields](#conditionally-required-fields)
        -   [Customising form styles](#customising-form-styles)
-   [Further reading](#further-reading)

## Installation

You must install both a Python package and an NPM package.

### Install the Python package

Install using pip:

```bash
pip install tbxforms
```

Add `django-crispy-forms` and `tbxforms` to your installed apps:

```python
INSTALLED_APPS = [
  ...
  'crispy_forms',  # django-crispy-forms
  'tbxforms',
]
```

Now add the following settings to tell `django-crispy-forms` to use this theme:

```python
CRISPY_ALLOWED_TEMPLATE_PACKS = ["tbx"]
CRISPY_TEMPLATE_PACK = "tbx"
```

### Install the NPM package

Install using NPM:

```bash
npm install tbxforms
```

Instantiate your forms:

```javascript
import TbxForms from '@tbxforms/tbxforms';

document.addEventListener('DOMContentLoaded', () => {
    for (const form of document.querySelectorAll(TbxForms.selector())) {
        new TbxForms(form);
    }
});
```

Import the styles into your project:

```sass
@import 'node_modules/@tbxforms/tbxforms/static/sass/tbxforms';
```

## Usage

### Creating a Django form

```python
from tbxforms.forms import BaseForm as TbxFormsBaseForm

class ExampleForm(TbxFormsBaseForm, forms.Form):
    # < Your field definitions >

    # Although not required, it is recommended to add a helper:
    @property
    def helper(self):
        fh = super().helper
        fh.html5_required = True
        fh.label_size = Size.MEDIUM
        fh.legend_size = Size.MEDIUM
        fh.form_error_title = _("There is a problem with your submission")
        return fh

class ExampleModelForm(TbxFormsBaseForm, forms.ModelForm):
    # < Your field definitions and ModelForm config >

    # Although not required, it is recommended to add a helper:
    @property
    def helper(self):
        fh = super().helper
        fh.html5_required = True
        fh.label_size = Size.MEDIUM
        fh.legend_size = Size.MEDIUM
        fh.form_error_title = _("There is a problem with your submission")
        return fh
```

### Creating a Wagtail form

Two parts are required for this to work:

1. Add a `helper` property to the Wagtail form
2. Instruct a Wagtail Page model to use the newly created form

#### Add a `helper` property to the Wagtail form

```python
from wagtail.contrib.forms.forms import BaseForm as WagtailBaseForm
from tbxforms.forms import BaseForm as TbxFormsBaseForm

class ExampleWagtailForm(TbxFormsBaseForm, WagtailBaseForm):
    @property
    def helper(self):
        fh = super().helper
        fh.html5_required = True
        fh.label_size = Size.MEDIUM
        fh.legend_size = Size.MEDIUM
        fh.form_error_title = _("There is a problem with your submission")
        fh.add_input(
            Button.primary(
                name="submit",
                type="submit",
                value=_("Submit"),
            )
        )
        return fh
```

#### Instruct a Wagtail Page model to use the newly created form

```python
# in your forms definitions (e.g. forms.py)

from tbxforms.forms import BaseWagtailFormBuilder as TbxFormsBaseWagtailFormBuilder
from path.to.your.forms import ExampleWagtailForm

class WagtailFormBuilder(TbxFormsBaseWagtailFormBuilder):
    def get_form_class(self):
        return type(str("WagtailForm"), (ExampleWagtailForm,), self.formfields)

# in your page models (e.g. models.py)

from path.to.your.forms import WagtailFormBuilder

class ExampleFormPage(...):
    ...
    form_builder = WagtailFormBuilder
    ...
```

### Render a form

Just like Django Crispy Forms, you need to pass your form object to the
`{% crispy ... %}` template tag, e.g.:

```
{% load crispy_forms_tags %}
{% crispy your_form %}
```

### Conditionally-required fields

TODO: add instructions.

### Customising form styles

Out of the box, forms created with `tbxforms` will look like the
[GOV.UK Design System](https://design-system.service.gov.uk/), though many
variables can be customised.

To customise a variable, define it in your project.
See [tbxforms/static/sass/abstracts/\_variables.scss](https://github.com/kbayliss/tbxforms/blob/main/tbxforms/static/sass/abstracts/_variables.scss) for options.

# Further reading

-   Download the [PyPi package](http://pypi.python.org/pypi/tbxforms)
-   Download the [NPM package](https://www.npmjs.com/package/tbxforms)
-   Learn more about [Django Crispy Forms](https://django-crispy-forms.readthedocs.io/en/latest/)
-   Learn more about [Crispy Forms GDS](https://github.com/wildfish/crispy-forms-gds)
-   Learn more about [GOV.UK Design System](https://design-system.service.gov.uk/)
