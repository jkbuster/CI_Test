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
        checkout scm
        echo 'In the container!'
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
