def containerImage
pipeline {
  agent none

  triggers {
      eventTrigger jmespathQuery("eventType=='containerImagePush'")
  }
  
  stages {
    stage('Anchore Scan') {
      agent {
        kubernetes {
          label 'docker-client'
          yamlFile 'dockerClientPod.yml'
        }
      }
      when { 
        triggeredBy 'EventTriggerCause' 
        beforeAgent true
      }
      environment {
        TOKEN = credentials('beedemo-admin-api-key')
      }
      steps {
        script {
          containerImage = sh(script: """
             curl -u 'beedemo-admin':$TOKEN --silent ${BUILD_URL}/api/json| jq '.actions[0].causes[0].event.image'
          """, returnStdout: true)
        }
        echo containerImage
        container('docker-client'){
          sh "curl -s https://ci-tools.anchore.io/inline_scan-v0.3.3 | bash -s -- -f -b ./.anchore_policy.json -p ${containerImage}"
        }
      }
    }
  }
}
