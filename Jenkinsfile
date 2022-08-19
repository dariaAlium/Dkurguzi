

enum OS {
    macOS, win_x64
}

def configure() {
    echo "Running on ${NODE_NAME}"
    execute('Configuring...', "cmake -B ${BUILD_DIR} -DCMAKE_BUILD_TYPE=Release")
}

def build(OS os) {

    if (params.CLEAN_BUILD) {
        echo 'Cleaning...'
        dir("${BUILD_DIR}") {
            deleteDir()
        }
    }

    dir("${BUILD_DIR}") {
        execute('Configuring...', "cmake --build ${BUILD_DIR} --config Release")
}

    cleanInstallDir()
}


pipeline {
    agent any
    environment {
        WIN_X64_WORKSPACE = ''
        MAC_WORKSPACE = ''
    }

   parameters {
        booleanParam(name: 'CLEAN_BUILD', defaultValue: defaultCleanBuild, description: 'Clean build directory before building')
   }

    stages {
        stage('Setup, Build') {
            parallel {
                stage('macOS') {
                    agent { label "${params.MACOS_BUILDSERVER} && build" }
                    options {
                        timeout(time: 180, unit: 'MINUTES')
                    }
                    environment {
                        SOURCE_DIR = "${WORKSPACE}"
                        BUILD_DIR = "${WORKSPACE}/build"
                        INSTALL_DIR = "${WORKSPACE}/install/${BUILD_ID}"
                        DEVELOPMENT_STAGE = "${DEVELOPMENT_STAGE}"
                        GTEST_OUTPUT = "xml:${WORKSPACE}/test-results/"
                    }
                    when {
                        beforeAgent true
                        expression { params.OS_MACOS }
                    }
                    steps {
                        script {
                            setParam("MACOS_BUILDSERVER", "${NODE_NAME}")
                            configure()
                            MAC_WORKSPACE = WORKSPACE
                            build(OS.macOS)
                        }
                    }
                    post {
                        always {
                            script {
                                uploadTestResults()
                            }
                        }
                    }
                }
                stage('Ubuntu') {
                    agent { label "${params.UBUNTU_BUILDSERVER} && build" }
                    options {
                        timeout(time: 180, unit: 'MINUTES')
                    }
                    environment {
                        SOURCE_DIR = "${WORKSPACE}"
                        BUILD_DIR = "${WORKSPACE}/build"
                        INSTALL_DIR = "${WORKSPACE}/install/${BUILD_ID}"
                        DEVELOPMENT_STAGE = "${DEVELOPMENT_STAGE}"
                        GTEST_OUTPUT = "xml:${WORKSPACE}/test-results/"
                    }
                    when {
                        beforeAgent true
                        expression { params.OS_UBUNTU }
                    }
                    steps {
                        script {
                            setParam("UBUNTU_BUILDSERVER", "${NODE_NAME}")
                            setup()
                            UBUNTU_WORKSPACE = WORKSPACE
                            build(OS.ubuntu)
                        }
                    }
                    post {
                        always {
                            script {
                                uploadTestResults()
                            }
                        }
                    }
                }
                stage('Android') {
                    agent { label "${params.ANDROID_BUILDSERVER} && build" }
                    options {
                        timeout(time: 180, unit: 'MINUTES')
                    }
                    environment {
                        ANDROID_KEYSTORE_PATH = credentials('ANDROID_KEYSTORE_PATH')
                        ANDROID_KEYSTORE_PASSOWORD = credentials('ANDROID_KEYSTORE_PASSOWORD')
                        ANDROID_KEY_PASSOWORD = credentials('ANDROID_KEY_PASSOWORD')
                        ANDROID_KEY_ALIAS = credentials('ANDROID_KEY_ALIAS')
                        SOURCE_DIR = "${WORKSPACE}"
                        BUILD_DIR = "${WORKSPACE}/build"
                        INSTALL_DIR = "${WORKSPACE}/install/${BUILD_ID}"
                        DEVELOPMENT_STAGE = "${DEVELOPMENT_STAGE}"
                        GTEST_OUTPUT = "xml:${WORKSPACE}/test-results/"
                    }
                    when {
                        beforeAgent true
                        expression { params.OS_ANDROID }
                    }
                    steps {
                        script {
                            setParam("ANDROID_BUILDSERVER", "${NODE_NAME}")
                            setup()
                            ANDROID_WORKSPACE = WORKSPACE
                            build(OS.android)
                        }
                    }
                // post {
                //     always {
                //         script {
                //             uploadTestResults()
                //         }
                //     }
                // }
                }
                stage('Windows x64') {
                    agent { label "${params.WINDOWS_X64_BUILDSERVER} && build" }
                    options {
                        timeout(time: 180, unit: 'MINUTES')
                    }
                    environment {
                        SOURCE_DIR = "${WORKSPACE}"
                        BUILD_DIR = "${WORKSPACE}/build"
                        INSTALL_DIR = "${WORKSPACE}/install/${BUILD_ID}"
                        GTEST_OUTPUT = "xml:${WORKSPACE}/test-results/"
                    }
                    when {
                        beforeAgent true
                        expression { params.OS_WINDOWS }
                    }
                    steps {
                        script {
                            setParam("WINDOWS_X64_BUILDSERVER","${NODE_NAME}")
                            setup()
                            WIN_X64_WORKSPACE = WORKSPACE
                            build(OS.win_x64)
                        }
                    }
                    post {
                        always {
                            script {
                                uploadTestResults()
                            }
                        }
                    }
                }
                stage('Windows x86') {
                    agent { label "${params.WINDOWS_X86_BUILDSERVER} && build" }
                    options {
                        timeout(time: 180, unit: 'MINUTES')
                    }
                    environment {
                        SOURCE_DIR = "${WORKSPACE}"
                        BUILD_DIR = "${WORKSPACE}/build"
                        INSTALL_DIR = "${WORKSPACE}/install/${BUILD_ID}"
                        GTEST_OUTPUT = "xml:${WORKSPACE}/test-results/"
                    }
                    when {
                        beforeAgent true
                        expression { params.OS_WINDOWS }
                    }
                    steps {
                        script {
                            setParam("WINDOWS_X86_BUILDSERVER","${NODE_NAME}")
                            setup()
                            WIN_X86_WORKSPACE = WORKSPACE
                            build(OS.win_x86)
                        }
                    }
                    post {
                        always {
                            script {
                                uploadTestResults()
                            }
                        }
                    }
                }
            }
        }

    }
}