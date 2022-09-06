def imageName = 'manikanta07.jfrog.io/default-maven-local/valaxy-rtp'
def registry  = 'https://manikanta07.jfrog.io/'
def version   = '1.0.3'
def app
pipeline {
    agent {
       node {
         label "valaxy"
      }
    }
    stages {
        stage('Build') {
            steps {
                echo '<--------------- Building --------------->'
                sh 'printenv'
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo '<------------- Build completed ----------->'
            }
        }
         
        stage('Unit Test') {
            steps {
                echo '<--------------- Unit Testing started  --------------->'
                sh 'mvn surefire-report:report'
                echo '<------------- Unit Testing stopped  --------------->'
            }
        }
           stage('Sonar Analysis') {
      
            environment {
                scannerHome = tool 'SonarQubeScanner'
            }
            steps {
                echo '<--------------- Sonar Analysis Started --------------->'
                withSonarQubeEnv('SonarQube'){
                    sh "${scannerHome}/bin/sonar-scanner"
                }    
                echo '<--------------- Sonar Analysis Ends --------------->'
            }    
        }
   stage("Docker Build") {
          steps {
            script {
               echo '<--------------- Docker Build Started --------------->'
               app = docker.build(imageName)
               echo '<--------------- Docker Build Ends --------------->'
            }
          }
        }
        
      stage("Docker Publish") {
          steps {
            script {
               echo '<--------------- Docker Publish Started --------------->'
               docker.withRegistry(registry, 'artifactorycredentialid'){
                 docker.image(imageName).push(version)
               }
               echo '<--------------- Docker Publish Ends --------------->'
            }
          }
        }

    }
 }
