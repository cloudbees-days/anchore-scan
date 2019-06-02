pipeline {
  agent { label 'default-jnlp' }
  
  triggers {
    eventTrigger jmespathQuery("eventType=='containerImagePush'")
  }
  
  stages {
    stage('Anchore Scan') {
      steps {
        script {
          if (currentBuild.getBuildCauses("com.cloudbees.jenkins.plugins.pipeline.events.EventTriggerCause").size() > 0) {
            containerImage = sh(script: """
              currentBuild.getBuildCauses()[0].event.toString() | 	jq '.image'
            """, returnStdout: true)
            echo containerImage
          }
        }
      }
    }
  }
}
