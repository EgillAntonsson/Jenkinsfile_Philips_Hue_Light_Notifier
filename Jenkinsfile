import groovy.transform.Field
import groovy.json.JsonOutput

@Field int lightId = 4

@Field String credIdIp = 'be2f44ab-6be1-4f5a-aa22-c4d7b69a7a93'
@Field String credIdUser = 'dbfbcc84-49b1-42b5-8d43-ed03a88b1d62'

int hueRed = 0
int hueYellow = 12750
int hueGreen = 25717
int hueBlue = 46920

pipeline {
	agent any
	stages {
		stage('notifying build start') {
			steps {
				notifyHueLight([hue: hueBlue, sat: 254, bri: 254, alert: 'lselect', transitiontime: 1, on: true])
			}
		}
		stage('build') {
			steps {
				// Simulating build by sleeping for some seconds
				powershell 'Start-Sleep -s 1'
			}
		}
	}
	post {
		success {
			notifyHueLight([hue: hueGreen, sat: 254, bri: 254, alert: 'none', transitiontime: 1, on: true])
		}
		failure {
			notifyHueLight([hue: hueRed, sat: 254, bri: 254, alert: 'none', transitiontime: 1, on: true])
		}
		unstable {
			notifyHueLight([hue: hueYellow, sat: 254, bri: 254, alert: 'none', transitiontime: 1, on: true])
		}
		aborted {
			notifyHueLight([hue: hueYellow, sat: 150, bri: 50, alert: 'none', transitiontime: 1, on: true])
		}
	}
}

void notifyHueLight(LinkedHashMap body) {
	withCredentials([string(credentialsId: credIdIp, variable: 'IP'), string(credentialsId: credIdUser, variable: 'USER')]) {
		String jsonBody = JsonOutput.toJson(body)
		httpRequest httpMode: 'PUT', url: "http://$IP/api/$USER/lights/$lightId/state", requestBody: jsonBody, consoleLogResponseBody: true
	}
}