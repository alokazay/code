
# Random Number Without Random Generator (Epic Bad Code)

This project demonstrates how to generate a random number between 1 and 100 without using a random number generator. The goal is to write code in an obfuscated, "bad code" style, perfect for fun coding challenges or competitions!

## Overview

In this code, we use the current system time (milliseconds) to simulate randomness. By applying a modulo operation, we can get a "random" number between 1 and 100.

To add some fun, the code has been obfuscated, and the core logic is encoded using `btoa` for extra obscurity!

## Usage

### Clone the repository:

```bash
git clone https://github.com/your-username/repo-name.git
```

### Open the project in your favorite code editor.

### Run the code in a browser or Node.js environment.

To generate a random number between 1 and 100, execute the following code:

```javascript
eval(atob('Y29uc29sZS5sb2coRGF0ZS5ub3coKSUxMDArMSk='));
```

This will output a random number in the console.

## How It Works

- The code retrieves the current time in milliseconds using `Date.now()`.
- The time value is then divided by 100 using the modulo operator to get a remainder between 0 and 99.
- Adding 1 ensures the final result is between 1 and 100.
- The code is obfuscated by encoding the logic with `btoa`, and then decoded and executed using `eval` and `atob`.

## Warning

This code is written to be intentionally difficult to read and maintain. It is for fun and educational purposes only! **Do not use this approach in production environments.**

## Kubernetes Deployment

### Prerequisites:

1. **Kubernetes Cluster**: You should have a running Kubernetes cluster. This could be on any cloud provider (Google Cloud, AWS, Azure) or your own setup.
2. **kubectl**: Make sure you have `kubectl` installed and configured to interact with your Kubernetes cluster.
3. **Docker**: The project should be containerized using Docker.

### Step 1: Containerize the Application

First, create a `Dockerfile` to package the application:

```Dockerfile
# Use a Node.js base image
FROM node:16-alpine

# Set the working directory
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . .

# Install dependencies (if any)
RUN npm install

# Expose port 8080
EXPOSE 8080

# Run the app
CMD ["node", "app.js"]
```

Build and push the Docker image to your preferred container registry (e.g., Docker Hub):

```bash
docker build -t your-username/repo-name .
docker push your-username/repo-name
```

### Step 2: Create a Kubernetes Deployment

Create a Kubernetes deployment file `deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: random-number-app
spec:
  replicas: 3  # You can scale this up based on your VPS availability
  selector:
    matchLabels:
      app: random-number
  template:
    metadata:
      labels:
        app: random-number
    spec:
      containers:
      - name: random-number
        image: your-username/repo-name:latest
        ports:
        - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: random-number-service
spec:
  type: LoadBalancer
  selector:
    app: random-number
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080
```

### Step 3: Apply the Kubernetes Configuration

Once your Docker image is ready and the Kubernetes configuration is prepared, apply the configuration:

```bash
kubectl apply -f deployment.yaml
```

This will deploy your application on Kubernetes and expose it via a load balancer. You can now scale your application horizontally by increasing the number of replicas in the `replicas` field of the deployment.

### Step 4: Scaling and Monitoring

To scale your deployment manually, you can use the following command:

```bash
kubectl scale deployment random-number-app --replicas=5
```

This will run 5 instances of your app distributed across different VPS nodes in your cluster, with traffic balanced by the load balancer.

### Step 5: Accessing the Application

Once the load balancer is created, you'll get an external IP address for your service. You can access the application using this external IP.

Run the following command to get the IP:

```bash
kubectl get services
```

Look for the `EXTERNAL-IP` field in the output. You can then access the application by visiting the external IP in your browser.

---

## Security Testing

### Step 1: Linting for Security Issues

First, you should run a linter like **ESLint** with a security plugin to catch potential issues in your code. To install ESLint and the security plugin, run:

```bash
npm install eslint eslint-plugin-security --save-dev
```

Next, configure `.eslintrc.json`:

```json
{
  "extends": ["plugin:security/recommended"],
  "plugins": ["security"]
}
```

To run the linter:

```bash
npx eslint .
```

This will identify common vulnerabilities in the JavaScript code, such as potential XSS or SQL injection risks.

### Step 2: Running Security Scans with OWASP ZAP

You can run OWASP ZAP to perform security scans on your application. Follow these steps:

1. Install OWASP ZAP on your machine.
2. Once the application is running, use ZAP to scan your external IP (obtained from Kubernetes) for common security vulnerabilities like XSS, CSRF, and insecure headers.

### Step 3: Dependency Scanning

Use **npm audit** to check for known vulnerabilities in your project's dependencies:

```bash
npm audit
```

This will report any security vulnerabilities in the packages you have installed.

## Running Tests

### Unit Tests

Create a simple unit test for the random number generator using a testing framework like **Jest**:

1. Install Jest:

```bash
npm install jest --save-dev
```

2. Create a `random.test.js` file:

```javascript
const randomWithoutRandom = require('./app');  // Assuming your logic is in app.js

test('Random number is between 1 and 100', () => {
  const number = randomWithoutRandom();
  expect(number).toBeGreaterThanOrEqual(1);
  expect(number).toBeLessThanOrEqual(100);
});
```

3. Add the following script to your `package.json`:

```json
"scripts": {
  "test": "jest"
}
```

4. Run the tests:

```bash
npm test
```

### Integration Testing with Kubernetes

To test the deployed application in Kubernetes, you can create a set of integration tests that send requests to the external IP and verify the response.

Example:

```javascript
const axios = require('axios');

test('Service returns a random number between 1 and 100', async () => {
  const response = await axios.get('http://your-external-ip');
  const number = parseInt(response.data, 10);
  expect(number).toBeGreaterThanOrEqual(1);
  expect(number).toBeLessThanOrEqual(100);
});
```

You can run these tests directly against the deployed application to ensure the random number generator works correctly in the production environment.

---

## License

This project is licensed under the MIT License.
