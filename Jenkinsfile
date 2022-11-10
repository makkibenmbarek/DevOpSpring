pipeline {

    agent any

    stages {
        stage('Getting project from Github') {
            steps {
                git branch: 'Maki', url:'https://github.com/makkibenmbarek/DevOpSpring.git',
                credentialsId:"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";
            }
        }
        stage('database connection') {
            steps{
                sh '''
                sudo docker stop mysql || true
                sudo docker restart mysql || true
                '''
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
        stage ('Unit Test') {
            steps{
                sh 'mvn  test'
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
                 sh 'docker build --build-arg IP=0.0.0.0 -t makkibenmbarek/devops  .'
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
        stage('Sending email'){
           steps {
            mail bcc: '', body: '''Hello from Jenkins,
            Devops Pipeline returned success.
            Best Regards''', cc: '', from: '', replyTo: '', subject: 'Devops Pipeline', to: 'makki.benmbarek@esprit.tn'
            }
       }

    }
}
