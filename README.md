# liquibase-example
This project could be a used as

  - a tutorial for learning the basics in liquibase
  - a base for dicussing on how-to-use liquibase in a collaboration development project
  
This project is a **simple example** of a liquibase-project with maven ( http://www.liquibase.org/ ).<br>
This liquibase-project is ready to run with MySQL and postgreSQL<br>
Dependencies to mysql- and posrgresql-connectors are found in the maven pom.xml

By stating this is simple example : Not using complex commands, nor using extension

This simple project contains 5 changesets :

  - 4 changesets that '*creates a table*' 
  - 1 changeset that '*alters a table*' by adding a column

# disucssion topic

In a project with multiple developers using a version control system (VCS)<br>

### working as a team
  - Guarantees that all the developers have the same schema on their local computer
    - how : by version controlling the changelog-files
  - Every developer has a unique liquibase.properties-file (contains schema&credentials)
    - how : ignoring the liquibase.properties-file (if  using git, update the .gitignore-file)

### updating the schema in a test-environment using a CI-tool

Check if you are able to update your database in your stage/test-enviroment with liquibase.

  1. Create a liquibase-project as a module amongs your other projects.
  1. version control the project.
  1. CI-tool: Run this project as a build-step before the module that depends on the db.
    1. **obs** : credentials are stored in the liquibase.properties-file

# prereq

**Database**

In this example the database is called '**denmark**'.<br>
You **have to** create the database before running the project<br>

  - see the liquibase.properties-file
    - setting: url=jdbc:mysql://localhost/**denmark**
    
**Software:**

  - java
  - [maven](https://maven.apache.org/) 
  - a database-engine of choice 
  
# important files for maven.

  - pom.xml
    - db: mysql-connector-java (version  '5.1.37')
    - db: postgresql (version '9.1-901-1.jdbc4')
    - liquibase: liquibase-maven-plugin 

# Files for the liquibase-project.

### Necessary files

  - **liquibase.properties**
    - contains : driver and url to the database
    - contains : credentials
    - contains : path to the master.xml-file
    - contains : additional such as ;  'verbose = true', '**dropFirst = false**'
  - **master.xml**
    - contains : path to 'db.changelog-x.y.xml' , x.y are version-nr.
  - **db.changelog-1.0.xml**
    - contains : all of your changesets (*note : every changeset has to has a unique id*)

You are able to have multiple 'db.changelog-x.y.xml'-files<br>
This project contains 2 files ; db.changelog-1.0.xml and db.changelog-5.0.xml <br>
In this example every file has its responsibility<br>
  - The db.changelog-1.0.xml contains all your changesets, 
  - The db.changelog-5.0.xml contains a reference to an .sql-file<br>
    - The default-insert-for-admin_config.sql contains example content.

### Other files

The following files are **not necessary** for the project.<br>
They are just here as a configuration example.

  - **liquibase.mysql.properties**
  - **liquibase.postgresql.properties**

## Writing changelogs in liquibase

In this example changesets are written in XML<br>
You are not restrained to using XML for changesets.<br>
Other formats are YAML, JSON and SQL<br>
[How-To-Documentation](http://www.liquibase.org/documentation/index.html) 

**Simple rules**

  - You create new changelogs in the changelog-file
  - You do not delete old changelogs
  - Every changelog has a unique id

# How to run the Liquibase-project
To run the project<br>
type '**mvn  clean install**' in the same directory that the pom.xml-file resides

Check your database <br>
The following 5 tables should have been created<br>

  - **3 tables defined in the changelog-files**
    - ADMIN_CONFIG 
    - IMAGE
    - MEDIA
  - **Additional 2 liquibase-tables**
    - DATABASECHANGELOG
    - DATABASECHANGELOGLOCK



# Additional stuff
### Creating db-documentation

command-line tool:

"*Using change information stored in the change logs and an existing database, Liquibase can generate database change documentation* "
[How-To-dbdoc](http://www.liquibase.org/documentation/dbdoc.html)


**NOTE :** 'javadoc'-style documenation is created with this tool

### Going from a 'legacy'-database to a changeset-file.

command-line tool:

"*When starting to use Liquibase on an existing database, it is often useful, particularly for testing, to have a way to generate the change log to create the current database schema. Liquibase allows you to do this with the “generateChangeLog” command_line command.*"
[How-To-Generate_changelogs](http://www.liquibase.org/documentation/generating_changelogs.html)

"*Note that this command currently has some limitations. It does not export the following types of objects:
Stored procedures, functions, packages,Triggers*"
