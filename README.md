# Student-Attendance-Management-System
To develop an Android application to enter student details, mark attendance and calculate attendance percentage.

# EXPERIMENT: STUDENT ATTENDANCE MANAGEMENT SYSTEM USING ANDROID STUDIO

## 1. AIM
To design and develop an Android application using Android Studio that allows users to enter student details, mark students as Present or Absent, and calculate the attendance percentage through a user-friendly graphical interface.

## 2. SOFTWARE REQUIREMENTS
- Android Studio
- Java Programming Language
- XML Layout Editor
- Android Emulator or Android Mobile
- Android SDK

## 3. THEORY
Android Studio is an Integrated Development Environment (IDE) used to develop Android applications using Java and XML.

A Student Attendance Management System is an application used to maintain student information and record their daily attendance. It helps faculty members mark attendance and calculate the attendance percentage efficiently.

The application uses the following Android UI components:

- **EditText:** To enter student register number and name.
- **Button:** To add students and mark attendance as Present or Absent.
- **TextView:** To display student details, attendance status and attendance percentage.
- **LinearLayout:** To arrange the user interface components.
- **ScrollView:** To display multiple student records.

Java event-handling methods are used to process student details, record attendance and calculate attendance percentages.

The attendance percentage is calculated using the formula:

Attendance Percentage = (Number of Classes Attended / Total Number of Classes Conducted) × 100

For the application, the percentage can also be calculated based on the number of students marked Present out of the total students entered for a particular attendance session:

Present Percentage = (Number of Students Present / Total Number of Students) × 100

## 4. PROCEDURE
1. Open Android Studio.
2. Select New Project and choose Empty Views Activity.
3. Enter the project name as `StudentAttendance`.
4. Select Java as the programming language.
5. Design the user interface using the XML Layout Editor.
6. Add EditText fields to enter student register number and name.
7. Add a Button to add student details.
8. Add Present and Absent buttons to record attendance.
9. Add TextViews to display student details and attendance summary.
10. Open MainActivity.java and write the Java program to manage student information and attendance.
11. Use setOnClickListener() to handle button-click events.
12. Store student names and attendance status using suitable data structures.
13. Calculate the total number of students and students present.
14. Calculate the attendance percentage using the appropriate formula.
15. Display the attendance summary on the application screen.
16. Execute the application using an Android Emulator or physical Android device.
17. Enter student details, mark attendance and verify the calculated percentage.

## 5. ALGORITHM
1. Start the application.
2. Initialize the Android user interface components.
3. Accept the student register number.
4. Accept the student name.
5. Click the Add Student button.
6. Store the student details.
7. Display the student information.
8. Mark the student as Present or Absent.
9. Count the total number of students.
10. Count the number of students marked Present.
11. Calculate the attendance percentage using:

   Present Percentage = (Present Students / Total Students) × 100

12. Display the attendance summary.
13. Repeat the process for other students.
14. Stop.
# PROGRAM
XML
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <EditText
        android:id="@+id/etRegNo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Register Number"
        android:inputType="text" />

    <EditText
        android:id="@+id/etName"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Student Name"
        android:inputType="textPersonName"
        android:layout_marginTop="8dp" />

    <Button
        android:id="@+id/btnAdd"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Add Student"
        android:layout_marginTop="12dp" />

    <TextView
        android:id="@+id/tvSummary"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Total: 0 | Present: 0 | Attendance: 0.00%"
        android:textSize="16sp"
        android:textStyle="bold"
        android:layout_marginTop="16dp"
        android:layout_marginBottom="8dp" />

    <ScrollView
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:layout_marginTop="8dp">

        <LinearLayout
            android:id="@+id/studentContainer"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical" />
    </ScrollView>

</LinearLayout>
KT
package com.example.studentattendance

import android.os.Bundle
import android.view.ViewGroup
import android.widget.Button
import android.widget.EditText
import android.widget.LinearLayout
import android.widget.TextView
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {
    private lateinit var etRegNo: EditText
    private lateinit var etName: EditText
    private lateinit var btnAdd: Button
    private lateinit var tvSummary: TextView
    private lateinit var studentContainer: LinearLayout

    private val studentNames = ArrayList<String>()
    private val attendance = ArrayList<Boolean>()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        etRegNo = findViewById(R.id.etRegNo)
        etName = findViewById(R.id.etName)
        btnAdd = findViewById(R.id.btnAdd)
        tvSummary = findViewById(R.id.tvSummary)
        studentContainer = findViewById(R.id.studentContainer)

        btnAdd.setOnClickListener { addStudent() }
        updateSummary()
    }

    private fun addStudent() {
        val reg = etRegNo.text.toString().trim()
        val name = etName.text.toString().trim()

        if (reg.isEmpty() || name.isEmpty()) {
            Toast.makeText(
                this,
                "Enter all student details",
                Toast.LENGTH_SHORT
            ).show()
            return
        }

        if (studentNames.contains(reg)) {
            Toast.makeText(
                this,
                "Register number already exists",
                Toast.LENGTH_SHORT
            ).show()
            return
        }

        studentNames.add(reg)
        attendance.add(false)

        val row = LinearLayout(this).apply {
            orientation = LinearLayout.VERTICAL
            setPadding(8, 12, 8, 12)
        }

        val student = TextView(this).apply {
            text = "$reg - $name"
            textSize = 17f
        }

        val buttons = LinearLayout(this).apply {
            orientation = LinearLayout.HORIZONTAL
        }

        val present = Button(this).apply {
            text = "Present"
        }

        val absent = Button(this).apply {
            text = "Absent"
        }

        buttons.addView(
            present,
            LinearLayout.LayoutParams(
                0,
                ViewGroup.LayoutParams.WRAP_CONTENT,
                1f
            )
        )

        buttons.addView(
            absent,
            LinearLayout.LayoutParams(
                0,
                ViewGroup.LayoutParams.WRAP_CONTENT,
                1f
            )
        )

        row.addView(student)
        row.addView(buttons)

        studentContainer.addView(row)

        val index = attendance.size - 1

        present.setOnClickListener {
            attendance[index] = true
            updateSummary()
            Toast.makeText(
                this, "$name Present",
                Toast.LENGTH_SHORT
            ).show()
        }

        absent.setOnClickListener {
            attendance[index] = false
            updateSummary()
            Toast.makeText(
                this, "$name Absent",
                Toast.LENGTH_SHORT
            ).show()
        }

        etRegNo.setText("")
        etName.setText("")

        updateSummary()
    }

    private fun updateSummary() {
        val total = attendance.size
        var present = 0

        for (status in attendance) {
            if (status == true) {
                present++
            }
        }

        val percentage = if (total == 0) 0.0 else (present * 100.0) / total

        tvSummary.text = "Total: $total | Present: $present | Attendance: " +
                String.format("%.2f", percentage) + "%"
    }
}

## 6. EXPECTED OUTPUT
<img width="1868" height="1020" alt="Screenshot 2026-11-26 140800" src="https://github.com/user-attachments/assets/521faf34-483e-4037-b869-f7987f523c31" />



## 7. APPLICATION FEATURES
- User-friendly graphical interface.
- Add student register numbers and names.
- Mark students as Present or Absent.
- Display student attendance details.
- Calculate present and absent counts.
- Calculate attendance percentage automatically.
- Support multiple student entries.
- Validate student information before adding records.

## 8. RESULT
Thus, the Student Attendance Management System Android application was successfully designed and developed using Android Studio with Java and XML. The application allows users to enter student details, mark attendance as Present or Absent, and calculate the attendance percentage successfully.

## 9. CONCLUSION
The experiment provides practical knowledge of Android application development, XML-based user interface design, Java programming, event handling, data management and percentage calculations using Android Studio.
