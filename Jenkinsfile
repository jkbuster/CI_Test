#!groovy

podTemplate(label: 'docker-build',  containers: [
  containerTemplate(name: 'docker', image: 'docker:1.11-dind', ttyEnabled: true, command: 'cat'),
  containerTemplate(name: 'kubectl', image: 'lachlanevenson/k8s-kubectl:latest', ttyEnabled: true, command: 'cat')
],
volumes: [
  hostPathVolume(mountPath: "/var/run/docker.sock", hostPath: "/var/run/docker.sock")
]) {
  node('docker-build') {
    checkout scm

    // Variables
    def commit_id = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
    def branch_id = env.BRANCH_NAME
    def build_id = env.BUILD_NUMBER
    def registry = 'quay.io/jkbuster/cidemo'
    def cur_version = sh(git describe --abbrev=0 --tags).trim()

    stage('Build') {
      echo 'Building..'

      container('docker') {
        sh "docker build -t ${registry}:${build_id}_${commit_id} ."

        withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'Quay.io',
          usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
          sh "docker login -u $USERNAME -p $PASSWORD quay.io/jkbuster"
          sh "docker push ${registry}:${build_id}_${commit_id}"
        }
      }
    }

    stage('Test') {
      echo 'Testing..'

      container('kubectl') {
        sh "kubectl run cidemo-${branch_id}-${build_id} --image=${registry}:${build_id}_${commit_id} --port=80 --expose"
        sh "sleep 10; wget http://cidemo-${branch_id}-${build_id}"
        sh "kubectl delete deploy/cidemo-${branch_id}-${build_id} svc/cidemo-${branch_id}-${build_id}"
      }
    }
    stage('Deploy') {
      echo 'Deploying....'
      echo "Previous version: ${cur_version}"
      container('docker') {
        sh "docker tag ${registry}:${build_id}_${commit_id} ${registry}:${branch_id}"
        withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'Quay.io',
          usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
          sh "docker login -u $USERNAME -p $PASSWORD quay.io/jkbuster"
          sh "docker push ${registry}:${branch_id}"
        }
      }
      container('kubectl') {
        sh "kubectl set image deploy/cidemo cidemo=${registry}:${build_id}_${commit_id}"
      }
    }
  }
}
