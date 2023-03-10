def registry = 'https://devopshunger.jfrog.io'
def imageName = 'https://devopshunger.jfrog.io/dockernodejs1-docker/demo-nodejs'
def version   = '1.0.2'
pipeline{
    agent {
        node {
            label "slave-node"
        }
    }
    tools {nodejs 'Nodejs'}

    stages {
        stage ("Clone the code"){
            steps{
                git branch: 'main', url: 'https://github.com/devops-mk03/nodejs-project.git'
            }
        }
        stage('build') {
            steps{
                echo "------------ build started ---------"
               	sh "npm install"
                echo "------------ build completed ---------"
        }
      }

        stage('Unit Test') {
            steps {
                echo '<--------------- Unit Testing started  --------------->'
                sh 'npm test'
                echo '<------------- Unit Testing stopped  --------------->'
            }
        }

stage(" Docker Build ") {
      steps {
        script {
           echo '<--------------- Docker Build Started --------------->'
           app = docker.build(imageName+":"+version)
           echo '<--------------- Docker Build Ends --------------->'
        }
      }
    }

    stage (" Docker Publish "){
        steps {
            script {
               echo '<--------------- Docker Publish Started --------------->'  
                docker.withRegistry(registry, 'jfrog-access'){
                    app.push()
                }    
               echo '<--------------- Docker Publish Ended --------------->'  
            }
        }
    }
          //  stage('Deployment') {
           // steps {
              //  echo '<--------------- deployment started  --------------->'
              //  sh './deploy.sh'
              //  echo '<------------- deployment stopped  --------------->'
           // }
        //}  
    }
}
