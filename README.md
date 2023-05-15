Deploy a WordPress on Kubernetes (using Minicube) with Helm and
automation with Jenkins.
Prerequisites:
1. Install the necessary tools: Minicube, Helm and Jenkins.
- I already have Minikube and Jenkins, i will install only helm.
- To install Helm on Windows, you can follow these steps:

Download the latest Helm binary for Windows from the Helm GitHub repository: https://github.com/helm/helm/releases

Extract the downloaded binary to a folder in your PATH environment variable. For example, you can extract the binary to the C:\bin folder.

Add the folder containing the Helm binary to your PATH environment variable by following these steps:

Open the Control Panel and click on System and Security.
Click on System and then click on Advanced system settings.
In the System Properties window, click on the Environment Variables button.
In the Environment Variables window, under System Variables, select the Path variable and click on Edit.
Click on New and add the path to the folder containing the Helm binary.
Click on OK to close all windows.
Test the Helm installation by running the following command in the Command Prompt:

2. Separate repo in your GitHub Profile named: Final Project Assessment for Scalefocus Academy
Requirement for the Project Assessment:


1. Download Helm chart for WordPress. ( Bitnami chart:
https://github.com/bitnami/charts/tree/main/bitnami/wordpress )
2. In values.yaml, you need to change line 543 from type: LoadBalancer to type: ClusterIP ( Hint: there
will be one more problem when deploying. Resolve it. )
3. Create a Jenkins pipeline that checks if wp namespace exists, if it doesn’t then it creates one.
Checks if WordPress exists, if it doesn’t then it installs the chart.
4. Name the Helm Deployment as: final-project-wp-scalefocus.
5. Deploy the helm chart using the Jenkins pipeline.
6. Load the home page of the WordPress to see the final result.
7. Explain the project directly in a README.md file in your project repo.
BONUS POINTS:
- Instead of using Minicube, consider using a different Kubernetes flavor of your choice, like k3s, k8s,
microK8s for bonus points during the grading.
8. Send the URL of your Project repo on email to ana.zjovanovska@scalefocus.com &
kiro.velkovski@scalefocus.com titled “Final Project Assessment for Scalefocus Academy”.
