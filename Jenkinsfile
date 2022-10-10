node {  

  def app

  stages {
    stage('Clone Repository'){

      checkout scm
    }

    stage('Update GIT') {
      script {
        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE'){
          withCredentials([usernamePassword(credentialsId:'github', passwordVariable:'GIT_PASSWORD', usernameVariable:'GIT_USERNAME')]) {
            sh "git config user.email luiz.holanda06@gmail.com"
            sh "git config user.name luizclaudioholanda"

            sh "cat deployment.yaml"
            sh "sed -i 's+luizholandadocker/node-docker.*+luizholandadocker/node-docker:${DOCKERTAG}+g' deployment.yaml"

            sh "cat deployment.yaml"
            sh "git add ."

            sh "git commit -m 'Done Jenkins change job manifest:${env.BUILD_NUMBER}'"
            sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/node-docker-manifest.git HEAD:master"
          }
        }
      }
    }

    stage('Deploying into k8s'){
      script{
        withKubeConfig([credentialsId: 'minikube']) {
          sh 'kubectl apply -f deployment.yaml'
        }
      }
    }
  }
  
}