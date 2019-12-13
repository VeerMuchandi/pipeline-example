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
         copenshift.withProject("development") {
            openshift.selector("dc", "myapp").rollout().latest()
          }
         }
        }
        script {
         openshift.withCluster() {
          openshift.withProject("development") {
            openshift.selector("dc", "myapp").scale("--replicas=3")
          }
         }
        }
      }
    }
    stage('Promote to QA') {
      steps {
        script {
          openshift.withCluster() {
           openshift.withProject("development") {
            openshift.tag("development/myapp:latest", "development/myapp:promoteToQA")
           }
          }
        }
      }
    }
    stage('Deploy In QA') {
      steps {
        script {
         openshift.withCluster() {
         copenshift.withProject("testing") {
            openshift.selector("dc", "myapp").rollout().latest()
          }
         }
        }
        script {
         openshift.withCluster() {
          openshift.withProject("testing") {
            openshift.selector("dc", "myapp").scale("--replicas=2")
          }
         }
        }
      }
    }
    
  }
}
