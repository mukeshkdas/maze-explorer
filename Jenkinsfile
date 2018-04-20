
    node("master")
{

def W_M2_HOME = tool 'maven3'

    stages {
        stage('install and sonar parallel') {
            steps {
                
                parallel(
                        install: {
                            sh "${W_M2_HOME}/bin/mvn -U clean test cobertura:cobertura -Dcobertura.report.format=xml"
                        },
                        sonar: {
                            sh "${W_M2_HOME}/bin/mvn sonar:sonar -Dsonar.host.url==http://sonarqube:9000"
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
                sh "${W_M2_HOME}/bin/mvn deploy -DskipTests -Dartifactory_url=http://159.65.148.210:8081/artifactory"
            }
        }
    }
}

