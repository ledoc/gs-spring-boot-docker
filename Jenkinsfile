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
		steps {
		    println "build lance"
	            sh 'mvn -B -V -U -e clean package'
		}
        }
        stage('build Docker Image') {
            steps {
                script {
                    image = docker.build("${IMAGE}")
                    println "Newly generated image, " + image.id
                }
            }
        }
    }
}
