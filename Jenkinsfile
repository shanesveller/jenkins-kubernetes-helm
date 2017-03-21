podTemplate(
  label: 'jenkins-kubernetes-helm',
            containers: [
              containerTemplate(name: 'jnlp', image: 'jenkinsci/jnlp-slave:2.62', args: '${computer.jnlpmac} ${computer.name}', command: ''),
              // Minikube v0.17.1
              containerTemplate(name: 'docker', image: 'docker:1.11.1', ttyEnabled: true, command: 'cat'),
            ],
            volumes: [hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock')]
) {

  node ('jenkins-kubernetes-helm') {
    stage('Checkout') {
      checkout scm
    }

    container('docker') {
      stage('Build Service') {
        sh 'docker build -t shanesveller/jenkins:latest images/jenkins/'
      }

      stage('Push Service') {
        sh "echo docker login -u shanesveller -p ..."
        sh 'echo docker push ...'
      }
    }
  }
}
