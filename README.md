Blue/green deployment to release a single service
=================================================

> This repository contains deployment codes for the hello-gitops app. 
> Project structure includes codes for the current version, as well as other app versions
> which are stored in "versions" subdirectory. In order to deploy a new version all files
> from the needed version folder should replace respective file in the main projects directories
> E.g. versions/v1/k8s/deployment.yaml file should go to k8s/deployment.yaml 

## Followed steps: 

1. version 1 is deployed on GKE and running


    --Test if the deployment was successful--
        --check which namespace current version is deployed on--
        kubectl get ns | grep python-app
        "python-app-blue" or "python-app-green"
        
        --switch to this namespace and list pods--
        kubens python-app-green 
        kubectl get po 

        --choose any pod and run port-forwarding--
        kubectl port-forward {pod_id} 8111:8050

        --open a new terminal an run curl--
        curl localhost:8111
        "Hello, world!" 


2. version 2 is deployed without stopping v1



    --To see the deployment in action, open a new terminal and run the following--
    command:
    $ watch kubectl get po

    --Wait for all the version 2 pods to be running--
    $ kubectl rollout status deploy hello-gitops-v1 -w
    deployment "hello-gitops-v2" successfully rolled out

3. incoming traffic is switched from version 1 to version 2


    --switch traffic --
     $ kubectl patch service my-app -p '{"spec":{"selector":{"version":"v2.0.0"}}}'

    --Test if the deployment was successful--
    kubectl -n hello-gitops port-forward $(kubectl -n hello-gitops get po -o name) 8111:8050
    curl localhost:8111
    "Hello, Sasha!" 

4. version 1 is shutdown


    $ kubectl delete deploy hello-gitops-v1



