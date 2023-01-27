Blue/green deployment to release a single service
=================================================

> This repository contains deployment codes for the hello-gitops app. 
> Project structure includes codes both for the versions 1 and 2,
> which are stored in "versions" subdirectory. In order to deploy a new version all files
> from the needed version folder should replace respective files in the main project directories
> E.g. versions/v1/k8s/deployment.yaml file should go to k8s/deployment.yaml while deploying v1

## Steps to follow: 

1. version 1 is deployed on GKE and running


    --V1 is deployed to "python-app-blue" namespace--
        
        --switch to this namespace--
        kubens python-app-blue

        -- wait till ingress has been created(monitor it in GKE) and curl it--
        curl $(kubectl get ing -o jsonpath={.items..status.loadBalancer.ingress[0].ip}) ; echo


2. version 2 is deployed without stopping v1    


        --Check deployemnt--
        $ while sleep 2; do curl $(kubectl get ing -o jsonpath={.items..status.loadBalancer.ingress[0].ip}); echo; done
    

3. version 1 is shutdown


    $ kubectl delete deploy hello-gitops-v1



