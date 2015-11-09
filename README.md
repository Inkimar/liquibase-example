# liquibase-example
This project is a simple example of liquibase ( http://www.liquibase.org/ ) with maven.

This liquibase-project is ready to run with MySQL and postgreSQL<br>
Dependencies to mysql- and posrgresql-connectors are found in the pom.xml

#disucssion topic

In a project with multiple developers using a version control system (VCS)<br>

## working together
  - Enables the developers To have the same schema on their local computer
    - by Version controlling the changelog-files
  - Every developer has a unique liquibase.properties-file (contains schema&credentials)
    - ignoring the liquibase.properties-file (if  using git, update the .gitignore-file)

## deploying to a stage-environment using a CI-tool
  - Add a liquibase-project.
  - Run this project as a build-step in the CI-tool

#prereq

In this example the database is called '**denmark**'.<br>
You **have to** create this database  before running the project<br>

  - see the liquibase.properties-file
    - setting: url=jdbc:mysql://localhost/**denmark**
    
Software :

  - java
  - maven
  - a database-engine of choice 
  
#important files for maven.

  - pom.xml
    - db: mysql-connector-java version  '5.1.37'
    - db: postgresql version '9.1-901-1.jdbc4'
    - liquibase: liquibase-maven-plugin 

#Files for the liquibase-project.

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

You are not restrained to using XML for changesets.<br>
Other formats are YAML, JSON and SQL

#how to run the Liquibase-project
To run the project<br>
type '**mvn  clean install**' in the same directory that the pom.xml-file resides


# Additional
### Creating documentation
" *Using change information stored in the change logs and an existing database, Liquibase can generate database change documentation* "

http://www.liquibase.org/documentation/dbdoc.html

**Note : The documenation created with this tool is 'javadoc'-style**

### From a 'legacy'-database to a changeset.xml-file
"*When starting to use Liquibase on an existing database, it is often useful, particularly for testing, to have a way to generate the change log to create the current database schema. Liquibase allows you to do this with the “generateChangeLog” command_line command.*"

http://www.liquibase.org/documentation/generating_changelogs.html