pipeline {
    agent any
    tools{
        maven 'Maven'
    }
    stages {
        stage('SonarQube Scan') {
            steps {
                bat 'mvn sonar:sonar \
                  -Dsonar.projectKey=sample-java-maven-app \
                  -Dsonar.host.url=http://localhost:9000 \
                  -Dsonar.login=dc0d67768f0ed7156a9148407d9e391b0fd91129'
            }
        }
        stage('Build') {
            steps {
                bat 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                bat 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage("Publish to Artifactory"){
            steps{
                rtMavenDeployer(
                    id: 'deployer',
                    serverId: 'Pipline_repository@artifactory',
                    releaseRepo: 'Pipline_repository',
                    snapshotRepo: 'Pipline_repository'
                )
                rtMavenRun(
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: 'deployer'
                    )
                rtPublishBuildInfo(
                    serverId: 'Pipline_repository@artifactory',
                )
            }
        }
    }
}
