import org.jenkinsci.plugins.pipeline.modeldefinition.Utils

OPTIONS = [
    'provider-1',
    'provider-2'
]

properties([
    parameters([
        [
            $class: 'ChoiceParameter',
            choiceType: 'PT_CHECKBOX',
            description: 'Choose application',
            filterable: false,
            name: 'APP_NAME',
            script: [
                $class: 'GroovyScript',
                fallbackScript: [
                    classpath: [],
                    sandbox: true,
                    script: 'return []'
                ],
                script: [
                    classpath: [],
                    sandbox: true,
                    script: """
                        return ${OPTIONS.inspect()}
                    """
                ]
            ]
        ]
    ])
])

pipeline {
    agent {
        dockerfile {
            filename 'Dockerfile.test'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    stages {
        stage('Checkout sources from GitHub') {
            when {
                expression { params.APP_NAME != '' }
            }

            steps {
                git branch: 'main',
                    url: 'https://github.com/STI26/test_jenkins_2.git'

                script {
                    echo "Selected applications: ${params.APP_NAME}"
                    echo 'TEST___VAR(1): $TEST___VAR'
                    sh 'env | grep TEST___VAR'
                    
                    selected_apps = params.APP_NAME.split(',')
                    build_app(OPTIONS, selected_apps)
                }
            }
            
            post {
                always {
                    script {
                        currentBuild.displayName = "${BUILD_NUMBER}-${APP_NAME}"
                    }
                }
            }
        }

        stage('Collect artifacts') {
            when {
                expression { params.APP_NAME != '' }
            }

            steps {
                script {
                    echo 'TEST___VAR(3): $TEST___VAR'
                    sh 'env | grep TEST___VAR'
                    archiveArtifacts 'app.sh'
                }
            }
        }
    }
}

def build_app(list_of_apps, list_of_selected_apps) {
    list_of_apps.each { app ->
        stage_name = "Build ${app}"
        stage(stage_name) {
            if (list_of_selected_apps.contains(app)) {
                script {
                    echo "build - ${app}"
                    echo 'TEST___VAR(2): $TEST___VAR'
                    sh 'env | grep TEST___VAR'

                    rev = sh (
                        script: 'date +%H%M%S',
                        returnStdout: true
                    ).trim()
                    f_name = "${app}-${rev}.json"

                    echo "${app}-${rev}.json"
                    
                    def buildInfo = [
                            buildVersion: "${app}-${rev}"
                    ]
                    writeJSON file: f_name, json: buildInfo
                    archiveArtifacts f_name
                }
            } else {
                Utils.markStageSkippedForConditional(stage_name)
            }
        }
    }
}
