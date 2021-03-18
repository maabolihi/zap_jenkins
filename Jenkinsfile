def GIT_REPO = "zap_jenkins"

node {
    stage ('Pre-Requisites') {
        step([$class: 'WsCleanup'])
           sh "docker pull owasp/zap2docker-stable"
           sh "docker run --name deepika -u zap -p 8090:8090 -d owasp/zap2docker-stable zap.sh -daemon -port 8090 -host 0.0.0.0 -config api.disablekey=true"
           sh "docker exec deepika zap-cli -p 8090 open-url http://testhtml5.vulnweb.com/"
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
