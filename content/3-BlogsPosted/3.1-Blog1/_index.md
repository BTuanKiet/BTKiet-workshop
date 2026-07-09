---
title: "Blog 1"
date: 2026-07-08
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# BETTER TOGETHER: AMAZON EKS AUTO MODE AND ISTIO AMBIENT MESH

As microservices architectures continue to grow from a few services to hundreds of workloads, organizations face two major challenges:

- Managing Kubernetes infrastructure efficiently with minimal operational overhead.
- Securing service-to-service communication while maintaining high performance and scalability.

AWS addresses these challenges by combining **Amazon EKS Auto Mode** and **Istio Ambient Mesh**, allowing teams to automate infrastructure management while simplifying service mesh operations.

## Amazon EKS Auto Mode

Amazon EKS Auto Mode simplifies Kubernetes cluster management by automatically handling the underlying compute infrastructure.

Key capabilities include:

- Automatic node provisioning, scaling, patching, and updates.
- Built-in support for the Bottlerocket operating system, optimized for containers.
- AWS-managed Kubernetes add-ons such as Amazon VPC CNI, kube-proxy, CoreDNS, and Amazon EBS CSI Driver.
- Intelligent compute optimization powered by Karpenter to select the most suitable EC2 instances and reduce infrastructure costs.

With EKS Auto Mode, developers can focus on deploying applications instead of managing cluster infrastructure.

## Istio Ambient Mesh

Istio Ambient Mesh introduces a sidecarless service mesh architecture that reduces resource consumption while maintaining secure communication between services.

The architecture consists of three major components:

### ztunnel

A lightweight Layer 3/4 proxy running on each node that provides:

- Mutual TLS (mTLS)
- Service identity
- Layer 4 authorization
- TCP telemetry

### Waypoint Proxy

An optional Layer 7 proxy used only when advanced application-aware features are required, including:

- HTTP routing
- Traffic management
- Retries
- Circuit breaking
- HTTP authorization policies

### HBONE Protocol

HBONE (HTTP-Based Overlay Network Environment) enables secure mTLS communication between ztunnel and Waypoint Proxy without requiring sidecars for every Pod.

## Benefits of Combining Both Solutions

Using Amazon EKS Auto Mode together with Istio Ambient Mesh provides several advantages:

- Reduced operational overhead for Kubernetes infrastructure.
- Secure service-to-service communication with automatic mTLS.
- Lower resource consumption through sidecarless architecture.
- Intelligent infrastructure scaling and cost optimization.
- Simplified deployment using tools such as Terraform and Helm.

## Architecture Overview

![EKS Auto Mode and Istio Ambient Mesh Architecture](/images/3-BlogsPosted/3.1-Blog1/c-170-1.jpg)

## Reference

- [AWS Containers Blog – Better Together: Amazon EKS Auto Mode and Istio Ambient Mesh](https://aws.amazon.com/blogs/containers/better-together-amazon-eks-auto-mode-and-istio-ambient-mesh/)
