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

The `onCreate` callback from our class inheriting from superclass `SQLiteOpenHelper` only runs once: when the database does not exist. It is not run when we create the instance of the class derived from `SQLiteOpenHelper`. Instead, it is invokedwhen we access the database using the `writableDatabase property`.

```kotlin
val dbHelper = DbHelper(this)
val db = dbHelper.writableDatabase
```

The database will be created in our filesystem, in the `databases` folder:

> ![Location of the database in the filesystem.](/images/09/database-directory.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Location of the database in the filesystem.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

With our database created we can now make insertions, updates and queries.

We can do the above operations in two different ways. The first is using the individual methods of the `dbHelper.writableDatabase`. For example, for insertions we can use the following:

```kotlin
var values = ContentValues().apply {
    put("subject_id", 1)
    put("subject_name", "Math")
}

var newRowId = db?.insert("Subject", null, values)
```

The second approach relies on the `execSQL` method, that allows us to run SQL statements directly. For example, we can easily load the initial contents of a database like this:

```kotlin
val command_list = listOf(
   "insert into 'Student' values(1,'Robert','robert@gmail.com')",
   "insert into 'Student' values(2,'James','james@gmail.com')",
   "insert into 'Student' values(3,'Ada','ada@gmail.com')",
   "insert into 'Subject_Student' values(1,1,'2022-12-01',6)",
   "insert into 'Subject_Student' values(1,3,'2022-12-01',9)",
   "insert into 'Subject_Student' values(2,2,'2022-12-01',3)",
   "insert into 'Subject_Student' values(2,3,'2022-12-01',4)"
)

for (command in command_list) {
   db.execSQL(command)
}
```

Android SQLite does **not** support the execution of multiple statements separated by the character `;` within a single string.

**Updates**. For example, to change the subject name with `subject_id` equal to 1 to `“Mathematics”` we would do the following:

```kotlin
val values = ContentValues().apply {
               put("subject_name", "Mathematics")
             }

val selection = " subject_id=1"
val selectionArgs = arrayOf(1)
val count = db.update(
               "Subject",
               values,
               selection,
               null)
```

**Updates**. We can use the following code to delete the triples from the `subject_student` relationship where the `id` of the student or the course is equal to 1.

```kotlin
val selection = "substu_subject_id=1 and substu_student_id=1"
val selectionArgs = null
val deletedRows = db.delete( "Subject_Student", selection, selectionArgs)
```


**Queries**. Queries are built with **cursors**. A cursor is an iterator over data in the database that meets certain conditions.

In the following example, we want the cursor to provide us with the name and email of students who have identifiers one or three, in descending alphabetical order by name.

```kotlin
val projection = arrayOf("student_name","student_email")

val selection2 = " student_id = 3 or student_id = 1 "
val selectionArgs2 = null


val sortOrder = " student_name DESC"

val cursor = db.query(
   "Student,"
   projection,
   selection2,
   selectionArgs2,
   null,
   null,
   sortOrder
)

with(cursor) {
   while (moveToNext()) {
       val student_name = getString(getColumnIndexOrThrow("student_name"))
       val student_email = getString(getColumnIndexOrThrow("student_email"))

   }
}
cursor.close()
```

Method `moveToNext()` is what allows us to move from one result to the following one. When we are finished, we should always remember to close the cursor with `cursor.close()`.

This querying method greatly limits the power of queries that SQLite can respond to. As we have explained, we can directly execute complex SQL statements that will generate a cursor for us. 

```kotlin
val cursor2 = db.rawQuery(
   "SELECT subject_name, substu_grade FROM Subject_Student, Subject " +
           "WHERE substu_student_id = 3 and substu_subject_id=subject_id"
           , null)

with(cursor2) {
   while (moveToNext()) {
       val subject_name = getString(getColumnIndexOrThrow("subject_name"))
       val substu_grade = getInt(getColumnIndexOrThrow("substu_grade"))

   }
}
cursor2.close()
```

We use the `rawQuery` method to run a query that uses the relationship to get results. The cursor operates in the same way as in the first cursor example.

Finally, if we are only going to run query operations it is better to access:

```kotlin
db_read = dbHelper.readableDatabase
```




## 9.4.2. Room