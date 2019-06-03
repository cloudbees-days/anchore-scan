eventTrigger(jmespathQuery("eventType=='containerImagePush'"))

node('default-jnlp') {
  if (currentBuild.getBuildCauses("com.cloudbees.jenkins.plugins.pipeline.events.EventTriggerCause").size() > 0) {
    //sh 'ls'
    //writeFile file: "eventTriggerCause.json", text: currentBuild.getBuildCauses()[0].event.toString()
    //def eventTriggerCause = currentBuild.getBuildCauses()[0].event.toString()
    //echo eventTriggerCause
    //stash name: "json", includes: "eventTriggerCause.json"
    //unstash "json"
    withCredentials([string(credentialsId: 'beedemo-admin-api-key', variable: 'TOKEN')]) {
      def containerImage = sh(script: """
         curl -u 'beedemo-admin':$TOKEN --silent ${BUILD_URL}/api/json| jq '.actions[0].causes[0].event.image'
      """, returnStdout: true)
      echo containerImage
      sh "curl -s https://ci-tools.anchore.io/inline_scan-v0.3.3 | bash -s -- -f -p ./policy-bundle.json ${containerImage}"
    }
  }
}
