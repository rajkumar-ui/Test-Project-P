pipeline {
	    agent any
	
	    parameters {
	         string(name: 'tomcat_DEV', defaultValue: '172.31.19.4', description: 'Staging Server')
	         string(name: 'tomcat_UAT', defaultValue: '172.31.18.83', description: 'Production Server')
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
	                stage ('Deploy to QA'){
	                    steps {
	                        sh "scp -p -r /var/lib/jenkins/workspace/maven project1/target/vprofile-v1.war jenkins@${params.tomcat_DEV}:/usr/local/apache-tomcat-8.5.50/webapps"
	                    }
	                }
	
	                stage ("Deploy to Production"){
	                    steps {
	                        sh "scp -p -r /var/lib/jenkins/workspace/maven project1/target/vprofile-v1.war jenkins@${params.tomcat_UAT}:/usr/local/apache-tomcat-8.5.50/webapps"
	                    }
	                }
	            }
	        }
	    }
	}
