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
    def commit_id = readFile('commit').trim()
    def branch_id = env.BRANCH_NAME
    def build_id = env.BUILD_NUMBER
    def registry = 'quay.io/jkbuster/cidemo'


    stage('Build') {
      echo 'Building..'

      container('docker') {
        sh "docker build -t ${registry}:${build_id}_${commit_id} ."
        sh "docker tag ${registry}:${build_id}_${commit_id} ${registry}:${branch_id}"

        withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'Quay.io',
          usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
          sh "docker login -u $USERNAME -p $PASSWORD quay.io/jkbuster"
          sh "docker push ${registry}:${build_id}_${commit_id}"
          sh "docker push ${registry}:${branch_id}"
        }
      }
    }

    stage('Test') {
      echo 'Testing..'

      container('docker') {
        docker.image("${registry}:${build_id}_${commit_id}").withRun('-p 5130:80') {c ->
          sh "curl -i http://${hostIp(c)}:5130/"
        }
      }
    }
    stage('Deploy') {
      echo 'Deploying....'
    }
  }
}
