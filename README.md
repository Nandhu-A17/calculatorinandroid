## Ex.No:5 Develop a simple calculator using android studio.

## AIM:
To develop a program to develop a simple calculator in Android Studio.

## EQUIPMENTS REQUIRED:
Android Studio

## ALGORITHM:

Step 1: Open Android Stdio and then click on File -> New -> New project.

Step 2: Then type the Application name as calculator and click Next.

Step 3: Then select the Minimum SDK as shown below and click Next.

Step 4: Then select the Empty Activity and click Next. Finally click Finish.

Step 5: Design layout using UI components in activity_main.xml.

Step 6: Display the calculator operation in MainActivity file.

Step 7: Save and run the application.

## PROGRAM:

```
Program to print the text “calculator operation”.
Developed by: NANDEESWARI A
Registeration Number : 212224220068

```
## activity_main.xml
```

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:background="#000000"
    android:padding="12dp">

    <TextView
        android:id="@+id/display"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:text="0"
        android:textColor="#FFFFFF"
        android:textSize="55sp"
        android:gravity="bottom|right"
        android:padding="20dp"
        android:maxLines="1"
        android:singleLine="true" />

    <!-- First Row -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="75dp"
        android:orientation="horizontal">

        <Button
            style="@style/CalcButton"
            android:text="C"
            android:onClick="onButtonClick"
            android:backgroundTint="#A5A5A5"
            android:textColor="#000000" />

        <Button
            style="@style/CalcButton"
            android:text="DEL"
            android:onClick="onButtonClick"
            android:backgroundTint="#A5A5A5"
            android:textColor="#000000" />

        <Button
            style="@style/CalcButton"
            android:text="%"
            android:onClick="onButtonClick"
            android:backgroundTint="#A5A5A5"
            android:textColor="#000000" />

        <Button
            style="@style/CalcButton"
            android:text="÷"
            android:onClick="onButtonClick"
            android:backgroundTint="#FF9500" />
    </LinearLayout>

    <!-- Second Row -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="75dp"
        android:orientation="horizontal">

        <Button style="@style/CalcButton" android:text="7" android:onClick="onButtonClick" />
        <Button style="@style/CalcButton" android:text="8" android:onClick="onButtonClick" />
        <Button style="@style/CalcButton" android:text="9" android:onClick="onButtonClick" />
        <Button style="@style/CalcButton" android:text="×" android:onClick="onButtonClick"
            android:backgroundTint="#FF9500" />
    </LinearLayout>

    <!-- Third Row -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="75dp"
        android:orientation="horizontal">

        <Button style="@style/CalcButton" android:text="4" android:onClick="onButtonClick" />
        <Button style="@style/CalcButton" android:text="5" android:onClick="onButtonClick" />
        <Button style="@style/CalcButton" android:text="6" android:onClick="onButtonClick" />
        <Button style="@style/CalcButton" android:text="−" android:onClick="onButtonClick"
            android:backgroundTint="#FF9500" />
    </LinearLayout>

    <!-- Fourth Row -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="75dp"
        android:orientation="horizontal">

        <Button style="@style/CalcButton" android:text="1" android:onClick="onButtonClick" />
        <Button style="@style/CalcButton" android:text="2" android:onClick="onButtonClick" />
        <Button style="@style/CalcButton" android:text="3" android:onClick="onButtonClick" />
        <Button style="@style/CalcButton" android:text="+" android:onClick="onButtonClick"
            android:backgroundTint="#FF9500" />
    </LinearLayout>

    <!-- Fifth Row -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="75dp"
        android:orientation="horizontal">

        <Button
            style="@style/CalcButton"
            android:text="0"
            android:onClick="onButtonClick" />

        <Button
            style="@style/CalcButton"
            android:text="."
            android:onClick="onButtonClick" />

        <Button
            style="@style/CalcButton"
            android:text="="
            android:onClick="onButtonClick"
            android:backgroundTint="#FF9500"
            android:layout_weight="2" />
    </LinearLayout>

</LinearLayout>
```
## MainActivity.java
```
package com.example.calculator;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;
import java.text.DecimalFormat;

public class MainActivity extends AppCompatActivity {

    TextView display;

    double firstValue = 0;
    String operator = "";
    String currentInput = "0";
    boolean startNewNumber = false;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        display = findViewById(R.id.display);
    }

    public void onButtonClick(View view) {

        Button button = (Button) view;
        String value = button.getText().toString();

        if (value.matches("[0-9]")) {

            if (startNewNumber || currentInput.equals("0")) {
                currentInput = value;
                startNewNumber = false;
            } else {
                currentInput += value;
            }

        } else if (value.equals(".")) {

            if (startNewNumber) {
                currentInput = "0";
                startNewNumber = false;
            }

            if (!currentInput.contains(".")) {
                currentInput += ".";
            }

        } else if (value.equals("C")) {

            currentInput = "0";
            firstValue = 0;
            operator = "";
            startNewNumber = false;

        } else if (value.equals("DEL")) {

            if (!startNewNumber && currentInput.length() > 1) {
                currentInput = currentInput.substring(
                        0, currentInput.length() - 1);
            } else {
                currentInput = "0";
            }

        } else if (value.equals("%")) {

            double number = Double.parseDouble(currentInput);
            currentInput = format(number / 100);
            startNewNumber = true;

        } else if (value.equals("+") || value.equals("−")
                || value.equals("×") || value.equals("÷")) {

            if (!operator.isEmpty() && !startNewNumber) {
                calculate();
            }

            firstValue = Double.parseDouble(currentInput);
            operator = value;
            startNewNumber = true;

        } else if (value.equals("=")) {

            if (!operator.isEmpty()) {
                calculate();
                operator = "";
                startNewNumber = true;
            }
        }

        display.setText(currentInput);
    }

    private void calculate() {

        double secondValue = Double.parseDouble(currentInput);
        double result = 0;

        switch (operator) {

            case "+":
                result = firstValue + secondValue;
                break;

            case "−":
                result = firstValue - secondValue;
                break;

            case "×":
                result = firstValue * secondValue;
                break;

            case "÷":
                if (secondValue == 0) {
                    currentInput = "Cannot divide by zero";
                    return;
                }
                result = firstValue / secondValue;
                break;
        }

        currentInput = format(result);
        firstValue = result;
    }

    private String format(double number) {
        return new DecimalFormat("0.##########").format(number);
    }
}
```
## styles.xml
```
<?xml version="1.0" encoding="utf-8"?>
<resources>

    <style name="CalcButton">
        <item name="android:layout_width">0dp</item>
        <item name="android:layout_height">match_parent</item>
        <item name="android:layout_weight">1</item>
        <item name="android:layout_margin">4dp</item>
        <item name="android:textSize">23sp</item>
        <item name="android:textColor">#FFFFFF</item>
        <item name="android:backgroundTint">#333333</item>
        <item name="android:gravity">center</item>
    </style>

</resources>
```

## Output

<img width="1920" height="1080" alt="Screenshot 2026-11-06 155704" src="https://github.com/user-attachments/assets/a585a2f6-e8c0-4b92-a616-1cec57c28f38" />


<img width="1920" height="1080" alt="Screenshot 2026-11-06 155821" src="https://github.com/user-attachments/assets/8c598413-12a5-42c0-a2ca-1936d33523bd" />


## RESULT

Thus a Simple Android Application develop a program to create simple calculator in Android Studio is developed and executed successfully.
