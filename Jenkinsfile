podTemplate(label: 'docker-build',  containers: [
  containerTemplate(name: 'docker', image: 'docker:1.11-dind', ttyEnabled: true, command: 'cat')
],
volumes: [
  hostPathVolume(mountPath: "/var/run/docker.sock", hostPath: "/var/run/docker.sock")
]) {
  node('docker-build') {
    stage('Checkout') {
      checkout scm
    }
    stage('Build') {
      echo 'Building..'
      container('docker') {
        sh 'docker login -u jkbuster_jenkins quay.io/jkbuster'
        sh 'ls -al'
        sh 'pwd'
        sh "docker build -t quay.io/jkbuster/cidemo:${env.GIT_COMMIT} ."
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
