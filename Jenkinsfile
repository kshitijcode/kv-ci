properties([parameters([string(defaultValue: 'master', description: 'Please specify the branch.', name: 'branch', trim: false),string(defaultValue: '1.0', description: '', name: 'RC', trim: false)])])
pipeline {

    agent any


    stages {
        stage('Clone repository') {
            steps {
                 git url: 'https://github.com/kshitijcode/kv', branch: "${params.branch}"

           }
    }

       stage('Build Image'){
            steps {

                sh "docker build -t kv/app:${params.rc} ."

            }
        }

        stage('Scan Image'){
            steps {

                sh 'echo Here we can scan and analyze our images using any scanning tools.'

            }
        }
        
            

        stage('Deploy to DEV'){
            steps {

                sh 'echo Here we can deploy the build to any of the cloud platforms. For Now I am simply running docker run'
                sh "docker run -d -t --entrypoint=/bin/sh kv/app:${params.rc}"

            }
        }

        stage('DEV-Test'){
                    
                    steps {
                          sh 'echo Running Dev Tests. These tests can be different Jobs as well.'
                          sh 'pip install -r requirements.txt'
                          sh 'python tests/unit_test.py'
                          }
                    post {
                         always {
                            junit 'test-reports/*.xml'
                                }
                        }   


        }

        stage('Push to Repository'){

                steps {
                    sh 'echo Push to artifactory..'
                }
        }
        
        stage('Prod-Deploy-Approval'){
            
            steps {
                input 'Do you wish to proceed with the Prod Deploy?'
            }
        }


        stage('Prod-Deploy'){
            
            steps {
                sh 'echo Prod Deploy'
            }
        }

        stage('Prod-Test'){
            
            steps {
                sh 'echo Prod Test'
            }
        }

         stage('Flip'){
            
            steps {
                input 'Do you wish to flip?'
            }
        }

        stage('Merge Feature to Master') {
            steps {

                sh "echo Merging the feature branch to the master.."
                sh 'git checkout master'
                sh "git merge --no-ff ${params.branch}"
            }

        }

        stage('Cleanup'){

            steps {
                sh 'echo Cleaning up resources..'
                deleteDir()
            }
        }

    }

}