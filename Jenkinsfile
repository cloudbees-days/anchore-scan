def buildUrl = env.BUILD_URL
def eventTriggerCause = currentBuild.getBuildCauses()[0].event.toString()
eventTrigger(jmespathQuery("eventType=='containerImagePush'"))

node('label 'default-jnlp') {
  if (currentBuild.getBuildCauses("com.cloudbees.jenkins.plugins.pipeline.events.EventTriggerCause").size() > 0) {
    //sh 'ls'
    //writeFile file: "eventTriggerCause.json", text: currentBuild.getBuildCauses()[0].event.toString()
    //def eventTriggerCause = currentBuild.getBuildCauses()[0].event.toString()
    //echo eventTriggerCause
    //stash name: "json", includes: "eventTriggerCause.json"
    //unstash "json"
    def containerImage = sh(script: '''
      echo $buildUrl
      echo $eventTriggerCause
      curl --silent $buildUrl/api/json | jq '.image'
    ''', returnStdout: true)
    echo containerImage
  }
}
