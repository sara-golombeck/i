pipeline {
    agent any
    tools {
        maven 'Maven-3.6.2'
        jdk 'OpenJDK 8'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'git@gitlab.com:saraGo/sara-thumbnailer-image-io.git'
            }
        }
        stage('Build and Deploy') {
            steps {
                configFileProvider([configFile(fileId: 'maven-settings', variable: 'MAVEN_SETTINGS')]) {
                    sh "mvn -s $MAVEN_SETTINGS clean deploy"
                }
            }
        }
    }
}