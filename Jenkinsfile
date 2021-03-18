def GIT_REPO = "zap_jenkins"

node {
agent {
docker { image 'zap2docker-stable' }
    }
    stage ('Pre-Requisites') {
        step([$class: 'WsCleanup'])
        sh """
	git clone https://github.com/maabolihi/zap_jenkins.git
	cd \$WORKSPACE/${GIT_REPO}
        chmod +x pull_docker.sh
        ./pull_docker.sh -u http://www.itsecgames.com
	
        """
        }

    stage ('Run ZAP Scan') {
        sh """
        
	# Run Scan
        docker exec zap zap-cli --verbose quick-scan http://www.itsecgames.com -l Medium
        
        """
        }

    stage ('Generate Report') {
        sh """

        docker exec zap zap-cli --verbose report -o /zap/reports/owasp-quick-scan-report.html --output-format html

        """
        }
}
