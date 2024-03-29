pipeline {
  agent any
   options {
      timeout(time: 1, unit: 'HOURS') 
  }
  parameters {
    gitParameter branchFilter: 'origin/(.*)', defaultValue: 'master', name: 'BRANCH_SELECTOR', type: 'PT_BRANCH'  }
  stages {
   stage('Clean Workspace') {
      steps {
        cleanWs()
      }
    }

    stage('Checkout') {
      steps {
        //git branch: "${params.BRANCH_SELECTOR}", credentialsId: '62bb76d6-c3c8-42a1-8838-978511d17bce', url: 'https://github.com/sonaliSuchi/CI.git'
		git branch: "${params.BRANCH_SELECTOR}", credentialsId: '0abe4fed-1ba8-429e-b98f-71cfbee36b9d', url: 'https://github.com/sahoosar/CI.git'
      }
    }
	 stage('Build') {
      steps {
        // build, build stages can be made in parallel aswell
        // build stage can call other stages
        // can trigger other jenkins pipelines and copy artifact from that pipeline
		 bat 'mvn clean install'
      }
    }
	stage('Test') {
      steps {
        // Test (Unit test / Automation test(Selenium/Robot framework) / etc.)
		script{
			if("${params.SKIP_TEST}")
			{
			echo 'Skipping Testcases'
			}
			
		}
      }
    }

    stage('Code Analysis') {
      steps {
        // Static Code analysis (Coverity/ SonarQube /openvas/Nessus etc.)
		script{
			if("${params.SKIP_ANALYSIS}")
			{
			echo 'Skipping SonarQube Code Analysis'
			}
			
		}
      }
    }

    stage('Generate Release Notes') {
      steps {
        // Release note generation .
		echo 'Generate release note' 
      }
    }

    stage('Tagging') {
      steps {
        // Tagging specific version number
		echo 'Tagging'
      }
    }

    stage('Release') {
      steps {
        // release specific versions(Snapshot / release / etc.)
		echo 'release specific version'
      }
    }

	stage('Deploy')
    {
				
		steps{
			//deploy adapters: [tomcat9(credentialsId: '99179fb5-2453-4ed2-919f-e36ea5dbf9be', path: '', url: 'http://localhost:8085')], contextPath: 'TestCICD-0.0.1', war: '**/*.war'
			deploy adapters: [tomcat9(credentialsId: '99179fb5-2453-4ed2-919f-e36ea5dbf9be', path: '', url: 'http://localhost:8085')], contextPath: 'TestCICD-0.0.1', war: 'target/*.war'
			}
    }
  }
  post {
    success {
      echo "SUCCESS"
    }
    failure {
      echo "FAILURE"
    }
    changed {
      echo "Status Changed: [From: $currentBuild.previousBuild.result, To: $currentBuild.result]"
    }
    always {
      script {
        def result = currentBuild.result
        if (result == null) {
          result = "SUCCESS"
        }
      }
    }
  }
}
