Controllers
===========

A controller is just a standalone class that implements so-called "action methods" that respond to route matches. A route map itself must be defined in your `Module.php` in `getRoutes()` method. All controller classes must follow PSR-0 and must be located under `Controller` directories.

A typical directory structure, looks like so:

    module
     - Site
       - Controller
         - Main.php

Here two rules for all controllers:

[1] All your controller classes must extend `\Krystal\Controller\AbstractController`.
[2] Action methods must be public and must return a response string.




    namespace Site\Controller;
    
    use Krystal\Controller\AbstractController;
    
    class Main extends AbstractController
    {
    	public function indexAction()
    	{
    		return 'Welcome';
    	}
    }

As a best practice, controllers must be slim. That means, they only have to know how to call methods and pass resulting values to view. They should never process data, and contain complicated logic. If you have a logic, that must be in your model layer.

Routes
======

As mentioned before, routes are defined in `getRoutes()` method in your `Module.php`.


    <?php
    
    namespace Site;
    
    use Krystal\Module\AbstractModule;
    
    class Module extends AbstractModule
    {
    	// ...
    
    	public function getRoutes()
    	{
    		return array(
    			'/' => array(
    				'controller' => 'Welcome@indexAction'
    			)
    		);
    	}
    
    	// ...
    }
    
    ?>

That means, when route `/` is matched, the controller under target module (which is `Site`) named `Welcome` will be instantiated and its `indexAction` method will be executed and its response will be returned automatically.

Route configuration
===================

There's only one thing that can be configured - default action, that's a method which gets executed when no route maches found. It can be found in configuration file (which is usually located at `/config/app.php`) under `components -> route` section. Here's how it looks like:

    // ...
    
    'router' => array(
    	'default' => '...',
    ),
    
    // ...

To define a default method when no route match is found, simply define a controller as if you were defining it in route map, prepending module name. For example,

    'default' => 'Site:Welcome@notFoundAction'

Then `/Site/Controller/Welcome::notFoundAction` will be executed when no route matches are found.

# Route variables

It's very common to have variables in URI string. Route variables are defined in route map as `(:var)` keyword, like this:

    /page/(:var) => array(
    	'controller' => 'Foo@indexAction'
    )

Then variables are automatically become available as arguments in controller actions.

    public function indexAction($id) // <- The value of (:var) will be passed here
    {
    
    }

If you define several variables, they will be passed as arguments exactly as in defined order.

# Triggering 404 error

To trigger 404 error manually, you can simply return `false` in controller action. Once you do, then the default action will be executed. That is useful, if you want to handle call to invalid parameters.

For example, that might look like so:

    public function indexAction($id)
    {
    	$id = '...do fetch some record ...';
    
    	if (!$id) {
    		// If record isn't valid, then trigger 404
    		return false;
    	} else {
    		// Otherwise, process it somehow
    	}
    }

