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

	   stage('RunSCAAnalysisUsingSnyk') {
		   steps {
			           withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
						   sh 'mvn snyk:test -fn'

					   }
		   }

	}

	    stage('Build') {
			steps {
				withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
					script{
					app = docker.build("asg")
					}
				}
			}
	}

	    stage('Push') {
			steps {
				script{
					docker.withRegistry('https://879381260310.dkr.ecr.us-west-2.amazonaws.com', 'ecr:us-west-2:aws-credentials') {
					app.push("latest")
					}
				}
			}
			
		}

	   stage('Kubernetes Deployment of DevSecOps Application') {
		   steps {
			   withKubeConfig([credentialsId: 'kubelogin']) {
				   sh('kubectl delete all --all -n devsecops')
				   sh('kubectl apply -f deployment.yaml --namespace=devsecops')
					  }
					}
				}

			
  }
}
