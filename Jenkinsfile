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
        JENKINS_CLI = credentials('cli-username-token')
      }
      steps {
        script {
          containerImage = sh(script: """
             curl -u $JENKINS_CLI_USR:$JENKINS_CLI_PSW --silent ${BUILD_URL}api/json | jq -r '.actions[0].causes[0].event.image'
          """, returnStdout: true)
        }
        echo containerImage
        writeFile file: 'anchore_images', text: containerImage
        anchore name: 'anchore_images', engineurl: 'http://anchore-anchore-engine-api.anchore.svc.cluster.local:8228/v1/', engineCredentialsId: 'anchore-engine-creds', annotations: [[key: 'added-by', value: 'jenkins']]
      }
    }
  }
}
