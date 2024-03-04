pipeline {
  agent any
  tools {
    maven "maven"
  }

  environment {
      def mvn = tool 'maven'

      // NEXUS_VERSION = "nexus3"
      // NEXUS_PROTOCOL = "http"
      // NEXUS_URL = "172.17.0.2:8081"
      // NEXUS_REPOSITORY = "repoJenkinsLy"
      // NEXUS_CREDENTIAL_ID = ""
      // ARTIFACT_VERSION = "${BUILD_NUMBER}"
  }
  
  stages {
    stage('Git Check out') {
      steps{
        checkout scm
      } 
    }

    stage('Maven build') {
      steps {
        sh "${mvn} clean package "
      }
    }

    stage('SonarQube Analysis') {
      steps{
        withSonarQubeEnv('sonarqube') {
        sh "${mvn} clean verify sonar:sonar -Dsonar.projectKey=java-jenkins  -Dsonar.projectName=java-jenkins -Dsonar.host.url=http://localhost:9000   -Dsonar.login=sqp_66f6fed3114b1362ce21b12ae95c64994395c96b"
      }
    }
     
  }

  // stage("publish to nexus") {
  //           steps {
  //               script {
  //                   // Read POM xml file using 'readMavenPom' step , this step 'readMavenPom' is included in: https://plugins.jenkins.io/pipeline-utility-steps
  //                   pom = readMavenPom file: "pom.xml";
  //                   // Find built artifact under target folder
  //                   filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
  //                   // Print some info from the artifact found
  //                   echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
  //                   // Extract the path from the File found
  //                   artifactPath = filesByGlob[0].path;
  //                   // Assign to a boolean response verifying If the artifact name exists
  //                   artifactExists = fileExists artifactPath;

  //                   if(artifactExists) {
  //                       echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";

  //                       nexusArtifactUploader(
  //                           nexusVersion: NEXUS_VERSION,
  //                           protocol: NEXUS_PROTOCOL,
  //                           nexusUrl: NEXUS_URL,
  //                           groupId: pom.groupId,
  //                           version: ARTIFACT_VERSION,
  //                           repository: NEXUS_REPOSITORY,
  //                           credentialsId: NEXUS_CREDENTIAL_ID,
  //                           artifacts: [
  //                               // Artifact generated such as .jar, .ear and .war files.
  //                               [artifactId: pom.artifactId,
  //                               classifier: '',
  //                               file: artifactPath,
  //                               type: pom.packaging]
  //                           ]
  //                       );

  //                   } else {
  //                       error "*** File: ${artifactPath}, could not be found";
  //                   }
  //               }
  //           }
  //       }
 }
}
