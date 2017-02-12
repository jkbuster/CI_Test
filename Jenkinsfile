podTemplate(label: 'docker-build', cloud: 'default',  containers: [
  containerTemplate(name: 'docker', image: 'docker:dind', ttyEnabled: true, command: 'cat', privileged: true, instanceCap: 1)
],
volumes: [
  hostPathVolume(mountPath: "/var/run/docker.sock", hostPath: "/var/run/docker.sock")
]) {
  stage('Build') {
    container('docker') {
      echo 'Building..'
      checkout scm
      sh './test_script.sh'
      sh 'docker build .'
    }
  }
  stage('Test') {
    echo 'Testing..'
  }
  stage('Deploy') {
    echo 'Deploying....'
  }
}
