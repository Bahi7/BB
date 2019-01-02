
pipeline {
    agent any
    tools {
     maven 'MVN3'   
     jdk 'JDK8'
    }
    stages {
    stage('Build'){
      steps {
        bat 'mvn clean install'
      }
        post{
            success{
                junit 'target/surefire-reports/**/*.xml'
            }
        }  
    }
    
    stage('Test'){
      steps {
        bat "mvn test"
      }
      post{
            success{
                junit 'target/surefire-reports/**/*.xml'
            }
        } 
    }
        stage('Coverage Code'){
      steps {
        bat "mvn cobertura:cobertura -Dcobertura.report.format=xml"
      }
      post{
            success{
                cobertura coberturaReportFile: '**/target/site/cobertura/coverage.xml'
            }
        }

    }


    stage('JavaDoc Generation'){
      steps {
        bat "mvn javadoc:javadoc"
        bat "mvn site"
      }
    }

    stage("build & SonarQube analysis") {
            agent any
            steps {
              withSonarQubeEnv('SonarCube') {
                bat 'mvn clean package sonar:sonar'
              }
            }
          }

    
    stage('Deploy'){
      steps {
         bat "mvn deploy"
        echo 'Deploying ..'
      }
    }
        
     stage('Release') {
            when {
                expression { params.RELEASE }
            }
            steps {
                ansiColor("xterm") {
                    bat 'mvn -B release:prepare'
                    bat 'mvn -B release:perform'
                }
            }
        }


  }  
}
