podTemplate(label: 'docker-build', containers: [
  containerTemplate(name: 'docker', image: 'docker:dind')
],
volumes: [
  hostPathVolume(mountPath: "/var/run/docker.sock", hostPath: "/var/run/docker.sock")
]) {
  stage('Build') {
    node('docker-build') {
      echo 'Building..'
      container('docker') {
        stage 'Testing in container'
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
