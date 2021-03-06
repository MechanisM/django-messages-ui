jQuery messages-UI plugin package for Django
============================================

django-messages-ui adds JS and HTML to support the Django contrib.messages
app. It should be called on the message list element, and accepts options for
message selectors, transient messages (that disappear on click or key-press),
and close-links.

Messages can be dynamically added via ICanHaz.js, and there's a Python
middleware to automatically add messages from the request into Ajax JSON
responses.


Dependencies
------------

- `jQuery`_ library
- `jQuery doTimeout`_ plugin
- (optionally) `ICanHaz.js`_
- (optionally) `django-icanhaz`_ 0.2.0+

.. _jQuery: http://jquery.com/
.. _jQuery doTimeout: http://benalman.com/projects/jquery-dotimeout-plugin/
.. _ICanHaz.js: http://icanhazjs.com/
.. _django-icanhaz: https://github.com/carljm/django-icanhaz

Installation
------------

In your Django project settings, add "messages_ui" to your INSTALLED_APPS.


Usage
-----

Linking the JS::

    <script src="{{ STATIC_URL }}messages_ui/jquery.messages-ui.js"></script>

Including the default HTML Template::

    {% include "messages_ui/_messages.html" %}

Calling the plugin::

    $('#messages').messages();

Calling the plugin with a variety of options explicitly configured to their
default values::

    $('#messages').messages({
        message: '.message',          // Selector for individual messages
        closeLink: '.close',          // Selector for link to close message
                                      //      ...set to ``false`` to disable
        transientMessage: '.success', // Selector for transient messages
        transientDelay: 500,          // Transient message fade delay (ms)
        transientFadeSpeed: 3000,     // Transient message fade speed (ms)
        handleAjax: false             // Enable automatic AJAX handling
    });

Note: After the plugin is called once, subsequent calls on the same element
will default to the options passed the first time, unless new options are
explicitly provided.

Adding a message in JS (requires `ICanHaz.js`_)::

    $(ich.message({message: "Sample Message", tags: "info"}).appendTo(
        $('#messages'));

To override the default JS template, add a ``message.html`` file to a
directory listed in your ``ICANHAZ_DIRS`` setting (a `django-icanhaz`_
setting).


Ajax
~~~~

To enable automatic handling of messages from Ajax requests, add
``"messages_ui.middleware.AjaxMessagesMiddleware"`` to your
``MIDDLEWARE_CLASSES`` setting (directly after
``django.contrib.messages.middleware.MessageMiddleware``), and pass
``handleAjax: true`` to the plugin initialization.

.. warning::

    ``AjaxMessagesMiddleware`` converts all HTML AJAX responses into JSON
    responses with a ``messages`` key, and the HTML embedded in an ``html``
    key. If your site uses HTML AJAX responses, this will likely require
    updates to other Ajax-handling code in your site.

    Similarly, ``handleAjax: true`` globally sets the default expected
    dataType for AJAX requests to ``"json"``.
