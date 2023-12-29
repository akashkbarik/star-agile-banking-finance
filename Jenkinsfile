def containerName="akash-banking-finance"
def tag="latest"
def dockerHubUser="akashkbarik12"
def gitURL="https://github.com/akashkbarik/star-agile-banking-finance.git"

node {
      stage('checkout') {
	         git changelog: false, credentialsId: 'GitHubCreds', poll: false, url: 'https://github.com/akashkbarik/star-agile-banking-finance.git'
	  }
	  stage('build') {
	         sh "mvn clean install"
	  }
	  stage('clean any existing docker image'){
	         sh "docker image prune -f"
	  }
	  stage('build the docker image'){
	         sh "docker build -t $containerName:$tag  --pull --no-cache ."
			 echo  "*****image build sucessfully completed******"
	  }
	  stage('push to docker hub'){
	         withCredentials([usernamePassword(credentialsId: 'dockerhubcreds', passwordVariable: 'dockerHubPass', usernameVariable: 'dockerHubUser')]) {
                sh "docker login -u $dockerHubUser -p $dockerHubPass"
				sh "docker tag $containerName:$tag $dockerHubUser/$containerName:$tag"
				sh "docker push $dockerHubUser/$containerName:$tag"
				echo "***********image push sucessfully done*********"
            }
	  }
	  stage('deploy the container'){
	         sh "ansible-playbook ansible-playbook.yml"
	  }
	  
     }
