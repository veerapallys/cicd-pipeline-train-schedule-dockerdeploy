pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker Image'){
            when {
                branch 'master'
            }
            steps {
                script{
                    app = docker.build("ramineniv335/train-schedule")
                    app.inside{
                        sh 'echo $(curl localhost:8080)'
                    }
                }
            }
        }
        stage('Push docker Image'){
            when {
                branch 'master'
            }
            steps {
                script{
                     docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")       
                }
            }
        }
    }
    
}
