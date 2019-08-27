def containerImage
pipeline {
  agent any
  options { 
    buildDiscarder(logRotator(numToKeepStr: '20'))
  }

  triggers {
      eventTrigger jmespathQuery("eventType=='containerImagePush'")
  }
  
  stages {
    stage('Anchore Scan') {
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
        writeFile file: 'anchore_images', text: containerImage
        anchore name: 'anchore_images', engineurl: 'http://anchore.svc.cluster.local:8228/v1/', engineCredentialsId: 'anchore-engine-creds', annotations: [[key: 'added-by', value: 'jenkins']]
      }
    }
  }
}
