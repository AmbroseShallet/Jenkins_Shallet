**Method 1: Install Jenkins Directly on Windows**
Download Jenkins:

Go to the Jenkins download page.
Download the Windows installer (usually a .msi file).
Install Jenkins:

Run the downloaded installer and follow the installation prompts.
Choose the installation directory and the components you want to install.
Start Jenkins:

After installation, Jenkins should start automatically. If not, you can start it from the Start menu.
By default, Jenkins runs on http://localhost:8080.
Unlock Jenkins:

Open your web browser and go to http://localhost:8080.
You will be prompted to unlock Jenkins using an initial admin password.
The password can be found in the file located at C:\Program Files (x86)\Jenkins\secrets\initialAdminPassword.
Set Up Jenkins:

Follow the setup wizard to install recommended plugins and create your first admin user.

**Method 2: Run Jenkins in Minikube**
Start Minikube:

Open a command prompt and start Minikube if it’s not running:
bash
Copy code
minikube start
Set Up Jenkins Deployment:

Create a YAML file for Jenkins deployment (e.g., jenkins-deployment.yaml):

apiVersion: apps/v1
kind: Deployment
metadata:
  name: jenkins
spec:
  replicas: 1
  selector:
    matchLabels:
      app: jenkins
  template:
    metadata:
      labels:
        app: jenkins
    spec:
      containers:
      - name: jenkins
        image: jenkins/jenkins:lts
        ports:
        - containerPort: 8080
        - containerPort: 50000

        
Deploy Jenkins:

Apply the deployment file:

bash
kubectl apply -f jenkins-deployment.yaml

Expose Jenkins Service:

Create a service to expose Jenkins:

apiVersion: v1
kind: Service
metadata:
  name: jenkins
spec:
  type: NodePort
  ports:
    - port: 8080
      targetPort: 8080
      nodePort: 30000
  selector:
    app: jenkins
    
Apply the service configuration:
bash
kubectl apply -f jenkins-service.yaml

Access Jenkins:

Get the Minikube IP:
bash
minikube ip

Access Jenkins using http://<minikube-ip>:30000.
Follow the unlocking and setup process similar to the direct installation.
