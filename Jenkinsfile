podTemplate(containers: [
        containerTemplate(name: 'maven', image: 'maven:3.8.4-openjdk-17-slim', command: 'sleep', args: '99d')
]) {
    node(POD_LABEL) {
        stage('Checkout scm') {
            scmVars = checkout scm
        }
        container('maven') {
            git url: scm.userRemoteConfigs[0].url, branch: scmVars.GIT_BRANCH
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
