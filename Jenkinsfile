pipeline {
    agent any
    environment {
        main = 'main'
        jarVersion = '1.0'
        tagName = 'latest'
        dockerTagName = tagName.toLowerCase()
        // Define the Maven path
        mavenPath = 'C:\\apache-maven-3.9.4\\bin\\mvn'
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
                // Use the specified Maven path and adjust the command for the correct POM location
                bat "${mavenPath} clean"
                echo "Clean End"
            }
        }

        stage('Compile') {
            steps {
                echo "Code Compilation Started"
                // Use the specified Maven path and adjust the command for the correct POM location
                bat "${mavenPath} compile"
                echo "Code Compilation End"
            }
        }

        stage('Build Image') {
            steps {
                echo "Build Image Started"
                // Use the specified Maven path and adjust the command for the correct POM location
                bat "${mavenPath} package -Dmaven.test.skip=true"
                bat "docker build --build-arg VER=${jarVersion} -f Dockerfile -t docker_demo:${dockerTagName} ."
                echo "Build Image End"
            }
        }
    }
}

// bat "docker build --build-arg VER=${jarVersion} -f dockerfile -t docker_demo:${dockerTagName} ."
