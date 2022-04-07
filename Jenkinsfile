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
//            sh 'apt-get update -y'
//            sh 'npm install pm2 -g'
//            sh 'npm install npm@latest -g'
            sh 'npm install'
            sh 'npm uninstall sequelize'
            sh 'npm install sequelize@v6.6.2'
        }
    } 
    
    // build the project //

    stage(" building the project"){

        steps {
            sh 'npm run build'
        }
    }

    // container creation and push to hub //

    stage(' Docker Build and Push '){
        steps{
            echo 'Building Started'
            script{
                docker.withRegistry('docker.io', 'docker-creds'){
                    def customerimage = docker .build('https://hub.docker.com/repository/docker/sagar512/demoproject7:${currentBuild.id}')
                    customerImage.push('${currentBuild.id}')
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
