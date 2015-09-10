View
====

The view service deals with template rendering work-flow and its available in controllers as `view` property. All templates must be located under `/{module}/View/Template/{theme}/`, where {module} is a name of particular module, and {theme} is a theme name which has been defined in configuration file. All template files must end with `.phtml` extension.

For example, suppose you have a template in `/module/Site/View/Template/home.phtml` and you want to render it in controller's `indexAction`. To do so, you'd simply call `render()` passing `home` as a first argument. That would look like so:

    public function indexAction()
    {
    	return $this->view->render('home');
    }

Obviously, the controller must belong to the same module.

# Configuration

The configuration stored under `view` key in `components`. It typically contains a nested array with the following keys:

- theme - a name of the theme to be used, when doing render.
- obfuscate - defines whether to compress an output or not.
-  plugins - an array of asset plugins. A plugin itself should contain a name of a plugin as a key with and a nested array of definitions as a value. Path definitions should contain `scripts` and `stylesheets` with an array of paths.

For example, let's define `jquery` as a plugin:

    'view' => array(
    	'plugins' => array(
    		'jquery' => array(
    			'scripts' => array(
    				'@Site/js/jquery.min.js'
    			)
    			// jquery has no stylesheets, so we can omit that
    		)
    	)
    )

And then in any controller, we can simply load that plugin, like so:

    $this->view->getPluginBag()->load('jquery');

Note, that prefixing a module name with `@` is just defining a path to assets folder for that module, so that instead of writing:

    /module/Site/Assets/

you can simply write it as

    @Site/


## Available methods

You can call these methods on `view` service in controllers and in directly in templates.

## setComporess($compress)

Defines whether to compress outputting template at runtime.

## url($controller, ...)

Generates URL to given controllers action.

## addVariable($name, $value)

Adds a variable to template.

## addVariables($variables)

Add variables to the template. That must be an array with names with their associated values.

## getTheme()

Returns current theme name.

## asset($path, $module = null)

Returns a full-qualified asset path.

## setLayout($layout)

Sets the master template.

## disableLayout()

Disables the master template.

## hasLayout()

Determines whether a master template has been defined.

## templateExists($template)

Determines whether template exists.

## show($string, $translate = true)

Prints a string. If second argument is true, then it would try to translate it.

## render($template, array $vars = array())

Renders a template and returns it as a string. If a layout has been defined, it renders a template with defined layout.

## renderRaw($module, $theme, $template, array $vars = array())

Renders a template as a layout from a module. This is useful if you want to render a file for email-attachement. 
In fact, it's been added exactly for this purpose.

## loadBlock($block)

Loads a block. To learn more about defining blocks, refer to `BlockBag`.

## loadBlocks($array $blocks)

Loads many blocks at once.

## translate($string)

Returns a translated string.



