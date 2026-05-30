pipeline {
    agent any

    tools {
        jdk 'MyJDK17'
        maven 'MyMaven'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                echo '开始编译项目...'
                bat 'mvn clean compile'
            }
        }
        stage('Test & PMD') {
            steps {
                echo '运行单元测试与PMD代码检查...'
                bat 'mvn test pmd:pmd'
            }
        }
        stage('Package & Javadoc') {
            steps {
                echo '打包项目并生成文档...'
                bat 'mvn package javadoc:javadoc'
            }
        }
    }

    post {
        success {
            echo '🎉 流水线执行成功！'
            archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            archiveArtifacts artifacts: '**/target/site/javadoc/**', fingerprint: true
        }
        failure {
            echo '❌ 流水线执行失败，请检查日志！'
        }
    }
}
