pipeline{
    agent any
    // place only below agent can make env params avaliable in the entire jenkins file.
    // BTW, Better using all-capitalized letters in keys.
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
    parameters {
        string(
            name: 'STATEMENT', 
            defaultValue: 'hello; ls /', 
            description: 'What should I say?'
        )
        choice(
            name: 'PARAMETER_01',
            choices: ['ONE', 'TWO']
        )
        booleanParam(
            name: 'BOOLEAN',
            defaultValue: true, 
            description: '' 
        )
        // Text param allows multiple lines.
        text(
            name: 'MULTI-LINE-STRING',
            defaultValue: '''
            this is a multi-line 
            string parameter example
            '''
        )
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
                echo "Jenkins TARGET_URL: ${JENKINS_URL}"
                echo "Jenkins Node Name: ${NODE_NAME}"
                echo "Jenkins Workspace: ${WORKSPACE}"
                echo "CC: ${CC}"
                echo "DEBUG_FLAGS: ${DEBUG_FLAGS}"
                echo "GREETING: ${GREETING}"
                echo "EXIT_STATUS: ${EXIT_STATUS}"
                sh 'printenv'
            }
            post {
                always {
                    echo "Testing post after stage env ..."
                }
            }
        }
        stage('credential') {
            environment{
                // The credential would have been configured in Jenkins with their respective credential IDs.
                // secret text credential
                JENKINS_TEST_SECRET_TEXT = credentials("jenkinsfile-test-secret-text")
                // secret username & password credential
                JENKINS_TEST_SECRET_USERNAME_AND_PASSWORD = credentials("jenkinsfile-test-secret-username-and-password")
            }
            steps{
                // It will detect whether the command is revealing the sensitive info and use asterisks to cover them.
                echo "Test echo credential JENKINS_TEST_SECRET_TEXT: ${JENKINS_TEST_SECRET_TEXT}"
                echo "Test echo credential JENKINS_TEST_SECRET_USERNAME_AND_PASSWORD: ${JENKINS_TEST_SECRET_USERNAME_AND_PASSWORD}"
                echo "Test echo credential JENKINS_TEST_SECRET_USERNAME_AND_PASSWORD username: ${JENKINS_TEST_SECRET_USERNAME_AND_PASSWORD_USR}"
                echo "Test echo credential JENKINS_TEST_SECRET_USERNAME_AND_PASSWORD password: ${JENKINS_TEST_SECRET_USERNAME_AND_PASSWORD_PSW}"
                
                // When using command like sh, bat or powershell, we shall use single quotes instead of double quotes.
                sh('echo ${JENKINS_TEST_SECRET_TEXT}')
                sh('echo ${JENKINS_TEST_SECRET_USERNAME_AND_PASSWORD}')
                sh('echo ${JENKINS_TEST_SECRET_USERNAME_AND_PASSWORD_USR}')
                sh('echo ${JENKINS_TEST_SECRET_USERNAME_AND_PASSWORD_PSW}')
                
                // secret private SSH key credential
                withCredentials(
                    [sshUserPrivateKey(
                        credentialsId: 'macOS-private-key', 
                        keyFileVariable: 'SSH_KEY_FOR_MAC'
                    )]
                ) {
                    echo "SSH key for macOS host: ${SSH_KEY_FOR_MAC}"
                }
            }
            post {
                always {
                    echo "Testing post after stage credential ..."
                }
            }
        }
        stage('string-interpolation') {
            environment {
                HOST = 'JENKINS'
                HTTP_METHOD = 'GET'
                TARGET_URL = 'https://google.com'
            }
            steps {
                // Only double-quotes can support the dollar-sign ($) based string interpolation.
                echo 'Hello, ${HOST}'
                echo "Hello, ${HOST}"
                
                // Avoid using double-quotes which can reveal sensitive info, instead, use single-quotes.
                sh("curl -X ${HTTP_METHOD} ${TARGET_URL}") /*** WRONG ***/
                sh('curl -X ${HTTP_METHOD} ${TARGET_URL}') /*** CORRECT ***/
                
                // Injection via interpolation
                // The firdt one will echo 'hello' then execute 'ls' as well as deisplaying the content.
                sh("echo ${params.STATEMENT}") /*** WRONG ***/
                // Using single-quote means a standard java String.
                // If we want to use single-quotes, we can use params w/o prefix params.
                // Related stackoverflow: https://stackoverflow.com/questions/45836682/using-parameters-within-a-shell-command-in-jenkinsfile-for-jenkins-pipeline
                sh('echo ${STATEMENT}') /*** CORRECT ***/

            }
        }
    }
    // The post section defines one or more additional steps that are run upon the completion of a Pipeline’s or stage’s run. (depending on the location of the post section within the Pipeline)
    // For all supported condition blocks, see: https://www.jenkins.io/doc/book/pipeline/syntax/#post
    post {
        // Execute the commands in always block regardless the state of pipeline
        always {
            echo "Always running this command ..."
        }
        // Execute the commands in success block if the state is "success."
        // Web UI --> green or blue
        success {
            echo "Success !"
        }
        // Execute the commands in unstable block if the state is "unstable" like test failures, code violation, etc.
        // Web UI --> yellow
        unstable {
            echo "Unstable ..."
        }
        // Execute the commands in failure block if the state is "failed."
        // Web UI --> red
        failure {
            echo "Failure ..."
        }
        // Run the steps in cleanup block after every other post condition has been evaluated, regardless of the Pipeline or stage’s status.
        cleanup {
            echo "Cleanup executed after all post commands ..."
        }

    }
}