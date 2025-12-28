pipeline {
  agent {
    kubernetes {
      inheritFrom 'go'
      containerTemplate {
        name 'go'
        image 'golang:1.22'
      }

    }

  }
  stages {
    stage('Clone repository') {
      steps {
        checkout([$class: 'GitSCM', branches: [[name: 'master']],
                    extensions: [[$class: 'CloneOption', depth: 1, shallow: true]], userRemoteConfigs: [[url: 'https://github.com/chsendev/kratos-helloworld.git']]
                ])
      }
    }

    stage('Download dependencies') {
      steps {
        container('go') {
          sh 'go mod download'
        }

      }
    }

    stage('Run test') {
      steps {
        container('go') {
          sh 'go test ./...'
        }

      }
    }

    stage('Run build') {
      agent none
      steps {
        container('go') {
          sh 'cd cmd/helloworld && go build -buildvcs=false -o service && mv service ../../'
        }

      }
    }

    stage('Archive artifacts') {
      steps {
        archiveArtifacts(artifacts: 'service', followSymlinks: false)
      }
    }

  }
}