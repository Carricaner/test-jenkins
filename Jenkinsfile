pipeline{
    agent any
    stages {
        stage('env-printing') {
            steps{
                // Remember to use double quotes
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
                echo "JAVA_HOME: ${env.JAVA_HOME}"
                echo "Jenkins URL: ${JENKINS_URL}"
                echo "Jenkins Node Name: ${NODE_NAME}"
                echo "Jenkins Workspace: ${WORKSPACE}"
            }
        }
    }
}