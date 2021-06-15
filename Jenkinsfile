pipeline {
    agent any

    stages {
        stage('build') {
            steps {
                sh "mvn clean package"
            }
        }
        stage('docker build and push') {
            steps {
                script {
                    tag = "dev"
                    imagename = """${JOB_BASE_NAME}:${tag}"""

                    // push docker image to prviate docker registry
                    docker.withRegistry("""${DOCKER_REGISTRY}""", "docker_registry"){
                        image = docker.build(imagename)
                        image.push()
                    }
                    
                    // delete docker image for disk size
                    sh """docker rmi ${imagename}"""
                }
            }
        }
    }
}
