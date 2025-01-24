pipeline { 
    agent any 
    stages {
        stage('Build') { 
            steps {
                withMaven(maven : 'apache_maven_3.9.6'){
                        sh "mvn clean compile"
                }
            }
        }
        stage('Test'){
            steps {
                withMaven(maven : 'apache_maven_3.9.6'){
                        sh "mvn test"
                }
            }
        }
        stage('build && SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonarserver') {
                    sh '/var/lib/jenkins/sw/maven/bin/mvn sonar:sonar -Dsonar.projectKey=HelloWorldMaven -Dsonar.sources=./src/main/'
                }
            }
        }
        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    // Requires SonarScanner for Jenkins 2.7+
                    waitForQualityGate abortPipeline: true
                }
            }
}
        stage('Deploy') {
            steps {
               withMaven(maven : 'apache_maven_3.9.6'){
                        sh "mvn deploy"
                }
            }
        }
    }
}
