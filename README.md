# Capacitor SQLite Plugin
Capacitor SQlite  Plugin is a custom Native Capacitor plugin to store permanently data to SQLite on IOS and Android platforms. 


    *********************************************************************
    WARNING this is an ALPHA version with only the IOS plugin implemented
    *********************************************************************


## Methods available

    open({name:string})                         open a database
    execute({statements:string})                execute a batch of raw SQL statements           
    run({statement:string,values:Array<Array<any>>})   run a SQL statement
    query({statement:string})                   query a SELECT SQL statement
    deleteDatabase({name:string})               delete a database

## Usage
### open
```
let result:any = await db.open({name:"testsqlite"});
``` 

### execute
```
      let sqlcmd: string = `
      BEGIN TRANSACTION;
      CREATE TABLE IF NOT EXISTS users (
          id INTEGER PRIMARY KEY NOT NULL,
          email TEXT UNIQUE NOT NULL,
          name TEXT,
          age INTEGER
      );
      CREATE TABLE IF NOT EXISTS posts (
          id INTEGER PRIMARY KEY NOT NULL,
          title TEXT NOT NULL,
          body TEXT NOT NULL,
          published_at DATETIME
      );
      PRAGMA user_version = 1;
      COMMIT TRANSACTION;
      `;
      const retExe: any = await db.execute({statements:sqlcmd});
      const res: Number = retExe.result // should be 0
```

```
      sqlcmd = `
      BEGIN TRANSACTION;
      DELETE FROM users;
      INSERT INTO users (name,email,age) VALUES ("Whiteley","Whiteley.com",30);
      INSERT INTO users (name,email,age) VALUES ("Jones","Jones.com",44);
      COMMIT TRANSACTION;
      `;
      const retExe: any = await db.execute({statements:sqlcmd});
      const res: Number = retExe.result // should be 1
```

### run
```
      sqlcmd = "INSERT INTO users (name,email,age) VALUES (?,?,?)";
      let values: Array<Array<any>>  = [["Simpson","TEXT"],["Simpson@example.com","TEXT"],[69,"INTEGER"]];
      const retRun: any = await db.run({statement:sqlcmd,values:values});
      const res: Number = retRun.result // should be 1
```

```
      sqlcmd = `INSERT INTO users (name,email,age) VALUES ("Brown","Brown@example.com",15)`;
      const retRun: any = await db.run({statement:sqlcmd,values:[]});
      const res: Number = retRun.result // should be 1
``` 

### query
 - Select all Users
```
      sqlcmd = "SELECT * FROM users";
      const retSelect: any = await db.query({statement:sqlcmd});
      const res: Number = retSelect.result.length    
```

## To use the Plugin in your Project
```bash
npm install --save capacitor-sqlite@latest
```

Ionic App showing an integration of [capacitor-sqlite plugin](https://github.com/jepiqueau/ionic-capacitor-sqlite)



## Remarks
This release of the plugin includes the Native IOS code (Objective-C/Swift) using Capacitor v1.2.1

## Dependencies
The IOS code is based on SQLite.swift as wrapper for SQLite.


