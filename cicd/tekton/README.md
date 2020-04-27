# Tekton 

[Official documentation](https://tekton.dev/docs/)

## Setup 
You will need a working Kubernetes cluster in order to use Tekton.

Install the [tekton cli tool](https://tekton.dev/docs/cli/). This will read your kubeconfig to send commands to your kubernetes cluster.

Install pipeline:
```bash
kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml
```

Install Dashboard Tekton's UI:
```bash
kubectl apply --filename https://github.com/tektoncd/dashboard/releases/latest/download/tekton-dashboard-release.yaml
```

Forward the port to your localhost in order to access it in at [http://localhost:9097](http://localhost:9097):

```bash
kubectl --namespace tekton-pipelines port-forward svc/tekton-dashboard 9097:9097
```

This pipeline will use Kaniko, to build and push the image. You will need to create a pod for it:

```bash
kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/master/kaniko/kaniko.yaml
```

### Credentials

- Change the url of [resource-git.yaml](./resource-git.yaml) with your repository.
- The pipeline will build and push an image to Docker Hub, so change the user and password content of [secret.yaml](./secret.yaml)
- Change you image registry to store the image created in [resource-image.yaml](./resource-image.yaml)

No credentials were created for the git repository because I asssume it will be public

### Running

After setting the required credentials, you need to create the components of the pipeline. You may run it with:

```bash
kubectl apply -f resource-git.yaml,secret.yaml,service-account.yaml,resource-image.yaml,task-test.yaml,pipeline.yaml
```


To execute the pipeline, do the following:
```bash
kubectl create -f run-pipeline.yaml
```


You can check the results of the pipeline with:
```bash
tkn pipelinerun logs -f
```





