# Deploy a WordPress on Kubernetes using Minicube with Helm and automation with Jenkins.
- Prerequisites:
1. First let's start with installing the necessary tools: Minicube, Helm and Jenkins.
- Minikube is a tool that lets you run Kubernetes locally.
- Here is step by step guide how to install Minikube: https://minikube.sigs.k8s.io/docs/start/

2. Helm is a package manager for Kubernetes. 
- In order to use helm, we need to download the Helm binary from here: https://github.com/helm/helm/releases
- We need to extract the binary file and copy it to the /usr/local/bin directory.
- Also verify the installation by running the following command: "helm version"
#
- The Project Assessment:
1. Download Helm chart for WordPress. 
- You can use this Bitnami chart: https://github.com/bitnami/charts/tree/main/bitnami/wordpress 
- I downloaded locally bitnami/wordpress. Also we can use the repository and download from there, but i chose to download it locally as you can see here:
![image](https://github.com/Stan15321/Final-Project-Assessment-for-Scalefocus-Academy/assets/109627707/33817c2e-4e90-4778-a84a-40a75872f608)

2. In values.yaml, you need to change line 543 from type: LoadBalancer to type: ClusterIP.
- In values.yaml, from the condition in the project it's said that we need to change LoadBalander to type: ClusterIP in values.yaml:
![image](https://github.com/Stan15321/Final-Project-Assessment-for-Scalefocus-Academy/assets/109627707/fe53203e-2da0-4700-95c1-fe8b5934402e)
- For me it was on line 534, not 543, so keep that in mind.
3. Now it's time to install the chart: 
- To install the chart, we can open PowerShell and there we need to use helm as a packet manager for kubernetes with the following command: ```"helm install wp1 wordpress/"```
- We use the command ```"helm install wp1 wordpress/"``` to deploy the WordPress Helm chart onto the Kubernetes cluster. This command installs the specified chart with the release name wp1. The WordPress chart contains the necessary components required to run a WordPress application on a Kubernetes cluster, such as a database, a WordPress web server, and a service to expose the application to the Internet. The helm tool simplifies the deployment of these components by providing a standardized way to package, configure, and deploy the chart onto a Kubernetes cluster:
![image](https://github.com/Stan15321/Final-Project-Assessment-for-Scalefocus-Academy/assets/109627707/1268cb83-c65c-42f7-bcbd-8692b4995b0a)
- Also we can see that when we get the services, "wp1-wordpress" type is not LoadBalancer and it is "ClusterIP":
![image](https://github.com/Stan15321/Final-Project-Assessment-for-Scalefocus-Academy/assets/109627707/99b78532-cc24-4dbd-8052-219cb44538e6)

4. Now with that being said, we can set up a local port forwarding in order to login:
![image](https://github.com/Stan15321/Final-Project-Assessment-for-Scalefocus-Academy/assets/109627707/ac7a20fb-b35b-41dc-9ad3-0155d8cc66df)
- The command ```"kubectl port-forward --namespace default svc/wp1-wordpress 3080:80"``` sets up a local port forwarding from port 3080 on the local machine to port 80 on the service wp1-wordpress in the Kubernetes cluster. This is useful because it allows you to access the WordPress application running in the cluster from your local machine. By forwarding traffic from your local machine to the Kubernetes cluster, you can interact with the WordPress application as if it were running on your local machine.
- We will see this login page:
![image](https://github.com/Stan15321/Final-Project-Assessment-for-Scalefocus-Academy/assets/109627707/76b7d6dc-ba04-4e0c-8f7f-7e4252319a98)

- In order to login, we need to add to the URL: http://localhost:3080/login
![image](https://github.com/Stan15321/Final-Project-Assessment-for-Scalefocus-Academy/assets/109627707/601d5dab-4cd7-4a56-bf73-7e00c691c80c)

- From here we can see that the username is user and in order to see the password, we need to execute this command in order to decode and see our password.
![image](https://github.com/Stan15321/Final-Project-Assessment-for-Scalefocus-Academy/assets/109627707/e640d6a3-4178-46ea-ab81-a149650339ce)

- We will see the Dashboard after we login:
![image](https://github.com/Stan15321/Final-Project-Assessment-for-Scalefocus-Academy/assets/109627707/b9e3a82b-cf57-40f2-8ec6-dbf6c7249b94)



5. Create a Jenkins pipeline that checks if wp namespace exists, if it doesn’t then it creates one, also naming the Helm Deployment as: final-project-wp-scalefocus.
Checks if WordPress exists, if it doesn’t then it installs the chart as we can see, also we specify the name of the stage to be: "final-project-wp-scalefocus"
- Now we need to do the deployment with the Jenkins pipeline, in order to do that we need to create a pipeline: 
- Here is the pipeline that i created:
```
pipeline {
  agent any
  environment {
    // Setting the KUBECONFIG environment variable
    KUBECONFIG = 'C:/Users/stani/.kube/config'
  }
  stages {
    stage('Verify') {
      steps {
        // This command verifies that the authentication and configuration are working correctly
        bat 'kubectl get services'
      }
    }
    stage('Deploy : final-project-wp-scalefocus') {
      steps {
        script {
          try {
            // Check if the namespace exists
            def namespaceExists = bat(script: 'kubectl get namespace default', returnStatus: true) == 0
            if (!namespaceExists) {
              // Create the namespace
              bat 'kubectl create namespace wp'
            }
            // Deploy the application using Helm
            bat 'helm dependency build C:/Users/stani/Desktop/DevOpsAcademy/Final-Project-Assessment-for-Scalefocus-Academy/bitnami/wordpress'
            bat 'helm install final-project-wp-scalefocus C:/Users/stani/Desktop/DevOpsAcademy/Final-Project-Assessment-for-Scalefocus-Academy/bitnami/wordpress -f C:/Users/stani/Desktop/DevOpsAcademy/Final-Project-Assessment-for-Scalefocus-Academy/bitnami/wordpress/values.yaml'
          } catch (Exception e) {
            // Handles any deployment errors
            error "Deployment failed: ${e.getMessage()}"
          }
        }
      }
    }
  }
}
```
6. Deploy the helm chart using the Jenkins pipeline.
- Here we have already created the pipeline, lets start it:
![image](https://github.com/Stan15321/Final-Project-Assessment-for-Scalefocus-Academy/assets/109627707/e6040fa6-0a71-4e15-a9da-23525ba08970)
- I had some issues with the paths of the files, but i managed to fix them to be full paths.
- Here is the conosole output from the pipeline:
```
Стартирана от потребител: „Stanislav“
[Pipeline] Start of Pipeline (hide)
[Pipeline] node
Running on Jenkins in C:\ProgramData\Jenkins\.jenkins\workspace\Final-Project_Scalefocus
[Pipeline] {
[Pipeline] withEnv
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Verify)
[Pipeline] bat

C:\ProgramData\Jenkins\.jenkins\workspace\Final-Project_Scalefocus>kubectl get services 
NAME            TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
kubernetes      ClusterIP   10.96.0.1        <none>        443/TCP          42d
wp1-mariadb     ClusterIP   10.102.27.87     <none>        3306/TCP         24h
wp1-wordpress   ClusterIP   10.100.235.219   <none>        80/TCP,443/TCP   24h
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Deploy : final-project-wp-scalefocus)
[Pipeline] script
[Pipeline] {
[Pipeline] bat

C:\ProgramData\Jenkins\.jenkins\workspace\Final-Project_Scalefocus>kubectl get namespace default 
NAME      STATUS   AGE
default   Active   42d
[Pipeline] bat

C:\ProgramData\Jenkins\.jenkins\workspace\Final-Project_Scalefocus>helm dependency build C:/Users/stani/Desktop/DevOpsAcademy/Final-Project-Assessment-for-Scalefocus-Academy/bitnami/wordpress 
Saving 3 charts
Downloading memcached from repo oci://registry-1.docker.io/bitnamicharts
Pulled: registry-1.docker.io/bitnamicharts/memcached:6.4.2
Digest: sha256:ac800af4f9b6be921043eb5cd2ba07828ad6fc404b5762f2630657d9fdf5a6fe
Downloading mariadb from repo oci://registry-1.docker.io/bitnamicharts
Pulled: registry-1.docker.io/bitnamicharts/mariadb:12.2.2
Digest: sha256:f18fd0e930041ef6a1dff0789eb801f2c4c52f1e8e0ff7c610b109ae8304d74c
Downloading common from repo oci://registry-1.docker.io/bitnamicharts
Pulled: registry-1.docker.io/bitnamicharts/common:2.2.5
Digest: sha256:a088a039a53958fdd4ddff5a9799c0dba38d1c480bc768a9141cb87e7fcf7036
Deleting outdated charts
[Pipeline] bat

C:\ProgramData\Jenkins\.jenkins\workspace\Final-Project_Scalefocus>helm install final-project-wp-scalefocus C:/Users/stani/Desktop/DevOpsAcademy/Final-Project-Assessment-for-Scalefocus-Academy/bitnami/wordpress -f C:/Users/stani/Desktop/DevOpsAcademy/Final-Project-Assessment-for-Scalefocus-Academy/bitnami/wordpress/values.yaml 
NAME: final-project-wp-scalefocus
LAST DEPLOYED: Tue May 16 16:00:26 2023
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
CHART NAME: wordpress
CHART VERSION: 16.1.2
APP VERSION: 6.2.0

** Please be patient while the chart is being deployed **

Your WordPress site can be accessed through the following DNS name from within your cluster:

    final-project-wp-scalefocus-wordpress.default.svc.cluster.local (port 80)

To access your WordPress site from outside the cluster follow the steps below:

1. Get the WordPress URL by running these commands:

   kubectl port-forward --namespace default svc/final-project-wp-scalefocus-wordpress 80:80 &
   echo "WordPress URL: http://127.0.0.1//"
   echo "WordPress Admin URL: http://127.0.0.1//admin"

2. Open a browser and access WordPress using the obtained URL.

3. Login with the following credentials below to see your blog:

  echo Username: user
  echo Password: $(kubectl get secret --namespace default final-project-wp-scalefocus-wordpress -o jsonpath="{.data.wordpress-password}" | base64 -d)
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS
```
8. Load the home page of the WordPress to see the final result.
- When we open the URL with the port, that i forwarded, which is: 3080, we can see a wordpress deployed by Jenkins,
![image](https://github.com/Stan15321/Final-Project-Assessment-for-Scalefocus-Academy/assets/109627707/bba03bd7-576e-4914-8091-df9548ccc64c)

- I uploaded everything that worked on in this repository.
- Thanks for reading my documentation!
