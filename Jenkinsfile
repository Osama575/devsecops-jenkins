pipeline {
  agent any
  tools { 
        maven 'Maven_3_9_11'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=osama575_devsecops -Dsonar.organization=osama575 -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=a0197bcea3ce434191109cfc8cf9254df6f95b7c -B'
			}
        } 
  }
}
