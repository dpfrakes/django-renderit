.. _templatefallback:

=================
Template Fallback
=================

.. contents::
   :depth: 3

Fallback template paths are generated based on the arguments supplied. A list 
templates is created and then ``select_template`` is called on the list.

Simple Example
==============

.. code-block:: django

    {% renderit auth.user %}
    
Generated List::

    ['/renderit/auth_user.html',
     '/renderit/default.html']


.. note::

    The default template root path can be changed via :ref:`root_template_path`
    
     
Arguments Example
=================


Single Argument
---------------

.. code-block:: django

    {% renderit auth.user auth %}
    
Generated list::

    ['/renderit/auth_user_auth.html',
     '/renderit/auth_user.html',
     '/renderit/default.html']
     
Mulitple Arguments
------------------

.. code-block:: django

    {% renderit auth.user auth homepage %}
    
Generated list::

    ['/renderit/auth_user_auth_homepage.html',
     '/renderit/auth_user_auth.html',
     '/renderit/auth_user.html',
     '/renderit/default.html']
     
Prefix
------

Suppling a prefix will gerernate two sets of templates, one set with the prefix 
and one set without the prefix

.. code-block:: django

    {% renderit auth.user with prefix=userinfo %}
    
Generated List::

    ['/renderit/userinfo_auth_user.html',
     '/renderit/auth_user.html',
     '/renderit/default.html']
     
**With Arguments**

.. code-block:: django

    {% renderit auth.user homepage custom with prefix=userinfo %}
    
Generated List::

    ['/renderit/userinfo_auth_user_homapage_custom.html',
     '/renderit/userinfo_auth_user_homepage.html',
     '/renderit/userinfo_auth_user.html',
     
     '/renderit/auth_user_homepage_custom.html',
     '/renderit/auth_user_homapage.html',
     '/renderit/auth_user.html',
     
     '/renderit/default.html']
     
Group
-----

Group will append the string and act like a directory rather then a extra 
template path string.

.. code-block:: django

    {% renderit auth.user with group=users %}
    
Generated List::

    ['/renderit/users/auth_user.html',
     '/renderit/auth_user.html',
     '/renderit/default.html']
     
The list of generated template paths can get rather large when using multiple 
arguments, prefix and group together

.. code-block:: django

    {% renderit auth.user hompage custom with prefix=logininfo group=users %}
    
Generated List::

    ['/renderit/users/logininfo_auth_user_homepage_custom.html',
     '/renderit/users/logininfo_auth_user_homepage.html',
     '/renderit/users/logininfo_auth_user.html',
     
     '/renderit/users/auth_user_homepage_custom.html',
     '/renderit/users/auth_user_homepage.html',
     '/renderit/users/auth_user.html',
     
     '/renderit/logininfo_auth_user_homepage_custom.html',
     '/renderit/logininfo_auth_user_homepage.html',
     '/renderit/logininfo_auth_user.html',
     
     '/renderit/auth_user_homepage_custom.html',
     '/renderit/auth_user_homepage.html',
     '/renderit/auth_user.html',
     
     '/renderit/default.html']
     
What we have here is 2 sets with 2 inner sets of templates, one set has the 
group **users** one without, the inner set has one set with 
prefix **logininfo** and one set without.

.. note::

    This is just the generated template path list, these templates do not have 
    to exist, this is simply a way to fallback to other templates in case a 
    template does not exist.
    
The other arguments
===================

concat
------

This argumennt is taken literally and will not create any extra sets. If we 
take the last example and add concatination string to be __ (double underscore)
we would get:

.. code-block:: django

    {% renderit auth.user hompage custom with prefix=logininfo group=users concat="__" %}
    
Generated List::

    ['/renderit/users/logininfo__auth__user__homepage__custom.html',
     '/renderit/users/logininfo__auth__user__homepage.html',
     '/renderit/users/logininfo__auth__user.html',
     
     '/renderit/users/auth__user__homepage__custom.html',
     '/renderit/users/auth__user__homepage.html',
     '/renderit/users/auth__user.html',
     
     '/renderit/logininfo__auth__user__homepage__custom.html',
     '/renderit/logininfo__auth__user__homepage.html',
     '/renderit/logininfo__auth__user.html',
     
     '/renderit/auth__user__homepage__custom.html',
     '/renderit/auth__user__homepage.html',
     '/renderit/auth__user.html',
     
     '/renderit/default.html']
     

