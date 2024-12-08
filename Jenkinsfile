pipeline {
    agent any
   
    parameters {
        choice choices: ['blue', 'green'], name: 'environment_selection'
    }    
    tools {
        maven 'maventool'
    }


    stages {
        stage("GIT Checkout") {
            steps {
                git 'https://github.com/datawithanand/maven-project.git'
            }
        }
        stage("Building Artifact") {
            steps {
                input(message: 'Do you want to proceed?')
                sh 'mvn clean package'
            }
        }
        stage("Test and Code Coverage") {
            steps {
                // Run tests and generate code coverage report
                sh 'mvn test jacoco:report'
            }
        }
    }

    post {
        success {
            script {
                withSonarQubeEnv('sonar') { // 'sonar' is the SonarQube server configuration name
                    sh 'mvn sonar:sonar'
                }
            }
        }
        failure {
            echo "The execution has failed."
        }
    }
}
