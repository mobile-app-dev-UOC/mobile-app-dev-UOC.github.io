---
layout: default
title: 9.4. Relational databases
parent: 9. Local data persistence
nav_order: 4
---

# 9.4. Relational databases

Relational databases allow us to save structured information in a way that complex queries can be executed optimally. Elements are organized as entities and the relationships between them. Entities are stored as tables. Meanwhile, relationships may be storerd as tables or columns in a table depending on its type.

## 9.4.1. SQLite

[SQLite](https://www.sqlite.org) is a relational database deployed with a library, that is, it is not a server. It’s very fast, it meets SQL standards, and is typically used in local environments.

Free software can be used to design the database and test it locally before integrating it with the application. An example of such tools is [DB Browser for SQLite](https://sqlitebrowser.org/dl/).

For example, let us create a small database to model the information system of a school. The database has two entities: *subject* and *students*. It also has an *M:N:P* student_subject relationship that stores triples of the form (student, subject, date). Each triple indicates the date in which a student took a subject. This relationship also stores the grade that the student received. A student can take a subject more than once, until he/she passes it.

> ![Entity-relationship diagram for our relational database.](/images/09/er-diagram.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Entity-relationship diagram for our relational database.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

DB Browser generates the SQL code needed to create each table and we can insert it directly into our application.

```kotlin
class DbHelper(context: Context) : SQLiteOpenHelper(context, "school", null, 1) {

     private val SQL_CREATE_SUBJECT = """
      CREATE TABLE "Subject" (
  "subject_id" INTEGER,
  subject_name TEXT,
  PRIMARY KEY("subject_id")
);
               """

   private val SQL_CREATE_student = """
       CREATE TABLE "Student" (
  "student_id" INTEGER,
  "student_name" TEXT,
  "student_email" TEXT,
  PRIMARY KEY("student_id")
);
       """

   private val SQL_CREATE_subject_student = """
      CREATE TABLE "Subject_Student" (
  "substu_subject_id" INTEGER NOT NULL,
  "substu_student_id" INTEGER NOT NULL,
  "substu_date" TEXT,
  "substu_grade" INTEGER,
  PRIMARY KEY("substu_subject_id","substu_student_id","substu_date"),
  FOREIGN KEY("substu_student_id") REFERENCES "Student"("student_id"),
  FOREIGN KEY("substu_subject_id") REFERENCES "Subject"("subject_id")
);
                """


   override fun onCreate(db: SQLiteDatabase) {

       Try {
           db.execSQL(SQL_CREATE_SUBJECT)
           db.execSQL(SQL_CREATE_student)
           db.execSQL(SQL_CREATE_subject_student)
       }
       catch(err:Exception){
           Log.d("error",err.message!!)
       }
   }
}
```

The `onCreate` callback from our class inheriting from superclass `SQLiteOpenHelper` only runs once: when the database does not exist. It is not run when we create the instance of the class derived from `SQLiteOpenHelper`. Instead, it is invokedwhen we access the database using the `writableDatabase property.




## 9.4.2. Room