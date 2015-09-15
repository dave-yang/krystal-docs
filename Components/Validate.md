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


In case a constraint has parameters, they must be defined as array in `value` section. In case there's only one parameter, you can define it as a value itself. For example, for `Between` constraint the definition might look so:

    'value' => array(1, 10)

For `RegExMatch` constraint, that might look so:

    'value' => '~(.*)~'

### Between

Checks whether numeric value is in range between specified values.

Parameters: First value is a starting range. Second value is an ending range.

### Boolean

Checks whether provided value looks as a boolean. Non-empty values, `true`, '1' are considered as true.

### Callable

Checks whether provider string value can be considered as PHP callable. For example, that can be a function name, or a class method in a format like `Class::method`.

### CAPTCHA

Validates CAPTCHA's response.

Parameters: Requires CAPTCHA service.


### Charset

Checks whether provided charset is supported by current PHP environment.


### Contains

Checks whether string contains a character defined in collection.

Parameters: A string that represents a character to be matched.
Or an array of such string.

### DateFormat

Checks whether string looks as a date format.

### DateFormatMatch

Checks whether string matches a particular date format.

Parameters: Date format, or an array of date formats.


### Day

Checks whether string value is less than 31.


### DirectoryPath

Checks whether string value is a path to a directory.


### Domain

Checks whether value looks like as a domain.


### EmailPattern

Checks whether a string looks like as email address.

### Empty

Checks whether string is empty.


### EndsWith

Checks whether string ends with a value.

Parameters: The value to be checked at the end itself.

### Even

Checks whether numeric string is even.

### Extension

Checks whether string contains an extension.

Parameters: The extension itself without a dot.


### FilePath

Checks whether value points to a file path on the file-system.

### FileReadable

Checks whether value points to a file path, which is readable.

### FileSize

Checks whether file size is expected

Parameters: File size in bytes

### Float

Checks whether value is float.


### GreaterThan

Checks whether value is greater than.

Parameters: The value to be compared against.


### Identity

Checks whether values are identical.

Parameters: Target value to be compared.


### InCollection

Checks whether a value in collection (i.e belongs to particular set of values).

Parameters: The array of values itself (i.e the collection)

### Integer

Checks whether value is an integer.

### IpPattern

Checks whether value looks as IP address.

### Json

Checks whether value represents JSON string.

### LessOrEqual

Checks whether value is less than or equals.

Parameters: The target value to be compared.

### LessThan

Checks whether value is less than.

### Lowercase

Checks whether a value is lower-cased.

### MacAddress

Checks whether value looks as a MAC-address.

### MaxLength

Checks the maximal length of a value.

Parameters

The maximal length.

### MinLength

Checks the minimal length of a value.

Parameters

The minimal length.

### Negative

Checks whether a value is negative.

### NoChar

Checks whether a value doesn't contain a character.

Parameters: An array of characters or a character string to be matched against a value.

### NotEmpty

Checks whether a value isn't empty.

### Numeric

Checks whether a value is numeric.

### Odd

Checks whether a value is odd.

### Positive

Checks whether a value is positive.

### RegEx

Checks whether regular expression is valid.

### RegExMatch

Checks whether a value matches regular expression.

Parameters: Regular expression to be matched against a value.


### Serialized

Checks whether a value has been serialized.

Parameters: Serialization adapter. To learn more, refer to Serialization component.

### StartsWith

Checks whether a value starts with a character.

Parameters: The character itself.

### Timestamp

Checks whether a value looks as a UNIX-timestamp.

### Uppercase

Checks whether a value is upper-cased.

### UrlPattern

Checks whether a value looks as URL.

### XDigit

Checks whether a value is like XDigit.

### Year

Checks whether a value looks like a year.


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

