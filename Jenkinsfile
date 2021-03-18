def checkoutGitSCM(branch,gitUrl) {
	checkout([$class: 'GitSCM',
		branches: [[name: branch ]],
		doGenerateSubmoduleConfigurations: false,
		extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: '.']],
		submoduleCfg: [],
		userRemoteConfigs: [[url: gitUrl]]
	])
}
pipeline {
	agent {
		node { label 'test' }
	}
    options {
		timestamps()
		disableConcurrentBuilds()
		buildDiscarder(logRotator(numToKeepStr: '10'))
		timeout(time: 180, unit: 'MINUTES')
	}
	parameters {
		string(name: 'ZAP_TARGET_URL', defaultValue:'https:planningtasks.com', description:'')
		choice(name: 'ZAP_ALERT_LVL', choices: ['High', 'Medium', 'Low'], description: 'See Zap documentation, default High')
	} 
	stages{
		stage('Initialize'){
			steps{
				script {
					currentBuild.displayName = "ZAP scan='${env.BUILD_NUMBER}''"
					currentWorkspace=pwd()
					cleanWs()					
				}
			}
		}
		stage('ZAP'){
			when { branch 'main' }
			steps{
				sh("echo ${env.WORKSPACE}; ls -l;")
				checkoutGitSCM("master","https://github.com/maabolihi/zap_jenkins.git")
				sh("bash -c \"chmod +x ${env.WORKSPACE}/*.sh\"")
				sh("${env.WORKSPACE}/validate_input.sh")
				sh("${env.WORKSPACE}/runZapScan.sh ${params.ZAP_TARGET_URL} ${env.WORKSPACE} ${params.ZAP_ALERT_LVL}")
			}
		}
		stage('Publish'){
			when { branch 'main' }
			steps{
				publishHTML([allowMissing: false,
				alwaysLinkToLastBuild: false,
				keepAll: false,
				reportDir: './reports',
				reportFiles: 'report.html',
				reportName: 'ZAP scan report',
				reportTitles: ''])
			}
		}
	}
	 post {
        always {
            sh("${env.WORKSPACE}/runCleanup.sh")
        }	
	}
}
