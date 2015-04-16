# Overview of the workshop

In this 4-hour workshop, participants will learn the basics of data modelling for relational databases, the relational database development process, and querying relational databases using SQL (Structured Query Language). The workshop will also present an overview of how relational databases are integrated into websites and other types of applications. The workshop will include a number of hands-on exercises and the chance to create, populate, and query a simple database.

# How relational databases work

Relational databases strucutre data in tables, and provide mechanisms for linking (relating) those tables together to so that the data can be queried and managed efficiently. For example, if we wanted to manage a list of books, we would create a table that contained some data about those books:

![Spacer image](assets/spacer.jpg)
![Some sample books](assets/sample_book_list.png)
![Spacer image](assets/spacer.jpg)

Each row in the table describes a single book, and the data is organized into columns, with each intersection of a row and a column containing a single piece of data. But if each intersection of a row and a column can contain only one piece of data, how do we handle data that can apply more than once to each book, such as its author? It's pretty common for a book to have more than one author.

Relational databases organize data into multiple tables, and link the tables together so that all the data about something (in our example, a single book) can be assembled from the relevant tables as needed. If we put data about authors in its own table, we can allow each book to have multiple authors. Because each book can have many authors, and each author can have written more than one book, we say that books and authors have a "many-to-many" relationship with each other. Relational databases accommodate this type of relationship by using a third table whose function is to relate the two things described in separate tables, as illustrated in this diagram:


![Books and authors](assets/BooksAuthors.jpg)


For this method of breaking up data into multiple tables to work reliably, we to ensure that each row in the books table and each row in the authors table can be referenced uniquely, we need to assign identifiers to each rown in the book and authors tables, and we use those identifiers to relate the two tables to each other in the third table.

"One-to-many" relationships don't use a third table. This type of relationship is expressed by linking two tables, one containing the data that is on the "one" side of the relationship and the other that is on the "many" side. For example, each book can have many editions, but each edition only applies to a single book:


![Books and editions](assets/BooksEditions.jpg)


One-to-many relationships require unique IDs to link the tables reliably, but unlike in the intermediate table used in the many-to-many relationship, the table that contains the data describing the "many" side of the relationship has a column reserved for the ID of the "one" side of the relationship. 

The IDs used to uniquely identify the things described in tables are called "primary keys". If these IDs are used in other tables, they are called "foreign keys" in those tables. For example, the "book_id" column in the Books table is that table's primary key, but the "book_id" column in the Editions table is a foreign key.

Putting together all of our tables, we get:

![Books and editions](assets/BooksAuthorsEditions.jpg)

Now that we've created our database, we can query it using SQL (Structured Query Language):

```sql
SELECT Books.book_id, title, ISBN, date_of_publication
FROM Books, Editions
WHERE Books.book_id = Editions.book_id
AND Editions.date_of_publication >  '2003'
```

This query returns the folowing results:

```
+---------+------------------------------------------------------+---------------+---------------------+
| book_id | title                                                | ISBN          | date_of_publication |
+---------+------------------------------------------------------+---------------+---------------------+
|       2 | Relational databases for really, really smart people | 9876543212345 |                2012 |
|       3 | My life with relational databases: a memoir          | 3212345678909 |                2005 |
|       3 | My life with relational databases: a memoir          | 3212345678909 |                2009 |
+---------+------------------------------------------------------+---------------+---------------------+
3 rows in set (0.00 sec)
```

This query asks for the authors of the book with book_id 1:

```
SELECT DISTINCT first_name, last_name
FROM Authors, BooksAuthors, Books
WHERE BooksAuthors.author_id = Authors.author_id
AND BooksAuthors.book_id = 1
```
The results are:

```
+------------+---------------+
| first_name | last_name     |
+------------+---------------+
| Christina  | Lopez Baranda |
| Hannah     | Jones         |
| Tandice    | Turay         |
+------------+---------------+
3 rows in set (0.01 sec)
```

## Selected relational database platforms

Common RDBMS systems: [MySQL](https://www.mysql.com/), [PostgreSQL](http://www.postgresql.org/), [MariaDB](https://mariadb.org/), [SQLite](http://www.sqlite.org/), [MS Access](https://products.office.com/en-us/access), [Filemaker](http://www.filemaker.com/products/overview.html), [MS SQL Server](http://www.microsoft.com/en-ca/server-cloud/products/sql-server/), [Oracle](https://www.oracle.com/database/index.html)

Tools for managing relational databases: command line, web-based management apps (e.g. web apps like [PHPMyAdmin](http://www.phpmyadmin.net/home_page/index.php) and [Adminer](http://www.adminer.org/), desktop management apps like [MySQL Workbench](https://www.mysql.com/products/workbench/)), custom applications.

The following is an "SQL file" produced by phpMyAdmin:

```sql
-- phpMyAdmin SQL Dump
-- version 4.0.10deb1
-- http://www.phpmyadmin.net
--
-- Host: localhost
-- Generation Time: Apr 16, 2015 at 07:16 AM
-- Server version: 5.5.41-0ubuntu0.14.04.1-log
-- PHP Version: 5.5.9-1ubuntu4.7

SET SQL_MODE = "NO_AUTO_VALUE_ON_ZERO";
SET time_zone = "+00:00";


/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!40101 SET NAMES utf8 */;

--
-- Database: `ClassSchedules`
--

-- --------------------------------------------------------

--
-- Table structure for table `classes`
--

CREATE TABLE IF NOT EXISTS `classes` (
  `class_id` int(11) NOT NULL,
  `date` date DEFAULT NULL,
  `time` time DEFAULT NULL,
  PRIMARY KEY (`class_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci;

-- --------------------------------------------------------

--
-- Table structure for table `courses`
--

CREATE TABLE IF NOT EXISTS `courses` (
  `course_id` int(11) NOT NULL,
  `coursescol` varchar(20) COLLATE utf8_unicode_ci DEFAULT NULL,
  PRIMARY KEY (`course_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci;

-- --------------------------------------------------------

--
-- Table structure for table `instructors`
--

CREATE TABLE IF NOT EXISTS `instructors` (
  `instructor_id` int(11) NOT NULL,
  `last_name` varchar(100) COLLATE utf8_unicode_ci DEFAULT NULL COMMENT 'The instructor''s last name (or name conventionally used in alphabetical lists)\n',
  `first_name` varchar(100) COLLATE utf8_unicode_ci DEFAULT NULL,
  `email_address` varchar(100) COLLATE utf8_unicode_ci DEFAULT NULL,
  PRIMARY KEY (`instructor_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci;

-- --------------------------------------------------------

--
-- Table structure for table `locations`
--

CREATE TABLE IF NOT EXISTS `locations` (
  `location_id` int(11) NOT NULL,
  `room_number` varchar(45) COLLATE utf8_unicode_ci DEFAULT NULL,
  `room_name` varchar(100) COLLATE utf8_unicode_ci DEFAULT NULL,
  `type` varchar(45) COLLATE utf8_unicode_ci DEFAULT NULL,
  PRIMARY KEY (`location_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci;

/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
/*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */;
/*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */;
```

The following screenshots show the database in Adminer and in MySQL Workbench:

![Adminer tables view](assets/adminer_tables_view.png)
Adminer tables view

![Adminer schema view](assets/adminer_schema_view.png)
Adminer schema view

![MySQL Workbench schema view](assets/mysqlworkbench_schema.png)
MySQL Workbench schema view

![MySQL Workbench ER diagram view](assets/mysqlworkbench_er_diagram.png)
MySQL Workbench ER diagram view

# Data modeling for relational databases

* Entity-relationship modeling
* Normalization

## Relational database development process

![Database development process](assets/DB_Development_process.jpg)

Lists, then ER diagrams

## Entity-relationship modeling

List all entities (things) and their attributes

Class scheduling database, whose purpose is to aid in the scheduling of classes in a given semester.

* Classes
  * Date
  * Time
  * Course
  * Location
* Courses
  * Number
  * Title
  * Instructor
  * Department
  * Semester [Do we need semsester if we have dates?]
* Locations
  * Room number
  * Room name
  * Building
  * Type (classroom, seminar, amphitheatre, etc.)
  * Built-in projector [maybe split out into Room Details table?]
* Instructors
  * Last name
  * First name
  * Department [Do we need department here and in Courses?]
  * Email address

[@todo: Insert complete ER Diagram version of the above list, for MySQL Workbench at least and maybe Adminer too?]

## Normalization

## First Normal Form

Each column/row intersection can contain only one value. In our class locations database, courses.instructor can only have one instructor ID. 

## Second Normal Form

Applies to association tables with a composite key. All non-key columns must describe the entire composite key.

## Third Normal Form

Second Normal Form for non-association tables. Non non-key column must be dependent on another non-key column.

## Fourth and Fifth Normal Forms

Don't worry about these.

# Populating and querying relational databases

```sql
SELECT * FROM foo WHERE id = 3;
```


![Example of an autocomplete field for selecting values from linked tables](assets/xataface_linked_table_example.png)
![Spacer image](assets/spacer.jpg)
Example of a user interface built using [Xataface](http://xataface.com/) for selecting values from linked tables. Image courtesy of John Dingle and Margaret Linley.

# Exercises

## Selecting data from the classes database

## Inserting data 

## Modifying data

# Integrating relational databases into applications

Examples: Wordpress database model (https://codex.wordpress.org/images/9/97/WP3.8-ERD.png); Firefox's history feature (https://developer.mozilla.org/en-US/docs/Mozilla/Tech/Places)

Database frameworks like [Symfony](http://symfony.com/) for PHP, [Django](https://www.djangoproject.com/) for Python, [Rails](http://rubyonrails.org/) for Ruby, [Play](https://www.playframework.com/) for Java. 

# Relational databases compared to other types of databases

NoSQL databases: Store non-tabular data. Examples include [CouchDB](http://couchdb.apache.org/), [MongoDB](https://www.mongodb.org/). Typical applications serve large-scale structured data or as complements to relational database applications. No standardized query language.

XML databases: Stores and queries XML documents, not tables. Typical application is for advanced queries against a set of XML documents using teh XPath or XQuery languages. Examples include [eXist](http://exist-db.org/), [BaseX](http://basex.org/).

Triplestores: Store statements comprised of subject, predicate, object as defined by RDF (Resource Description Framework). Typical application is in providing a search endpoint for Linked Data via the SPARQL query language. Examples include [Fuseki](http://jena.apache.org/documentation/fuseki2/index.html), [Virtuoso](http://virtuoso.openlinksw.com/).


# Exercises

## Data modeling for relational databases

Sample topics:
* Database that tracks which articles cite which other articles
* Personal music, book (or other) collection
* Research project status reporter (for producing periodic updates)


# License

Except where noted, <a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/80x15.png" /></a><br /><span xmlns:dct="http://purl.org/dc/terms/" href="http://purl.org/dc/dcmitype/Text" property="dct:title" rel="dct:type">Introduction to Relational Databases</span> by <span xmlns:cc="http://creativecommons.org/ns#" property="cc:attributionName">Mark Jordan</span> is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a>.