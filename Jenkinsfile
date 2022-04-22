pipeline {
  agent any
  stages {
    stage('build') {
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      steps {
        echo 'compiling sysfoo app...'
        sh 'mvn compile'
      }
    }

    stage('test') {
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      steps {
        echo 'running unit tests....'
        sh 'mvn clean test'
      }
    }

    stage('package') {
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      steps {
        
        if (env.BRANCH_NAME == 'master'){
        echo 'generating artifact....'
        sh 'mvn package -DskipTests'
        archiveArtifacts 'target/*.war'
        } else {
         echo 'This is not master branch'}
      
    }

    stage('Docker BnP.') {
      agent any
      steps {
        script {
          if (env.BRANCH_NAME == 'master'){
          docker.withRegistry('https://index.docker.io/v1/', 'dockerlogin') {
            def dockerImage = docker.build("mohammedimrantk/sysfoo:v${env.BUILD_ID}", "./")
            dockerImage.push()
            dockerImage.push("latest")
            dockerImage.push("dev")
          } 
          }else {
            echo 'This is not master branch'}
          }
        }

      }
    }
  }
  
  tools {
    maven 'Maven 3.6.3'
  }
  post {
    always {
      echo 'This pipeline is completed..'
    }

  }
}
