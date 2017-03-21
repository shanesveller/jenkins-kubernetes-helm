podTemplate(label: 'catalog-service-builder', containers: [
    containerTemplate(name: 'jnlp', image: 'jenkinsci/jnlp-slave:2.52', args: '${computer.jnlpmac} ${computer.name}', command: ''),
    containerTemplate(name: 'docker', image: 'docker:1.11.2', ttyEnabled: true, command: 'cat'),
    containerTemplate(name: 'stock-service-chart', image: 'enxebre/stock-service-chart:v2', alwaysPullImage: true, ttyEnabled: true, command: 'helm serve --address 0.0.0.0:8878 --repo-path /usr/stock-service-chart/charts/stock'),
    containerTemplate(name: 'prometheus-operated-chart', image: 'enxebre/prometheus-operated-chart', alwaysPullImage: true, ttyEnabled: true, command: 'helm serve --address 0.0.0.0:8879 --repo-path /usr/prometheus-operated-chart'),
  ],
  volumes: [hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock')]) {

    node ('catalog-service-builder') {

        stage 'Checkout'
        checkout scm

        container('docker') {
            stage 'Build Service'
            sh 'docker build -t enxebre/catalog:v2 .'

            stage 'Push Service'
            sh "docker login -u enxebre -p ${dockerHubPass}"
            sh 'docker push enxebre/catalog:v2'
        }

        container('stock-service-chart') {
            stage 'Get Helm dependencies'
            sh 'helm init'
            sh 'helm repo add stock-service-chart http://localhost:8878'
            sh 'helm repo add prometheus-operated-chart http://localhost:8879'
            sh 'helm dependency update charts/catalog'

            stage 'Deploy release'
            sh "helm upgrade ${RELEASE_NAME} charts/catalog --set=IngressDomain=${INGRESS_DOMAIN},prometheus-operated-chart.IngressDomain=${INGRESS_DOMAIN},prometheus-operated-chart.prometheus.alertmanagers.namespace=${NAMESPACE} --install --namespace=${NAMESPACE}"
        }
    }
}
