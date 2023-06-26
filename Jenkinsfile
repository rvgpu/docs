pipeline {
    agent {
        label 'sphinx'
    }

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
                echo "Deploy html to github repos"
                dir ("_build/html/") {
                    sh 'touch .nojekyll'
                    sh 'git init'
                    sh 'git add . '
                    sh 'git commit -m "push to github pages"'
                    sh 'git remote add origin git@github.com:rvgpu/pages.git'
                    sh 'git push origin main --force'
                }
            }
        }
    }
}

