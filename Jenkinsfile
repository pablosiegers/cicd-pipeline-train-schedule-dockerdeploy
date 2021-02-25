node {
    stage('Build'){
        echo 'Running build automation'
        sh './gradlew build --no-daemon'
        archiveArtifacts artifacts: 'dist/trainSchedule.zip'
    }
    stage('Build Docker Image'){
        if (env.BRANCH_NAME == 'master'){
            app = docker.build("pablosiegers/train-schedule")
            app.inside {
            sh 'echo $(curl localhost:8080)'   
            }
        }
    }
    stage('Push Docker Image') {
        if (env.BRANCH_NAME == 'master'){
            docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                app.push("${env.BUILD_NUMBER}")
                app.push("latest")
            }
        }
    }
}
