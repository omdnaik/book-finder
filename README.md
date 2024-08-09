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

import groovy.json.JsonSlurper
import groovy.json.JsonOutput

// Variables - Update these values
def confluenceUrl = "https://your-confluence-instance/wiki"
def parentPageTitle = "Parent Page Title"
def spaceKey = "YOUR_SPACE_KEY"
def username = "your_username"
def password = "your_password"
def markdownFile = args[0]
def newPageTitle = "New Page Title"

// Function to fetch the page ID of the parent page
def getParentPageId() {
    def url = "${confluenceUrl}/rest/api/content?title=${URLEncoder.encode(parentPageTitle, 'UTF-8')}&spaceKey=${spaceKey}"
    def conn = new URL(url).openConnection()
    conn.requestMethod = 'GET'
    conn.setRequestProperty('Content-Type', 'application/json')
    def auth = "${username}:${password}".bytes.encodeBase64().toString()
    conn.setRequestProperty('Authorization', "Basic ${auth}")

    def response = conn.inputStream.text
    def json = new JsonSlurper().parseText(response)
    def pageId = json.results[0]?.id
    return pageId
}

// Function to read Markdown file content
def readMarkdownFile(markdownFile) {
    new File(markdownFile).text
}

// Function to create a new page as a child of the parent page
def createChildPage(parentPageId, markdownBody) {
    def url = "${confluenceUrl}/rest/api/content/"
    def conn = new URL(url).openConnection()
    conn.requestMethod = 'POST'
    conn.setRequestProperty('Content-Type', 'application/json')
    def auth = "${username}:${password}".bytes.encodeBase64().toString()
    conn.setRequestProperty('Authorization', "Basic ${auth}")
    conn.doOutput = true

    def data = [
        type: 'page',
        title: newPageTitle,
        ancestors: [[id: parentPageId]],
        body: [
            storage: [
                value: markdownBody,
                representation: 'markdown'
            ]
        ]
    ]

    def json = JsonOutput.toJson(data)
    conn.outputStream.withWriter('UTF-8') { writer ->
        writer << json
    }

    def response = conn.inputStream.text
    println "Response: ${response}"
}

// Main script execution
def parentPageId = getParentPageId()

if (!parentPageId) {
    println "Error: Parent page not found."
    System.exit(1)
}

def markdownBody = readMarkdownFile(markdownFile)
createChildPage(parentPageId, markdownBody)

println "Page created successfully as a child of the parent page."

