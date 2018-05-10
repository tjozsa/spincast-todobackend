pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /root/.m2:/root/.m2 --network=docker_cd-tools-network' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn clean package' 
            }
        }
        stage('Test') {
            steps {
               echo 'Run tests here...'
            }
        }
        stage('Publish Artifact') {
            steps {
                withEnv(["ARTIFACTORY_SERVER_URL=${env.ARTIFACTORY_SERVER}", "ARTIFACTORY_LOGIN=admin", "ARTIFACTORY_PASSWORD=password", "ARTIFACT=spincast-all-1.0.0-SNAPSHOT.jar"]) {
                    sh 'curl -u ${ARTIFACTORY_LOGIN}:${ARTIFACTORY_PASSWORD} -X PUT "${ARTIFACTORY_SERVER_URL}/todo-backend/java/${ARTIFACT}" -T ${ARTIFACT}'
                }
            }
        }
    }
}