# Project 2: Deploying Microservices on Amazon EKS

## Project Overview

This project demonstrates how I deployed a containerized WordPress application on Amazon Elastic Kubernetes Service (EKS) using Helm, Kubernetes, and AWS infrastructure.

The goal was to learn how to deploy, manage, monitor, and troubleshoot containerized applications in a cloud environment.

## Technologies Used

- **Amazon EKS:** Managed Kubernetes environment
- **Amazon EC2:** Worker nodes running application pods
- **Kubernetes:** Container orchestration
- **Helm:** Application deployment and configuration
- **WordPress:** Containerized web application
- **MariaDB:** WordPress database
- **Amazon EBS:** Persistent application and database storage
- **AWS Load Balancer:** Public access to WordPress
- **Horizontal Pod Autoscaler (HPA):** CPU-based pod autoscaling
- **Siege:** Application load testing
- **Git and GitHub:** Version control and project documentation

## Project Architecture

The application was deployed on an EKS cluster named `enterprise-cluster` in the AWS `us-east-1` region.

The environment included three EC2 worker nodes, a WordPress deployment, a MariaDB deployment, persistent EBS storage, and an AWS Load Balancer.

I used Helm to manage the WordPress deployment and configured an HPA with a minimum of one pod, a maximum of five pods, and a CPU utilization target of 50%.

## Deployment Process

1. Installed and configured AWS CLI, eksctl, kubectl, and Helm.
2. Created an Amazon EKS cluster with three worker nodes.
3. Verified that all Kubernetes worker nodes were ready.
4. Created and configured a Helm chart for WordPress.
5. Deployed MariaDB and configured Kubernetes Secrets for database credentials.
6. Configured persistent storage using Amazon EBS.
7. Installed WordPress using Helm.
8. Exposed WordPress through an AWS Load Balancer.
9. Configured a Horizontal Pod Autoscaler.
10. Performed load testing using Siege.
11. Troubleshot application memory and storage problems.
12. Verified that WordPress was accessible through the Load Balancer.

## Load Testing Results

I used Siege to simulate 100 concurrent users accessing WordPress for approximately five minutes.

The test recorded 239 successful transactions and 786 failed transactions, revealing performance problems.

The HPA was configured and monitored, but the test did not trigger an increase in pod replicas.

## Troubleshooting

### WordPress Memory Issue

During load testing, WordPress experienced an `OOMKilled` error.

I increased the container memory limit from `512Mi` to `1Gi` using Helm and verified that Kubernetes applied the new configuration.

### EBS Storage Conflict

During the Helm upgrade, a new WordPress pod could not attach the existing EBS volume because it was already attached to another node.

I resolved the conflict by scaling the deployment down, allowing the storage to be released, and scaling the application back up.

After troubleshooting, WordPress was running with zero restarts, and the website was accessible.

## Production Improvements

For a production deployment, I would:

- Use Amazon RDS for a managed database.
- Consider Amazon EFS for shared WordPress storage across nodes.
- Configure HTTPS using AWS Certificate Manager.
- Implement Amazon CloudWatch monitoring and alerts.
- Improve health checks, autoscaling, security, and high availability.

## Project Cleanup

After completing the deployment and testing, I removed the application resources and persistent storage from Kubernetes to reduce unnecessary AWS charges.

The live environment is no longer maintained as part of this project.

## Lessons Learned

This project strengthened my understanding of Amazon EKS, Kubernetes deployments, Helm, persistent storage, load testing, autoscaling configuration, and troubleshooting containerized applications.