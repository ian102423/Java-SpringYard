# SpringYard! Getting Started

Start SpringYard and add some functionality.

## Introducing... SpringYard!  

Starting with this project, you will be putting together an "aggregate" project. In other words, future activities and projects will build on what you're creating here. We're going to refer to this as the "SpringYard" project. Going forward you will see things like "Open up your SpringYard project and add X". You might want to consider including this project as part of your GitHub profile when you're done.

## Getting Started  

For this part of the project you will be:

- Creating a customer database in postgres
- Creating a Spring Boot project with JDBC and postgres dependencies
- Importing the project into IntelliJ
- Working on a git `jdbc` branch
- Modeling the database entries as a POJO
- In the repository layer (package) using `JdbcTemplate` to CRUD the database
- In the service layer (package) passing through the calls to the repository layer
- Wrapping the methods that change the database (insert, update, delete) in a transaction
- Testing the service layer

## Setup postgres  

Start the postgres server if it is not already running.

The database and table introduced in this lesson will be used for future activities, projects and lessons (SpringYard).

Create a postgres user. Type `customerpass` when prompted for a password

- `createuser customeruser -l -P`

Create a database that allows the `customeruser` access

- `createdb -O "customeruser" customerdb`

Log in

- `psql -U customeruser customerdb`

While logged in create a customer table

- `create table customer (id serial not null, firstName text not null, lastName text not null, phone text, email text, constraint customer_pkey primary key (id));`

While logged in verify the table was created

- `\d` the output should be similar to

```
Schema |      Name       |   Type   |    Owner     
--------+-----------------+----------+--------------
 public | customer        | table    | customeruser
 public | customer_id_seq | sequence | customeruser
```

Log out of the psql command line

- `\q`

## Spring Boot Setup  

The spring boot project that will be used for this lesson will be used for subsequent lessons.

Bring up the **[Spring Initializr](https://start.spring.io/)** web page.

- Add `JDBC`, `PostgreSQL` and `Thymeleaf` as dependencies.
- Leave the Group alone but change the Artifact to "customer".
- Click the Generate Project button to download it.
- Unzip it

Now import the project in IntelliJ

- Open IntelliJ
- Click File -> New -> Projects from Existing Sources...
- Select the pom.xml and click OK
- On all the subsequent windows the defaults should be fine

## Setup GitHub  

In this section you will be setting up a GitHub repo and branching. If you are comfortable with the basic setup, feel free to skip down to "Branching"

#### GitHub Initialization  

In the customer directory create a local git repository

- `git init`
- `git add .`
- `git commit -m "initial version"`

Take note that a perfectly good `.gitignore` file already exists.

Create a `README.md` file. Add the following line to this file (without the double quotes)

- "Spring boot project used to implement Repositories (i.e. JDBC, Transactions and JPA), Controllers and Services"
Add the README.md file

- `git add .`
- `git commit -m "readme"`

Create a GitHub repository in the browser but DO NOT add a .gitignore or a README or a license. Copy the repository URL.

Back in the terminal in the aggregate directory add the remote repository.

- `git remote add origin <paste url from clipboard>`

Push the local repository to the remote repository.

- `git push origin master`

#### Branching to `jdbc`  

Now work on a "jdbc" branch

- `git checkout -b jdbc`

Each time we add to this project we will work on a branch and once done merge into master. This is an excellent coding habit to develop.

### Model the Database Table  

From the `com.example.customer` package create a subpackage `model`. In the `com.example.customer.model` package create a JavaBean that matches the customer table (you created this in "Setup postgres" above).

### Customer Repository Interface and Implementation  

From the `com.example.customer` package create a subpackage `respository`. In the `com.example.customer.repository` package create a `CustomerRepository` interface that defines CRUD methods:

- Add a customer
- Update a customer
- Get a customer by id
- Get all customers
- Delete a customer

In the `com.example.customer.repository` package create a `CustomerRepositoryImpl` class that implements the `CustomerRepository` interface.

### Customer Service Interface and Implementation  

From the `com.example.customer` package create a subpackage `service`. In the c`om.example.customer.service` package create a `CustomerService` interface that are the same as the CustomerRepository interface methods.

In the `com.example.customer.service` package create a CustomerServiceImpl class that implements the `CustomerService` interface.

The methods in this class "pass through" the calls to the auto wired `PersonRepository`

Remember to annotate any methods that change (insert, update, or delete) the database with the `@Transactional` annotation.

### application.properties  

Update the `application.properties` file with the connection information.

### Customer Service Test  

In the test tree create a `com.example.customer.service` package. In it create a `CustomerServiceTest` test for `CustomerService`.

Do not write a test for `CustomerRepository`. The reason for this will be explained when we move to using an ORM (Object Relational Mapping).

Write a `testAddGet()` method which will test to make sure you can successfully add and retrieve records. Run it.

If time permits, also write a `testUpdate()` method and a `testDelete()` method. Run them.

### GitHub Merge  

Now that the code is written and the tests run commit and push the code to the jdbc branch and then merge into master.