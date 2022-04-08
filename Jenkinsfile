pipeline {

agent { node { label 'spider' } }

    
options {

    skipDefaultCheckout()
    disableConcurrentBuilds()
    disableResume()
    }
    
 environment {
    imagename = "sagar512/demoproject7"
    dockerImage = ''
    //scannerHome = tool 'SonarQubeScanner 
  }   
parameters {

    string(name: 'imagename', defaultValue: '1.0', description: 'tagname')
    string(name: 'version', defaultValue: '1.0', description: 'tagname')
}

//    tools {nodejs "node12.14.1"}

stages{

    // checkout stage //
    stage('checkout from GIT'){
        steps{
            checkout scm
        }
    }

    // installing dependencies //
    stage("Install Project Dependencies"){

        steps{
            sh 'npm install'
        }
    } 
    
    // build the project //

    stage(" building the project"){

        steps {
            sh 'npm run'
        }
    }
    
    stage('SonarQube Quality Gate') {
        
         steps {
             def scannerHome = tool 'SonarScanner 9.4';
             withSonarQubeEnv('sonarqube') {
                 sh 'npm run sonar'
             }
         }
     }
    
        // container creation and push to hub //

    stage(' Docker Build and Push '){
        steps{
            echo 'Building Started'
            script{
                dockerImage = docker.build imagename
                docker.withRegistry('', 'docker-creds') {
                dockerImage.push("$BUILD_NUMBER")
                dockerImage.push('latest')    

                }


            }


        }
    }

    // Deploy //



    }
    
}
