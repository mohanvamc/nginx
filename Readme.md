# 1. First cd alpine-nginx go through readme and build the image 

# 2. create nginx statefulset with pvc and resource limits

```bash
 Step 1: Define the StatefulSet object and its metadata.
 Step 2: Specify the replica count 
 Step 3: volume mounts, and resource limits.
 Step 4: pvc and storage
 ```


# 3. create github action to deploy above stateful set

```bash
 This  GitHub action will be triggered on push events to the main branch
 Step 1: Checkout the code from the repository
 Step 2: Build image and push to Docker Hub -- Not required as we already image in docker hub
 Step 3: Deploy the k8s-nginx-statefulset.yml to the cluster
````