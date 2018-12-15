import groovy.transform.Field
import groovy.json.JsonOutput

// Change the light id to the one you want to use.
LIGHT_ID = 4

// Change to your Jenkins secret text credential IDs for your Hue Bridge IP address and username.
CREDENTIALS_ID_IP = 'be2f44ab-6be1-4f5a-aa22-c4d7b69a7a93'
CREDENTIALS_ID_USER = 'dbfbcc84-49b1-42b5-8d43-ed03a88b1d62'

final int HUE_RED = 0
final int HUE_YELLOW = 12750
final int HUE_GREEN = 25717
final int HUE_BLUE = 46920

final int SAT_MAX = 254
final int BRI_MAX = 254

pipeline {
	agent any
	stages {
		stage('Notify build start') {
			steps {
				notifyWithPhilipsHueLight([hue: HUE_BLUE, sat: SAT_MAX, bri: BRI_MAX, alert: 'lselect', transitiontime: 1, on: true])
			}
		}
		stage('build') {
			steps {
				sleep 1
			}
		}
	}
	post {
		success {
			notifyWithPhilipsHueLight([hue: HUE_GREEN, sat: SAT_MAX, bri: BRI_MAX, alert: 'none', transitiontime: 1, on: true])
		}
		failure {
			notifyWithPhilipsHueLight([hue: HUE_RED, sat: SAT_MAX, bri: BRI_MAX, alert: 'none', transitiontime: 1, on: true])
		}
		unstable {
			notifyWithPhilipsHueLight([hue: HUE_YELLOW, sat: SAT_MAX, bri: BRI_MAX, alert: 'none', transitiontime: 1, on: true])
		}
		aborted {
			// Less than max values in saturation and brightness affect the hue color, in this case resulting in a 'dimmed greyish' color.
			notifyWithPhilipsHueLight([hue: HUE_YELLOW, sat: 150, bri: 50, alert: 'none', transitiontime: 1, on: true])
		}
	}
}

void notifyWithPhilipsHueLight(body) {
	withCredentials([string(credentialsId: CREDENTIALS_ID_IP, variable: 'IP'), string(credentialsId: CREDENTIALS_ID_USER, variable: 'USER')]) {
		String jsonBody = JsonOutput.toJson(body)
		httpRequest httpMode: 'PUT', url: "http://$IP/api/$USER/lights/$LIGHT_ID/state", requestBody: jsonBody
	}
}