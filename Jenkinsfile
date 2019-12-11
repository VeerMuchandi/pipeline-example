node('') {
stage ('buildInDevelopment')
  {sh 'oc start-build myapp -n development'}
stage ('deployInDevelopment')
  {
   sh 'oc rollout latest dc myapp -n development'
   sh 'oc scale --replicas=2 myapp -n development'
   }
stage ('deployInTesting')
  {
    sh 'oc tag development/myapp:latest  development/myapp:promoteToQA'
    sh 'oc rollout latest dc myapp -n testing'
    sh 'oc scale --replicas=3 myapp -n testing' }
}
