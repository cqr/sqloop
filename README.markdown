Sqloop is a very basic ORM for PHP. Right now, it is tied to MySQL, though there is no reason that it will remain so in the long term because it uses a separate object to actually build the query from the properties and settings applied to it.

Sqloop is designed to be usable in a number of ways. The most basic is to create a new *SqloopObject* and start setting properties:

    $cats = new SqloopQuery;
    $cats->setting('table', 'cats');
    $cats->property('color', 'brown');
    print_r($cats->first);

Most methods return *$this*, so they can be chained. The above can be written as:

    print_r($cats->setting('table', 'cats')->property('color', 'brown')->first);

You can also use a shorthand for setting properties, like so:

    print_r($cats->table('cats')->color('brown')->first);

In most cases, you will want to have a place to modify the models that are returned by the above, so you can do the following, instead:

    class Cat extends SqloopBase { }
    print_r(Cat::find->color('brown')->first);

All of the above will generate the exact same query, which is:

    SELECT * FROM cats WHERE color = 'brown' LIMIT 1;

This query is not executed until the first() method is called.

All methods with no arguments can be called without parenthesis. As such, first is actually a method call and can be accessed as either ``->first()'' or ``->first''.
