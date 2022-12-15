pipeline {
    agent {
        kubernetes {
            containerTemplate {
                name 'maven'
                image 'maven:3.8.4-jdk-17'
                command 'sleep'
                args '99d'
            }
            defaultContainer 'maven'
        }
    }
    stages {
        stage('build') {
            steps {
                sh '''
                mvn clean package -DskipTests
                '''
            }
        }
        stage('test') {
            steps {
                sh '''
                mvn test surefire-report:report
                '''
            }
        }
    }

    post {
        success {
            junit '**/target/surefire-reports/TEST-*.xml'
            archiveArtifacts 'target/*.jar'
            step( [ $class: 'JacocoPublisher' ] )
        }
        cleanup {
            cleanWs deleteDirs: true
        }
    }
}
