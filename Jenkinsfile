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

        stage('Destroy server') {
           steps {
                script {
                    echo "destroy"
                    dir('Terraform') {
                        withCredentials([[
                            $class: 'AmazonWebServicesCredentialsBinding', credentialsId: "my-aws-cred",accessKeyVariable: 'AWS_ACCESS_KEY_ID',secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                            // AWS Code
                            sh "terraform destroy -auto-approve -var 'region=us-east-1'"
                        }
                    }
                }
            }
        }
        
        // stage('deploy') {
        //     environment {
        //         DOCKER_CREDS = credentials('dockerhubcredentials')
        //     }

        //     steps {
        //         script {
        //            echo "waiting for EC2 server to initialize" 
        //            sleep(time: 90, unit: "SECONDS") 
        //            echo 'deploying docker image to EC2...'
        //            echo "${EC2_PUBLIC_IP}"
        //            /* def shellCmd = "bash ./server-cmds.sh ${IMAGE_NAME}"
        //            sh "echo 'IMAGE=${IMAGE_NAME}' > .env" */
        //            //bir scriptin basinnda bash ile baslarsak direk executable olarak calistirmis oluyoruz.
        //            def shellCmd = "bash ./server-cmds.sh ${IMAGE_NAME} ${DOCKER_CREDS_USR} ${DOCKER_CREDS_PSW}"
        //            def ec2Instance = "ec2-user@${EC2_PUBLIC_IP}"

        //             sshagent(['ssh-key']) {
        //               /*sh "scp -o StrictHostKeyChecking=no ./.env ec2-user@${EC2_PUBLIC_IP}:/home/ec2-user"  
        //                 sh "scp -o StrictHostKeyChecking=no docker-compose.yaml ec2-user@${EC2_PUBLIC_IP}:/home/ec2-user"
        //                 sh 'ssh -o StrictHostKeyChecking=no ec2-user@${EC2_PUBLIC_IP} ${shellCmd}' 
        //                 sh "ssh -o StrictHostKeyChecking=no ec2-user@${EC2_PUBLIC_IP} echo $PASS | docker login -u $USER --password-stdin && docker-compose -f docker-compose.yaml up --detach"
        //                  
        //                 sh "scp -o StrictHostKeyChecking=no server-cmds.sh ${ec2Instance}:/home/ec2-user" // sh dosyasini ec2 ya at
        //                 sh "scp -o StrictHostKeyChecking=no docker-compose.yaml ${ec2Instance}:/home/ec2-user" //sonrada docker-compose.yml dosyasini ec2 ya at
        //                 sh "ssh -o StrictHostKeyChecking=no ${ec2Instance} ${shellCmd}" //ssh ile ec2 da shellcmd yi calistir
        //             }
                   
        //         }
        //     }
        // }
    }
}
