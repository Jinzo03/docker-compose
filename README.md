# LibraryCRUD-Angular-Spring-MySql
A simple Library Application using CRUD operations with 3-tier architecture (frontend, backend and relational database). 

## Stack:
```

## Run the containerized application

Docker Compose builds and starts the Angular frontend, Spring Boot API, and MySQL database:

```sh
docker compose up --build
```

Open the application at <http://localhost:8081>. MySQL data is stored in the `mysql_data` named volume and survives container recreation. To stop the services, run `docker compose down`; to also delete the database volume, run `docker compose down -v`.

The Compose file uses development-only default database passwords. For a shared or deployed environment, create a `.env` file and set `MYSQL_PASSWORD` and `MYSQL_ROOT_PASSWORD` to strong values. To prepare image names for Docker Hub, also set `DOCKERHUB_USERNAME` and optionally `IMAGE_TAG`:

```dotenv
DOCKERHUB_USERNAME=your-dockerhub-username
IMAGE_TAG=latest
MYSQL_PASSWORD=change-this-password
MYSQL_ROOT_PASSWORD=change-this-root-password
```

Build the images and sign in to Docker Hub before pushing:

```sh
docker compose build
docker login
docker compose push
```

Deploy on a Docker host with the same Compose file and image settings using `docker compose pull` followed by `docker compose up -d`. The MySQL service is built from the official image and is not pushed as an application image.

The frontend sends API requests to `/api`, which its Nginx container proxies to the backend. The backend connects to MySQL using the Compose service name `mysql`.
Angular 16 
Spring Boot
MySql
```

### Overview Frontend:
1) Each Book has id, title, category, availability.
2) We can create, retrieve, update, delete Books.
3) There is a Search bar for finding Books by title.

<img src="https://github.com/andrea-attardo/LibraryCRUD-Angular-Spring-MySql/blob/main/biblioteca1.png?raw=true" width="500" >
<img src="https://github.com/andrea-attardo/LibraryCRUD-Angular-Spring-MySql/blob/main/biblioteca2.png?raw=true" width="500" >

#### Rest API contract:
```
Methods	    Urls	                    Actions
POST        /api/books	              create new Book
GET         /api/books	              retrieve all Books
GET         /api/books/:id	      retrieve a Book by :id
PUT         /api/books/:id	      update a Book by :id
DELETE      /api/books/:id	      delete a Book by :id
DELETE      /api/books	              delete all Books
GET         /api/books?title=[keyword]	find all Books which title contains keyword
```


### To run frontend on local:

1) Install Angular 16: https://kinsta.com/knowledgebase/install-angular/
2) ```git clone https://github.com/andrea-attardo/LibraryCRUD-Angular-Spring-MySql.git```
3) ```cd librarycrud-frontend```
4) ```ng serve --port 8081```


## Overview Backend:
A Rest CRUD API using Spring Boot, Spring Data JPA, Hibernate, Maven to interact with MySQL.

<img src="https://github.com/andrea-attardo/LibraryCRUD-Angular-Spring-MySql/blob/main/project_structure.png?raw=true" width="250" >


+ Book and Category data model class corresponds to entity and table on database.
+ Category field in Book entity is not a proper key but a ManyToMany relantionship with Category entity with unique constrain (category's name).
+ BookRepository and CategoryRepository is an interface that extends JpaRepository for CRUD methods and custom finder methods. 
+ BookController and CategoryController is a RestController which has request mapping methods for RESTful requests such as: findAll, findById, create, save, update, delete.
+ Configuration for Spring Datasource, JPA & Hibernate in application.properties.
+ pom.xml contains dependencies for Spring Boot and MySQL.

#### Run backend:

1) From terminal run: ```mvn spring-boot:run ```

## Setup a local Database:

1) Install MySql locally: https://dev.mysql.com/downloads/installer/

2) Under src/main/resources folder, open application.properties and modified these lines with your local values.
```
spring.datasource.url= jdbc:mysql://localhost:3306/librarydb?useSSL=false
spring.datasource.username= root
spring.datasource.password= 123456

# for Spring Boot 2
# spring.jpa.properties.hibernate.dialect= org.hibernate.dialect.MySQL5InnoDBDialect

# for Spring Boot 3
spring.jpa.properties.hibernate.dialect= org.hibernate.dialect.MySQLDialect

# Hibernate ddl auto (create, create-drop, validate, update)
spring.jpa.hibernate.ddl-auto= update
```
3) Run MySQl database locally before run backend.
   

