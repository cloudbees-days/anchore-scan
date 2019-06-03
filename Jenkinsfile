eventTrigger(jmespathQuery("eventType=='containerImagePush'"))

def label = "docker-${UUID.randomUUID().toString()}"

podTemplate(label: label, yaml: """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: docker
    image: gcr.io/technologists/docker-client:0.0.3
    command: ['cat']
    tty: true
    volumeMounts:
    - name: dockersock
      mountPath: /var/run/docker.sock
  volumes:
  - name: dockersock
    hostPath:
      path: /var/run/docker.sock
""") {

  node(label) {
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
        container('docker'){
          sh "curl -s https://ci-tools.anchore.io/inline_scan-v0.3.3 | bash -s -- -f -b ./policy-bundle.json -p ${containerImage}"
        }
      }
    }
  }
}
