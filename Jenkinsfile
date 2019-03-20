def label = "mypod-${UUID.randomUUID().toString()}"
podTemplate(label: label, containers: [
    containerTemplate(name: 'maven', image: 'maven:3.6.0-jdk-8-slim', ttyEnabled: true, command: 'cat'),
    containerTemplate(name: 'gradle', image: 'gradle:5.2.1-jdk8', command: 'cat', ttyEnabled: true),
    containerTemplate(name: 'docker', image: 'docker', command: 'cat', ttyEnabled: true)
  ],
volumes: [
  hostPathVolume(mountPath: '/home/maven/.m2', hostPath: '/tmp/jenkins/.m2'),
  hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock')
]) {

    node(label) {
        stage('Get a Maven project') {
            checkout scm
            container('maven') {
                stage('Build a Maven project') {
                    sh 'mvn -B clean install'
                }
            stage('Scan components Maven project') {
                    sh 'mvn -B dependency-check:check'
                }
                
                stage 'Maven Static Analysis'
                    withSonarQubeEnv {
                        sh "mvn  sonar:sonar"
                    }
            }
        }

    }
}
