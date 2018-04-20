pipeline {
    agent 'TestMachine-ut'

    tools {
        maven 'maven3'
        // jdk 'jdk8'
        
    }

    stages {
        stage('install and sonar parallel') {
            steps {
                parallel(
                        install: {
                            sh "mvn -U clean test cobertura:cobertura -Dcobertura.report.format=xml"
                        },
                        sonar: {
                            sh "mvn sonar:sonar -Dsonar.host.url=http://139.59.90.202:9000"
                        }
                )
            }
            post {
                always {
                    junit '**/target/*-reports/TEST-*.xml'
                    step([$class: 'CoberturaPublisher', coberturaReportFile: 'target/site/cobertura/coverage.xml'])
                }
            }
        }
        stage('deploy') {
            steps {
                sh "mvn deploy -DskipTests -Dartifactory_url=http://159.65.148.210:8081/artifactory"
            }
        }
    }
}	
