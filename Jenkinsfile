pipeline {
    agent any

    environment {
        SONARQUBE = 'SonarQubeServer'
        ARTIFACTORY = 'JFrogArtifactory'
        WAR_NAME = 'ILP_HCLStore.war'
        ARTIFACT_PATH = 'libs-release-local/com/hcl/ILP_HCLStore/1.0.0/ILP_HCLStore-1.0.0.war'
    }

    triggers {
        githubPush()
    }

    stages {
        stage('Checkout Source') {
            steps {
                git url: 'https://github.com/your-org/HCL_Store_CICD.git', branch: 'main'
            }
        }

        stage('Code Quality - SonarQube') {
            steps {
                withSonarQubeEnv("${SONARQUBE}") {
                    sh 'mvn clean verify sonar:sonar'
                }
            }
        }

        stage('SonarQube Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Build + Unit Test') {
            steps {
                sh 'mvn clean install'
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
                sh '''
                curl -u your_jfrog_user:your_jfrog_api_key -O https://your-jfrog-server/artifactory/${ARTIFACT_PATH}
                curl -u tomcat_user:tomcat_password -T ${WAR_NAME} "http://localhost:8080/manager/text/deploy?path=/HCLStore&update=true"
                '''
            }
        }
    }
}
