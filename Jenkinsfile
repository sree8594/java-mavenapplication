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
       stage("removing the old docker images") {
            steps {
                sh '''
                        docker stop app1
                        docker rm app1
                        docker rmi sreekanthreddyv/mavenapp:v1
                '''
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
       stage("deploy into prod") {
            steps {
                sh '''
                        docker rmi sreekanthreddyv/mavenapp:v1
                        docker pull sreekanthreddyv/mavenapp:v1
                        docker run -d --name app1 -p 80:8080 sreekanthreddyv/mavenapp:v1
                '''
            }
        }
    }
}
