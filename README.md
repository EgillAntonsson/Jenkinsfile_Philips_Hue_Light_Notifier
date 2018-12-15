# Jenkinsfile Philips Hue Light Notifier

See you build status change be notified via Philips Hue light!

On build start the light turns Blue and blinks (for max 15 seconds),
and then build result:
* success -> light turns Green
* failure -> light turns Red
* unstable -> light turns Yellow
* aborted -> light turns 'Greyish'

## What is needed

* [Philips Hue Color Light](https://www2.meethue.com/en-gb/produkter#filters=BULBS_SU%2CLIGHTSTRIPS_SU%2CLAMPS_SU%2CFK_WHITE_AND_COLOR_AMBIANCE&sliders=&support=&price=&priceBoxes=&page=1&layout=12.subcategory.p-grid-icon)
* [Philips Hue Bridge](https://www2.meethue.com/sv-se/p/hue-brygga/8718696511800)
* [Jenkins CI server](https://jenkins.io)
* [Pipeline plugin and Pipeline Jenkins job setup for your code repo](https://jenkins.io/doc/book/pipeline/)
* [HTTP Request plugin](https://plugins.jenkins.io/http_request)
* [Credentials Binding plugin](https://plugins.jenkins.io/credentials-binding)

## How it works

In the Jenkinsfile (declarative Pipeline) I coded HTTP requests to a Hue light via Hue Bridge IP address.
Here are the steps to get it working for you:
1. Find your desired light ID and update the variable value in the Jenkinsfile. Light IDs start from 1 and are basically in the order of your added lights to your Hue Bridge.
2. Find out your [Hue Bridge IP address and create a username for it.](https://developers.meethue.com/develop/get-started-2/)
3. [On Jenkins, add two secret text credentials](https://jenkins.io/doc/book/using/using-credentials/) for the Hue Bridge IP address and the username. Then copy the associated credential IDs and paste them into the variable values in the Jenkinsfile.
NOTE: If you don't mind writing the IP address and username directly into the Jenkinsfile, you can remove the Credentials code and replace the $IP and $USER in the httpRequest url parameter.

Commit and push the changes to your VCS and build the Jenkins job for the repo, and enjoy the light show!

Play with changing the colors and other values to get your custom build groove going!
