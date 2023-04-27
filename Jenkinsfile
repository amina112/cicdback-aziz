pipeline{
    agent any



    stages {


        stage('Getting project from Git') {
            steps{
      			checkout([$class: 'GitSCM', branches: [[name: '*/main']],
			extensions: [],
			userRemoteConfigs: [[url: 'https://github.com/amina112/cicdback-aziz.git']]])
            }
        }


       stage('Cleaning the project') {
            steps{
                	sh "mvn -B -DskipTests clean  "
            }
        }



        stage('Artifact Construction') {
            steps{
                	sh "mvn -B -DskipTests package "
            }
        }



         stage('Unit Tests') {
            steps{
               		 sh "mvn test "
            }
        }



        stage('Code Quality Check via SonarQube') {
            steps{

             		sh "  mvn sonar:sonar -Dsonar.projectKey=cicdback -Dsonar.host.url=http://192.168.1.71:9000 -Dsonar.login=ab18de57970ec3eefe16eb25363d0194ac1a5bce"

            }
        }


        stage('Publish to Nexus') {
            steps {


  sh 'mvn clean package deploy:deploy-file -DgroupId=com.esprit.examen -DartifactId=tpAchatProject -Dversion=1.2 -DgeneratePom=true -Dpackaging=jar -DrepositoryId=deploymentRepo -Durl=http://localhost:8081/repository/maven-releases/ -Dfile=target/tpAchatProject-1.2.jar'


            }
        }

stage('Build Docker Image') {
                      steps {
                          script {
                            sh 'docker build -t amina112/spring-app:latest .'
                          }
                      }
                  }

                  stage('login dockerhub') {
                                        steps {
                                     // sh 'echo dckr_pat_-SnwrdC_ELsL6it2JT6cgIcAlrs |docker login -u amina112 --password-stdin'
				sh 'docker login -u azizbenhaha --password dckr_pat_XhRLl3NFeq1alvvyyQmbnbbB--o'
                                            }
		  }
	    
	                      stage('Push Docker Image') {
                                        steps {
                                   sh 'docker push amina112/spring-app:latest'
                                            }
		  }

		   stage('stop local mysql') {
                                                  steps {
                                             sh 'docker stop 188'
                                                      }
          		  }

		   stage('Run Spring && MySQL Containers') {
                                steps {
                                    script {
                                      sh 'docker-compose up -d'
                                    }
                                }
                            }

	    



     
}

	    
// post {

     //  always {
     
//       cleanWs()
      // }
 //   }

    
	
}
       
