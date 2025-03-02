pipeline {
    agent any
    environment {
        MAVEN_HOME = '/usr/share/maven'
        PATH = "${MAVEN_HOME}/bin:${env.PATH}"
    }
    stages {
        stage("checkout") {
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/sree8594/java-mavenapplication.git']])
            }
        }
        stage("validate the code") {
            steps {
                sh 'mvn validate'
            }
        }
        stage("compile the code") {
            steps {
                sh 'mvn compile'
            }
        }
        stage("test the code") {
            steps {
                sh 'mvn test'
            }
        }
        stage("package the code") {
            steps {
                sh 'mvn package'
            }
        }
        stage("archieveArtifact") {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
            }
        }
        stage("push to DockerHub") {
            steps {
                sh '''
                        docker build /var/lib/jenkins/workspace/pipeline -t sreekanthreddyv/mavenapp:v1
                        echo "Sreekanth@123" | docker login -u sreekanthreddyv --password-stdin
                        docker push sreekanthreddyv/mavenapp:v1
                '''
            }
        }
    }
}
