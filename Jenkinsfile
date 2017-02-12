podTemplate(label: 'docker-build', containers: [
    containerTemplate(name: 'docker', image: 'docker:dind', ttyEnabled: true, command: 'cat', privileged: true, instanceCap: 1),
    containerTemplate(name: 'jnlp', image: 'jenkinsci/jnlp-slave:alpine', args: '${computer.jnlpmac} ${computer.name}')
  ],
  volumes: [
        hostPathVolume(mountPath: "/var/run/docker.sock", hostPath: "/var/run/docker.sock")
  ]) {

    node ('docker-build') {
      stage('Build') {
        echo 'Building..'
        container('docker') {
          sh "pwd"
          sh "ls"
          sh "env"
          sh "docker build ."
        }
      }
      stage('Test') {
        echo 'Testing..'
      }
      stage('Deploy') {
        echo 'Deploying....'
      }
    }
}
