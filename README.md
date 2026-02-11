## Kubernetes Architecture

## Kubernetes components

Kube APiServer

Kube Scheduler

Kube Controller Manager

ETCD

## Pre-requisites

Infrastructure provisioning using the below script,

A Resource Group

An AKS Kubernetes Cluster

An Image container registry

A SQL Server

A SQL Database

## Create and Run in Cloud Shell a bash script with the following
#!/bin/bash

REGION="westus"

RGP="day11-demo-rg"

CLUSTER_NAME="day11-demo-cluster"

ACR_NAME="day11demoacr"

SQLSERVER="day11-demo-sqlserver"

DB="mhcdb"


#Create Resource group
az group create --name $RGP --location $REGION

#Deploy AKS
az aks create --resource-group $RGP --name $CLUSTER_NAME --enable-addons monitoring --generate-ssh-keys --location $REGION

#Deploy ACR
az acr create --resource-group $RGP --name $ACR_NAME --sku Standard --location $REGION

#Authenticate with ACR to AKS
az aks update -n $CLUSTER_NAME -g $RGP --attach-acr $ACR_NAME

#Create SQL Server and DB
az sql server create -l $REGION -g $RGP -n $SQLSERVER -u sqladmin -p P2ssw0rd1234

az sql db create -g $RGP -s $SQLSERVER -n $DB --service-objective S0

## Change the Firewall settings of the SQL server
Under Networking, allow Azure services and resources to access this server

## Setup Azure DevOps Project

Make sure the below Azure DevOps extensions are installed and enabled in your organization
- Replace Token
- Kubernetes extension

Once the infra is ready, go to dev.azure.com --> Project --> repos and import the below git repo, which has the source code and pipeline code - https://github.com/piyushsachdeva/MyHealthClinic-AKS

## Build and Release Pipeline
You can create your pipeline by following along the video or editing the existing pipeline. The below details need to be updated in the pipeline:
Azure Service connection
Token pattern
Pipeline variables
The Kubectl version should be the latest in the release pipeline
Secrets should be updated in the deployment step
ACR details in the pipeline should be updated
