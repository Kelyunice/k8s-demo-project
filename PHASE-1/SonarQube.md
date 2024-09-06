#!/bin/bash

# Update the system
echo "Updating system..."
sudo apt update && sudo apt upgrade -y

# Install OpenJDK 11
echo "Installing OpenJDK 11..."
sudo apt install -y openjdk-11-jdk

# Check if Java is installed
echo "Checking Java version..."
java -version

# Create a new user for SonarQube
echo "Creating SonarQube user..."
sudo useradd -r -s /bin/bash sonarqube

# Download SonarQube
echo "Downloading SonarQube..."
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.9.0.65566.zip

# Install unzip if not installed
echo "Installing unzip..."
sudo apt install -y unzip

# Extract the downloaded SonarQube package
echo "Extracting SonarQube..."
unzip sonarqube-9.9.0.65566.zip

# Move SonarQube to /opt directory
echo "Moving SonarQube to /opt/sonarqube..."
sudo mv sonarqube-9.9.0.65566 /opt/sonarqube
sudo chown -R sonarqube:sonarqube /opt/sonarqube

# Create a systemd service for SonarQube
echo "Creating systemd service file for SonarQube..."
sudo tee /etc/systemd/system/sonarqube.service > /dev/null <<EOF
[Unit]
Description=SonarQube service
After=network.target

[Service]
Type=forking
User=sonarqube
Group=sonarqube
ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop
Restart=always

[Install]
WantedBy=multi-user.target
EOF

# Enable and start the SonarQube service
echo "Enabling and starting SonarQube service..."
sudo systemctl enable sonarqube
sudo systemctl start sonarqube

# Allow SonarQube to run on port 9000 through the firewall
echo "Configuring firewall for SonarQube..."
sudo ufw allow 9000/tcp

# Output completion message
echo "SonarQube installation completed. Access it at http://<YOUR_VM_IP>:9000"

