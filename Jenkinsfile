// // // // pipeline {
// // // //    agent any
// // // //    tools {
// // // //        maven 'Maven-3.6.2'
// // // //        jdk 'JAVA_8'
// // // //    }
   
// // // //    stages {
// // // //        stage('Checkout') {
// // // //            steps {
// // // //                checkout scm
// // // //            }
// // // //        }
       
// // // //        stage('Build and Deploy') {
// // // //            steps {
// // // //                configFileProvider([configFile(fileId: 'Artifactory-Settings', variable: 'MAVEN_SETTINGS')]) {
// // // //                    sh "mvn -s $MAVEN_SETTINGS clean deploy"
// // // //                }
// // // //            }
// // // //        }
// // // //    }
// // // // }


// // // pipeline {
// // //     agent any
// // //     tools {
// // //         maven 'Maven-3.6.2'
// // //         jdk 'JAVA_8'
// // //     }
    
// // //     parameters {
// // //         string(name: 'RELEASE_VERSION', defaultValue: '', description: 'Release version')
// // //     }
    
// // //     stages {
// // //         stage('Checkout') {
// // //             steps {
// // //                 checkout scm
// // //             }
// // //         }
        
// // //         stage('Build and Deploy Snapshot') {
// // //             when {
// // //                 not { expression { params.RELEASE_VERSION } }
// // //             }
// // //             steps {
// // //                 configFileProvider([configFile(fileId: 'Artifactory-Settings', variable: 'MAVEN_SETTINGS')]) {
// // //                     sh "mvn -s $MAVEN_SETTINGS clean deploy"
// // //                 }
// // //             }
// // //         }
        
// // //         stage('Perform Release') {
// // //             when {
// // //                 expression { params.RELEASE_VERSION }
// // //             }
// // //             steps {
// // //                 configFileProvider([configFile(fileId: 'Artifactory-Settings', variable: 'MAVEN_SETTINGS')]) {
// // //                     // Set release version
// // //                     sh "mvn versions:set -DnewVersion=${params.RELEASE_VERSION}"
// // //                     // Build and deploy release
// // //                     sh "mvn -s $MAVEN_SETTINGS clean deploy"
// // //                 }
// // //             }
// // //         }
// // //     }
// // // }

// // pipeline {
// //     agent any
// //     tools {
// //         maven 'Maven-3.6.2'
// //         jdk 'JAVA_8'
// //     }
    
// //     parameters {
// //         string(name: 'RELEASE_VERSION', description: 'Version to release (e.g. 1.0.0)')
// //     }
    
// //     stages {
// //         stage('Checkout') {
// //             steps {
// //                 checkout scm
// //             }
// //         }
        
// //         stage('Build and Deploy') {
// //             when {
// //                 not {
// //                     expression { params.RELEASE_VERSION }
// //                 }
// //             }
// //             steps {
// //                 configFileProvider([configFile(fileId: 'Artifactory-Settings', variable: 'MAVEN_SETTINGS')]) {
// //                     sh "mvn -s $MAVEN_SETTINGS clean deploy"
// //                 }
// //             }
// //         }
        
// //         stage('Release') {
// //             when {
// //                 expression { params.RELEASE_VERSION }
// //             }
// //             steps {
// //                 configFileProvider([configFile(fileId: 'Artifactory-Settings', variable: 'MAVEN_SETTINGS')]) {
// //                     sh "mvn versions:set -DnewVersion=${params.RELEASE_VERSION}"
// //                     sh "mvn -s $MAVEN_SETTINGS clean deploy"
// //                 }
// //             }
// //         }
// //     }
// // }


// pipeline {
//     agent any
//     tools {
//         maven 'Maven-3.6.2'
//         jdk 'JAVA_8'
//     }
    
//     parameters {
//         string(name: 'RELEASE_VERSION', description: 'Version number for the release')
//     }
    
//     stages {
//         stage('Checkout') {
//             steps {
//                 checkout scm
//             }
//         }
        
//         stage('Update Version') {
//             steps {
//                 sh "mvn versions:set -DnewVersion=${params.RELEASE_VERSION}"
//             }
//         }
        
//         stage('Build and Deploy') {
//             steps {
//                 configFileProvider([configFile(fileId: 'Artifactory-Settings', variable: 'MAVEN_SETTINGS')]) {
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
                // ראשית נתקין את ה-plugin הנדרש
                sh """
                    mvn org.apache.maven.plugins:maven-dependency-plugin:3.6.1:get \
                    -DgroupId=org.codehaus.mojo \
                    -DartifactId=versions-maven-plugin \
                    -Dversion=2.16.2 \
                    -Dtransitive=false
                """
                
                // כעת נעדכן את הגרסה
                sh "mvn org.codehaus.mojo:versions-maven-plugin:2.16.2:set -DnewVersion=${params.RELEASE_VERSION}"
            }
        }
        
        stage('Build and Deploy') {
            steps {
                configFileProvider([configFile(fileId: 'Artifactory-Settings', variable: 'MAVEN_SETTINGS')]) {
                    // נוסיף את הדגל להתעלמות מבעיות SSL
                    sh "mvn -s $MAVEN_SETTINGS -Dmaven.wagon.http.ssl.insecure=true -Dmaven.wagon.http.ssl.allowall=true clean deploy"
                }
            }
        }
    }
}