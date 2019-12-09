pipeline {
    agent any

    tools {
      go 'go-1.13.4'
    }

    environment {
      // We use localhost:5000 to tell docker to use a local registry
      appImageName = "localhost:5000/catalogue"
      dbImageName = "localhost:5000/catalogue-db"
    }

    stages {
        stage('docker-build-app') {
            steps {
                def newImages = docker.build(appImageName + ":$BUILD_NUMBER", "./docker/catalogue")
                newImages.push()
                newImages.push('latest')
            }
        }
        stage('docker-build-db') {
            steps {
                script {
                    def newImages = docker.build(dbImageName + ":$BUILD_NUMBER", "./docker/catalogue-db")
                    newImages.push()
                    newImages.push('latest')
                }
            }
        }
    }
}
