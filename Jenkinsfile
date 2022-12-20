pipeline {
	agent any
	stages{

	  stage('Fetch Code'){

	    agent {label 'docker'}
        
        steps {
	        
	        git branch: 'master' , url:'https://github.com/automationlearner17/devops_kubernetes.git'
	    }

	  }

	  stage('build Code'){
        
        agent {label 'docker'}

        tools {

          jdk "Openjdk"

        }

        steps {


            sh 'mvn install -DskipTests'

        }
	  }

	  stage('Test Code'){

	    agent {label 'docker'}

	    tools {

	      jdk "Openjdk"
	    }

	    steps {

	       sh 'mvn test'
	    }

	  }

	  stage('Code_Analysis') {

	   agent {label 'docker'}

	   tools {

	     jdk "Openjdk"
	   }

	   steps{

          sh 'mvn checkstyle:checkstyle'   

	   }

	}

        stage('SonarAnalysis') {
            
             agent {label 'docker'}

	     environment {
             scannerHome = tool 'sonar'
             } 

	    steps {


               withSonarQubeEnv('sonar') {
               sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                   -Dsonar.projectName=vprofile-repo \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''

               }
	    }
	 }

	
	 stage('Build image'){

	    agent {label 'docker'}

	    steps {

	       
	        sh 'docker build -t automationlearner/kubeapp:$BUILD_NUMBER app/'
	        sh 'docker build -t automationlearner/kubedb:$BUILD_NUMBER db/'

	    }
	 }

	 stage('Upload Image'){

	    agent {label 'docker'}

	    steps{

            sh 'docker push automationlearner/kubeapp:$BUILD_NUMBER'
            sh 'docker push automationlearner/kubedb:$BUILD_NUMBER'
	     
	    }
	 }

	 stage('Deploy-app on k8s') {
        
        agent {label 'KOPS'}

        steps {

           sh 'helm upgrade app-stack helm/vprofilechart --namespace vprofile'

        }

	 }


    }
}
