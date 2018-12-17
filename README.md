# [Jenkinsfile](Jenkinsfile) Philips Hue Light Notifier

See you build status change via Philips Hue light!

On build start the light turns Purple and blinks (for max 15 seconds),
and then on build result:
* success -> light turns Green
* failure -> light turns Red
* unstable -> light turns Yellow
* aborted -> light turns dim White / Grey

Play with changing the color and other values in the [Jenkinsfile](Jenkinsfile) to get your custom build groove going!

## What is needed

* [Philips Hue Color Light](https://www2.meethue.com/sv-se/produkter#filters=BULBS_SU%2CLIGHTSTRIPS_SU%2CLAMPS_SU%2CFK_WHITE_AND_COLOR_AMBIANCE&sliders=&support=&price=&priceBoxes=&page=1&layout=12.subcategory.p-grid-icon)
* [Philips Hue Bridge](https://www2.meethue.com/sv-se/p/hue-brygga/8718696511800)
* [Jenkins CI server](https://jenkins.io)
* [Pipeline plugin suite](https://jenkins.io/doc/book/pipeline/)
* [A Multi-branch Pipeline Jenkins job for your code repo](https://jenkins.io/pipeline/getting-started-pipelines/#creating-multi-branch-pipelines)
* [HTTP Request plugin](https://plugins.jenkins.io/http_request)
* [Credentials Binding plugin (can be skipped)](https://plugins.jenkins.io/credentials-binding)

## How it works

In the [Jenkinsfile](Jenkinsfile) ([declarative Pipeline syntax](https://jenkins.io/doc/book/pipeline/#declarative-versus-scripted-pipeline-syntax)) I coded HTTP requests to a Hue light via [Hue API](https://developers.meethue.com/develop/hue-api/) on the build status changes mention above.

Here are the steps to get it working for you:
1. Find your desired light ID and update the variable value in the [Jenkinsfile](Jenkinsfile). Light IDs start from 1 and are basically in the order of your added lights to your Hue Bridge.
2. Find out your [Hue Bridge IP address and create a username for it.](https://developers.meethue.com/develop/get-started-2/)
3. [On Jenkins, add two secret text credentials](https://jenkins.io/doc/book/using/using-credentials/) for the Hue Bridge IP address and the username. Then copy the associated credential IDs and paste them into the variable values in the [Jenkinsfile](Jenkinsfile).
(NOTE: If you don't mind writing your IP address and username directly into the [Jenkinsfile](Jenkinsfile), you can skip 3. and remove the Credentials code and replace the $IP and $USER in the httpRequest url parameter in the [Jenkinsfile.](Jenkinsfile)

Commit and push the changes to your VCS and build the Jenkins job for the code repo, and enjoy the light show!

## License

This is the [license](LICENSE) for this code repo.

"Hue Personal Wireless Lighting" is a trademark owned by Koninklijke Philips Electronics N.V., see www.meethue.com for more information. I am in no way affiliated with the Philips organization.