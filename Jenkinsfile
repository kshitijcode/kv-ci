properties([parameters([string(defaultValue: 'master', description: 'Please specify the branch.', name: 'branch', trim: false)])])

pipeline {

    agent { dockerfile true }

    stages {
        stage('Clone repository') {
            steps {
                 git url: 'https://github.com/kshitijcode/kv', branch: "${params.branch}"
            }
    }


    }



}