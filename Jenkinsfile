eventTrigger(jmespathQuery("eventType=='containerImagePush'"))

node('default-jnlp') {
  if (currentBuild.getBuildCauses("com.cloudbees.jenkins.plugins.pipeline.events.EventTriggerCause").size() > 0) {
    //sh 'ls'
    //writeFile file: "eventTriggerCause.json", text: currentBuild.getBuildCauses()[0].event.toString()
    //def eventTriggerCause = currentBuild.getBuildCauses()[0].event.toString()
    //echo eventTriggerCause
    //stash name: "json", includes: "eventTriggerCause.json"
    //unstash "json"
    def containerImage = sh(script: """
       curl -s -D "/dev/stderr" --silent ${BUILD_URL}/api/json| jq ".image"
    """, returnStdout: true)
    echo containerImage
  }
}
