pipeline {
  agent any
  
  stages {
    stage('Build') {
      steps {
        sh 'mvn clean install'
      }
    }
    
    stage('Test') {
      steps {
        sh 'mvn test'
      }
    }
    
    stage('Package') {
      steps {
        sh 'mvn package'
      }
    }
    
    stage('SonarQube') {
      steps {
        sh 'mvn sonar:sonar'
      }
    }
    
    stage('Publish JAR to JFrog Artifactory') {
      steps {
        script {
          def server = Artifactory.server('i-0c748afafc5022c23')
          def rtMaven = Artifactory.newMavenBuild()
          rtMaven.tool = 'maven'
          rtMaven.deployer releaseRepo: 'geoapp', snapshotRepo: 'geoapp-snapshot', server: server
          rtMaven.deployer.deployArtifacts = false
          rtMaven.run pom: 'pom.xml', goals: 'install'
        }
      }
    }
  }
}