bottle-MethodsClass
===================

OOP method routes for Bottle.   Allows the creation of a object (similar to a child of webapp2.RequestHandler or flask.views.MethodView) to route requests via their HTTP method.

USAGE
=====
```python
from bottle_methods import Methods, app
 
class HelloHandler(Methods):
    route = '/<name>'
    def get(self, name=None):
         name = name if name else "World"
         return "Hello %s !" % name.title()
 
app.run()
```
