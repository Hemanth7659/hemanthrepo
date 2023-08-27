pipeline {
    agent any
    environment {
        PATH = "$PATH:/opt/maven3/apache-maven/bin"
        properties([parameters([choice(choices: ['value1=feature1', 'value2=main', 'value3=petclinic'], name: '')])])
    }
    stages {
        stage ('scm checkout') {
            steps {
                git branch: 'petclinic', url: 'https://github.com/Hemanth7659/hemanthrepo.git'
            }
        }
        stage ('build') {
            steps {
                sh "mvn clean package"
            }
        }
        stage('sonar analysis') {
            steps {
                withSonarQubeEnv('sonar_server') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage ('nexus upload') {
            steps {
                script {
                    def mavenPom = readMavenPom file: 'pom.xml'
                    nexusArtifactUploader artifacts: [[artifactId: 'petclinic', classifier: '', file: "target/petclinic.war", type: 'war']], credentialsId: '7e8b81b0-9b41-4d3d-8a2c-1e216065df53', groupId: 'hemanth', nexusUrl: '172.31.23.79:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'myapp_demo', version: "${mavenPom.version}"
                }
            }
        }
        stage ('Deploy To Dev') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'eb719147-af7c-4c2c-9c74-42efd420a2fd', path: '', url: 'http://http://13.49.241.180:9090')], contextPath: 'petclinic', war: '**/petclinic.war'
            }
        }
    }
}
