# Kubernetes and Minikube

## What is Minikube?

Minikube is a lightweight Kubernetes implementation that creates a virtual machine on your local machine and deploys a simple cluster with a single node. It is mainly used for testing, development, and learning Kubernetes without requiring a full-scale cluster setup.

### Key Features of Minikube:

- Supports multiple Kubernetes versions
- Provides a built-in load balancer
- Allows enabling and disabling add-ons like dashboard, Ingress, etc.
- Supports various container runtimes such as Docker, containerd, and CRI-O
- Compatible with Windows, macOS, and Linux

## Important Kubernetes YAML Files

Kubernetes uses YAML files for configuration and deployment of applications. Below are key YAML files used in Kubernetes:

### 1. Namespace YAML

A **Namespace** is used to create a virtual cluster within a Kubernetes cluster, allowing isolation of resources.

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: my-namespace
```

### 2. Deployment YAML

A **Deployment** is used to define the desired state of an application and manage replicas.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
  namespace: my-namespace
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: nginx
        ports:
        - containerPort: 80
```

### 3. ConfigMap YAML

A **ConfigMap** is used to store non-sensitive configuration data in key-value format that can be used by pods.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-config
  namespace: my-namespace
data:
  database_url: "mysql://db.example.com"
  log_level: "debug"
```

### 4. Secret YAML

A **Secret** is used to store sensitive data like passwords, API keys, and TLS certificates securely.

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
  namespace: my-namespace
type: Opaque
data:
  username: dXNlcm5hbWU=  # Base64 encoded
  password: cGFzc3dvcmQ=
```

### 5. Service YAML

A **Service** is used to expose the application inside or outside the cluster.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
  namespace: my-namespace
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP
```

### 6. Horizontal Pod Autoscaler (HPA) YAML

A **Horizontal Pod Autoscaler (HPA)** automatically scales the number of pod replicas based on CPU or memory utilization.

```yaml
apiVersion: autoscaling/v2beta2
kind: HorizontalPodAutoscaler
metadata:
  name: my-hpa
  namespace: my-namespace
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-deployment
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
```

### 7. Volumes YAML

A **Volume** is used to store persistent data for pods.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
  namespace: my-namespace
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

---

# Python OOP Concepts

## Class and Object

- **Class**: A blueprint for creating objects.
- **Object**: An instance of a class.

```python
class Car:
    def __init__(self, brand, model):
        self.brand = brand
        self.model = model
    
    def display_info(self):
        print(f"Car: {self.brand} {self.model}")

car1 = Car("Toyota", "Corolla")
car1.display_info()
```

## 4 Pillars of Object-Oriented Programming

### 1. Encapsulation

Encapsulation is the bundling of data and methods that operate on that data into a single unit (class). It restricts direct access to some components.

```python
class BankAccount:
    def __init__(self, balance):
        self.__balance = balance  # Private variable
    
    def deposit(self, amount):
        self.__balance += amount
    
    def get_balance(self):
        return self.__balance

account = BankAccount(1000)
account.deposit(500)
print(account.get_balance())  # Output: 1500
```

### 2. Inheritance

Inheritance allows a class to inherit attributes and methods from another class.

```python
class Vehicle:
    def __init__(self, brand):
        self.brand = brand
    
    def show_brand(self):
        print(f"Brand: {self.brand}")

class Car(Vehicle):
    def __init__(self, brand, model):
        super().__init__(brand)
        self.model = model
    
    def show_model(self):
        print(f"Model: {self.model}")

car = Car("Toyota", "Camry")
car.show_brand()
car.show_model()
```

### 3. Polymorphism

Polymorphism allows methods in different classes to be called through the same interface.

```python
class Animal:
    def make_sound(self):
        pass

class Dog(Animal):
    def make_sound(self):
        return "Bark"

class Cat(Animal):
    def make_sound(self):
        return "Meow"

animals = [Dog(), Cat()]
for animal in animals:
    print(animal.make_sound())
```

### 4. Abstraction

Abstraction hides implementation details and only exposes the necessary parts of an object.

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):
        return 3.14 * self.radius * self.radius

circle = Circle(5)
print(circle.area())  # Output: 78.5
```
