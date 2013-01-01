# !/usr/bin/env python
#
# Copyright (c) 2013, Luke Southam <luke@devthe.com>
# All rights reserved.
# http://opensource.org/licenses/BSD-3-Clause
"""OOP method routes for Bottle.

Allows the creation of a object (similar to a child of webapp2.RequestHandler)
to route requests via their HTTP method.

USAGE:
    >>> from bottle_methods import Methods, app

    >>> class HelloHandler(Methods):
    ...     route = ['/', /<name>']
    ...     def get(self, name=None):
    ...         name = name if name else "World"
    ...         return "Hello %s !" % name.title()

    >>> app.run()
"""
from bottle import Bottle

# The HTTP methods allowed
# Lower case for PEP8 python method names
METHODS = ["get", "post", "put", "delete"]


class MethodsMeta(type):
    def __new__(cls, name, bases, attrs):
        """Searches through the class's attrs for methods with the same name
        as HTTP ones and adds the bottle.request decorator to them.
        The route is set via self.route ."""
        if 'route' in attrs:  # to allow inheritance
            # Get the given route
            # or routes if a list is provided
            routes = attrs['route']\
                    if isinstance(attrs['route'], list)\
                    else [attrs['route']]

            # Go through the class's attributes
            for key, value in attrs.iteritems():
                # Only add decorator if HTTP method.
                # Allow uppercase methods but don't recommend it.
                if key.lower() in METHODS:
                    # Allows self arg in methods otherwise not passed
                    add_cls = lambda *args, **kwargs: value(cls,
                                                            *args, **kwargs)
                    # Make it possible for multiple routes
                    for route in routes:
                        # Add the decorator here, also the 'value' function is modified
                        # based on the 2nd to last comment.
                        attrs[key] = app.route(route, method=key.upper())(add_cls)
        return super(MethodsMeta, cls).__new__(cls, name, bases, attrs)


class Methods(object):
    """Used to inherit the metaclass"""
    __metaclass__ = MethodsMeta

app = Bottle()