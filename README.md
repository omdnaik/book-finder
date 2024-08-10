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

def markdownContent = readFile(env.MARKDOWN_FILE).trim()
                    
                    // Handling large content with Groovy
                    def contentJson = groovy.json.JsonOutput.toJson([
                        type: 'page',
                        title: env.NEW_PAGE_TITLE,
                        ancestors: [[id: env.PARENT_PAGE_ID]],
                        space: [key: env.SPACE_KEY],
                        body: [storage: [value: markdownContent, representation: 'markdown']]
                    ])

                    // Escape double quotes in JSON content
                    def escapedContent = contentJson.replaceAll('"', '\\"')
                    
                    sh """
                        curl -s -u ${USERNAME}:${PASSWORD} \
                             -X POST \
                             -H 'Content-Type: application/json' \
                             -d '${escapedContent}' \
                             "${CONFLUENCE_URL}/rest/api/content/"
                    """



                    

stage('Publish Markdown as Child Page') {
            steps {
                script {
                    def markdownContent = readFile(env.MARKDOWN_FILE)
                    sh(script: """
                        curl -u ${USERNAME}:${PASSWORD} \
                             -X POST \
                             -H 'Content-Type: application/json' \
                             -d '{
                                 "type": "page",
                                 "title": "${NEW_PAGE_TITLE}",
                                 "ancestors": [
                                     {
                                         "id": "${env.PARENT_PAGE_ID}"
                                     }
                                 ],
                                 "body": {
                                     "storage": {
                                         "value": "${markdownContent.replaceAll('"', '\\"')}",
                                         "representation": "markdown"
                                     }
                                 }
                             }' \
                             "${CONFLUENCE_URL}/rest/api/content/"
                    """)
                }
            }
        }
    }


