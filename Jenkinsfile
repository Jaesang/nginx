pipeline {
    agent {
        node  {
            label 'helmtools'
            customWorkspace "workspace/${env.JOB_NAME}/${env.BUILD_NUMBER}"
        }
    } 
    stages {
        stage('Clone repository') {
            steps {
                container('docker'){
                    git(
                         url: 'https://github.com/openinfradev/java-app-example.git',
                         branch: "master"
                       )
                }
            }
        }
        stage('Build image') {
            steps {
                container('docker'){
                    script {
                        sh "docker build --network=host -t taco-registry:5000/admin/java-app-example:jaesang ."
                    }
                }
            }
        }
        stage('Push image') {
            steps {
                container('docker'){
                    scripts {
                         sh """
                             docker login -u jaesang -p taco1130@ 192.168.51.18:5000
                             docker push taco-registry:5000/admin/java-app-example:jaesang
                         """
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                container('helm-kubectl'){
                    script {
                        sh "tail -f"
                    }
                }
            }
        }
    }
}
