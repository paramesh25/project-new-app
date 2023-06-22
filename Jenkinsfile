node{
   stage('SCM Checkout'){
     git 'https://github.com/damodaranj/my-app.git'
   }
stage('maven-buildstage'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
   stage('Build Docker Image'){
   sh 'docker build -t parameshn25/myweb:0.0.2 .'
   }
stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'DockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u parameshn25 -p ${dockerPassword}"
   }
   sh 'docker push parameshn25/myweb:0.0.2'
   }
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
stage('Docker deployment'){
   sh 'docker run -d -p 8099:8080 --name paramesh parameshn25/myweb:0.0.2' 
   }
}
