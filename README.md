
## LUCY (CI/CD version) MANUAL


**System Requirements**

* pulseaudio has to be installed on your system

**Installation Requirements for CI/CD version**

1. Jenkins server - to run jobs
	* give Jenkins permission to '/var/run/docker.sock' (Make sure to restart Jenkins server after this group change)
	  * sudo usermod -aG docker jenkins  (perfect for Linux, won't work on Mac)
	  * sudo chmod 775 /var/run/docker.sock (perfect for MacOS)

2. Jenkins-job builder - to push jobs to Jenkins

3. Kubernetes server (i.e. kubeadm)
	* put '.kube' directory to Jenkins user $HOME (Ubuntu: '/var/lib/jenkins', MacOS: '/Users/Shared/Jenkins/')
	(the best way is to do this right after Kubeadm installation. copy '.kube' dir to both jenkins and your user)


## Lucy manage

1. Initial Run
* Run the following command inside Lucy working dir:
**kubectl create -f kubernetes/deployment_k8s_lucy.yaml**
* this will create a pod with Lucy app and, if  pulseaudio is ON - Lucy will start working right away

2. Hard Stop Lucy
**kubectl delete deployment lucy-app-kube-dpl**
* this will delete the deployment(kubernetes object) created for Lucy. 
* The POD with lucy_app container will be destroyed. 
* You'll have to perform the 'Initial Run' to start it again
* the termination takes about 1 min, it is NOT instant

### Rollback actions
* Run the following command inside inside Lucy working dir
**kubectl set image deployment.v1.apps/lucy-app-kube-dpl lucy-app-ctr=bzumby/lucy_app:v1.XXXX**
* Note that 'v1.XXXX' is the image tag of the stable version.
* You have to explicitly specify it here  
