# Sequelize-null-to-full

#### sequelize is a popular ORM used in nodejs.

This doccument focus on sequelize-cli with MySQL.

if installing globally (recommended):  
```sh
$ npm i -g sequelize-cli
```  
or   
inside node project  
```sh
$ npm i --save-dev sequelize-cli
```
### Accesssing sequelize-cli:  
if installed globally  
```sh
$ sequalize <options>
```  
or  
if installed for project  
```sh
$ ./node_modules/.bin/sequalize <options>
```
### Using sequelize-cli:
To initialize Inside project root directory run:  
```sh
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
Change the config.json acording to your database 
according to environment
```json
    "username": "root"
    "password": "password"
    "database": "databse_name"
    "host": "127.0.0.1"
    "dialect": "mysql"
```  
_Note:  
If project is using docker-compose then host will be service name of the database container_  
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
##### user.js
```js
'use strict';
module.exports = (sequelize, DataTypes) => {
  var User = sequelize.define('User', {
    firstName: DataTypes.STRING,
    lastName: DataTypes.STRING,
    email: DataTypes.STRING
  }, {});
  User.associate = function(models) {
    // associations can be git defined here
  };
  return User;
};
```  
##### xxxxxxxxxxxx-create-user.js  
```js
'use strict';
module.exports = {
  up: (queryInterface, Sequelize) => {
    return queryInterface.createTable('Users', {
      id: {
        allowNull: false,
        autoIncrement: true,
        primaryKey: true,
        type: Sequelize.INTEGER
      },
      firstName: {
        type: Sequelize.STRING
      },
      lastName: {
        type: Sequelize.STRING
      },
      email: {
        type: Sequelize.STRING
      },
      createdAt: {
        allowNull: false,
        type: Sequelize.DATE
      },
      updatedAt: {
        allowNull: false,
        type: Sequelize.DATE
      }
    });
  },
  down: (queryInterface, Sequelize) => {
    return queryInterface.dropTable('Users');
  }
};
```

where xxxxxxxxxxxx is numbers from datetime.  ex:20180322160029  
The migration files are used to track the changes in models  
for more customisation alter model (user.js) and create new migration file to modify the table.  

```sh
$ sequelize migration:generate --name <migration-name>
```
It will generate
```
/migrations/xxxxxxxxxx-miration-name.js
```
```js
'use strict';

module.exports = {
  up: (queryInterface, Sequelize) => {
    /*
      Add altering commands here.
      Return a promise to correctly handle asynchronicity.

      Example:
      return queryInterface.createTable('users', { id: Sequelize.INTEGER });
    */
  },

  down: (queryInterface, Sequelize) => {
    /*
      Add reverting commands here.
      Return a promise to correctly handle asynchronicity.

      Example:
      return queryInterface.dropTable('users');
    */
  }
};

```
**up**:is called while migrating  
**down**:is called while undoing migration

## Model Validation:
you can specify validation on each attribute of modal
for validating user email.
```js
email: { 
    type: Sequelize.STRING(300),
    unique: true,
    validate: {
         isEmail: true
         }
    }
```  
whenever you alter model same need to be reflected in new migration file, ignore validation attribute in migration since they are not used in database its only used by sequelizer.
## Migration:  
Is the process of converting models in to tables in database.  
to migrate run:  
```sh
$ sequelize db:migrate
```  
which exicutes the migration files in the incremental order and stores the name of the miration file in table **SequelizeMeta**.This prevents already exicuted migration file from running again on next migration. 

_Note:  
All the changes made to the Model performed after executing the above command must be written in a new migration file._
### Tyeps of migrations
#### Add Column:
```js
queryInterface.addColumn('TableA', 'ColumnA', {
        allowNull: true,
        type: Sequelize.STRING,
      }).then((res) => {
        console.log('Added  ColumnA to  TableA');
      }).catch((err) => {
        console.log('Error! ', err);
      })
```
#### Remove Column:
```js
queryInterface.removeColumn('TableA', 'ColumnA').then((res) => {
        console.log('removed ColumnA  from TableA');
      }).catch((err) => {
        console.log('Error! ', err);
      }),
```
#### Rename Column:
```js
queryInterface.renameColumn('TableA', 'ColumOldName','ColumnNewName').then((res) => {
        console.log('ColumOldName renamed to ColumnNewName on TableA');
      }).catch((err) => {
        console.log('Error! ', err);
      }),
```

## Associations:
| Associations | usage in Model | Foreign key in table  |
|--------------|----------------|----------|
| belongsTo | Student.belongsTo(Department) | Student
| hasMany | Department.hasMany(Student) | Student 

### Migration file for _belongsTo_ and _hasMany_:
for belongsTo store foreign key source table  
for hasMany store foreign key in targer table 

_Note:  
To avoid confussion over column name set following options while defining model_
```js
freezeTableName: true,
underscored: true
```
#### In model file:
```js
Student.associate = function (models) {
    // associations can be defined here
    Student.belongsTo(models.Department);
  };
```
#### In migration file:
```js
ueryInterface.addColumn(
    'Student', // name of source model
    'department_id', // name of foreign key we are adding
    {
      type: Sequelize.INTEGER,
      references: {
        model: 'Department', // name of Target model
        key: 'id', // key in Target model that we're referencing
      },
      onUpdate: 'CASCADE',
      onDelete: 'SET NULL',
    },
  )
```
## Querying
TODO






