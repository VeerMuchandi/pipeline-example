pipeline {
  agent any
  stages {
    stage('Build In Development') {
      steps {
        script {
          openshift.withProject("development") {
            openshift.selector("bc", "myapp").startBuild()
          }
        }
      }
    }
    stage('Deploy In Development') {
      steps {
        script {
          openshift.withProject("development") {
            openshift.selector("dc", "myapp").rollout().latest()
          }
        }
      }
    }
  }
}
