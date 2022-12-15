podTemplate(containers: [
        containerTemplate(name: 'maven', image: 'maven:3.8.4-openjdk-17-slim', command: 'sleep', args: '99d')
]) {

    node(POD_LABEL) {
        stages {
            container('maven') {
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
        }
    }

    post {
        success {
            junit '**/target/surefire-reports/TEST-*.xml'
            archiveArtifacts 'target/*.jar'
            step([$class: 'JacocoPublisher'])
        }
        cleanup {
            cleanWs deleteDirs: true
        }
    }
}
