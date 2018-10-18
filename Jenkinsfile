pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /root/.m2:/root/.m2 -p 44419:44419 --network=docker_cd-tools-network' 
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
                withEnv(["ARTIFACTORY_SERVER_URL=${env.ARTIFACTORY_SERVER}", "ARTIFACTORY_LOGIN=admin", "ARTIFACTORY_PASSWORD=password", "ARTIFACT=spincast-todobackend-inmemory-1.0.2.jar", "WORKSPACE=${env.JOB_NAME}"]) {
                    sh 'curl -u ${ARTIFACTORY_LOGIN}:${ARTIFACTORY_PASSWORD} -X PUT "${ARTIFACTORY_SERVER_URL}/artifactory/libs-snapshot-local/${WORKSPACE}/${ARTIFACT}" -T /var/jenkins_home/workspace/${WORKSPACE}/spincast-todobackend-inmemory/target/${ARTIFACT}'
                }
            }
        }
    }
}
