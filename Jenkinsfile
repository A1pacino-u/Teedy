pipeline {
    agent any

    tools {
        jdk 'MyJDK17'
        maven 'MyMaven'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/master']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/A1pacino-u/Teedy.git',
                        // 强制跳过 SSL 验证
                        extensions: [[$class: 'UserConfigProvider', configs: [[key: 'http.sslVerify', value: 'false']]]]
                    ]]
                ])
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
            post {
                success {
                    archiveArtifacts artifacts: '**/*.jar', fingerprint: true
                }
            }
        }
        stage('PMD Check') {
            steps {
                sh 'mvn pmd:pmd'
            }
            post {
                always {
                    pmd canRunOnFailed: true, pattern: 'target/pmd.xml'
                }
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('JavaDoc') {
            steps {
                sh 'mvn javadoc:jar'
            }
            post {
                success {
                    archiveArtifacts artifacts: 'target/*-javadoc.jar', fingerprint: true
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
