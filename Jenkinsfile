pipeline {
    agent any
stages {
    stage ('scm checkout'){
        steps {
            git branch: 'petclinic', url: 'https://github.com/Hemanth7659/hemanthrepo.git'
        }
    }
    stage ('build') {
        steps {
            withMaven(globalMavenSettingsConfig: '', jdk: 'java17', maven: 'maven3', mavenSettingsConfig: '', traceability: true) {
                sh "mvn clean package"
            }
        }
    }
    stage('sonar analysis'){
        steps {
            withSonarQubeEnv('sonar_server') {
                sh 'mvn sonar:sonar'
            }
        }
    }
    stage ('nexus upload'){
        steps {
            nexusArtifactUploader artifacts: [[artifactId: 'petclinic', classifier: '', file: '/var/lib/jenkins/workspace/hemanth76598/target/petclinic.war', type: 'war']], credentialsId: '256f9a29-d769-4333-9ea3-24e97a3696f4', groupId: 'petclinic', nexusUrl: '16.171.139.119:8081', nexusVersion: 'nexus2', protocol: 'http', repository: 'maven-snapshots', version: '1.0.0'
        }
    }
}
}