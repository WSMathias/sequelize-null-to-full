# sequelize-null-to-full  

#### sequelize is a popular ORM used in nodejs.  

This doccument focus on sequelize-cli with MySQL.
if installing globally (recommended):
    ```
    # npm i -g sequelize-cli
    ```
    or inside node project
    ```
    $ npm i --save-dev sequelize-cli
    ```
### accesssing sequelize-cli:  
if installed globall  
    ```
    $ sequalize <options>
    ```
or if installed for project  
    ```
    $ ./node_modules/.bin/sequalize <options>
    ```
### Using sequelize-cli:  
To initialize Inside project root directory run:  
    ```
    $ sequelize init
    ```
Above command will create few folders shown below:  
```
    /config/config.json
    /models/index.js
    /migrations
    /seeders
````
#### config.json  
Change the config.json acording to your database:  
according to environment  
```
    "username": "root"
    "password": ""
    "database": "TestDB"
    "host": "127.0.0.1"
    "dialect": "mysql"
```
Note: if projecting is using docker then host will be service name of the database container  
### Creating database:  
create databse specified in config.json  
    ```
    $ sequelize db:create
    ```
### Creating models:  
To create model using cli  
    ```
    $ sequelize model:generate --name User --attributes firstName:string,lastName:string,email:string
    ```
Above command creates two files  
```
/models/user.js
/migrations/xxxxxxxxxxxx-create-user.js
```
where xxxxxxxxxxxx is numbers from datetime ex:20180322160029  
The migration files are used to track the changes in models   
for more customisation alter model (user.js) and create new migration file by incrementing xxxxxxxxxxxx-update-user.js  
