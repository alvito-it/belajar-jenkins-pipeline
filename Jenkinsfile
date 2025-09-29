pipeline {
    agent {
        node {
            label "linux && java17"
        }
    }
    stages {
        stage("Hello") {
            steps {
                echo "Hello World"
            }
        }
    }
    post {
        always {
            echo "I will always say hello again!"
        }
        success {
            echo "Yay, success!"
        }
        failure {
            echo "Oh no, failure!"
        }
        cleanup {
            echo "Don't care success or error, this cleanup will running after all the post run"
        }
    }
}