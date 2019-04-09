// start - Auxiliary functions
def node_details() {
    sh """
    printf "Node          : ${env.NODE_NAME}\nHostname      : \$(hostname)\n\$(docker --version)\n"
    """
}

// end - Auxiliary fuctions

pipeline {
    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        disableConcurrentBuilds()
    }
    environment{
        test_file='test.txt'
    }

    stages {
        stage('init x86_64') {
            steps {
                script {
                    node_details()
                }
            }
        }
        stage('Artifacts Test'){
            steps{
                archiveArtifacts 'test.txt'
                archiveArtifacts 'test.html'
            }
        }
    }
    post {
        always {
            deleteDir()
        }
        unstable {
              echo "UNSTABLE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})"
        }
        failure {
              echo "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})"
        }
    }
}
