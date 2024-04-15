# kubernete

CI pipeline - https://github.com/nksinghfirst/Pipeline-ci.git

Step1 - Created an EC2 instance of t2.medium because it was pre-requisite for minikube (2CPU).

Step2 - Then installed docker on it for creating and testing images.

Setp3 - Created a docker image and tested it and it was working fine.
$ docker run -d -p 3333:3333 <image-name>

Step4 - Then created git repo and uploaded the files that is required for creating an image.

Step5 - Then created Jenkinsfile for CI part of the pipeline, to create an image, push it on dockerhub and starting the next job for CD.

Step6 - Then created a webhook in github so that the pipeline/job gets triggered when ever any changes happned in the git repo. 

Step7 - Then I installed Java and Jenkins on EC2.

Step8 - Then installed docker plugins in jenkins.

Step9 - Creted dockerhub and github credential so that it can access it.(created a tocken for github credential)

Step10 - Then created jenkins job for CI of pipeline selected webhook trigger and provided the link for github repo that contain files - Jenkinsfile and files for building image.

Step11 - Then buid the job to see if it was working or not.


Step12 - Then I created another git repo that contain Jenkinsfile for updating the daocker image tag in deployment.yaml file and yaml files for k8s and argocd

Step13 - Then created pipeline for it.

Step14 - Then I installed minikube, kubectl and argocd.

Step15 - Then I opened argocd and created a New App filled the detail and then created it.

Step16 - APP Health was Healthy.

Step17 - Then I make changes in deployment.yaml file changed 'replicas: 2 to replicas: 3' and then clicked on Refresh or Sync then it Synced it then their was 3 replicas.


Step18 - Then tested my CI CD setup by makings changes in code in git repo.
       
       - After making changes in repo the jenkins CI job started building for createing an image, push it on dockerhub and starting the next job for CD,
       
       - After successful build it trigger the build of another CD job for updating the tag in deployment.yaml file,
       
       - After that I waited for around 3 min for ArgoCD to check that the git repo has been updated (that it is out of sync) (AgroCD automatically check for changes eery 3 min)
       
       - Then AgroCD make changes in APP according to the desired state (according to department.yaml file)


Step19 - Rollouts Strategy Canary Release - What Canary Release does is it expose a portion of your traffic to the new version like a new beta version of your application.

Step20 - Installed rollouts by following offical documents https://argo-rollouts.readthedocs.io/en/stable/installation/ .

Step21 - Now make some changes in the images and create a new image of it by running the CI pipeline again.

Step22 - Then create a yaml for canary rollout in the same git repo as deployment.yaml. 

Step23 - And make sure that in deployment.yaml the older version is mention and the newer version in roll.yaml.

Step24 - After creating sync the changes in argocd and now you can also see the rollout in it.

Step25 - Once everything was good APP health was Healthy and new version was working fine then I promote the rollout and check its status.

Step26 - Then the new image will be running completely and the old one will get deleted.

Step27 - Clean UP - Delete the app that you created in the ArgoCD  using ArgoCD UI and once you do that all the resources of your app will get deleted from you K8s cluster.
     - Also by selecting PRUNE RESOURCES while creating APP what it does is it deleted the resources that are not presented in the github repo
       

# Problems I faced while doing all this
1. Jenkins wasn't able to push the image to dockerhub because it wasn’t able to access the docker.sock

sol. sudo chmod 777 docker.sock

2. Jenkins and AgroCD uses the same port for while running, So I change the port of Jenkins.

sol. sudo vi /lib/systemd/system/jenkins.service

3. Unable to use ArgoCD UI.

sol. kubectl edit svc argocd-server -n argocd  (change ‘type: ClusterIP’ to ‘type: NodePort’ ) &
   
   kubectl port-forward --address 0.0.0.0 svc/argocd-server -n argocd 8080:443

4. While ArgoCD was running, I wasn't able to use the terminal.

sol. So I also connected it through SSH Client. 

5. First I use LoadBlancer to get the external link for the website but the APP Health was stuck on 'Processing' 

sol. Then I used NodePort and then the APP Health was 'Healthy'.
