podTemplate(label: 'testing', containers: [
    containerTemplate(name: 'jnlp', image: 'jenkinsci/jnlp-slave:alpine', args: '${computer.jnlpmac} ${computer.name}')
  ]) {
  node('testing') {
    stage('Preparation')
    container('docker') {
      echo "Preparing..."
    }
  }
}
