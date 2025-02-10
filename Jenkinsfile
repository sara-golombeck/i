// pipeline {
//     agent any
//     tools {
//         maven 'Maven-3.6.2'
//         jdk 'JAVA_8'
//     }
//     stages {
//        stage('Checkout') {
//            when {
//             //    expression { 
//             //        return env.BRANCH_NAME ==~ /(master|staging|feature\/.*)/
//             //    }
//            }
//            steps {
//                checkout scm
//            }
//        }
    
        
//         stage('Build and Deploy') {
//             steps {
//                 configFileProvider([configFile(fileId: 'maven-settings', variable: 'MAVEN_SETTINGS')]) {
//                     sh "mvn -s $MAVEN_SETTINGS clean deploy"
//                 }
//             }
//         }
//     }
// }


pipeline {
   agent any
   tools {
       maven 'Maven-3.6.2'
       jdk 'JAVA_8'
   }
   
   stages {
       stage('Checkout') {
           steps {
               checkout scm
           }
       }
       
       stage('Build and Deploy') {
           steps {
               configFileProvider([configFile(fileId: 'Artifactory-Settings', variable: 'MAVEN_SETTINGS')]) {
                   sh "mvn -s $MAVEN_SETTINGS clean deploy"
               }
           }
       }
   }
}