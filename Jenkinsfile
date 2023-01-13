properties([
    parameters([
        [
            $class: 'ChoiceParameter',
            choiceType: 'PT_CHECKBOX',
            description: 'Choose application',
            name: 'APP_NAME',
            script: [
                $class: 'ScriptlerScript',
                parameters: [
                    'provider-1',
                    'provider-2'
                ]
            ]
        ]
    ])
])

pipeline {
    agent {
        dockerfile {
            filename 'Dockerfile.test'
            args '-v /var/run/docker.sock:/var/run/docker.sock \
                  --group-add docker'
        }
    }

    stages {
        stage('Checkout sources from GitHub') {
            when {
                expression { params.APP_NAME != '' }
            }

            steps {
                git branch: 'master',
                    url: 'https://STI26/test_jenkins.git'

                script {
                    echo "application ${params.APP_NAME}"
                    sh "cat /etc/*release"
                    sh "env"
                }
            }
        }

        stage('Build app') {
            when {
                expression { params.APP_NAME != '' }
            }

            steps {
                script {
                    sh "pwd"
                    sh "ls"
                    sh "cat /etc/*release"
                    sh "env"
                }
            }
        }

        stage('Collect artifacts') {
            when {
                expression { params.APP_NAME != '' }
            }

            steps {
                script {
                    archiveArtifacts 'app.sh'
                }
            }
        }
    }
}
