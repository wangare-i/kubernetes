Kubernetes ConfigMaps & Secrets: Why Mounting is Better Than Environment Variables
When managing configurations and sensitive data in Kubernetes, you can either use environment variables or mount ConfigMaps and Secrets as volumes. While environment variables may seem simpler, mounting is often the better choice. Here's why.
ConfigMaps & Secrets: What They Solve
A ConfigMap stores non-sensitive configuration data like database ports, URLs, and feature flags. A Secret stores sensitive information like passwords and API keys.
Creating a ConfigMap
You created a ConfigMap to store your database port:-
```apiVersion: v1
kind: ConfigMap
metadata:
  name: test-cm
data:
  db-port: "3306" ```
Applying it:
kubectl apply -f cm.yml
Verifying:
kubectl get cm
Creating a Secret
You also created a Secret for sensitive credentials:
kubectl create secret generic test-secrets --from-literal=db-port="3306"
Why Mounting is Better Than Environment Variables
1. You Can Update Configurations Without Restarting Pods
If you inject a ConfigMap or Secret as an environment variable, the value is set only when the Pod starts. Any changes require a Pod restart.
Mounting it as a volume allows updates to be reflected dynamically.
2. Security & Least Privilege Access

Mounted Secrets remain in-memory, not written to the container's environment, reducing the risk of exposure.

With RBAC, you can restrict access to mounted Secrets, whereas environment variables can be accessed by any process in the Pod.

3. Sharing Configurations Between Containers
Multiple containers in a Pod can access the same mounted ConfigMap or Secret without duplicating environment variables.
Using Mounted ConfigMaps & Secrets in Deployment
Here's your deployment YAML, updated to mount the ConfigMap:
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sample-python-app
spec:
  replicas: 2
  selector: 
    matchLabels:
      app: sample-python-app
  template:
    metadata:
      labels:
        app: sample-python-app
    spec:
      containers:
      - name: python-app
        image: wangare/python-sample:v2
        volumeMounts:
         - name: db-connection
           mountPath: /opt
        ports:
        - containerPort: 8000
      volumes:
       - name: db-connection
         configMap:
          name: test-cm
After applying:
kubectl apply -f deployment.yml
You can verify that the ConfigMap is mounted correctly:
kubectl exec -it <pod-name> -- /bin/bash
ls /opt
cat /opt/db-port  # Should output 3306
For Secrets, the approach is similar:
volumes:
        - name: secret-volume
          secret:
            secretName: test-secrets
Conclusion
Mounting ConfigMaps and Secrets as volumes is the best practice in Kubernetes. It enhances security, allows dynamic updates, and improves flexibility when managing configurations. While environment variables might seem convenient, the risks outweigh the benefits in most cases.
Got any thoughts? Let's discuss in the comments! 🚀
