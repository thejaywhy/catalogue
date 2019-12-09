pipeline {
    agent any

    tools {
      go 'go-1.13.4'
    }

    stages {
        stage('Test') {
            steps {
                echo 'Testing..'
                sh '''
                  echo "Validating..."
                  which go
                  go version
                  export GOPATH=$WORKSPACE/opt/go
                  export PATH="$WORKSPACE/opt/go/bin:$PATH"

                  echo "Gopath:" $GOPATH
                  echo "Path:" $PATH

                  echo "Install gvt..."
                  go get -u github.com/FiloSottile/gvt

                  echo "clone repo..."
                  cd /tmp/
                  rm -r *
                  git clone https://github.com/udbc/catalogue.git/
                  cd catalogue/
                  cp -r images /tmp/images
                  gvt restore
                '''
            }
        }
        stage('Package') {
            steps {
                echo 'Packaging....'
                sh '''
                  echo "Validating..."
                  which go
                  go version
                  export GOPATH=$WORKSPACE/opt/go
                  export PATH="$WORKSPACE/opt/go/bin:$PATH"
                   
                  echo "Gopath:" $GOPATH
                  echo "Path:" $PATH
                   
                  echo "Install gvt..."
                  go get -u github.com/FiloSottile/gvt
                   
                  echo "I: Setup the repo..."
                  mkdir -p $WORKSPACE/opt/go/src/github.com/microservices-demo
                  cd $WORKSPACE/opt/go/src/github.com/microservices-demo
                  git clone https://github.com/udbc/catalogue.git
                   
                  echo "I: Compile the code ..."
                  cd catalogue/
                  cp -r images $WORKSPACE/images
                  gvt restore
                   
                  cd cmd/cataloguesvc/
                  CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o $WORKSPACE/app/catalogue github.com/microservices-demo/catalogue/cmd/cataloguesvc
                  cd $WORKSPACE/app/
                  zip catalogue-$GIT_TAG_NAME.zip catalogue
                '''
                archiveArtifacts artifacts: 'catalogue-*.zip', fingerprint: true 
            }
        }
    }
}
