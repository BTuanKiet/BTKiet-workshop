---
title: "Blog 2"
date: 2026-07-08
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# DEPLOYING MULTI-TURN RL INFRASTRUCTURE FOR AMAZON NOVA ON AMAZON SAGEMAKER HYPERPOD

As AI agents become capable of solving complex tasks involving multiple interactions, traditional reinforcement learning approaches are no longer sufficient. AWS introduces a reference architecture for deploying **Multi-Turn Reinforcement Learning (RL)** infrastructure using **Amazon SageMaker HyperPod** and **Amazon Nova** to efficiently train AI agents that must reason across multiple turns.

## Why Multi-Turn RL?

Traditional reinforcement learning methods such as RLHF mainly optimize individual responses. However, AI agents often need to complete tasks involving multiple steps, including:

- Calling external APIs
- Querying databases
- Handling system errors
- Using multiple tools during a conversation

The quality of each action depends on future decisions, making Multi-Turn RL a better training approach for these scenarios.

## Core Architecture

The solution follows an event-driven architecture consisting of three major components:

### Amazon SageMaker HyperPod

Amazon SageMaker HyperPod provides GPU clusters (P5 instances) that perform model inference and training. During training, the model is optimized using the **Group Relative Policy Optimization (GRPO)** algorithm based on rewards collected from the environment.

### AWS Fargate (Amazon ECS)

Amazon ECS running on AWS Fargate hosts the **Reward Environment**. In the AWS demonstration, the environment is a Wordle game that evaluates the model's responses and returns reward signals.

### Amazon Nova Forge SDK

Amazon Nova Forge SDK coordinates communication between the model and the reward environment while maintaining the conversation state across multiple interaction turns.

## Two-Phase Deployment

The infrastructure is deployed using two distinct phases.

### Phase 1 – Infrastructure Deployment

AWS CDK provisions the foundational infrastructure, including:

- Amazon VPC
- Amazon EKS
- Amazon ECS
- Amazon S3
- AWS Step Functions

This infrastructure only needs to be deployed once.

### Phase 2 – Training Runtime

When a `.jsonl` training dataset is uploaded to Amazon S3, the workflow automatically starts.

AWS Step Functions provisions the required training resources, launches the HyperPod training job, coordinates the reward environment, and releases the compute resources after training finishes, minimizing GPU idle time and reducing operational costs.

## Benefits

This architecture provides several advantages:

- Fully automated training pipeline
- Event-driven workflow
- Efficient GPU resource utilization
- Reduced infrastructure cost
- Easy monitoring through AWS Step Functions
- Scalable design for large-scale reinforcement learning workloads

## Architecture Diagram

![Multi-Turn RL Infrastructure for Amazon Nova on SageMaker HyperPod](/images/3-BlogsPosted/3.2-Blog2/ML-20450-1.png)

## Reference

- AWS Machine Learning Blog:
  https://aws.amazon.com/blogs/machine-learning/deploying-multi-turn-rl-infrastructure-for-amazon-nova-on-amazon-sagemaker-hyperpod/