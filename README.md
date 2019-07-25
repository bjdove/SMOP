# SMOP
Synthea Mass Opiod Project
Dallas K8s Project - Public Market Team

This project is designed to show how kubernetes-based technologies can provide value when deploying, running, and managing applications. The application itself shows how a life sciences company can improve Oncology Research and Clinical Trials through improved engagement with patients using IoT devices. This application uses several IBM Cloud cognitive and data services with Node-Red at the core of the workload flow. The application is deployed into IKS and ICP for a multi-cloud deployment scenario to achieve resiliency & availability. Monitoring & management is accomplished using IBM's Multi-cloud manager.
Getting Started

The following documentation will discuss the details of how this project was deployed and the benefits of the various technologies used.
Prerequisites

The only tangible prerequisite for this project is a paid IBM Cloud account. For successful deployment, it is also important to have an understanding of cloud native application development, kubernetes systems, and other cloud technologies.

An IBM Cloud account can be obtained here
Technology Used

    IBM Kubernetes Service - IBM Public Cloud Kubernetes Service
    IBM Cloud Private - IBM Private Cloud Kubernetes Service
    RedHat Openshift - Red Hat Private Cloud Kubernetes Service
    Multi-Cloud Manager - IBM tool to manage multi-cluster environments
    IBM Node Red - Tool to manage & develop microservice workflows
    IBM Cloudant - Scalable JSON Database
    IBM Watson Assistant - IBM Watson Chatbot Service
    IBM Watson Text to Speech - Watson Service to translate written text to audio output
    IBM Watson Language Translator - Watson Service to translate written text from one language to another
    IBM Key Protect - IBM Cloud Key Management Service
    IBM Cloud Object Storage - Resilient, Scalable, Affordable Object Storage
    IBM Continuous Delivery - IBM Cloud tool to provide continuous delivery, integration, and other DevOps best practices

Project Architecture

alt text
Part 1: The Application

Info on the application flow itself & Node-Red goes here.

Give an example

Part 2: Kubernetes Clusters & Multi-Cloud Manager

ICP, IKS, OCP, and MCM content goes here.

Give an example

Part 3: Additional Services

Security, PVC to IBM COS, LDAP, etc goes here
Persistent Volume: Cloud Object Storage

We enabled our IKS cluster to write to IBM Cloud Object Storage. Writing to a persistent volume such as COS enables the data to still be available, even if the container, the worker node, or the cluster is removed. This is critical for stateful apps, core business data, data that must be available due to legal requirements, auditing, and data that must be accessed and shared across app instances.

This is done by creation of a .yaml file that configures a persistent volume claim. This persistent volume claim has information about the cloud object storage bucket to be created that allows IBM Cloud to automate the creation of the bucket and communication of data to the bucket.

kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: kube-cos-pvc
  namespace: prod
  annotations:
    ibm.io/auto-create-bucket: "true"
    ibm.io/auto-delete-bucket: "false"
    ibm.io/bucket: "ikstestbucketburgess"
    ibm.io/object-path: ""
    ibm.io/secret-name: "cos-write-access"
    ibm.io/endpoint: "https://s3.private.us-south.cloud-object-storage.appdomain.cloud"
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 8Gi 
  storageClassName: ibmc-s3fs-flex-regional

The following screenshot indicates successful creation of the PVC: alt text

and the following screenshot shows the bucket that we created in IBM Cloud: alt text

Instructions to create IBM Cloud Object Storage as a persistent volume in your own cluster can be found here.
Wrap up

Anything else we need a section for?
License

This project is licensed as the intellectual property of IBM Corporation.
Acknowledgments

    Developed by Adam Torres, Alen Dranikov, Bruno Neves, Dayal Sachdev, Gary Stone, Kaleb Mekonnen, and William Burgess
    Thank you to our K8s enablement leaders!
