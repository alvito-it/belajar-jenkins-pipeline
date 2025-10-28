pipeline {
    agent none

    environment {
        AUTHOR = "Muhammad Alvito Madisyahputra"
        EMAIL = "alvito@example.com"
    }

//     triggers {
//         // cron("*/5 * * * *")
//         pollSCM("*/5 * * * *")
//         // upstream(upstreamProjects: "job1,job2", threshold: hudson.model.Result.SUCCESS)
//     }

    parameters {
        string(name: "NAME", defaultValue: "Guest", description: "What is your name?")
        text(name: "DESCRIPTION", defaultValue: "", description: "Tell me about yourself!")
        booleanParam(name: "DEPLOY", defaultValue: false, description: "Is it need to deploy?")
        choice(name: "SOCIAL_MEDIA", choices: ["Instagram", "Facebook", "Twitter"], description: "Which social media that you use?")
        password(name: "SECRET", defaultValue: "", description: "Encrypt Key")
    }

    options {
        disableConcurrentBuilds()
        timeout(time: 10, unit: 'MINUTES')
    }

    stages {
        stage("Preparation") {
            parallel {
                stage("Prepare Java") {
                    agent {
                        node {
                            label "linux && java17"
                        }
                    }

                    steps {
                        echo("Prepare Java")
                        sleep(time: 5, unit: 'SECONDS')
                    }
                }

                stage("Prepare Maven") {
                    agent {
                        node {
                            label "linux && java17"
                        }
                    }

                    steps {
                        echo("Prepare Maven")
                        sleep(time: 5, unit: 'SECONDS')
                    }
                }
            }
        }

        stage("Parameter") {
            agent {
                node {
                    label "linux && java17"
                }
            }

            steps {
                echo("Hello ${params.NAME}")
                echo("Description:  ${params.DESCRIPTION}")
                echo("Deploy: ${params.DEPLOY}")
                echo("Social Media:  ${params.SOCIAL_MEDIA}")
                echo("Secret: ${params.SECRET}")
            }
        }

        stage("Prepare") {
            environment {
                APP = credentials("alvito")
            }
            
            agent {
                node {
                    label "linux && java17"
                }
            }

            steps {
               echo("Start Job: ${env.JOB_NAME}")
               echo("Start Build: ${env.BUILD_NUMBER}")
               echo("Branch Name: ${env.BRANCH_NAME}")
               echo("Author: ${AUTHOR}")
               echo("Email: ${EMAIL}")
               echo("App User: ${APP_USR}")
               sh('echo "App Password: $APP_PSW" > "secret.txt"')
            }
        }

        stage("Build") {
            agent {
                node {
                    label "linux && java17"
                }
            }

            steps {
                script {
                    for (int i = 0; i < 10; i++) {
                        echo("Script ${i}")
                    }
                }
                echo("Start Build")
                sh("./mvnw clean compile test-compile")
                echo("Finish Build")
            }
        }

        stage("Test") {
            agent {
                node {
                    label "linux && java17"
                }
            }

            steps {
                script {
                    def data = [
                        "name": "Alvito",
                        "age": 22
                    ]
                    writeJSON(file: "data.json", json: data)
                }
                echo("Start Test")
                sh("./mvnw test")
                echo("Finish Test")
            }
        }

        stage("Deploy") {
            input {
                message "Should we continue?"
                ok "Yes, we should."
                submitter "ags,alvito"
                parameters {
                    choice(name: "TARGET_ENV", choices: ["DEV", "QA", "PROD"], description: "Which environment that we should to choose?")
                }
            }
            agent {
                node {
                    label "linux && java17"
                }
            }

            steps {
                echo("Hello Deploy 1")
                echo("Hello Deploy 2")
                echo("Hello Deploy 3")
                echo("Deploy to ${TARGET_ENV} environment")
            }
        }

        stage("Release") {
            when {
                expression {
                    return params.DEPLOY
                }
            }

            agent {
                node {
                    label "linux && java17"
                }
            }

            steps {
                echo("Release success")
            }
        }
    }

    post {
        always {
            echo("I will always say hello again!")
        }

        success {
            echo("Yay, success!")
        }

        failure {
            echo("Oh no, failure!")
        }

        cleanup {
            echo("Don't care success or error, this cleanup will running after all the post run")
        }
    }
}