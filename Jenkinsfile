pipeline{
	agent{
		kubernetes{
			label "netcorebuild"
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
  - name: docker-sock
    hostPath: 
      path: /var/run/docker.sock
      mountPath: /var/run/docker.sock
  - name: registry-secret
    secret:
      path: /dockerconfig
      secretName: "${params.registrySecretName}"
"""
		}
	}
	parameters{
		string(description: "Application Name", name: "appName", defaultValue: "netcoreapp")
		string(description: "Registry Secret Name", name: "registrySecretName", defaultValue: "ibmcloud-registry-secret")
		string(description: "Kubernetes Config Secret Name", name: "kubeConfigSecretName", defaultValue: "ibmcloud-secret")
	}
	def imagetag
	def namespace
	def imagename = params.appName
	stages{
		stage("Extract"){
			steps{
				container("jnlp"){
					checkout scm
				}
			}
		}
		stage("Build source"){
			steps{
				container("netcoresdk"){
					  sh("dotnet restore src && dotnet publish src/ -c Release -o ../out")
				}
			}
		}
		stage("Build image"){
			steps{
				container("docker"){
					print "Image to build: us.icr.io/${namespace}/${imagename}:${imagetag}"
					sh("ln -s -f /dockerconfig/.dockerconfigjson /home/jenkins/.dockercfg")
					sh("mkdir -p /home/jenkins/.docker")
					sh("ln -s -f /dockerconfig/.dockerconfigjson /home/jenkins/.docker/config.json")
					sh("docker build -t us.icr.io/${namespace}/${imagename}:${imagetag} .")
				}
			}
		}
		stage("Commit image"){
			steps{
				container("netcoresdk"){
					print "TODO"
				}
			}
		}
		stage("Deploy"){
			steps{
				container("netcoresdk"){
					print "TODO"
				}
			}
		}
		//stage("Verify"){}
	}
}
