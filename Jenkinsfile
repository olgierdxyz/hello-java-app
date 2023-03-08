pipeline {
    agent any
    options {
        timeout(time: 1, unit: 'MINUTES')
        quietPeriod(1)
        retry(0)
        disableConcurrentBuilds abortPrevious: true
        buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')
        timestamps()
        ansiColor('xterm')
    }
    tools {
        maven "M3"
    }
    triggers {
        githubPush()
        pollSCM('H H(3-5) * * *')
        cron('H H(5-6) * * *')
        //
    }
    stages {
        stage('Build') {
            steps {
                sh "mvn -Dmaven.test.failure.ignore=true clean compile package"
            }
            post {
                success {
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
    }
}
