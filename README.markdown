Sqloop is a very basic ORM for PHP. Right now, it is tied to MySQL, though there is no reason that it will remain so in the long term because it uses a separate object to actually build the query from the properties and settings applied to it.

Sqloop is designed to be usable in a number of ways. The most basic is to create a new *SqloopQuery* and start setting properties:

    $pterodactyls = new SqloopQuery(new MySqloop($username, $password, $hostname, $database));
    $pterodactyls->setting('table', 'pterodactyls');
    $pterodactyls->property('kills', '>', 2);
    $pterodactlys->property('sex', 'male');
    print_r($pterodactyls->first);

Most methods return *$this*, so they can be chained. The above can be written as:

    print_r(
      $pterodactyls->setting('table', 'pterodactyls')
                   ->property('kills', '>', 2)
                   ->property('sex', 'male')
                   ->first
           );

You can also use a shorthand for setting properties, like so:

    print_r(
      $pterodactyls->table('pterodactyls')
                   ->kills('>', 2)
                   ->sex('male')
                   ->first
           );

Or, if you prefer:

    $pterodactyls->setting('table', 'pterodactyls');
    $pterodactyls->kills = array( '>' => 2 );
    $pterodactyls->sex   = 'male';
    print_r($cats->first);

In most cases, you will want to have a place to modify the models that are returned by the above, so you can do the following, instead:

    class Pterodactyl extends SqloopBase { }
    print_r(Pterodactyl::find->kills('>',2)->sex('male')->first);

All of the above will generate the exact same query, which is:

    SELECT * FROM pterodactyls WHERE kills > 2 AND sex = 'male' LIMIT 1;

This query is not executed until the first() method is called.

There are some things that you can place in your class definition to make your tables act differently. For instance, sqloop assumes that your table is named the same as your class with an s at the end, which is frequently not true. You can also manage relationships between tables:

    class Pterodactyl extends SqloopBase
    {
      static function setup(){
        Pterodactyl::table = 'pterodacti'; // I know, it should be pterodactyls, but that's a silly example.
        Pterodactyl::belongs_to_a('PterodactylRider');
        Pterodactyl::has_many_of('PterodactylEggs');
        Pterodactyl::has_many_of('PterodactylNest')->through('pterodactyls_nests_join'); // Many-to-many relation
      }
    }

All methods with no arguments can be called without parenthesis. As such, first is actually a method call and can be accessed as either ``->first()`` or ``->first``.

Sqloop is written to be used as a library, but is also included as a plugin which is installed by default with *pea*, another of chrisrhoden's projects. *peadmin*, the administrative interface for pea, relies on sqloop.

Full documentation can be found in the github project's online wiki, at http://wiki.github.com/chrisrhoden/sqloop