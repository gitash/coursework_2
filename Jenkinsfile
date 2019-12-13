node{
    def scannerHome = tool 'SonarQube';
    def app
    def remote = [:]
    remote.name = 'prod-node'
    remote.host = '40.71.251.3'
    remote.user = 'master'
    remote.password = 'Password123!'
    remote.allowAnyHosts = true

  stage('Clone repository') {
        checkout scm
    }

  stage('Build image') {
        app = docker.build("dockasher/coursework2")
    }

    stage('Test image') {
        withSonarQubeEnv('SonarQubeScanner') { // If you have configured more than one global server connection, you can specify its name
            sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectName=coursework_2 -Dsonar.projectKey=coursework_2:app -Dsonar.sources=."
        }
    }

    stage('Push image') {
            docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}
