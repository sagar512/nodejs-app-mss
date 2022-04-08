pipeline {

agent { node { label 'panther' } }

options {

    skipDefaultCheckout()
    disableConcurrentBuilds()
    disableResume()
    }
    
 environment {
    imagename = "sagar512/demoproject7"
    dockerImage = ''
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
    
      stage('Run quality checks') {
         when {
             allOf {
                 branch 'master'
             }
         }
         environment {
             scannerHome = tool 'SonarQubeScanner'
         }
         steps {
             withSonarQubeEnv('sonarqube') {
                 sh '${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=reponame -Dsonar.sources=src'
             }
         }
     }
    
     // Code quality gate checks
     stage ("SonarQube Quality Gate") {
         when {
             allOf {
                 branch 'master'
             }
         }
         steps {
             script {
                 def qualitygate = waitForQualityGate() 
                 if (qualitygate.status != "OK") {
                     error "Pipeline aborted due to quality gate coverage failure: ${qualitygate.status}"
                 }
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

//    stage('Deployment Started'){

//        echo 'Building Started'
//        script{
//            def remote =
//       }
//    }

    }
    
}
