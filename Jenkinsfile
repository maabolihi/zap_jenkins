node {
    stage ('Pre-Requisites') {
        step([$class: 'WsCleanup'])
        sh """
        # Pull zap docker stable
        docker pull owasp/zap2docker-stable
	
        docker run --detach --name zap -u zap -v "\$(pwd)/reports":/zap/reports/:rw \
  	-i owasp/zap2docker-stable zap.sh -daemon -host 0.0.0.0 -port 8080 \
  	-config api.addrs.addr.name=.* -config api.addrs.addr.regex=true \
  	-config api.disablekey=true
	
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
