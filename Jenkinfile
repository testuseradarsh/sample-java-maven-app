pipeline {
    agent any
    tools{
        maven 'Maven'
    }
    stages {
        stage('SonarQube Scan') {
            steps {
                bat 'mvn sonar:sonar \
                  -Dsonar.projectKey=simple-java-maven-app \
                  -Dsonar.host.url=http://localhost:9000 \
                  -Dsonar.login=3e224092e819f668051ef32ab263efd0cc95c173'
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
