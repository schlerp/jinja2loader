jinja2loader
============

This package is to be used with simple cherrypy applications needing a quick
and easy way to load templates from the file system.

Importing and Setup
-------------------
In order to use this module correctly you must import it and then set the
`template_directory`. The `template_directory` is initially set to be
`os.path.join(os.getcwd(), 'templates')`.

If your templates are stored in './templates/' then you can just import this
module. ::
    
    import jinja2loader

However, if your templates are in another directory structure you will
need to import the module and then set the template `template_directory`. A
helper function has been create to allow this, you can use it via calling
`jinja2loader.set_template_directory('template_directory')`. ::
    
    import jinja2loader
    jinja2loader.set_template_directory('../resources/templates')


load_template
-------------
This Function wraps an application method which returns the
context for a template, this decorator takes a single arguement, which is
the `template_name` to pass to `Environment.get_template(template_name)`
When using this decorator all you need to do from your application
function is to return the context you want to pass into the template. In
addition you need to specify the `template_name` as the argument to the
decorator like `@load_template('template_name.html')`
See example below which loads a template 'test.html' using
`Environment.get_template(template_name)`. ::

    class Application(object):
        @cherrypy.expose
        @load_template('test.html')
        def index(self):
            context = {'name': 'derp',
                       'name_list': ['bleep',
                                     'blerp',
                                     'herp']}
            return context

The above example will result in the template 'test.html' being rendered
with the context from the function `Application.index()`
If nothing is passed in as the context like in the below example, the
decorator method will automatically replace this with an empty `Dict`
before the call the render. ::

    @load_template('test.html')
    def index(self):
        return

The above example will return the 'test.html' template, this is useful for
loading and returning static pages which do not require context.
