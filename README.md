
# LUCY MANUAL


**System Requirements**

* pulseaudio has to be installed on your system

**Installation Requirements**

1. Jenkins server - to run jobs
	* give Jenkins permission to '/var/run/docker.sock'
	  * sudo usermod -aG docker jenkins  (perfect for Linux, won't work on Mac)
	  * sudo chmod 775 /var/run/docker.sock (perfect for Mac)

2. Jenkins-job builder - to push jobs
3. give Jenkins permission to : /var/run/docker.sock
like this: sudo usermod -aG docker jenkins # perfect for Linux, won't work on Mac.
or: sudo chmod 775 /var/run/docker.sock # perfect for Mac
Make sure to restart Jenkins server after this group change
4. Jenkins + Kubernetes server
put '.kube' file to Jenkins user $HOME
ubuntu: /var/lib/jenkins 
# maybe better do this after Kubeadm installation. copy '.kube' to both user and jenkins


## Lucy manage

1. Initial Run
# inside lucy working dir
kubectl create -f kubernetes/deployment_k8s_lucy.yaml
# this will create the pod with Lucy app and, if  pulseaudio is ON - Lucy will start working

2. Hard Stop Lucy
kubectl delete deployment lucy-app-kube-dpl
# this will delete the deployment created for Lucy. 
# The POD with lucy_app container will be destroyed. 
# You'll have to perform the 'Initial Run' to start it again
# terminating for ~1 min. not instant

### Rollback actions
# inside lucy app working dir
kubectl set image -f kubernetes/deployment_k8s_lucy.yaml lucy-app-ctr=bzumby/lucy_app:v1.XXXX
# Note that 'v1.XXXX' is the image tag of the stable version.
# You have to explicitly specify it here  
