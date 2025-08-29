// pipeline {
//     agent {
//         label "master"
//     }
//     tools {
//         // Note: this should match with the tool name configured in your jenkins instance (JENKINS_URL/configureTools/)
//         maven "MVN_HOME"
// node {
//   stage('SCM') {
//     checkout scm
//   }
//   stage('SonarQube Analysis') {
//     def mvn = tool 'Default Maven';
//     withSonarQubeEnv() {
//       sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=customerApp"
//     }
//   }
// } 
//     }
// 	 environment {
//         // This can be nexus3 or nexus2
//         NEXUS_VERSION = "nexus3"
//         // This can be http or https
//         NEXUS_PROTOCOL = "http"
//         // Where your Nexus is running
//         NEXUS_URL = "3.94.89.205:8081/"
//         // Repository where we will upload the artifact
//         NEXUS_REPOSITORY = "SimpleCustomerApp"
//         // Jenkins credential id to authenticate to Nexus OSS
//         NEXUS_CREDENTIAL_ID = "admin"
//     }
//     stages {
//         stage("clone code") {
//             steps {
//                 script {
//                     // Let's clone the source
//                     git 'https://github.com/ratnasagar22/SimpleCustomerApp_test.git';
//                 }
//             }
//         }
//         stage("mvn build") {
//             steps {
//                 script {
//                     // If you are using Windows then you should use "bat" step
//                     // Since unit testing is out of the scope we skip them
//                     sh 'mvn -Dmaven.test.failure.ignore=true install'
//                 }
//             }
//         }
//         stage("publish to nexus") {
//             steps {
//                 script {
//                     // Read POM xml file using 'readMavenPom' step , this step 'readMavenPom' is included in: https://plugins.jenkins.io/pipeline-utility-steps
//                     pom = readMavenPom file: "pom.xml";
//                     // Find built artifact under target folder
//                     filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
//                     // Print some info from the artifact found
//                     echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
//                     // Extract the path from the File found
//                     artifactPath = filesByGlob[0].path;
//                     // Assign to a boolean response verifying If the artifact name exists
//                     artifactExists = fileExists artifactPath;
//                     if(artifactExists) {
//                         echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
//                         nexusArtifactUploader(
//                             nexusVersion: NEXUS_VERSION,
//                             protocol: NEXUS_PROTOCOL,
//                             nexusUrl: NEXUS_URL,
// 			    groupId: pom.groupId,
//                             version: pom.version,
//                             repository: NEXUS_REPOSITORY,
//                             credentialsId: NEXUS_CREDENTIAL_ID,
//                             artifacts: [
//                                 // Artifact generated such as .jar, .ear and .war files.
//                                 [artifactId: pom.artifactId,
//                                 classifier: '',
//                                 file: artifactPath,
//                                 type: pom.packaging],
//                                 // Lets upload the pom.xml file for additional information for Transitive dependencies
//                                 [artifactId: pom.artifactId,
//                                 classifier: '',
//                                 file: "pom.xml",
//                                 type: "pom"]
//                             ]
//                         );
//                     } else {
//                         error "*** File: ${artifactPath}, could not be found";
//                     }
//                 }
//             }
//         }
//     }
// }

pipeline {
    agent any

    tools {
        // Maven tool name must match what's configured in Jenkins > Global Tool Configuration
        maven 'jenkins file'
    }

    environment {
        // Optional: can be used if you need in future stages
        SONAR_PROJECT_KEY = "customerApp"
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Maven Build + SonarQube Analysis') {
            steps {
                script {
                    def mvnHome = tool name: 'MVN_HOME', type: 'maven'
                    withSonarQubeEnv('SonarQube') {
                        sh "${mvnHome}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=${SONAR_PROJECT_KEY}"
                    }
                }
            }
        }
    }
}
