pipeline{
	parameters{
		string(description: "Application Name", name: "appName")
		string(description: "Registry Secret Name", name: "registrySecretName")
		string(description: "Kubernetes Config Secret Name", name: "kubeConfigSecretName")
	}
	agent{
		kubernetes{
			defaultContainer "jnlp"
			yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
    - name: netcoresdk
      image: mcr.microsoft.com/dotnet/core/sdk:2.2
      command:
      - cat
      tty: true
    - name: docker
      image: docker:18.06.1-ce
      command: 
      - cat
      tty: true
    - name: helm
      image: ibmcom/microclimate-k8s-helm:v2.12.3
      command:
      - cat
      tty: true
  volumes:
  - name: docker.sock
    hostPath: 
      path: /var/run/docker.sock
      mountPath: /var/run/docker.sock
		}
	}
"""
	stages{
		stage("Extract"){
			try{
				container("jnlp"){
					checkout scm
				}
			}catch(Exception ex){
				error "Failed extract. Reason is: " + ex.toString()
			}
		}
		stage("Build source"){}
		stage("Build image"){}
		stage("Commit image"){}
		stage("Deploy"){}
		stage("Verify"){}
	}
}
