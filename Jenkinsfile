pipeline {

    agent any

    stages {
        stage('Getting project from Github') {
            steps {
                git branch: 'Maki', url:'https://github.com/makkibenmbarek/DevOpSpring.git',
                credentialsId:"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";
            }
        }
        
        stage('cleanig the project') {
            steps{
                sh 'mvn clean'
            }

        }
        stage ('artifact construction') {
            steps{
                sh 'mvn  package'
            }
        }
        stage ('Jacoco') {
            steps{
		 //sh 'mvn  test'
		jacoco()
               
            }
        }
        stage ('SonarQube analysis') {
            steps{
                sh '''
                mvn sonar:sonar
                '''
            }
        }
        stage('Nexus'){
            steps{
                sh """mvn deploy """
            }
        }
         stage('Docker build')
        {
            steps {
                 sh 'docker build --build-arg IP=172.20.10.4 -t makkibenmbarek/devops  .'
            }
        }
        stage('Docker login')
        {
            steps {
                sh 'echo $dockerhub_PSW | docker login -u makkibenmbarek -p dckr_pat_t9FvttA33hYvhaZ-st3etxXDwkE'
            }    
       
        }
      stage('Push') {

			steps {
				sh 'docker push makkibenmbarek/devops'
			}
		}
        
       stage('Run app With DockerCompose') {
              steps {
                  sh "docker-compose -f docker-compose.yml up -d  "
              }
              }
        

    }
post {
    always {
       mail to: 'makki.benmbarek@esprit.tn',
          subject: "Status of pipeline: ${currentBuild.fullDisplayName}",
          body: "${env.BUILD_URL} has result ${currentBuild.result}"
    }
  }
}
