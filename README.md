# BookFinder

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 7.0.4.

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The app will automatically reload if you change any of the source files.

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
