pipeline {
    agent any 
    environment {
        main = 'main' // Specify your desired branch name here with the correct case
        jarVersion = '1.0' // Specify your desired jar version here
        tagName = 'latest' // Specify your desired Docker image tag here
        dockerTagName = tagName.toLowerCase() // Convert tagName to lowercase for Docker tag
    }
    stages {
        stage('SCM Preparation') {
            steps {
                echo "BranchName: ${main}"
                echo "Code Update Started"
                checkout([$class: 'GitSCM',
                          branches: [[name: "${main}"]],
                          userRemoteConfigs: [[url: 'https://github.com/VenkataManeesh/docker_demo.git']]])
                echo "Code Update End"
            }
        }
        
        stage('Clean') {
            steps {
                echo "Clean Started"
                bat(script: 'mvn clean', returnStatus: true)
                echo "Clean End"
            }
        }

        stage('Compile') {
            steps {
                echo "Code Compilation Started"
                bat(script: 'mvn compile', returnStatus: true)
                echo "Code Compilation End"
            }
        }
        
        stage('Build Image') {
            steps {
                echo "Build Image Started"
                bat(script: "mvn package -Dmaven.test.skip=true", returnStatus: true)
                bat(script: "docker build --build-arg VER=${jarVersion} -f dockerfile -t docker_demo:${dockerTagName} .", returnStatus: true)
                echo "Build Image End"
            }
        }
    }
}
