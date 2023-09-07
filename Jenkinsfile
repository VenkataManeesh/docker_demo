// pipeline {
// 	agent none
//   stages {
//   	stage('Maven Install') {
// 		agent any
//       steps {
//       	sh 'mvn clean install'
//       }
//     }
//     stage('Docker Build') {
//     	agent any
//       steps {
//       	sh 'docker build -t spring-k8s-example:latest .'
//       }
//     }
//   }
// }

pipeline {
    agent any 
    environment {
        Main = 'Main' // Specify your desired branch name here with the correct case
        jarVersion = '1.0' // Specify your desired jar version here
        tagName = 'latest' // Specify your desired Docker image tag here
    }
    stages {

        stage('SCM Preparation') {
            steps {
                echo "BranchName: ${Main}"
                echo "Code Update Started"
                checkout([$class: 'GitSCM',
                          branches: [[name: "${Main}"]],
                          userRemoteConfigs: [[url: 'https://github.com/VenkataManeesh/DevOps_Project.git']]])
                echo "Code Update End"
            }
        }
        
        stage('Clean') {
            steps {
                echo "Clean Started"
                bat(script: 'mvn clean -f DevOps_Project\\pom.xml', returnStatus: true)
                echo "Clean End"
            }
        }

        stage('Compile') {
            steps {
                echo "Code Compilation Started"
                bat(script: 'mvn compile -f DevOps_Project\\pom.xml', returnStatus: true)
                echo "Code Compilation End"
            }
        }
        
        stage('Build Image') {
            steps {
                echo "Build Image Started"
                bat(script: 'mvn package -f DevOps_Project\\pom.xml -Dmaven.test.skip=true', returnStatus: true)
                bat(script: 'docker build --build-arg VER=${jarVersion} -f DevOps_Project\\dockerfile -t Docker_Demo:${tagName} .', returnStatus: true)
                echo "Build Image End"
            }
        }
    }
}
