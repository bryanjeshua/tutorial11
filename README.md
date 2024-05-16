# Reflection 1
1. The logs change noticeably when the application is exposed as a Service. Initially, they show the app's setup and serving on port 8080. After exposure, accessing the app through the proxy triggers additional log activity, indicating incoming HTTP requests. This illustrates the Service effectively directing traffic to the app pod, evident in the growing number of log entries per access.

2. The `-n` option in `kubectl get` helps filter resources by namespace. Without it, the command retrieves items from the default namespace, where user-deployed resources reside. Conversely, specifying `-n kube-system` narrows down the search to the `kube-system` namespace, which mainly hosts system-level components. Consequently, user-created pods and services won't appear in the output of the latter command.

# Reflection 2
1. The Rolling Update strategy gradually updates pods one by one. Kubernetes ensures availability throughout the process. Old pods are maintained while deploying new ones. This minimizes downtime and ensures zero-downtime updates. In contrast, the Recreate strategy terminates all existing pods before creating new ones. This results in downtime during deployment. The application becomes unavailable until the new pods are up and running. However, it guarantees a clean, complete transition to the new version without mixing old and new pods.

2. Deploying the Spring Petclinic REST using the Recreate deployment strategy involves specifying the strategy in the deployment manifest file. The `type` is set to `"Recreate"` under `spec.template.spec.strategy`. 

3. Below are snippets of YAML manifest files for a deployment with the Recreate strategy:
   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: spring-petclinic-rest
   spec:
     replicas: 3
     strategy:
       type: Recreate
     selector:
       matchLabels:
         app: spring-petclinic-rest
     template:
       metadata:
         labels:
           app: spring-petclinic-rest
       spec:
         containers:
         - name: spring-petclinic-rest
           image: docker.io/springcommunity/spring-petclinic-rest:4.0
           ports:
           - containerPort: 9966
   ```

4. Kubernetes manifest files offer several benefits. They enable the specification of the desired state of applications and infrastructure declaratively. Manifest files can be version-controlled using Git, facilitating change tracking, version rollback, and collaboration. Automating deployment with manifest files using `kubectl apply` ensures consistency and reduces the risk of errors. Additionally, it simplifies the reproducibility of deployments across various environments.