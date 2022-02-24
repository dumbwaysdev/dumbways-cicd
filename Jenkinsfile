def secret = 'GitHub'
def server = 'sgnd@192.168.3.174'
def directory = 'dumbways-cicd'
def branch = 'main'

pipeline{
    agent any
    stages{
        stage ('docker delete & git pull'){
            steps{
                sshagent([secret]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${directory}
                    docker-compose down
                    docker system prune -f
                    git pull origin ${branch}
                    exit
                    EOF"""
                }
            }
        }
        stage ('docker build'){
            steps{
                sshagent([secret]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${directory}
                    docker-compose build
                    exit
                    EOF"""
                }
            }
        }
        stage ('docker up'){
            steps{
                sshagent([secret]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${directory}
                    docker-compose up -d
                    exit
                    EOF"""
                }
            }
        }
    }
}
