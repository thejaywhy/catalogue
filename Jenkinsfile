pipeline {
    agent any

    environment {
      // We use localhost:5000 to tell docker to use a local registry
      appImageName = "localhost:5000/catalogue"
      dbImageName = "localhost:5000/catalogue-db"
    }

    stages {
        stage('docker-build-app') {
            steps {
                script {
                    def dockerfile = "./docker/catalogue/Dockerfile"
                    def newImages = docker.build(appImageName + ":$BUILD_NUMBER", "-f ${dockerfile} .")
                    newImages.push()
                    newImages.push('latest')
                }
            }
        }
        stage('docker-build-db') {
            steps {
                script {
                    def dockerfile = "./docker/catalogue-db/Dockerfile"
                    def newImages = docker.build(dbImageName + ":$BUILD_NUMBER", "-f ${dockerfile} .")
                    newImages.push()
                    newImages.push('latest')
                }
            }
        }
    }
}
