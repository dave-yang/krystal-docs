Validate
=======

As a rule of thumb, you should never trust data that comes from the request. Almost everything that supplied by end-users must be validated in some or another way, if you want to build a secure system. Krystal's validation component would help you to manage validation rules for input fields.

# Getting started

Validation should be done only in controllers. Controllers do instantiate a validation service and determine whether user's data looks valid. Then depending on that, you either return error messages or do the upcoming thing, such as inserting or updating a record.

Validation component has only one service, which is called `validatorFactory` and is available as a property in controllers. To instantiate a validator, you have to call `build()` on it providing configuration.

As an example of typical initialization, it might look like so:

    $formValidator = $this->validatorFactory->build(array(
    	'input' => array(
    		'source' => $this->request->getPost(),
    		'definition' => array(
    			'...field...' => '...rules...'
    		)
    	)
    ));

Let's de-construct that step by step.

1. First of all, we define a target input in `input -> source`, which is a copy of $_POST
2. Second in `input -> definition` we define a target key in the source we specified and attach validation rules on it.

Now let's learn how to defile validation rules.

# Defining validation rules

A validation rule for a field defines whether a field is optional and its collection of constraints. The `required` key specifies whether to run validation on particual field in case it's not empty. For example, when setting `requred` to `false`, the validation will be runned in case the target field isn't empty. The `rules` key defines an array with constraints and their associated options to be applied to that particular input.

For example, suppose you have a simple form, that has only `email` field and it looks like this:

    <form>
      <input type="text" name="email" />
    </form>

Typically, for email fields we have two requirements: it must be filled and it must look like as a real email-address. To implement such validation rule, we'd define two built-in constraints `NotEmpty` and `EmailPattern`.

    $formValidator = $this->validatorFactory->build(array(
    	'input' => array(
    		'source' => $this->request->getPost(),
    		'definition' => array(
    			'email' => array(
    				'required' => true,
    				'rules' => array(
    					'NotEmpty' => array(
    						'message' => 'Email cannot be blank'
    					),
    					'EmailPattern' => array(
    						'message' => 'Invalid email format'
    					)
    				)
    			)
    		)
    	)
    ));

After instantiating the validation service, you would usually want to run validation process itself. And if it fails, you would want to retrieve error messages. To do so, the validation service has two methods: `isValid()` and `getErrors()`

    if ($formValidator->isValid()) {
    	// Do the rest
    
    } else {
    
    	// Fail and return error messages
    	return $formValidator->getErrors();
    }

# Input constrains

@TODO

# Validating files

Sometimes in your forms, you might have a `file` input that lets your users to upload files. In case you expect users to upload a photo, then you would want to make sure that they selected an image, not a document or malicious php-script. To validate files, you'd simply append new `file` input. Its configuration is defined in exactly way as in `input`.

    $formValidator = $this->validatorFactory->build(array(
    	'input' => array(
    		'source' => $this->request->getPost(),
    		'definition' => array(
    			'...field...' => '...rules...'
    		)
    	),
    	'file' => array( // <- Append this new file input
    		'source' => $this->request->getFiles(),
    		'definition' => array(
    			'...field...' => '...rules...'
    		)
    	)
    ));

# Files constraints

@TODO

# Validation patterns

If you develop large application, you'd soon note how much duplicated rules you have. For example, in several places you might have rules for `title` field, that are completely identical. In order to reduce duplication in validation rules and make them reusable, you can use validation patterns. Validation patterns are represented via an object that internally returns an array of definitions.

So instead, of writing rules like so.

    'definition' => array(
    	'...field...' => '...rules...'
    )

you can now write patterned rules like so:

    'definition' => array(
    	'...field...' => new Pattern\SomePattern()
    )

