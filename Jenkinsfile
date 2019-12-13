node{
    def scannerHome = tool 'SonarQubeScanner';
    def app
    def remote = [:]
    remote.name = 'master'
    remote.host = '40.71.251.3'
    remote.user = 'master'
    remote.password = 'Password123!'
    remote.allowAnyHosts = true

    stage('Get repo') {
  
        checkout scm
    }

    stage('SonarQube Analysis') {
        withSonarQubeEnv('SonarQubeScanner') { 
            sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectName=coursework_2 -Dsonar.projectKey=coursework_2:app -Dsonar.sources=."
        }
    }
    
    stage('Create docker image') {
        app = docker.build("dockasher/coursework2")
    }

    stage('Push image to Dockerhub') {
            docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}