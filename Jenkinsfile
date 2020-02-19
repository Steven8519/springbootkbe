node{
    stage('SCM Checkout'){
        git credentialsId: 'GIT_CREDENTIALS', url:  'https://github.com/Steven8519/springbootkbe.git', branch: 'master'
    }

    stage("Gradle build"){
      sh "./gradlew clean build"

    }


    stage('Build Docker Image'){
        sh 'docker build -t steven8519/kayak-service .'
    }

    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'Docker_Hub_ID', variable: 'Docker_Hub_ID')]) {
          sh "docker login -u steven8519 -p ${Docker_Hub_ID}"
        }
        sh 'docker push steven8519/kayak-service'
     }

     stage("Deploy MongoDB"){
      kubernetesDeploy(
         kubeconfigId: 'kubeconfig',
         configs: 'deployment-mongo.yml',
         enableConfigSubstitution: true
      )
    }

      stage("Deploy app"){
        kubernetesDeploy(
            kubeconfigId: 'kubeconfig',
            configs: 'deployment.yml',
            enableConfigSubstitution: true
        )
      }

      stage("Deploy app"){
              kubernetesDeploy(
                  kubeconfigId: 'kubeconfig',
                  configs: 'istio-gateway.yml',
                  enableConfigSubstitution: true
              )
            }

}