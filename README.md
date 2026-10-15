Android Development Experiments (Kotlin)
This README documents two Android Studio experiments:
1. Demonstrate data persistence using SharedPreferences and SQLite
2. Implement data sharing using the Contacts Content Provider
Requirements
- Android Studio
- Kotlin
- Android SDK
- Android emulator or Android phone
- For the Contacts experiment, a device/emulator with contacts
  available
Experiment 1: Data Persistence Using SharedPreferences and SQLite
Aim
To demonstrate how an Android application stores and retrieves data
persistently using SharedPreferences and SQLite.
Concepts
SharedPreferences
SharedPreferences stores small key-value pairs, such as a username,
setting, or login preference. The data remains available after the app
is closed and reopened.
Typical operations: - Save a value using edit() and apply() - Read a
value using getString(), getInt(), etc. - Remove a value using
remove()
Example:
val prefs = getSharedPreferences("MyPrefs", MODE_PRIVATE)

// Save
prefs.edit().putString("username", "AndroidUser").apply()

// Read
val username = prefs.getString("username", "")
SQLite
SQLite is a local relational database suitable for structured data
arranged in tables and rows. Android provides SQLiteOpenHelper to
create and manage a database.
Typical operations: - Create a table in onCreate() - Insert data with
insert() - Read data with rawQuery() or query() - Update or delete
rows when required
Example database helper:
class DBHelper(context: Context) :
    SQLiteOpenHelper(context, "StudentDB", null, 1) {

    override fun onCreate(db: SQLiteDatabase) {
        db.execSQL(
            "CREATE TABLE students (" +
                "id INTEGER PRIMARY KEY AUTOINCREMENT, " +
                "name TEXT NOT NULL)"
        )
    }

    override fun onUpgrade(
        db: SQLiteDatabase,
        oldVersion: Int,
        newVersion: Int
    ) {
        db.execSQL("DROP TABLE IF EXISTS students")
        onCreate(db)
    }

    fun insertStudent(name: String): Long {
        val values = ContentValues().apply {
            put("name", name)
        }
        return writableDatabase.insert("students", null, values)
    }

    fun getStudents(): List<String> {
        val students = mutableListOf<String>()
        readableDatabase.rawQuery(
            "SELECT name FROM students ORDER BY id",
            null
        ).use { cursor ->
            while (cursor.moveToNext()) {
                students.add(cursor.getString(0))
            }
        }
        return students
    }
}
Add these imports where needed:
import android.content.ContentValues
import android.content.Context
import android.database.sqlite.SQLiteDatabase
import android.database.sqlite.SQLiteOpenHelper
Procedure
1. Create a Kotlin project in Android Studio.
2. Use SharedPreferences to save a sample value.
3. Read the saved value and display it in the app.
4. Create a database helper by extending SQLiteOpenHelper.
5. Create a table in onCreate().
6. Insert sample records and retrieve them from SQLite.
7. Run the app, close it, and reopen it to verify that the data
   persists.
Expected Output
- The value saved with SharedPreferences is displayed after it is
  read.
- The records inserted into SQLite are displayed when queried.
- Both types of data remain available after the application is closed
  and reopened, unless the app data is cleared or the data is
  explicitly deleted.
Result
Data persistence using SharedPreferences and SQLite was demonstrated
successfully.
