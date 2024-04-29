---
created: 2024-01-28T19:51:47 (UTC -03:00)
tags: []
source: https://blog.bitsrc.io/implementing-the-sidecar-pattern-in-nodejs-2ec3954fe9b6
author: Chameera Dulanga
---

# Implementing The Sidecar Pattern in a Microservices Based Application | Chameera Dulanga | Bits and Pieces

> ## Excerpt
> Build loosely coupled microservices by offshoring cross cutting concerns onto a sidecar with the help of the sidecar pattern in microservices, and learn how to build your very own sidecar implementation using Node.js

---
## Approach 02: Service Mesh Implementation

Implementing the Sidecar pattern using a service mesh like Istio involves several steps. Here’s a detailed guide to get you started with [Istio](https://istio.io/).

**Prerequisites**

-   A Kubernetes cluster where Istio can be deployed.
-   [kubectl](https://kubernetes.io/docs/reference/kubectl/) installed and configured to communicate with your Kubernetes cluster.
-   [Istio CLI (istioctl)](https://istio.io/latest/docs/ops/diagnostic-tools/istioctl/) installed on your local machine.

## **Step 1: Install Istio on Your Kubernetes Cluster**

Download the latest Istio version using the below curl command:

```
<span id="4f89" data-selectable-paragraph="">curl -L https://istio.io/downloadIstio | sh -</span>
```

Add istioctl to your path:

```
<span id="c661" data-selectable-paragraph="">export PATH="$PATH:/path-to-istio/bin"</span>
```

Install Istio on the cluster:

```
<span id="ff25" data-selectable-paragraph="">istioctl install --set profile=demo -y</span>
```

For Istio to automatically inject sidecars, enable sidecar injection in your Kubernetes namespace:

```
<span id="6d72" data-selectable-paragraph="">kubectl label namespace &lt;your-namespace&gt; istio-injection=enabled</span>
```

## **Step 2 — Create the Microservice**

Create a basic Node.js application as the primary microservice.

```
<span id="ef52" data-selectable-paragraph="">// Creating a directory for the app<br>mkdir myapp &amp;&amp; cd myapp<br><br>// Initializing the app<br>npm init</span>
```

Then, update a new file named **app.js** with the microservice code.

```
<span id="d118" data-selectable-paragraph="">const http = require('http');<br><br>const server = http.createServer((req, res) =&gt; {<br>    console.log("Received a request");<br>    res.writeHead(200);<br>    res.end("Hello, World!");<br>});<br><br>server.listen(3000, () =&gt; {<br>    console.log("Main Service running on port 3000");<br>});</span>
```

**Note:** Since we are using a service mesh (Istio) to implement the sidecar pattern, You don’t need to manually create a separate sidecar service. Istio automatically injects the sidecar proxy ([Envoy Proxy](https://www.envoyproxy.io/)) into your pods when deployed into the Kubernetes cluster.

## **Step 3: Dockerize the Services**

Create Dockerfiles for both the main application and the sidecar.

```
<span id="926d" data-selectable-paragraph="">FROM node:14<br>WORKDIR /app<br>COPY package.json package-lock.json ./<br>RUN npm install<br>COPY . .<br>EXPOSE 3000<br>CMD ["node", "app.js"]</span>
```

Then, build and push the Docker images using the below commands

```
<span id="b48e" data-selectable-paragraph="">docker build -t yourdockerhubusername/nodejs-app:latest .<br>docker push yourdockerhubusername/nodejs-app:latest</span>
```

## **Step 4: Create Kubernetes Deployment and Service Manifests**

Create two files named:

1.  **nodejs-deployment.yaml**
2.  **nodejs-service.yaml**

These files will be used to define the deployment and service manifest configurations.

The Deployment YAML file defines how your application should be deployed in the Kubernetes cluster. It specifies the number of replicas (instances of your application), the Docker image to use, and other configuration details.

```
<span id="252f" data-selectable-paragraph="">apiVersion: apps/v1<br>kind: Deployment<br>metadata:<br>  name: nodejs-app<br>spec:<br>  replicas: 2<br>  selector:<br>    matchLabels:<br>      app: nodejs-app<br>  template:<br>    metadata:<br>      labels:<br>        app: nodejs-app<br>    spec:<br>      containers:<br>      - name: nodejs-app<br>        image: yourdockerhubusername/nodejs-app:latest<br>        ports:<br>        - containerPort: 3000</span>
```

The Service YAML file defines how to access the pods your Deployment manages. It provides a single point of access to your application.

```
<span id="0fd6" data-selectable-paragraph="">apiVersion: v1<br>kind: Service<br>metadata:<br>  name: nodejs-app<br>spec:<br>  selector:<br>    app: nodejs-app<br>  ports:<br>  - protocol: TCP<br>    port: 80<br>    targetPort: 3000</span>
```

Once created, deploy these 2 configurations to Kubernetes:

```
<span id="f094" data-selectable-paragraph="">kubectl apply -f nodejs-deployment.yaml<br>kubectl apply -f nodejs-service.yaml</span>
```

## **Step 5: Configure Istio**

You need to configure Istio to manage and control the traffic flow to and from our service. This step involves creating Istio-specific custom resource definitions like **VirtualServices** and **DestinationRules** to define advanced routing capabilities and traffic policies.

Create a file named **nodejs-virtualservice.yaml** to configure how requests are routed to different service versions within the mesh. You can use it for canary deployments, A/B testing, or path-based routing.

```
<span id="52be" data-selectable-paragraph="">apiVersion: networking.istio.io/v1beta1<br>kind: VirtualService<br>metadata:<br>  name: nodejs-app<br>spec:<br>  hosts:<br>  - nodejs-app<br>  http:<br>  - route:<br>    - destination:<br>        host: nodejs-app</span>
```

Create a file named **nodejs-destinationrule.yaml** to define policies that apply after routing has occurred. You can use it to configure settings like load balancing, circuit breaking, and TLS settings for traffic towards a specific service.

```
<span id="62e4" data-selectable-paragraph="">apiVersion: networking.istio.io/v1beta1<br>kind: DestinationRule<br>metadata:<br>  name: nodejs-app<br>spec:<br>  host: nodejs-app<br>  trafficPolicy:<br>    loadBalancer:<br>      simple: ROUND_ROBIN</span>
```

Finally, apply the configurations using the below commands:

```
<span id="756b" data-selectable-paragraph="">kubectl apply -f nodejs-virtualservice.yaml<br>kubectl apply -f nodejs-destinationrule.yaml</span>
```

## Approach 03: Using Independent Components

Finally, you can explore modern build systems like [Bit](https://bit.dev/) to build your microservices and implement your custom Sidecar solution. Bit lets you design, develop and build independent Bit components that can be versioned and maintained in an isolated space.

By doing so, you’re able to work on different parts of the project independently without being coupled to a monolith.

All you have to do is create two applications (APIs) using Bit using this guide:

And you can include your Sidecar logic as independent Node components:

Afterward, you can work on your microservice as you’d normally do!

To gain an indepth insight on using Bit to build microservices, check this guide out:

## Wrapping Up

The Sidecar Pattern offers a practical solution for enhancing microservices without adding complexity to the application code.

> By implementing this pattern, developers can ensure their microservices are more maintainable, scalable, and efficient, and less coupled.

I hope you found this article helpful.

Thank you for reading!
