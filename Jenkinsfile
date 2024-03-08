def gv

pipeline {
    agent any
    environment {
        awsEcrCreds = "493467131089.dkr.ecr.us-east-2.amazonaws.com"
        awsEcrRegistry = "493467131089.dkr.ecr.us-east-2.amazonaws.com/java_image_store"
        imageRegUrl = "https://493467131089.dkr.ecr.us-east-2.amazonaws.com"
        awsRegion = "us-east-2"
    }
    tools {
        maven "Maven"
        jdk "JDK-17"
    }
    stages {
        stage ("init") {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }
        stage ("Fetch Code") {
            steps {
                script {
                    gv.fetchCode()
                }
            }
        }
        stage ('Build') {
            steps {
                script {
                    gv.buildCode()
                }
            }
            post {
                success {
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
        stage ('Build Image') {
            steps {
                script {
                    gv.buildImage()
                }
            }
        }
        stage ('Push Image to ECR') {
            steps {
                script {
                    gv.pushImage()
                }
            }
        }
        stage ('provision eks cluster') {
            steps {
                script {
                    gv.provisionEksCluster()
                }
            }
        }
        stage ('connect to eks cluster') {
            steps {
                script {
                    gv.connectEks()
                }
            }
        }
    }
}
