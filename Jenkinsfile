import groovy.json.JsonOutput

// Change the light id to the one you want to use.
LIGHT_ID = 4

// Change to your Jenkins secret text credential IDs for your Hue Bridge IP address and username.
CREDENTIALS_ID_IP = 'be2f44ab-6be1-4f5a-aa22-c4d7b69a7a93'
CREDENTIALS_ID_USER = 'dbfbcc84-49b1-42b5-8d43-ed03a88b1d62'

final def XY_RED = [0.692, 0.308]
final def XY_GREEN = [0.17, 0.7]
final def XY_BLUE = [0.153, 0.048]
final def XY_YELLOW = [0.511, 0.444]
final def XY_PURPLE = [0.208, 0.088]
final def XY_GREY = [0.323, 0.329]

final int BRI_MAX = 254

pipeline {
	agent any
	stages {
		stage('Notify build start') {
			steps {
				// alert: 'lselect' makes the light blink for max 15 seconds.
				notifyWithPhilipsHueLight([xy: XY_PURPLE, bri: BRI_MAX, alert: 'lselect', transitiontime: 1, on: true])
			}
		}
		stage('build') {
			steps {
				sleep 2
			}
		}
	}
	post {
		success {
			notifyWithPhilipsHueLight([xy: XY_GREEN, bri: BRI_MAX, alert: 'none', transitiontime: 1, on: true])
		}
		failure {
			notifyWithPhilipsHueLight([xy: XY_RED, bri: BRI_MAX, alert: 'none', transitiontime: 1, on: true])
		}
		unstable {
			notifyWithPhilipsHueLight([xy: XY_YELLOW, bri: BRI_MAX, alert: 'none', transitiontime: 1, on: true])
		}
		aborted {
			// light turns dimmed white / grey.
			notifyWithPhilipsHueLight([xy: XY_GREY, bri: 50, alert: 'none', transitiontime: 1, on: true])
		}
	}
}

void notifyWithPhilipsHueLight(body) {
	withCredentials([string(credentialsId: CREDENTIALS_ID_IP, variable: 'IP'), string(credentialsId: CREDENTIALS_ID_USER, variable: 'USER')]) {
		String jsonBody = JsonOutput.toJson(body)
		httpRequest httpMode: 'PUT', url: "http://$IP/api/$USER/lights/$LIGHT_ID/state", requestBody: jsonBody
	}
}