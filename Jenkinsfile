pipeline {
    agent {
        dockerfile {
            filename 'Dockerfile.test'
            args '-v /var/run/docker.sock:/var/run/docker.sock \
                  --group-add docker'
        }
    }

    parameters {
        choice(
            name: 'APP_NAME',
            choices: ['provider-1', 'provider-2'],
            description: 'Choose application'
        )
    }

    stages {
        stage('Checkout sources from GitHub') {
            steps {
                git branch: 'master',
                    url: 'https://STI26/test_jenkins.git'

                script {
                    echo "application ${params.APP_NAME}"
                    sh "cat /etc/*release"
                }
            }
        }

        stage('Build app') {
            steps {
                script {
                    sh "pwd"
                    sh "ls"
                    sh "cat /etc/*release"
                }
            }
        }

        stage('Collect artifacts') {
            steps {
                script {
                    archiveArtifacts 'app.sh'
                }
            }
        }
    }
}
