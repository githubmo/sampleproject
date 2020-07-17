pipeline {
    agent any

    parameters {
        text(name: 'GREETING', defaultValue: 'Hello Mo', description: 'Greeting at the beginning of the build')

        booleanParam(name: 'BUILD', defaultValue: false, description: 'Choose to expose the archive')
    }

    stages {
        stage ('GREETING') {
            steps {
                echo "${params.GREETING}"
            }
        }
        stage ('Clone Repo') {
            steps {
                git url: 'https://github.com/githubmo/sampleproject.git'
            }
        }
        stage('Check Python version') {
            steps {
                sh 'python3 --version'
            }
        }
        stage('Install dependecies') {
            steps {
                sh 'sudo python3 setup.py install'
            }
        }
        stage('Build & Test') {
            steps {
                 sh 'pytest --html=coverage/report.html'
            }
            post {
                always {
                    publishHTML (target : [ allowMissing: false,
                                         alwaysLinkToLastBuild: true,
                                         keepAll: true,
                                         reportDir: 'coverage',
                                         reportFiles: 'report.html',
                                         reportName: 'Pytest Reports',
                                         reportTitles: 'Unit test reports'])
                }
            }
        }
        stage('Deploy') {
            when {
                expression { params.BUILD }
            }
            steps {
                archive (includes: 'dist/*')
            }

        }
    }
}