#!/usr/bin/env groovy

pipeline {
    agent any
    tools {
        maven 'my-maven'
        // burada kullanilacak olan plug in leri belirtiyoruz. snippet generator dan olusturoyurz.
        terraform 'my-tf'
    }
    environment {
        IMAGE_NAME = "alish33/java-app-repo:javaApp-${BUILD_NUMBER}"
    }
    stages {
    
        /* stage('test') {
            steps {
                script {
                    echo "test the application"
                    sh 'mvn test'
                }
            }
        } */
 /*        stage('build jar') {
            steps {
                script {
                    echo "building the application jar"
                    sh 'mvn package'
                }
            }
        }
 */

        stage('Provision server') {
           steps {
                script {
                    echo "Provisioning server::: "
                    dir('Terraform') {
                        withCredentials([[
                            $class: 'AmazonWebServicesCredentialsBinding', credentialsId: "my-aws-cred",accessKeyVariable: 'AWS_ACCESS_KEY_ID',secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                            // AWS Code
                            sh "terraform destroy"
                        }
                    }
                }
            }
        }
    }
}
