Standard library
=============

This component provides a set of low-level tools.

# Virtual Entity

    \Krystal\Stdlib\VirtualEntity

If you prefer to store and access data via getters and setters, writing getters and setters will become tedious very quickly. Moreover, that leads to code duplication. Krystal provides a special service to address this problem, which is called `VirtualEntity`. When using it, you don't have to write setters and getters, because they're dynamic. Also dynamic setters do support method chaining.

For example, you can use it like this:

    <?php
    
    use Krystal\Stdlib\VirtualEntity;
    
    $user = new VirtualEntity();
    $user->setName('Dave')
         ->setAge(24);
    
    echo $user->getName(); // Dave
    echo $user->getAge(); // 24
    
    var_dump($user->getSomethingThatIsNotDefined()); // null

# Dumper

    \Krystal\Stdlib\Dumper::dump($var, $exit = true)

This is just a dumper. that prints data in nice looking format. There's only one method called `dump()`. As a first argument it accepts a variable itself, and the second argument defines whether to terminate a script right after output is done.

You can use it like this:

    <?php
    
    use Krystal\Stdlib\Dumper;
    
    $var = '.....';
    
    Dumper::dump($var); // Will output the content nicely


# ArrayUtils

    \Krystal\Stdlib\ArrayUtils

That's just a class with static methods to deal with arrays.

## Available methods


### arrayPartition($array, $key)

Drops an array into partition with equalent keys. For example, consider the following array:


    $users = array(
        array(
            'name' => 'Dave',
            'age' => 50,
            'country' => 'USA'
        ),
        array(
            'name' => 'Dereck',
            'age' => 44,
            'country' => 'USA'
        ),
        array(
            'name' => 'William',
            'age' => 25,
            'country' => 'UK'
        )
    );

There are two users that have the same `country` value. Let's drop this array into the new one by `country` key:

    <?php
    
    use Krystal\Stdlib\ArrayUtils;
    
    $result = ArrayUtils::arrayPartition($users, 'country');
    
    print_r($result);

It produces the following output:

    Array
    (
        [USA] => Array
            (
                [0] => Array
                    (
                        [name] => Dave
                        [age] => 50
                        [country] => USA
                    )
    
                [1] => Array
                    (
                        [name] => Dereck
                        [age] => 44
                        [country] => USA
                    )
    
            )
    
        [UK] => Array
            (
                [0] => Array
                    (
                        [name] => William
                        [age] => 25
                        [country] => UK
                    )
    
            )
    )

### arrayDropdown($array, $key, $partition, $key, $value)

Drops an array into partitions and creates `key => value` pairs inside them. This method comes in handy, in case you need to prepare an array for `select` DOM element with nested `optgroup` tags .

For example, consider the following array:

    $users = array(
        array(
            'id' => 1,
            'name' => 'Dave',
            'age' => 50,
            'country' => 'USA'
        ),
        array(
            'id' => 2,
            'name' => 'Dereck',
            'age' => 44,
            'country' => 'USA'
        ),
        array(
            'id' => 3,
            'name' => 'William',
            'age' => 25,
            'country' => 'UK'
        )
    );

Let's make from this array a structure for dropdown-like element:

    <?php
    
    use Krystal\Stdlib\ArrayUtils;
    
    $result = ArrayUtils::arrayDropdown($users, 'country', 'id', 'name');
    
    print_r($result);

This produces the following output:

    Array
    (
        [USA] => Array
            (
                [1] => Dave
                [2] => Dereck
            )
    
        [UK] => Array
            (
                [3] => William
            )
    )

From initial array, we used `country` key as a partition name, and `id` with `name` as corresponding `key => value` pairs for the new array.

### arrayList($array, $key, $value)

Turns a row's array into a hash-like list. For example, consider this:

    <?php
    
    use \Krystal\Stdlib\ArrayUtils;
    
    $rows = array(
       array(
          'key' => 'name',
          'value' => 'Dave'  
      ),
      array(
         'key' => 'name',
         'value' => 'Jack'
      )
    );
    
    $list = ArrayUtils::arrayList($rows, 'key', 'value');

The resulting `$list` array would look like as following:

    array(
       'name' => 'Dave',
       'name' => 'jack'
    );


### arrayWithout($array, $keys)

Returns a filtered array by removing keys. For example:

    <?php
    
    use \Krystal\Stdlib\ArrayUtils;
    
    $array = array(
       'foo' => 'bar',
       'x' => 'y',
       'a' => 'b'
    );

    $result = ArrayUtils::arrayWithout($array, array('x', 'a'));

And the resulting array `$result` would look like as following:

    array(
      'foo' => 'bar'
    )


### search($haystack, $needle)

This method is similar to `array_search()`, expect that it searches recursively.

### arrayUnique($array)

Removes duplicate values from an array. The difference between `array_unqiue()` and this method is that can deal with multi-dimensional arrays as well, while `array_unqiue()` can't.

### hasAtLeastOneArrayValue($array)

This method determines whether at least one array's value is an array by type.

### isSequential($array)

Determines whether an array is sequential (i.e non-associative)

### isAssoc($array)

Determines whether an array is associative.




