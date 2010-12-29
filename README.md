# Data-mullet

Data-mullet is a NoSQL-style API for SQL databases

* [Data-mullet Home Page](https://datamullet.com)

## Example

#### Insert a document and create a new column named 'x' in the SQL database

###### PYTHON

	str = POST("https://user:pwd@datamullet.com/users/stuff/_insert'",
	                   params = {'docs' : '[{"foo" : "bar"}]'},
	                   async = False )

###### CURL

	curl -u user:pwd --data 'docs=[{"x":1}]' 'https://datamullet.com/users/stuff/_insert'

###### PHP

	$conn = new Mullet;
	$coll = $conn->users->stuff;
	$doc = array( 
	   "x" => 1
	);
	$coll->insert( array($doc) );

## Installation

Unzip the archive and copy Mullet.php to your Web root:

    tar xvzf datamullet-0.1.tar.gz
    cd datamullet-0.1
    cp Mullet.php /path/to/htdocs/

.htaccess:

    RewriteEngine on
    RewriteCond %{REQUEST_FILENAME} !^/index.php
    RewriteCond %{DOCUMENT_ROOT}/%{REQUEST_FILENAME} !-f
    RewriteRule ^/(.+)$ /index.php/$1

## Roadmap

What we're working on:

* Create some documentation
* Set up a wiki
* Contribute to http://datamullet.posterous.com

## HTTP API

say "hello"
-------
curl -X GET 'http://example/_hello'

list of databases
-------
curl -X GET 'http://example/_all_dbs'

create a new database ("mydb")
-------
curl -X PUT 'http://example/mydb'

create collection ("mystuff") and insert a document ( 'x' => 1 )
-------
curl --data 'docs=[{"x":1}]' 'http://example/mydb/mystuff/_insert'

find a document
-------
curl -X GET 'http://example/mydb/mystuff/_find'

update a document
-------
curl --data 'criteria=[{"x":1}]&newobj=[{"x":2}]' 'http://example/mydb/mystuff/_update'

remove a document
-------
curl --data 'criteria=[{"x":1}]' 'http://example/mydb/mystuff/_remove'

## PHP API

connect to back-end ("party")
-------
$conn = new Mullet();
$conn = new Mullet('sqlite:/var/www/file');

create a database 'mydb' and collection 'mystuff' ("business")
-------
$coll = $conn->mydb->mystuff; 

insert document into database and create tables (mydb_mystuff) 
and fields (followers,profile,bookmarks)
-------
$doc = array( 
   "followers" => 1,
   "profile" => (object)array(
	    "name" => "Fremont",
      "id" => 102
   ),
   "bookmarks" => array(
	    09386,
	    5204785,
	    938175
	  )
);
$coll->insert( $doc );

find a document
-------
$coll->findOne();

update mydb_mystuff set x = 2 where x = 1
-------
$coll->update( array( 'x' => 1 ), array( 'x' => 2 ));

delete from mydb_mystuff where x = 1
-------
$coll->remove( array( 'x' => 1 ));

find up to 15 documents
-------
$cursor = $coll->find();
foreach ($cursor as $id => $value) {
    echo "$id: ";
    var_dump( $value );
}

find up to 15 documents with criteria
-------
$query = array( "x" => 1 );
$cursor = $coll->find( $query );
while( $cursor->hasNext() )
    var_dump( $cursor->getNext() );


