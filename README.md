# Ex.No: 1 To create a database table and to display the database table field using SQLite Database in Android Studio.

## AIM:

To create a database table and to display the database table field using SQLite Database in Android Studio.

## EQUIPMENTS REQUIRED:

Android Studio(Min.required Arctic Fox)

## ALGORITHM:

Step 1: Open Android Studio and then click on File -> New -> New project.

Step 2: Then type the Application name as sqllite and click Next. 

Step 3: Then select the Minimum SDK as shown below and click Next.

Step 4: Then select the Empty Activity and click Next. Finally click Finish.

Step 5: Design layout in activity_main.xml.

Step 6: Enter the code in MainActivity file.

Step 7: Save and run the application.



## PROGRAM:
### Program to print the DatabaseTable using the SQLite”.
### Developed by        : S.Sumyuktha Rani
### Registration Number : 212220230050
### MainActivity.java
```
package com.example.sqlitedatabase;

import androidx.appcompat.app.AppCompatActivity;

import android.annotation.SuppressLint;
import android.database.Cursor;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.EditText;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {
    EditText editUserID;
    EditText editUserName;
    EditText editUserPassword;

    DatabaseManager dbManager;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        editUserID= findViewById(R.id.editTextID);
        editUserName=findViewById(R.id.editTextUserName);
        editUserPassword=findViewById(R.id.editTextPassword);

        dbManager=new DatabaseManager(this);
        try{
            dbManager.open();
        }
        catch(Exception e){
            e.printStackTrace();
        }
    }
    public void btnInsertPressed(View v){
        dbManager.insert(editUserName.getText().toString(),editUserPassword.getText().toString());
    }

    public void btnFetchPressed(View v){
        Cursor cursor=dbManager.fetch();
        if (cursor.moveToFirst())
        {
            do{
                @SuppressLint("Range") String ID=cursor.getString(cursor.getColumnIndex(DatabaseHelper.USER_ID));
                @SuppressLint("Range") String username=cursor.getString(cursor.getColumnIndex(DatabaseHelper.USER_NAME));
                @SuppressLint("Range") String password=cursor.getString(cursor.getColumnIndex(DatabaseHelper.USER_PASSWORD));
                Log.i("DATABASE_TAG","I have read ID : "+ID+" Username : "+username+" password : "+password);
            }while(cursor.moveToNext());
        }
    }

    public void btnUpdatePressed(View v){
        dbManager.update(Long.parseLong(editUserID.getText().toString()),editUserName.getText().toString(),editUserPassword.getText().toString());
    }

    public void btnDeletePressed(View v){
        dbManager.delete(Long.parseLong(editUserID.getText().toString()));
    }
}
```
### DatabaseManager.java
```
package com.example.sqlitedatabase;

import android.content.ContentValues;
import android.content.Context;
import android.database.Cursor;
import android.database.sqlite.SQLiteDatabase;

import java.sql.SQLDataException;

public class DatabaseManager {
    private DatabaseHelper dbHelper;
    private Context context;
    private SQLiteDatabase database;

    public DatabaseManager(Context ctx){
        context=ctx;
    }
    public DatabaseManager open() throws SQLDataException {
        dbHelper = new DatabaseHelper(context);
        database = dbHelper.getWritableDatabase();
        return this;
    }
    public void close(){
        dbHelper.close();
    }
    public void insert (String username, String password){
        ContentValues contentValues=new ContentValues();
        contentValues.put(DatabaseHelper.USER_NAME,username);
        contentValues.put(DatabaseHelper.USER_PASSWORD,password);
        database.insert(DatabaseHelper.DATABASE_TABLE,null,contentValues);
    }
    public Cursor fetch(){
        String [] columns= new String[]{DatabaseHelper.USER_ID,DatabaseHelper.USER_NAME,DatabaseHelper.USER_PASSWORD};
        Cursor cursor=database.query(DatabaseHelper.DATABASE_TABLE,columns,null,null,null,null,null);
        if(cursor!=null){
            cursor.moveToFirst();
        }
        return cursor;
    }
    public int update(long _id,String username,String password){
        ContentValues contentValues=new ContentValues();
        contentValues.put(DatabaseHelper.USER_NAME,username);
        contentValues.put(DatabaseHelper.USER_PASSWORD,password);
        int ret = database.update(DatabaseHelper.DATABASE_TABLE,contentValues,DatabaseHelper.USER_ID+"="+_id,null);
        return ret;
    }
    public void delete(long id){
        database.delete(DatabaseHelper.DATABASE_TABLE,DatabaseHelper.USER_ID,null);

    }

}
```
### DatabaseHelper.java
```
package com.example.sqlitedatabase;

import android.content.Context;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;


public class DatabaseHelper extends SQLiteOpenHelper {
    static final String DATABASE_NAME="DataBASE.DB";
    static final int DATABASE_VERSION=1;

    static final String DATABASE_TABLE= "USERS";
    static final String USER_ID= "_ID";
    static final String USER_NAME="user_name";
    static final String USER_PASSWORD="password";

    private static final String CREATE_DB_QUERY = "CREATE TABLE "+DATABASE_TABLE +" ( "+ USER_ID+ " INTEGER PRIMARY KEY AUTOINCREMENT, "+USER_NAME+ " TEXT NOT NULL,   "+ USER_PASSWORD+ " TEXT NOT NULL );";
    public DatabaseHelper(Context context) {
        super(context, DATABASE_NAME, null, DATABASE_VERSION);
    }

    @Override
    public void onCreate(SQLiteDatabase db) {
        db.execSQL(CREATE_DB_QUERY);
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int i, int i1) {
        db.execSQL("DROP TABLE IF EXISTS "+ DATABASE_TABLE);
    }
}
```
### activity_main.xml
```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"

    tools:context=".MainActivity">

    <EditText
        android:id="@+id/editTextUserName"
        android:layout_width="236dp"
        android:layout_height="42dp"
        android:layout_marginTop="20dp"
        android:backgroundTint="@color/black"
        android:ems="10"
        android:hint="Enter your UserName"
        android:inputType="textPersonName"
        android:minHeight="48dp"
        android:textAlignment="center"
        android:textColor="@color/black"


        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.931"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/editTextID" />

    <EditText
        android:id="@+id/editTextID"
        android:layout_width="231dp"
        android:layout_height="39dp"
        android:backgroundTint="@color/black"
        android:ems="10"
        android:hint="Enter UserID"
        android:inputType="textPersonName"

        android:minHeight="48dp"
        android:textAlignment="center"
        android:textColor="@color/black"


        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.91"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.145" />

    <EditText
        android:id="@+id/editTextPassword"
        android:layout_width="234dp"
        android:layout_height="38dp"
        android:layout_marginTop="28dp"
        android:backgroundTint="@color/black"

        android:ems="10"
        android:hint="Enter your Password"
        android:inputType="textPassword"


        android:minHeight="48dp"
        android:textAlignment="center"
        android:textColor="@color/black"

        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.92"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/editTextUserName" />

    <Button
        android:id="@+id/button2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:background="@color/teal_200"
        android:backgroundTintMode="multiply"
        android:onClick="btnFetchPressed"
        android:text="Fetch"

        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.743"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.475" />

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:background="@color/teal_200"
        android:backgroundTintMode="multiply"
        android:onClick="btnInsertPressed"
        android:text="Insert"

        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.182"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.475" />

    <Button
        android:id="@+id/button3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:background="@color/teal_200"
        android:backgroundTintMode="multiply"

        android:onClick="btnUpdatePressed"
        android:text="Update"

        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.754"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.623" />

    <Button
        android:id="@+id/button4"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:background="@color/teal_200"
        android:backgroundTintMode="multiply"

        android:onClick="btnDeletePressed"
        android:text="Delete"

        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.177"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.623" />

    <TextView
        android:id="@+id/textView"
        android:layout_width="101dp"
        android:layout_height="38dp"
        android:background="@color/black"
        android:text="User ID"
        android:textAlignment="center"
        android:textColor="@color/white"
        android:textSize="25dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.083"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.147" />

    <TextView
        android:id="@+id/textView2"
        android:layout_width="111dp"
        android:layout_height="39dp"
        android:background="@color/black"
        android:text="Name"
        android:textAlignment="center"
        android:textColor="@color/white"
        android:textSize="25dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.086"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.235" />

    <TextView
        android:id="@+id/textView3"
        android:layout_width="120dp"
        android:layout_height="42dp"
        android:background="@color/black"
        android:text="Password"
        android:textAlignment="center"
        android:textColor="@color/white"
        android:textSize="25dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.085"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.328" />
</androidx.constraintlayout.widget.ConstraintLayout>
```
## OUTPUT:

![WhatsApp Image 2022-10-12 at 10 39 41 AM](https://user-images.githubusercontent.com/75235818/195516064-a829210e-eeff-478a-a426-b97172a8f9bf.jpeg)

### Insert

![WhatsApp Image 2022-10-12 at 10 39 40 AM](https://user-images.githubusercontent.com/75235818/195516447-701ec41e-ad69-47c8-86f1-162d17deb697.jpeg)

### Fetch

![WhatsApp Image 2022-10-12 at 10 39 40 AM](https://user-images.githubusercontent.com/75235818/195516486-3190d9f5-9a46-4f11-9766-ddbbacb705ac.jpeg)

### Update

![WhatsApp Image 2022-10-12 at 10 39 40 AM](https://user-images.githubusercontent.com/75235818/195516530-69a018e4-e89c-43c0-bf60-c573e45ed790.jpeg)

## RESULT:
Thus, a Simple Android Application to create a database table and to display the database table using SQLite Database in Android Studio is developed and executed successfully
