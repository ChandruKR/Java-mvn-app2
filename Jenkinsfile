pipeline {
    
    agent {
        label 'TEst'
    }

    tools 
    {
        maven 'MAVEN'
    }
    
    stages {
        stage('SCM-Checkout') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/ChandruKR/Java-mvn-app2.git'

            }
			}
		        stage("Compile"){
            steps{
                sh "mvn clean compile"
            }
        }
        
         stage("Test Cases"){
            steps{
                sh "mvn test"
            }
        }
        
        stage('Sonarqube Analysis & Build') {
            steps {
                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
                sh ''' mvn sonar:sonar -Dsonar.url=http://3.255.121.66:9000/ -Dsonar.login=sqa_4089ea6fd95566860b6d309a9892094b940bf48c -Dsonar.projectName=Project_sonar_Test \
                -Dsonar.java.binaries=. \
                    -Dsonar.projectKey=Project_sonar_Test '''
                
            }
			}
		 stage('Deploy to Test AppServer') {
            steps {
				script {
				    
				    sh "hostname "
                    sshPublisher(publishers: [sshPublisherDesc(configName: 'TEst', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '.', remoteDirectorySDF: false, removePrefix: 'target', sourceFiles: 'target/mvn-hello-chandru.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
				}
            }
		 }
			
        }
}
