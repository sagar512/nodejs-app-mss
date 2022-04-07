pipeline {

agent { node { label 'panther' } }

options {

    skipDefaultCheckout()
    disableConcurrentBuilds()
    disableResume()
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

    // container creation and push to hub //

    stage(' Docker Build and Push '){
        steps{
            echo 'Building Started'
            script{
                docker login('hub.docker.com', 'docker-creds') {
                    def customImage = docker .build('https://hub.docker.com/repository/docker/sagar512/demoproject7:${currentBuild.id}')
                    customImage.push('${currentBuild.id}')
                }


            }


        }
    }

    // Deploy //

//    stage('Deployment Started'){

//        echo 'Building Started'
//        script{
//            def remote =
//        }
//    }

    }
        
}
