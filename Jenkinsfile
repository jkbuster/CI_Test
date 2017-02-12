podTemplate(label: 'docker-build', cloud: 'default',  containers: [
  containerTemplate(name: 'docker', image: 'docker:dind', ttyEnabled: true, command: 'cat', privileged: true, instanceCap: 1)
]) {
  stage('Build') {
    node('docker-build') {
      echo 'Building..'
      container('docker') {
        echo 'In container'
        checkout scm
        sh './test_script.sh'
        sh 'docker build .'
      }
    }
  }
  stage('Test') {
    echo 'Testing..'
  }
  stage('Deploy') {
    echo 'Deploying....'
  }
}
