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
