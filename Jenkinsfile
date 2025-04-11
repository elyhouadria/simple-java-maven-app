pipeline {
    agent any
    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven399"
    }
    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/aymendr/simple-java-maven-app.git'
                bat 'echo Build Project'
                // Run Maven on a Unix agent.
                bat "mvn -Dmaven.test.failure.ignore=true clean compile"

                // To run Maven on a Windows agent, use

                // bat "mvn -Dmaven.test.failure.ignore=true clean package"

            }
        }
        stage('Test') {
            parallel{
                        stage("Test on windows"){
                            steps {
                                bat 'echo Testing on windows'
                                // Run Maven on a Unix agent.
                                bat "mvn test"
                            }
                        }
                        stage("Test on Linux"){
                            steps {
                                bat 'echo Testing on linux'
                                // Run Maven on a Unix agent.
                                // bat "mvn test"
                            }
                        }   
                    }             
                }
        stage('Packaging') {

            steps {

                bat 'echo Pakckaging...'

                // Run Maven on a Unix agent.

                bat "mvn package"

            }

            post {

                // If Maven was able to run the tests, even if some of the test

                // failed, record the test results and archive the jar file.

                success {

                    junit '**/target/surefire-reports/TEST-*.xml'

                    // archiveArtifacts 'target/*.jar'

                    archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false

                }

            }

        }

    }

}