pipeline{
	agent any
	tools{
	maven "maven_mvn"
	}
	stages{
	  stage('getcode'){
	    steps{
	      sh "echo 'cloning code' "
	      git credentialsId: 'github-credentials', url: 'https://github.com/okesinachi/maven-web-APP'
	    }
	  }
	  stage('test&build'){
	    steps{
          sh "echo 'running JUnit test cases to pass on artifacts' "
          sh "mvn clean package"
	    }
	  }
	  stage('codequality'){
	    steps{
	      sh "echo 'performing code quality analysis' "
	      sh "mvn sonar:sonar"
	    }
	  }
	  stage('uploadArtifacts'){
	    steps{
	      sh "echo 'uploading artifacts to nexus' "
	      sh "mvn deploy"
	    }
	  }
	  stage('deploy2uat'){
	    steps{
	      sh "echo 'deploying to UAT environment' "
	      deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://54.215.111.220:8080/')], contextPath: null, war: 'target/*war'
	    }
	  }
	  stage('approvalgate'){
	    steps{
	      timeout(time:7, unit:'DAYS'){
	      input message: 'please review and approve'
	      }
	    }
	  }
	  stage('deploy2production'){
	    steps{
	      deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://54.215.111.220:8080/')], contextPath: null, war: 'target/*war'
	    }
	  }
	}
	post{
	  always{
	    emailext body: '''hi everyone,

please check out the build status
thank you ''', recipientProviders: [buildUser(), contributor(), developers(), requestor(), upstreamDevelopers()], subject: 'check build status', to: 'brainycollins24@gmail.com'
	  }
	  success{
	    emailext body: '''congratulations everyone,

please check out the build status.
thank you ''', recipientProviders: [buildUser(), contributor(), developers(), requestor(), upstreamDevelopers()], subject: 'check build status', to: 'brainycollins24@gmail.com'
	  }
	  failure{
	    emailext body: '''hi everyone,

please check out the build status
thank you ''', recipientProviders: [buildUser(), contributor(), developers(), requestor(), upstreamDevelopers()], subject: 'check build status', to: 'brainycollins24@gmail.com'
	  }
	}
}
