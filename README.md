# homelab-casestudy
José Pablo Vargas first HomeLab case study annotations and documentation. Friday 26 September 2025.

Mini Enterprise IT Environment – HomeLab by José Pablo Vargas Chaves

Abstract

This case study meticulously documents the end-to-end process of designing, configuring, and deploying a personal homelab environment. The project aimed to simulate a small-scale enterprise IT infrastructure, encompassing network security, virtualization, containerization, and web application deployment. The journey reflects a commitment to hands-on learning, problem-solving, and the application of theoretical knowledge in real-world scenarios.

1. Introduction

In pursuit of practical experience and a deeper understanding of enterprise IT systems, I embarked on the creation of a homelab. The objectives were to:

Establish a secure network perimeter using pfSense.

Deploy virtualized machines to simulate client and server environments.

Implement containerization for application deployment.

Develop and host a Python-based web application.

This document serves as a comprehensive record of the methodologies employed, challenges encountered, and solutions implemented throughout the project.

2. System Design and Planning

The homelab architecture was designed to mirror a simplified enterprise IT environment. The primary components included:

pfSense: Serving as the network firewall and router, providing security and traffic management.

Ubuntu Server: Acting as the client machine, hosting various services and applications.

Docker: Facilitating containerization of applications for efficient deployment and management.

Portainer: Providing a user-friendly interface for managing Docker containers.

A network diagram was created to visualize the interconnections and data flow between these components, ensuring a coherent and scalable design.

3. Hardware and Software Selection

The homelab was built using the following hardware and software:

Host Machine: A Windows PC with virtualization support enabled in the BIOS.

Virtualization Platform: Oracle VM VirtualBox, chosen for its robustness and compatibility.

Operating Systems:

pfSense: A free, open-source firewall/router software.

Ubuntu Server 22.04 LTS: A stable and widely-used Linux distribution.

Additional Software:

Docker: For containerization of applications.

Portainer: For managing Docker containers via a web interface.

Firefox: For accessing web-based interfaces.

4. Implementation

4.1 pfSense Configuration

The pfSense virtual machine was configured with two network interfaces:

Adapter 1: NAT (WAN) – Providing internet access.

Adapter 2: Internal Network (LAN) – Isolated network for internal communication.

Upon installation, the LAN interface was assigned the IP address 192.168.100.1/24. The pfSense dashboard was accessible via https://192.168.100.1, allowing for further configuration.

<img width="1501" height="863" alt="image (1)" src="https://github.com/user-attachments/assets/67287434-f86f-423c-9ae6-ebee424fd7fe" />


4.2 Ubuntu Server Setup

The Ubuntu Server was installed as a virtual machine with the following network adapters:

Adapter 1: NAT – For internet connectivity.

Adapter 2: Internal Network (same name as pfSense LAN) – For internal communication.

Post-installation, the server was updated, and the default gateway was set to 192.168.100.1, enabling internet access through pfSense.

4.3 GUI Installation on Ubuntu

To facilitate ease of use, a lightweight graphical user interface (GUI) was installed:

sudo apt update
sudo apt install xfce4 xfce4-goodies xorg -y
startxfce4


Additionally, Firefox was installed to access web interfaces:

sudo apt install firefox -y

<img width="1509" height="867" alt="image (2)" src="https://github.com/user-attachments/assets/f615d6f2-e5b4-4ea4-aad4-b0a045e1858f" />



4.4 Docker and Portainer Deployment

Docker was installed to enable containerization:

sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker


Portainer was deployed to manage Docker containers:

sudo docker volume create portainer_data
sudo docker run -d -p 9000:9000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest

<img width="1508" height="867" alt="image (3)" src="https://github.com/user-attachments/assets/50f2feb8-7b73-47ff-a48c-4a855d144607" />



Portainer was accessible via http://<Ubuntu-IP>:9000, providing a graphical interface for container management.

<img width="1272" height="642" alt="image (4)" src="https://github.com/user-attachments/assets/7c1c058c-c94d-4987-8b5b-cefd5fe8949e" />


4.5 Python Web Application Deployment

A simple Python web application was developed using Flask. The application code was as follows:

from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello, this is Jose!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)


The corresponding Dockerfile was created:

FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY app.py .
EXPOSE 5000
CMD ["python", "app.py"]


The application was containerized and deployed:

sudo docker build -t python-webapp .
sudo docker run -d -p 5000:5000 python-webapp


The application was accessible via http://<Ubuntu-IP>:5000.

5. Challenges and Resolutions

Throughout the project, several challenges were encountered:

Network Connectivity Issues: Initially, the Ubuntu server could not access the internet. This was resolved by ensuring the correct network adapter settings in VirtualBox and verifying the default gateway configuration.

Docker Image Pull Failures: Attempts to pull Docker images resulted in errors. This was addressed by checking the network configuration and ensuring that pfSense allowed outbound traffic on the necessary ports.

Firewall Configuration: Proper firewall rules had to be established in pfSense to permit traffic between the WAN and LAN interfaces, as well as between the Ubuntu server and the internet.

Each challenge provided valuable learning experiences and reinforced the importance of meticulous configuration and troubleshooting.

6. Conclusion

The homelab project successfully simulated a small-scale enterprise IT environment, providing hands-on experience with networking, virtualization, containerization, and web application deployment. The process underscored the importance of careful planning, systematic implementation, and proactive problem-solving. The knowledge gained has been instrumental in enhancing my technical proficiency and understanding of enterprise IT systems.

7. Future Work

Future enhancements to the homelab may include:

Implementation of a Database Server: Deploying a database server such as MySQL or PostgreSQL to support data-driven applications.

Integration of Monitoring Tools: Utilizing tools like Prometheus and Grafana to monitor system performance and resource utilization.

Deployment of Additional Services: Implementing services like Nginx for reverse proxying, Jenkins for continuous integration, and Kubernetes for container orchestration.

These additions will further enrich the homelab environment and provide opportunities to explore advanced IT concepts and technologies.

8. References

pfSense Documentation: https://docs.netgate.com/pfsense/en/latest/

Ubuntu Server Guide: https://ubuntu.com/server/docs

Docker Documentation: https://docs.docker.com/

Portainer Documentation: [https://www.portainer.io/documentation](https://docs.portainer.io/start/architecture)
