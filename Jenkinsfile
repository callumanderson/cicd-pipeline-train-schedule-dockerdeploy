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
        stage('Build Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    app = docker.build("callumandersondocker/train-schedule")
                    app.inside {
                        sh 'echo $(curl localhost:8080)'
                    }
                }
            }
        }
        stage('Push Docker Image') {
          when {
            branch 'master'
          }
          steps {
            withDockerRegistry([ credentialsId: "docker_hub_login", url: "" ]) {
              sh 'docker push callumandersondocker/train-schedule'
            }
          }
        }
    }
}
