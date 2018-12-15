import groovy.json.JsonOutput

lightId = 4

credentialsIdIp = 'be2f44ab-6be1-4f5a-aa22-c4d7b69a7a93'
credentialsIdUser = 'dbfbcc84-49b1-42b5-8d43-ed03a88b1d62'

final int hueRed = 0
final int hueYellow = 12750
final int hueGreen = 25717
final int hueBlue = 46920

pipeline {
	agent any
	stages {
		stage('Notify build start') {
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

void notifyHueLight(body) {
	withCredentials([string(credentialsId: credentialsIdIp, variable: 'IP'), string(credentialsId: credentialsIdUser, variable: 'USER')]) {
		String jsonBody = JsonOutput.toJson(body)
		httpRequest httpMode: 'PUT', url: "http://$IP/api/$USER/lights/$lightId/state", requestBody: jsonBody
	}
}