pipeline {
    agent any
    triggers {
        pollSCM('*****')
    }
    stages {
        stage("Compile"){
            steps{
                sh "./gradlew compileJava"
            }
        }
        stage("Unit test"){
            steps{
                sh "./gradlew test"
            }
        }
        stage("Package"){
            steps{
                sh "./gradlew build"
            }
        }
        stage("Docker build"){
            steps {
                sh "docker build -t rlovuni/calculador ."
            }
        }
        stage("Docker push"){
            steps {
                sh "docker push rlovuni/calculador"
            }
        }
        stage("Deploy to staging"){
            steps {
                sh "docker run -d --rm -p 8765:8080 --name calculador rlovuni/calculador"
            }
        }
        stage("Acceptance test"){
            steps {
                sleep 60
                sh "chmod +x acceptance_test.sh && ./acceptance_test.sh"
            }
        }
    }
    post {
        always {
            sh "docker stop calculador"
        }
    }
}