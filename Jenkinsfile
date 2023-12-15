pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    sh "mvn clean install -Dmaven.test.skip=true"
                }
            }
        }

        stage('Test case execution') {
            steps {
                script {
                    sh "mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install -Pcoverage-per-test"
                }
            }
        }

        stage('archive artifacts') {
            steps {
                archiveArtifacts artifacts: 'target/*.war'
            }
        }

        stage('Deployment') {
            steps {
                script {
                    deploy adapters: [tomcat9(credentialsId: 'TomcatCred', path: '', url: 'http://18.208.214.69:8080/')], contextPath: 'Planview', onFailure: false, war: 'target/*.war'
                }
            }
        }
    }

    post {
        success {
            echo "Deployment successful! Access your application at http://your-tomcat-server:8080/Planview"
        }

        failure {
            echo "Deployment failed! Check the logs for more details."
        }
    }
}
