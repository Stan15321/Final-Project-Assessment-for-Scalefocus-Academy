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
- To install the chart, we can open PowerShell and there we need to use helm as a packet manager for kubernetes with the following command: "helm install wp1 wordpress/"
- We use the command "helm install wp1 wordpress/" to deploy the WordPress Helm chart onto the Kubernetes cluster. This command installs the specified chart with the release name wp1. The WordPress chart contains the necessary components required to run a WordPress application on a Kubernetes cluster, such as a database, a WordPress web server, and a service to expose the application to the Internet. The helm tool simplifies the deployment of these components by providing a standardized way to package, configure, and deploy the chart onto a Kubernetes cluster:
![image](https://github.com/Stan15321/Final-Project-Assessment-for-Scalefocus-Academy/assets/109627707/1268cb83-c65c-42f7-bcbd-8692b4995b0a)
- Also we can see that when we get the services, "wp1-wordpress" type is not LoadBalancer and it is "ClusterIP":
![image](https://github.com/Stan15321/Final-Project-Assessment-for-Scalefocus-Academy/assets/109627707/99b78532-cc24-4dbd-8052-219cb44538e6)

4. Now with that being said, we can set up a local port forwarding in order to login:
![image](https://github.com/Stan15321/Final-Project-Assessment-for-Scalefocus-Academy/assets/109627707/ac7a20fb-b35b-41dc-9ad3-0155d8cc66df)
- The command "kubectl port-forward --namespace default svc/wp1-wordpress 3080:80" sets up a local port forwarding from port 3080 on the local machine to port 80 on the service wp1-wordpress in the Kubernetes cluster. This is useful because it allows you to access the WordPress application running in the cluster from your local machine. By forwarding traffic from your local machine to the Kubernetes cluster, you can interact with the WordPress application as if it were running on your local machine.
- We will see this login page:
![image](https://github.com/Stan15321/Final-Project-Assessment-for-Scalefocus-Academy/assets/109627707/76b7d6dc-ba04-4e0c-8f7f-7e4252319a98)

- In order to login, we need to add to the URL: http://localhost:3080/login
![image](https://github.com/Stan15321/Final-Project-Assessment-for-Scalefocus-Academy/assets/109627707/601d5dab-4cd7-4a56-bf73-7e00c691c80c)

- From here we can see that the username is user and in order to see the password, we need to execute this command in order to decode and see our password.
![image](https://github.com/Stan15321/Final-Project-Assessment-for-Scalefocus-Academy/assets/109627707/e640d6a3-4178-46ea-ab81-a149650339ce)

- We will see the Dashboard after we login:
![image](https://github.com/Stan15321/Final-Project-Assessment-for-Scalefocus-Academy/assets/109627707/b9e3a82b-cf57-40f2-8ec6-dbf6c7249b94)



5. Create a Jenkins pipeline that checks if wp namespace exists, if it doesn’t then it creates one.
Checks if WordPress exists, if it doesn’t then it installs the chart.
4. Name the Helm Deployment as: final-project-wp-scalefocus.
5. Deploy the helm chart using the Jenkins pipeline.
6. Load the home page of the WordPress to see the final result.
