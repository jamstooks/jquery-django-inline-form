# jquery-django-inline-form
Quick JQuery plugin for expanding django inline forms - designed specifically for django's inline formsets

[DEMO](http://jsfiddle.net/jamstooks/fz6wv5L0/)

## Usage

    <script src="/path/to/jquery-django-inline-form/django-inline-form.js"></script>

### Your Django Form

In this example I'm using a formset called `authors_formset` and the prefix `authors_` for all classes and ids:

    <form ...>
      <!-- more form stuff -->
      {{ author_formset.management_form }}

      <!-- A div to contain your formset -->
      <div id="authors-container">
        {% for form in author_formset %}
          <fieldset class="authors-fieldset-{{ forloop.counter0 }}">
            {{ form.as_p }}
          </fieldset>
        {% endfor %}
      </div>

      <!-- buttons - these live outside of the container, above -->
      <div>
        <button id="add-author" type="button">Add</button>
        <button id="delete-author" type="button">Delete</button>
      </div>
      <!-- more form stuff -->
    </form>
  
Here's what we're doing:

  - wrapping the formset in a div (in this case, called `authors-container`)
  - giving the fieldset a class that includes the counter (`authors-fieldset-{{ forloop.counter0 }}`)
    - it doesn't have to be a fieldset, it can be a div too, if you want
  - add the "add" button
  - add the "delete" button (optional, but recommended)
  
Next you'll need a template for the empty form which mimics the formset (or div) above. That's what `empty_form` is for:

    <script type="text/html" id="authors-template">
      <fieldset class="authors-fieldset-__prefix__" style="display: none;">
          {{ author_formset.empty_form.as_p }}
      </fieldset>
    </script>
  
*Note: if you want your new form to animate in, set it to `display: none`.

Finally, you'll need to attach the action to the button:

    $('#add-author').djangoInlineForm({
      prefix: "authors",
      deleteButtonId: "delete-author"
    });

The `deleteButtonId` is optional, but useful.

## Options

`django-inline-form` will calculate most required fields from your prefix, but you can identify them specifically, if need be:

`prefix` - Not required, if you are explicit about all the other *Id fields (except deleteButtonId)

`containerId` - default: `'#' + this.prefix + '-container'`

`templateId` - default: `'#' + this.prefix + '-template'`

`totalFormsId` - the management form field, default: `'#id_' + this.prefix + '-TOTAL_FORMS'`

`maxFormsId` - the management form field, default: `'#id_' + this.prefix + '-MAX_NUM_FORMS'`

`postClick` - any method or javascript you want to execute after a form is added (like updating custom widgets)

`deleteButtonId` - the id of the delete button, default = `'#' + this.prefix + '-delete'`

`formHeight` - the height, in pixels, of the form. If this is set, the page will scroll down that value to accommodate the new form.

## Pitfalls

Since jquery is simply evaluating the number of forms based on the children of your container (`authors-container` above), you should only have elements that match your template. That is, only formsets, or divs containing forms. Any button or other DOM objects will affect the count.

## Credits

Loosely based on http://stackoverflow.com/questions/21260987/add-a-dynamic-form-to-a-django-formset-using-javascript-in-a-right-way
