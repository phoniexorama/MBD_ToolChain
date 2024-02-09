pipeline {
    agent {
        label 'WinLocalagent'
    }
    
    environment {
        LOGS_PATH = "./Code"
        ARTIFACTS_DOWNLOAD_PATH = "C:/Users/${env.GIT_USER_LOGIN}/Downloads"
    }

    stages {
        stage('Child pipelines') {
            parallel {
                stage('Driver Sw Request') {
                    steps {
                        script {
                            buildJobIfFilesChanged('driverSwRequest', ['Design/DriverSwRequest/**/*', 'driverSwRequest.groovy', 'Jenkinsfile', 'tools/**/*'])
                        }
                    }
                }
                stage('Cruise Control Mode') {
                    steps {
                        script {
                            buildJobIfFilesChanged('cruiseControlMode', ['Design/CruiseControlMode/**/*', 'cruiseControlMode.groovy', 'Jenkinsfile', 'tools/**/*'])
                        }
                    }
                }
                stage('Target Speed Throttle') {
                    steps {
                        script {
                            buildJobIfFilesChanged('targetSpeedThrottle', ['Design/TargetSpeedThrottle/**/*', 'targetSpeedThrottle.groovy', 'Jenkinsfile', 'tools/**/*'])
                        }
                    }
                }
                stage('CRS Controller') {
                    steps {
                        script {
                            buildJobIfFilesChanged('crs_controller', ['Design/crs_controller/**/*', 'crs_controller.groovy', 'Jenkinsfile', 'tools/**/*'])
                        }
                    }
                }
            }
        }
    }
}

def buildJobIfFilesChanged(jobName, filePatterns) {
    def changed = filePatterns.any { isFilesChanged(it) }
    if (changed) {
        build job: jobName, parameters: [], wait: false
    }
}

def isFilesChanged(String path) {
    def changedFiles = checkout([$class: 'GitSCM']).poll().getRemoteChangeset()
    return changedFiles.any { change ->
        change.getPath().startsWith(path)
    }
}
