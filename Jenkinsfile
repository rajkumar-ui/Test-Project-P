pipeline {
	    agent any
	
	    parameters {
	         string(name: 'redhat-dev-tom', defaultValue: '172.31.81.210', description: 'Staging Server')
	         string(name: 'redhat-qa-tom', defaultValue: '172.31.81.58', description: 'Production Server')
	    }
	
	    triggers {
	         pollSCM('* * * * *')
	     }
	
	stages{
	        stage('Build'){
	            steps {
	                sh 'mvn clean package'
	            }
	            post {
	                success {
	                    echo 'Now Archiving...'
	                    archiveArtifacts artifacts: '**/target/*.war'
	                }
	            }
	        }
	
	        stage ('Deployments'){
	            parallel{
	                stage ('redhat-qa-tom'){
	                    steps {
	                        sh "scp -p -r /var/lib/jenkins/workspace/jenkins-project1/target/vprofile-v1.war jenkins@${params.tomcat_dev}:/usr/local/apache-tomcat-8.5.50/webapps"
	                    }
	                }
	
	                stage ("Deploy to Production"){
	                    steps {
	                        sh "scp -p -r /var/lib/jenkins/workspace/jenkins-project1/target/vprofile-v1.war jenkins@${params.tomcat_prod}:/usr/local/apache-tomcat-8.5.50/webapps"
	                    }
	                }
	            }
	        }
	    }
	}
