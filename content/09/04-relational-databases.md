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

---

## 9.4.2. Room

The Room library is a code layer that works over SQLite and tries to get the programmer away from SQL-related aspects. Google recommends using this library. 

First, we must add Room to our project in the Gradle file:

1. In the plugins section, we must include the following:
```gradle
plugins {
   id 'com.android.application'
   id 'kotlin-android'
   id 'kotlin-kapt'
}
```
2. In the dependencies section we must add at least the following:
```gradle
dependencies {
   implementation "androidx.room:room-ktx:2.4.1"
   kapt "androidx.room:room-compiler:2.4.1"
   androidTestImplementation "androidx.room:room-testing:2.4.1"
}
```

If we want extra features, we must add additional libraries.

Second, we need to create the classes representing our entities in the model.

```kotlin
@Entity
data class Subject(
   @PrimaryKey val subject_id: Int,
   @ColumnInfo(name = "subject_name") val subject_name: String?
)
@Entity
data class Student(
   @PrimaryKey val student_id: Int,
   @ColumnInfo(name = "student_name") var student_name: String?,
   @ColumnInfo(name = "student_email") var student_email: String?
)

@Entity(primaryKeys = ["subject_id", "student_id"])
data class Subject_Student(
   val subject_id: Int,
   val student_id: Int
)
```

Notice that these classes have annotations or decorations such as: `@Entity`, `@PrimaryKey`, `@ColumnInfo`, ...  These annotations will allow Room to generate the database creation scheme. To write these annotations correctly, you need to know some SQL fundamentals, as we can even indicate foreign keys that will ensure better consistency or indices that will enable better performance. For instance, the `@PrimaryKey` annotation identifies the primary key of a table: a column with no repeated or null values where an index will be created to quickly access queries by that column.

We then create the Data Access Object (DAO) classes. These classes will provide the insertion, deletion, modification and query operations on the previous classes.

```kotlin
@Dao
 StudentDao interface {
   @Query(“SELECT * FROM Student”)
   fun getAll(): List<Student>

   @Insert
   fun insertAll(vararg users: Student)

   @Delete
   fun delete(user: Student)

   @Update
   fun update(user: Student)

}
```

The following annotations can be used:
- `@Query` indicates that the method is a single query
- `@Insert` indicates an insertion
- `@Delete` indicates a deletion
- `@Update` indicates a modification

Finally, we create the class that will represent our database:

```kotlin
@Database(entities = [Student::class,Subject::class,Subject_Student:::class], version = 1)
abstract class AppDatabase : RoomDatabase() {
   abstract fun studentDao(): StudentDao
}
```

> ![Architecture of an app using the Room library.](/images/09/room.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Architecture of an app using the Room library.*  
> Source: [Android developers](
https://developer.android.com/training/data-storage/room) License: [CC BY 2.5](http://creativecommons.org/licenses/by/2.5/)


Now we can use this database in our app. It is very important to understand that Room operations should be performed on a different execution thread than the main thread. As always, the idea is to prevent data access operations, even if they are performed locally in the device, from impacting the interface by being very slow.

For the above reason we launched a Kotlin coroutine and inside we put our example code:

```kotlin

GlobalScope.launch(Dispatchers.Default) {

 val db = Room.databaseBuilder(
     applicationContext,
     AppDatabase::class.java, "school2"
 ).build()

 val studentDao = db.studentDao()

    // More code
}
```

Within the coroutine, the first thing we indicate is that we want to create the database if it does not exist in a SQLite file that will be called `school2`. Then we get the Student Data Access Object and use the `insertAll` method to make insertions.

```kotlin
studentDao.insertAll(
   Student(1, "Robert", "robert@rrrr.com"),
   Student(2, "Ada", "ada@rrrr.com")
)
```


For a list of all students we can use:

```kotlin
var l = studentDao.getAll()
```
We can modify the email of the second student in the list by doing:

```kotlin
l[1].student_email = "new_mail@mail.com"
studentDao.update(l[1])
```

And now we can delete the first student by doing:
```kotlin
studentDao.delete(l[0])
```

We need to be careful because Room does not keep reference consistency as other libraries in other programming environments do. That is, the list `l` in our code still has the deleted value. If we want a list without the deleted value, we will need to run `getAll` again.


Many-to-many (M:N) relationships, as is our case, should be inserted manually. That is, it is necessary to create the DAO. 

```kotlin
@Dao
 Subject_StudentDao interface {
   @Insert
   fun insertAll(vararg subjects: Subject_Student)
}
```

We access the DAO and connect the student to the subject using their respective identifiers.

```kotlin
val subject_StudentDao =  db.subject_StudentDao()
subject_StudentDao.insertAll(Subject_Student(1,2))
```

Once students and subjects have been connected, we may want to have all the subjects you take automatically loaded when you load a student.

To do this we added a new class to our model:

```kotlin
@Entity
data class StudentWithSubjets(
   @Embedded val data: Student,
   @Relation(
       parentColumn = "student_id",
       entityColumn = "subject_id",
       associateBy = Junction(
           Subject_Student::class
       )
   )
   val subjects: MutableList<Subject>
)
```

This new class has the `@Embedded` annotation indicating it includes the Student class information defined above. It also has the annotation `@Relation` on the `subjects` property. This annotation indicates that the list should be populated using the `Subject_Student` class/relationship. In addition, it also indicates which properties provide the link between the tables:

```kotlin
   parentColumn = "student_id",
   entityColumn = "subject_id",
```

Now in the student DAO we add the method:

```kotlin
@Transaction
@Query("SELECT * FROM Student WHERE student_id = (:student_id)")
fun loadById(student_id: Int): StudentWithSubjets
```

The `@Transaction` annotation indicates that Room will need to run multiple queries. You need to run one to upload user data and one to upload your subjects. We can now run:

```kotlin
var student: StudentWithSubjects = studentDao.loadById(2)
```

We can see that the list of subjects is filled with the subject `"Math"`.

> ![Result of a complex query.](/images/09/query-data.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Result of a complex query.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)



