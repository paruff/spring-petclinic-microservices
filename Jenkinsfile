def label = "mypod-${UUID.randomUUID().toString()}"
podTemplate(label: label, containers: [
    containerTemplate(name: 'maven', image: 'maven:3.6.0-jdk-11-slim', ttyEnabled: true, command: 'cat'),
    containerTemplate(name: 'gradle', image: 'gradle:4.5.1-jdk9', command: 'cat', ttyEnabled: true),
    containerTemplate(name: 'golang', image: 'golang:1.8.0', ttyEnabled: true, command: 'cat')
  ]) {

    node(label) {
        stage('Get a Maven project') {
            checkout scm
            container('maven') {
                stage('Build a Maven project') {
                    sh 'mvn -B clean install'
                }
            
                stage 'Maven Static Analysis'
                    withSonarQubeEnv {
                        sh "mvn clean sonar:sonar"
                    }
            }
        }

    }
}
