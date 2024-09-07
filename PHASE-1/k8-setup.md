

# installation of minikube
```
  curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
  sudo install minikube-linux-amd64 /usr/local/bin/minikube
  minikube start
  sudo snap install --channel stable kubectl --classic
  snap install kubectx --classic
```
+++

#OPTION TWO
#===========

#-Kubeadm Setup  

# Step 1: Install required packages
```
sudo apt install -y apt-transport-https ca-certificates curl gpg
```

# Step 2: Create directory for keyrings (if needed) and download Kubernetes apt key
```
sudo mkdir -p -m 755 /etc/apt/keyrings
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
```

# Step 3: Add Kubernetes repository
```
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

# Step 4: Update package index and install Kubernetes packages
```
sudo apt update
sudo apt install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

# Optional Step 5: Enable kubelet service
```
sudo systemctl enable --now kubelet
```

# Step 6: Test the version of installed packages and display detailed success messages
```
echo "Kubelet version: $(kubelet --version)"
echo "Kubeadm version: $(kubeadm version -o short)"
echo "Kubectl version: $(kubectl version --client --short)"
echo "Installation of kubelet, kubeadm, and kubectl was successful!"
```


