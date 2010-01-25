Easymode settings
=================

.. _master_site:

MASTER_SITE
-----------

The ``MASTER_SITE`` directive must be set to ``True`` if a gettext catalog 
should be automatically populated when new contents are created. This way all 
contents can be translated using gettext. You can also populate the catalogs
manually using the :ref:`easy_locale` command.

In a multiple site context, you might not want to have all sites updating the
catalog. Because the content created on some of these sites might not need to
be translated because it is not used on any other sites. Content can flow from
'master site' to 'slave site' but not from 'slave site' to 'slave site'.

example::

    MASTER_SITE = True

.. _msgid_langugae:

MSGID_LANGUAGE
--------------

The ``MSGID_LANGUAGE`` is the language used for the message id's in the gettext
catalogs. Only when a content was created in this language, it will be added to
the gettext catalog. If ``MSGID_LANGUAGE`` is not defined, the ``LANGUAGE_CODE``
will be used instead. The msgid's in the gettext catalogs should be the same for 
all languages.

This setting should be used when there are different sites, each with a different 
``LANGUAGE_CODE`` set. These sites can all share the same catalogs.

example::
    
    MSGID_LANGUAGE = 'en'

.. _fallback_langugaes:

FALLBACK_LANGUAGES
------------------

The ``FALLBACK_LANGUAGES`` is a dictionary of values that looks like this::

    FALLBACK_LANGUAGES = (
        'en': [],
        'hu': ['en'],
        'be': ['en'],
        'ff' :['hu','en']
    )

Any string that is not translated in 'ff' will be taken from the 'hu' language.
If the 'hu' also has no translation, finally it will be taken from 'en'.

LOCALE_DIR
----------

Use the ``LOCALE_DIR`` setting if you want all contents to be collected in a
single gettext catalog. If ``LOCALE_DIR`` is not specified, the contents will
be grouped by app. When a model belongs to the 'foo' app, new contents will be
added to the catalog located in ``foo/locale``.

You might not want to have the dynamic contents wriiten to your app's locale, 
if you also have static translations. You can separate the dynamic and static
content by specifying the :ref:`locale_postfix`.

example::

    PROJECT_DIR = os.dirname(__file__)
    LOCALE_DIR = os.path.join(PROJECT_DIR, 'db_content')
    
.. _locale_postfix:

LOCALE_POSTFIX
--------------

The ``LOCALE_POSTFIX`` must be used like this::

    LOCALE_POSTFIX = '_content'

Contents that belong to models defined in the 'foo' app, will be added to the catalog
located at ``foo_content/locale`` instead of ``foo/locale``.

USE_SHORT_LANGUAGE_CODES
------------------------

Easymode has some utilities that help in having sites with multiple languages.
``LocaliseUrlsMiddleware`` and ``LocaleFromUrlMiddleWare`` help with adding 
and extracting the current language in the url eg:

http://example.com/**en**/page/1

When having many similar languages in a multi site context, you will have to
use 5 letter language codes:

en-us
en-gb

These language codes do not look pretty in an url:

http://example.com/en-us/page/1

and they might even be redundant because the country code is allready in the domain
extension:

http://example.co.uk/en-gb/page/1

When ``USE_SHORT_LANGUAGE_CODES`` is set to ``True``, the country codes are removed in
urls, leaving only the language code. This means the url would say:

http://example.com/en/page/1

even when the current language would be 'en-us'.

**THIS DIRECTIVE ONLY WORKS WHEN THERE IS NO AMBIGUITY IN YOUR** ``LANGUAGES`` **DIRECTIVE.**

This means i can not have the same language defined twice in my ``LANGUAGES``::

    LANGUAGES = (
        'en-us' : _('American English'),
        'en-gb' : _('British'),
    )

This will **NOT** work because both languages will be displayed in the url as 'en' which is
ambiguous.
