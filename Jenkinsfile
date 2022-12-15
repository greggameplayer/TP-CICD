podTemplate(containers: [
        containerTemplate(name: 'maven', image: 'maven:3.8.4-openjdk-17-slim', command: 'sleep', args: '99d')
]) {
    node(POD_LABEL) {
        container('maven') {
            git url: scm.userRemoteConfigs[0].url, branch: scm.branches[0].name
            stage('build') {
                sh '''
                mvn clean package -DskipTests
                '''
            }
            stage('test') {
                sh '''
                mvn test surefire-report:report
                '''
            }
        }
    }
}
