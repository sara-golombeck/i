
pipeline {
    agent any
    tools {
        maven 'Maven-3.6.2'
        jdk 'JAVA_8'
    }
    
    parameters {
        string(name: 'RELEASE_VERSION', description: 'Version number for the release')
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Update Version') {
            steps {
                sh "mvn versions:set -DnewVersion=${params.RELEASE_VERSION}"
            }
        }
        
        stage('Build and Deploy') {
            steps {
                configFileProvider([configFile(fileId: 'Jenkins-Artifactory', variable: 'MAVEN_SETTINGS')]) {
                    sh "mvn -s $MAVEN_SETTINGS clean deploy"
                }
            }
        }
    }
}