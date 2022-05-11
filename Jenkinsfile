pipeline {

    agent {
        node {
            label 'master'
        }
    }
    environment { 
        PATH = "/root/apictl:$PATH"
    }
    options {
        buildDiscarder logRotator( 
                    daysToKeepStr: '16', 
                    numToKeepStr: '10'
            )
    }

    stages {

        stage('Setup Environment for APICTL') {
            steps {
                sh """#!/bin/bash
                ENVCOUNT=\$(apictl get envs --format {{.}} | wc -l)
                if [ "\$ENVCOUNT" == "0" ]; then
                    apictl add-env -e dev --apim https://localhost:9443
                fi
                """
            }
        }

        stage('Deploy APIs To "DEV" Environment') {
            steps {
                sh """
                apictl login dev -u ${BUILD_USER_ID} -p ${password} -k
		apictl set --vcs-source-repo-path ${WORKSPACE}
		echo ${BUILD_USER}
                apictl vcs deploy -e dev -k
                """
            }
        }
    }   
}
