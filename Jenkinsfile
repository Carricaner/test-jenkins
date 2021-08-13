pipeline{
    agent any
    // place only below agent can make env params avaliable in the entire jenkins file.
    // BTW, Better using all-capitalized letters in keys
    environment { 
        CC = 'clang'
    }
    stages {
        stage('env') {
            // Env params within stage directive can only be used in the steps within the stage.
            environment{
                DEBUG_FLAGS = "-g"
                GREETING = "Greeting"
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
            }
        }
    }
}