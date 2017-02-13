#!groovy

podTemplate(label: 'docker-build',  containers: [
  containerTemplate(name: 'docker', image: 'docker:1.11-dind', ttyEnabled: true, command: 'cat')
],
volumes: [
  hostPathVolume(mountPath: "/var/run/docker.sock", hostPath: "/var/run/docker.sock")
]) {
  node('docker-build') {
    checkout scm

    // Variables
    sh 'git rev-parse --short HEAD > commit'
    sh 'cat commit'
    def commit_id = readFile('commit').trim()
    def branch_id = build.environment.get("BRANCH_NAME")
    def build_id = build.environment.get("BUILD_NUMBER")
    def registry = 'quay.io/jkbuster/cidemo'


    stage('Build') {
      echo 'Building..'

      container('docker') {
        sh "docker build -t ${registry}:${commit_id} ."
        sh "docker tag -t ${registry}:${commit_id} ${registry}:${branch_id}"

        withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'Quay.io',
          usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
          sh "docker login -u $USERNAME -p $PASSWORD quay.io/jkbuster"
          sh "docker push ${registry}"
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
}
