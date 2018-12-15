import groovy.transform.Field
import groovy.json.JsonOutput

@Field final int LIGHT_ID = 4

@Field final CREDENTIALS_ID_IP = 'be2f44ab-6be1-4f5a-aa22-c4d7b69a7a93'
@Field final CREDENTIALS_ID_USER = 'dbfbcc84-49b1-42b5-8d43-ed03a88b1d62'

final int HUE_RED = 0
final int HUE_YELLOW = 12750
final int HUE_GREEN = 25717
final int HUE_BLUE = 46920

pipeline {
	agent any
	stages {
		stage('Notify build start') {
			steps {
				notifyHueLight([hue: HUE_BLUE, sat: 254, bri: 254, alert: 'lselect', transitiontime: 1, on: true])
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
			notifyHueLight([hue: HUE_GREEN, sat: 254, bri: 254, alert: 'none', transitiontime: 1, on: true])
		}
		failure {
			notifyHueLight([hue: HUE_RED, sat: 254, bri: 254, alert: 'none', transitiontime: 1, on: true])
		}
		unstable {
			notifyHueLight([hue: HUE_YELLOW, sat: 254, bri: 254, alert: 'none', transitiontime: 1, on: true])
		}
		aborted {
			notifyHueLight([hue: HUE_YELLOW, sat: 150, bri: 50, alert: 'none', transitiontime: 1, on: true])
		}
	}
}

void notifyHueLight(body) {
	withCredentials([string(credentialsId: CREDENTIALS_ID_IP, variable: 'IP'), string(credentialsId: CREDENTIALS_ID_USER, variable: 'USER')]) {
		String jsonBody = JsonOutput.toJson(body)
		httpRequest httpMode: 'PUT', url: "http://$IP/api/$USER/lights/$LIGHT_ID/state", requestBody: jsonBody, quiet: true
	}
}