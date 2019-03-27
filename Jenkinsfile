pipeline {
    agent any
    
    stages {
        stage('---Build---') {
            steps {
                sh "docker stop \$(docker ps -q --filter ancestor=demo)"
                sh "docker build -t demo ."
                sh "docker run -d -p 4000:80 demo"
            }
        }
        stage('---Test---') {
            steps {
                script {
                    hook = registerWebhook()
                    withAWS(credentials: 'awsKeys') {
                        snsPublish(message: hook.getURL(), topicArn: "arn:aws:sns:us-west-2:600253944034:jenkins-test", subject: "Test message to jenkins")
                    }
                    slackSend(message: "Begin Testing", channel: "@trevor_garn")
                    data = waitForWebhook hook
                }
                slackSend(message: "Testing complete", channel: "@trevor_garn")
            }
        }
    }
}