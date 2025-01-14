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


name: Check if postinstallscr.dat exists
      stat:
        path: /path/to/postinstallscr.dat
      register: file_stat

    - name: Initialize post_install_file_list if it is not already set
      set_fact:
        post_install_file_list: "{{ post_install_file_list | default([]) }}"
      when: not file_stat.stat.exists or file_stat.stat.size == 0

    - name: Read and process the file content
      when: file_stat.stat.exists and file_stat.stat.size > 0
      block:
        - name: Read the file content
          slurp:
            path: /path/to/postinstallscr.dat
          register: file_content

        - name: Decode file content from base64 and append lines to the list
          set_fact:
            post_install_file_list: "{{ (post_install_file_list + (file_content.content | b64decode | split('\n'))) | unique }}"

    - name: Debug the post_install_file_list
      debug:
        msg: "Post install file list: {{ post_install_file_list }}"

- name: Identify the type of an object
  hosts: localhost
  tasks:
    - name: Define an example variable
      set_fact:
        example_variable: "{{ ['a', 'b', 'c'] }}"  # Change this to test different types

    - name: Determine the type of example_variable
      debug:
        msg: |
          {% if example_variable is string %}
            The type of `example_variable` is String
          {% elif example_variable is number %}
            The type of `example_variable` is Number
          {% elif example_variable is mapping %}
            The type of `example_variable` is Dictionary
          {% elif example_variable is sequence %}
            The type of `example_variable` is List
          {% else %}
            The type of `example_variable` is Unknown
          {% endif %}
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
