pipeline {
  agent {
    label 'Maven-Build-Env' // Use the Maven slave node for this pipeline
  }
  stages {
    stage('Validate Project') {
        steps {
            sh 'mvn validate'
        }
    }
    stage('Unit Test'){
        steps {
            sh 'mvn test'
        }
    }
    stage('Integration Test'){
        steps {
            sh 'mvn verify -DskipUnitTests'
        }
    }
    stage('App Packaging'){
        steps {
            sh 'mvn package'
        }
    }
    stage ('Checkstyle Code Analysis'){
        steps {
            sh 'mvn checkstyle:checkstyle'
        }
    }
    stage('SonarQube Inspection') {
        steps {
            sh  """mvn sonar:sonar \
                   -Dsonar.projectKey=Maven-JavaWebApp \
                   -Dsonar.host.url=http://172.31.58.76:9000 \
                   -Dsonar.login=4ccaf99a64cb1bdce2432454da170790d89eaa6b"""
        }
    }
    stage("Upload Artifact To Nexus"){
        steps{
             sh 'mvn deploy'
        }
        post {
            success {
              echo 'Successfully Uploaded Artifact to Nexus Artifactory'
        }
      }
    }
  }
}
