

enum OS {
    macOS
}

def execute(label, command) {
    sh label: label, script: "${command}"
}

def configure() {
    echo "Running on ${NODE_NAME}"
    execute('Configuring...', "cmake -B ${BUILD_DIR} -DCMAKE_BUILD_TYPE=Release")
}

def build() {

    if (params.CLEAN_BUILD) {
        echo 'Cleaning...'
        dir("${BUILD_DIR}") {
            deleteDir()
        }
    }

    dir("${BUILD_DIR}") {
        execute('Configuring...', "cmake --build ${BUILD_DIR} --config Release")
    }
}

def uploadToMaster(agentWorkspace) {
    if (agentWorkspace != WORKSPACE)
        deleteDir()

    dir ("${agentWorkspace}") {
        echo "Uploading from ${agentWorkspace}/install/${BUILD_ID}/package/* ..."
        archiveArtifacts artifacts: "install/${BUILD_ID}/package/*", fingerprint: true
    }
}

pipeline {
    agent any
    environment {
        WIN_X64_WORKSPACE = ''
        MAC_WORKSPACE = ''
    }
    stages {
        stage('Setup, Build') {
            agent { label "macOS" }
            environment {
                SOURCE_DIR = "${WORKSPACE}"
                BUILD_DIR = "${WORKSPACE}/build"
                INSTALL_DIR = "${WORKSPACE}/install/${BUILD_ID}"
            }
            steps {
                script {
                    configure()
                    build()
                }
            }
        }
        stage('Pack, Notarize, Upload') {
            when { 
                expression { currentBuild.currentResult == 'SUCCESS' && params.BUILD_PACKAGES }
            }
            agent { label "macOS" }
            steps {
                script {
                    uploadToMaster(MAC_WORKSPACE)
                }
            }
        }
    }
    post {
        success {
            emailext(
                subject: '[JENKINS] Build Succeeded',
                body: "Pipeline: ${currentBuild.fullDisplayName}\nStatus: ${currentBuild.currentResult}\n\nMore Info: ${env.BUILD_URL}",
                recipientProviders: [[$class: 'RequesterRecipientProvider']]
            )
        }
        failure {
            script {
                emailext(
                    subject: '[JENKINS] Build Failed',
                    body: "Pipeline: ${currentBuild.fullDisplayName}\nStatus: ${currentBuild.currentResult}\n\nMore Info: ${env.BUILD_URL}",
                    recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']]
                )
            }
        }
        unstable {
            script {
                emailext(
                    subject: '[JENKINS] Build Failed',
                    body: "Pipeline: ${currentBuild.fullDisplayName}\nStatus: ${currentBuild.currentResult}\n\nMore Info: ${env.BUILD_URL}",
                    recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']]
                )
            }
        }
    }
}
