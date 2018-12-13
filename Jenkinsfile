credIdIp = 'be2f44ab-6be1-4f5a-aa22-c4d7b69a7a93'
credIdUser = 'dbfbcc84-49b1-42b5-8d43-ed03a88b1d62'

pipeline {
	agent any
	stages {
		stage('notifying build start') {
			steps {
				notifyHueLight('{"hue":46920, "transitiontime":1, "on":true, "sat":254, "bri":254, "alert":"lselect"}')
			}
		}
		stage('build') {
			steps {
				// For now just simalting build by sleeping for some seconds
				powershell 'Start-Sleep -s 5'
			}
		}
	}
	post {
		success {
			notifyHueLight('{"hue":25717, "transitiontime":1, "on":true, "sat":254, "bri":254, "alert":"none"}')
		}
		failure {
			notifyHueLight('{"hue":25717, "transitiontime":1, "on":true, "sat":254, "bri":254, "alert":"none"}')
		}
		aborted {
			notifyHueLight('{"hue":12750, "transitiontime":1, "on":true, "sat":25, "bri":25, "alert":"none"}')
		}
		unstable {
			notifyHueLight('{"hue":12750, "transitiontime":1, "on":true, "sat":254, "bri":254, "alert":"none"}')
		}
	}
}

def notifyHueLight(body) {
	withCredentials([string(credentialsId: credIdIp, variable: 'IP'), string(credentialsId: credIdUser, variable: 'USER')]) {
		withEnv(["BODY=${body}"]) {
			httpRequest httpMode: 'PUT', url: "http://${env:IP}/api/${env:USER}/lights/4/state", requestBody: "${BODY}"
		 }
	}
}