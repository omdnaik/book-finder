# BookFinder

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 7.0.4.

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The app will automatically reload if you change any of the source files.

Absolutely! Here's a bash script to demonstrate switching JDBC driver libraries (e.g., H2 or Oracle) at runtime without changing the code or pom.xml, using the provided scope approach.


---

Assumptions

You have a compiled Java app jar: your-app.jar

You have multiple JDBC driver jars in lib/ directory, like:

lib/h2-1.4.200.jar

lib/h2-2.2.224.jar

lib/ojdbc8.jar

lib/ojdbc11.jar


Your main class is: com.example.DriverInfoPrinter



---

Script: run-with-driver.sh

#!/bin/bash

# Usage: ./run-with-driver.sh lib/h2-2.2.224.jar
# Usage: ./run-with-driver.sh lib/ojdbc8.jar

DRIVER_JAR=$1

if [[ -z "$DRIVER_JAR" ]]; then
  echo "Usage: $0 path_to_driver_jar"
  exit 1
fi

if [[ ! -f "$DRIVER_JAR" ]]; then
  echo "Error: Driver jar '$DRIVER_JAR' not found."
  exit 1
fi

APP_JAR="target/your-app.jar"
MAIN_CLASS="com.example.DriverInfoPrinter"

echo "Running with driver: $DRIVER_JAR"
java -cp "$DRIVER_JAR:$APP_JAR" $MAIN_CLASS


---

Make it executable

chmod +x run-with-driver.sh


---

Usage Examples

./run-with-driver.sh lib/h2-2.2.224.jar

./run-with-driver.sh lib/ojdbc8.jar

It will:

Run your Java app

Print the driver class and loaded JAR

Allow you to switch libraries easily at runtime



---

Would you like this enhanced to loop through all driver versions and print them in one go?





## Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also use `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory. Use the `--prod` flag for a production build.

## Running unit tests

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## Running end-to-end tests

Run `ng e2e` to execute the end-to-end tests via [Protractor](http://www.protractortest.org/).

## Further help

To get more help on the Angular CLI use `ng help` or go check out the [Angular CLI README](https://github.com/angular/angular-cli/blob/master/README.md).


 Sub AddDateAtCursor()
    Dim currentDate As String
    Dim cellContent As String
    Dim cursorPosition As Long

    ' Get current date in your preferred format
    currentDate = Format(Date, "yyyy-mm-dd") ' Change format as needed

    ' Get active cell content
    cellContent = ActiveCell.Value

    ' Insert the date at the current cursor position
    If TypeName(Selection) = "Range" And ActiveCell.HasFormula = False Then
        If Application.Selection.Text = "" Then
            ActiveCell.Value = currentDate
        Else
            cursorPosition = Application.Selection.Start
            ActiveCell.Value = Left(cellContent, cursorPosition) & currentDate & Mid(cellContent, cursorPosition + 1)
        End If
    End If
End Sub
