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
        
        --switch to this namespace 
        kubens python-app-blue

        -- wait till ingress has been created and curl it
        curl $(kubectl get ing -o jsonpath={.items..status.loadBalancer.ingress[0].ip}) ; echo


2. version 2 is deployed without stopping v1
    


        --Check deployemnt--
        $ while sleep 2; do curl $(kubectl get ing -o jsonpath={.items..status.loadBalancer.ingress[0].ip}) ; done
    

3. version 1 is shutdown


    $ kubectl delete deploy hello-gitops-v1



