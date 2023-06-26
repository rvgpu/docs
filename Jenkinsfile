pipeline {
    agent httpserver

    stages {
        stage('Build') {
            steps {
                echo 'Build docs to html files'
                sh "pwd"
                sh "make html"
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploy html to server"
                sh "rsync -rat _build/html/* ci@ubuntu-server-01:/home/rvgpu/pages/"
            }
        }
    }
}

