pipeline {
    agent any

    environment {
        SONARQUBE = 'SonarQubeServer'
        ARTIFACTORY = 'Artifactory'
        WAR_NAME = 'ILP_HCLStore.war'
        ARTIFACT_PATH = 'libs-release-local/com/hcl/ILP_HCLStore/1.0.0/ILP_HCLStore-1.0.0.war'
    }

    triggers {
        githubPush()
    }

    stages {
        stage('Checkout Source') {
            steps {
                git url: 'https://github.com/yulakhraj/ILPHCLStore.git', branch: 'main'
            }
        }

        stage('Code Quality - SonarQube') {
            when {
                expression { 
                    return env.SONARQUBE != null
                }
            }
            steps {
                script {
                    try {
                        withSonarQubeEnv("${SONARQUBE}") {
                            bat 'mvn clean verify sonar:sonar'
                        }
                    } catch (Exception e) {
                        echo "SonarQube analysis failed: ${e.message}"
                        currentBuild.result = 'UNSTABLE'
                    }
                }
            }
        }

        stage('SonarQube Quality Gate') {
            when {
                expression { 
                    return env.SONARQUBE != null && currentBuild.result != 'UNSTABLE'
                }
            }
            steps {
                script {
                    try {
                        timeout(time: 2, unit: 'MINUTES') {
                            waitForQualityGate abortPipeline: false
                        }
                    } catch (Exception e) {
                        echo "Quality Gate check failed: ${e.message}"
                        currentBuild.result = 'UNSTABLE'
                    }
                }
            }
        }

        stage('Build + Unit Test') {
            steps {
                bat 'mvn clean test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Upload WAR to Artifactory') {
            steps {
                rtUpload (
                    serverId: "${ARTIFACTORY}",
                    spec: '''{
                        "files": [{
                            "pattern": "target/${WAR_NAME}",
                            "target": "${ARTIFACT_PATH}"
                        }]
                    }'''
                )
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                bat '''
curl -u your_jfrog_user:your_jfrog_api_key -O https://your-jfrog-server/artifactory/${ARTIFACT_PATH}
curl -u tomcat_user:tomcat_password -T ${WAR_NAME} "http://localhost:8080/manager/text/deploy?path=/HCLStore&update=true"
'''
            }
        }
    }
}
