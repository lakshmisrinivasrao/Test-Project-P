pipeline {
	    agent any
	
	    parameters {
	         string(name: 'tomcat_dev', defaultValue: '172.31.19.173', description: 'Staging Server')
	         string(name: 'tomcat_prod', defaultValue: '172.31.18.51', description: 'Production Server')
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
	                stage ('Deploy to Dev'){
	                    steps {
	                        sh "scp -i /home/ubuntu/Jenkins-Masterrrr.pem /var/lib/jenkins/workspace/Package/target/*.war centos@${params.tomcat_dev}:/usr/local/apache-tomcat-8.5.31/webapps"
	                    }
	                }
	
	                stage ("Deploy to Production"){
	                    steps {
	                        sh "scp -i /home/ubuntu/Jenkins-Masterrrr.pem /var/lib/jenkins/workspace/Package/target/*.war centos@${params.tomcat_prod}:/usr/local/apache-tomcat-8.5.31/webapps"
	                    }
	                }
	            }
	        }
	    }
	}
