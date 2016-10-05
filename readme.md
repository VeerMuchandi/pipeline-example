## Creating your own pipeline

###Video 1: Creating your own pipeline

The build configuration used for creating the pipeline is 
[https://github.com/VeerMuchandi/pipeline-example/blob/master/pipeline.yml]()



### Video 2: Edit the pipeline to deploy across projects 

The initial Jenkinsfile is [https://github.com/VeerMuchandi/pipeline-example/blob/master/Jenkinsfile1.txt]()

The edited Jenkinsfile is [https://github.com/VeerMuchandi/pipeline-example/blob/master/Jenkinsfile2.txt]()


In this example, the pipeline runs in the CICD Project.  We will build and deploy an application first in a project named 'Development'. Later we will push the image created into a project named 'Testing'.

![](pipelines example.tiff)


Here are the commands I used from the OpenShift CLI:

Creating a `development` Project and to provide `edit` access to the `jenkins` service account in the `development` project.

```
oc new-project development

oc policy add-role-to-user edit system:serviceaccount:cicd:jenkins -n development
```

Creating a `testing` Project . Provide `edit` access to the `jenkins` service account in the `testing` project. Then provide `image-puller` access, so that `testing` project can pull an image from the `development` project.

```
oc new-project testing

oc policy add-role-to-user edit system:serviceaccount:cicd:jenkins -n testing

oc policy add-role-to-group system:image-puller system:serviceaccounts:testing -n development
```

To create a deployment configuration in the `testing` project that points to the image from development project, create a service and route:

```
oc create deploymentconfig myapp --image=<<RegistryServiceIP>>:5000/development/myapp:promoteToQA

oc expose dc myapp --port=8080

oc expose svc myapp

```


