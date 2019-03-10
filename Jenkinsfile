pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /home/marvin/.m2:/root/.m2'
        }
    }
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
        stage('prep') {
            steps {
                script {
                    env.GIT_HASH = sh(
                        script: "git show --oneline | head -1 | cut -d' ' -f1",
                        returnStdout: true
                    ).trim()
                }
            }
        }
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
	stage('Deploy Docker Image') {
	    steps {
		script {

			docker.withRegistry("${env.REGISTRY}", 'docker-hub-entree') {
        	            image.push("${GIT_HASH}")
                	    if ( "${env.BRANCH_NAME}" == "master" ) {
                		    image.push("LATEST")
		
                	    }
               		 }
		}
	    }
	}
    }
}
