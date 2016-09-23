node('maven') {
stage 'buildInDevelopment'
openshiftBuild(namespace: 'development', buildConfig: 'myapp', showBuildLogs: 'true')
stage 'deployInDevelopment'
openshiftDeploy(namespace: 'development', deploymentConfig: 'myapp')
openshiftScale(namespace: 'development', deploymentConfig: 'myapp',replicaCount: '2')
stage 'deployInTesting'
openshiftTag(namespace: 'development', sourceStream: 'myapp',  sourceTag: 'latest', destinationStream: 'myapp', destinationTag: 'promoteToQA')
openshiftDeploy(namespace: 'testing', deploymentConfig: 'myapp', )
openshiftScale(namespace: 'testing', deploymentConfig: 'myapp',replicaCount: '3')
}
