pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                slackSend (message: "Build Started")
            }
        }
    }
}
