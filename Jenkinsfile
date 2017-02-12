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
        sh "docker build -t quay.io/jkbuster/cidemo:${env.GIT_COMMIT} ."
        sh 'docker images'
        withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'Quay.io',
          usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
          sh "docker login -u $USERNAME -p $PASSWORD quay.io/jkbuster"
          sh "docker push quay.io/jkbuster/cidemo"
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
