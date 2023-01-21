pipeline {
    agent any
    
    tools { nodejs "node" }

    stages {
        stage('set remote'){
            steps{
                sshagent(["github-key-a-id"]){
                    script {
                        sh 'git remote set-url origin git@github.com:aadalid5/aa2-package.git'
                    }
                }
            }
        }

        stage ('install') {
            steps {
                sh 'node -v'
            }
        }

        stage('publish') {
            steps {
                withCredentials([gitUsernamePassword(credentialsId: 'git-hbrjenkins')]) {
                    sh "git fetch"
                    sh "git checkout main"
                    sh "git pull"
                    sh "git reset --hard HEAD"
                    
                }

                script {
                        sh "cat .npmrc"
                        sh "npm whoami"

                        //sh "npm version minor"
                        sh "npm publish"
                }

                withCredentials([gitUsernamePassword(credentialsId: 'git-hbrjenkins')]) { 
                    script {
                        sh "git push --no-verify"
                    }
                }
            }

        }

        stage('post'){
            steps {
                sh "echo 'postmessage'"
            }
        }
    }
}