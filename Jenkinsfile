#!/usr/bin/env groovy

node {
    properties([
    disableConcurrentBuilds()
])

    stage('Clone sources') {
        checkout([$class: 'GitSCM', 
                          branches: [[name: '*/master']], 
                          doGenerateSubmoduleConfigurations: false, 
                          extensions: [], 
                          gitTool: 'Default', 
                          submoduleCfg: [], 
                          userRemoteConfigs: [[credentialsId: jenkinscreds,url: 'git@github.com:aslobodskoy/toolbox.git']]
                        ])
    }

        stage('Push Docker image to Docker registry') {
                    docker.withRegistry("https://${awsid}.dkr.ecr.us-east-1.amazonaws.com",) {
                    sh("eval \$(aws ecr get-login --no-include-email --region us-east-1| sed 's|https://||')")
                    def customImage = docker.build("toolbox:master_${env.BUILD_ID}")

                    /* Push the container to the custom Registry */
                    customImage.push()
                    script {
                wrap([$class: 'BuildUser']) {
                GET_BUILD_USER = sh ( script: 'echo "${BUILD_USER}"', returnStdout: true).trim()
                JOB_USER_ID = "${BUILD_USER_ID}"
            }

        currentBuild.description = "toolbox:master_${env.BUILD_ID}"
      }

                    }
        }
    }
    
