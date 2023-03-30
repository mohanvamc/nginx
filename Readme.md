# 1. First cd alpine-nginx go through readme and build the image 

# 2. create nginx statefulset with pvc and resource limits

```bash
 Step 1: Define the StatefulSet object and its metadata.
 Step 2: Specify the replica count and label selector for the StatefulSet.
 Step 3: Define the Pod template for the StatefulSet, which includes the container specification, volume mounts, and resource limits.
 Step 4: Define the PersistentVolumeClaim template for the StatefulSet, which specifies the storage resource requirements.
 ```

# 3. create github action to deploy above stateful set

```bash
 This GitHub action will be triggered on push events to the main branch
 Step 1: Checkout the code from the repository
 Step 2: Build the Docker image and push it to the Docker Hub registry
 Step 3: Deploy the Kubernetes StatefulSet to the cluster
 Step 4: Set the KUBECONFIG environment variable to authenticate with the Kubernetes cluster
````