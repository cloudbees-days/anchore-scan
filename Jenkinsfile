eventTrigger(jmespathQuery("eventType=='containerImagePush'"))

node('default-jnlp') {
  if (currentBuild.getBuildCauses("com.cloudbees.jenkins.plugins.pipeline.events.EventTriggerCause").size() > 0) {
    //sh 'ls'
    //writeFile file: "eventTriggerCause.json", text: currentBuild.getBuildCauses()[0].event.toString()
    //def eventTriggerCause = currentBuild.getBuildCauses()[0].event.toString()
    //echo eventTriggerCause
    //stash name: "json", includes: "eventTriggerCause.json"
    //unstash "json"
    def eventTriggerCause = currentBuild.getBuildCauses()[0].event.toString()
    def escapedEventTriggerCause = groovy.json.JsonOutput.toJson(eventTriggerCause).replace(""", "\\"")
    echo eventTriggerCause
    def containerImage = sh(script:"""
      echo $escapedEventTriggerCause
      $escapedEventTriggerCause | jq '.image'
    """, returnStdout: true)
    echo containerImage
  }
}
