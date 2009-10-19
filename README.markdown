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

Or, if you prefer:

    $cats->setting('table', 'cats');
    $cats->color = 'brown';
    print_r($cats->first);

In most cases, you will want to have a place to modify the models that are returned by the above, so you can do the following, instead:

    class Cat extends SqloopBase { }
    print_r(Cat::find->color('brown')->first);

All of the above will generate the exact same query, which is:

    SELECT * FROM cats WHERE color = 'brown' LIMIT 1;

This query is not executed until the first() method is called.

There are some things that you can place in your class definition to make your tables act differently. For instance, sqloop assumes that your table is named the same as your class with an s at the end, which is frequently not true. You can also manage relationships between tables:

    class Cat extends SqloopBase
    {
      static function setup(){
        Cat::table = 'kittehs';
        Cat::belongs_to_a('Person');
        Cat::has_many_of('LitterBox')->through('kitteh_litterbox');
      }
    }

All methods with no arguments can be called without parenthesis. As such, first is actually a method call and can be accessed as either ``->first()`` or ``->first``.

Sqloop is written to be used as a library, but is also included as a plugin which is installed by default with *pea*, another of chrisrhoden's projects. *peadmin*, the administrative interface for pea, relies on sqloop.

Full documentation can be found in the github project's online wiki, at http://wiki.github.com/chrisrhoden/sqloop