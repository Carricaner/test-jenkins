pipeline{
    agent any
    // place only below agent can make env params avaliable in the entire jenkins file.
    // BTW, Better using all-capitalized letters in keys
    environment { 
        // Using returnStdout
        // Usually use trim() to strip off a trailing whitespace.
        CC = """${sh(
                returnStdout: true,
                script: 'echo "Darren-Li"'
            )}""".trim()
        // Using returnStatus
        // Normally, a nonzero status code will cause the step to fail with an exception.
        // If returnStatus is checked, the value will instead be status code.
        // You may compare it to zero, for example.
        EXIT_STATUS = """${sh(
                returnStatus: true,
                script: 'exit 1'
            )}"""
    }
    stages {
        stage('env') {
            // Env params within stage directive can only be used in the steps within the stage.
            environment{
                DEBUG_FLAGS = "-g"
                GREETING = "Greeting"
                // The credential would have been configured in Jenkins with their respective credential IDs.
                JENKINS_TEST_SECRET_TEXT = credentials('jenkinsfile-test-secret-text')
            }
            steps{
                // Remember to use double quotes
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
                echo "JAVA_HOME: ${env.JAVA_HOME}"
                echo "Jenkins URL: ${JENKINS_URL}"
                echo "Jenkins Node Name: ${NODE_NAME}"
                echo "Jenkins Workspace: ${WORKSPACE}"
                echo "CC: ${CC}"
                echo "DEBUG_FLAGS: ${DEBUG_FLAGS}"
                echo "GREETING: ${GREETING}"
                echo "EXIT_STATUS: ${EXIT_STATUS}"
                sh 'printenv'
                echo "Test echo credential JENKINS_TEST_SECRET_TEXT: ${JENKINS_TEST_SECRET_TEXT}"
            }
        }
    }
}