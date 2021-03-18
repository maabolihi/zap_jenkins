node {
    stage ('Pre-Requisites') {
        step([$class: 'WsCleanup'])
        sh """
        # Pull zap docker stable
        docker pull owasp/zap2docker-stable
        
	docker exec zap zap-cli --verbose quick-scan http://www.itsecgames.com -l Medium
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
