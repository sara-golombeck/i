// // pipeline {
// //    agent any
// //    tools {
// //        maven 'Maven-3.6.2'
// //        jdk 'JAVA_8'
// //    }
   
// //    stages {
// //        stage('Checkout') {
// //            steps {
// //                checkout scm
// //            }
// //        }
       
// //        stage('Build and Deploy') {
// //            steps {
// //                configFileProvider([configFile(fileId: 'Artifactory-Settings', variable: 'MAVEN_SETTINGS')]) {
// //                    sh "mvn -s $MAVEN_SETTINGS clean deploy"
// //                }
// //            }
// //        }
// //    }
// // }


// pipeline {
//     agent any
//     tools {
//         maven 'Maven-3.6.2'
//         jdk 'JAVA_8'
//     }
    
//     parameters {
//         string(name: 'RELEASE_VERSION', defaultValue: '', description: 'Release version')
//     }
    
//     stages {
//         stage('Checkout') {
//             steps {
//                 checkout scm
//             }
//         }
        
//         stage('Build and Deploy Snapshot') {
//             when {
//                 not { expression { params.RELEASE_VERSION } }
//             }
//             steps {
//                 configFileProvider([configFile(fileId: 'Artifactory-Settings', variable: 'MAVEN_SETTINGS')]) {
//                     sh "mvn -s $MAVEN_SETTINGS clean deploy"
//                 }
//             }
//         }
        
//         stage('Perform Release') {
//             when {
//                 expression { params.RELEASE_VERSION }
//             }
//             steps {
//                 configFileProvider([configFile(fileId: 'Artifactory-Settings', variable: 'MAVEN_SETTINGS')]) {
//                     // Set release version
//                     sh "mvn versions:set -DnewVersion=${params.RELEASE_VERSION}"
//                     // Build and deploy release
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
    
    parameters {
        string(name: 'RELEASE_VERSION', description: 'Version to release (e.g. 1.0.0)')
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build and Deploy') {
            when {
                not {
                    expression { params.RELEASE_VERSION }
                }
            }
            steps {
                configFileProvider([configFile(fileId: 'Artifactory-Settings', variable: 'MAVEN_SETTINGS')]) {
                    sh "mvn -s $MAVEN_SETTINGS clean deploy"
                }
            }
        }
        
        stage('Release') {
            when {
                expression { params.RELEASE_VERSION }
            }
            steps {
                configFileProvider([configFile(fileId: 'Artifactory-Settings', variable: 'MAVEN_SETTINGS')]) {
                    sh "mvn versions:set -DnewVersion=${params.RELEASE_VERSION}"
                    sh "mvn -s $MAVEN_SETTINGS clean deploy"
                }
            }
        }
    }
}