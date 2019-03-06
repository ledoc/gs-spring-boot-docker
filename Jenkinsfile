pipeline {
    agent any
    
    triggers {
        pollSCM('* * * * *')
    }
    options {
        disableConcurrentBuilds()
        timestamps()
    }
    environment {
        IMAGE = "h3rv3/gs-spring-boot-docker"
        REGISTRY = "https://registry.hub.docker.com"
    }
    stages {
        stage('Build') {
	    println "Build start"
            sh 'mvn -B -V -U -e clean package'
        }
    }
}
