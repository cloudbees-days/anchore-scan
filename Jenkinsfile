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
            env.eventTriggerCause = currentBuild.getBuildCauses()[0].event.toString()
            echo env.eventTriggerCause
            def containerImage = sh(script: '''
               $env.eventTriggerCause | jq '.image'
            ''', returnStdout: true)
            echo containerImage
          }
        }
      }
    }
  }
}
