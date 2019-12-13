pipeline {
  agent any
  stages {
    stage('Build In Development') {
      steps {
        script {
         openshift.withCluster() {
          openshift.withProject("development") {
            openshift.selector("bc", "myapp").startBuild().logs('-f')
          }
         }
        }
      }
    }
    stage('Deploy In Development') {
      steps {
        script {
         openshift.withCluster() {
          openshift.withProject("development") {
            openshift.selector("dc", "myapp").rollout().latest()
          }
         }
        }
      }
      steps {
        script {
         openshift.withCluster() {
          openshift.withProject("development") {
            openshift.selector("dc", "myapp").scale(3)
          }
         }
        }
      }
    }
    
  }
}
