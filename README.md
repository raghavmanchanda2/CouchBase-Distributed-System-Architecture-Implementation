# Overview

This project explores the implementation of key distributed system design(DSD) concepts, including replication, faulttolerance, containerization, and loadbalancing. Utilizing technologies like Couchbase for database management, Docker forcontainerization, Nginx for webserving, reverse proxying, load balancing, and AWS services such as Elastic Load Balancing(ELB) and Elastic Compute Cloud(EC2),we aim to effectively implement these DSD features for handling large-scale data.

# Introduction

This project explores the deployment of a distributed system application, leveraging the synergies among Couchbase and Docker. Emphasizing crucial aspects of distributed system design, such as load balancing and fault tolerance, our methodology integrates the advantages of containerization with the robust high availability and resilience offered by Docker, EC2 and ELB. This combination establishes a system configuration that is not only highly available but also resilient to faults. Our objective is to showcase a holistic approach for effectively managing extensive data in a distributed setting, addressing prevalent challenges in distributed systems, including traffic distribution, system stability, and data consistency.

# Preliminaries

## Couchbase
Couchbase is a NoSQL, distributed database designed for high-performance and scalable applications. With its support for both key-value and document-oriented data models, Couchbase is relevant in distributed designs by providing seamless scalability, data distribution, and flexible schema to accommodate the evolving needs of modern, distributed architectures, making it suitable for applications with dynamic and growing data requirements.

## Docker
Docker is a platform for developing, shipping, and running applications in containers. It is highly relevant in distributed designs as it offers containerization, allowing applications and their dependencies to be packaged together. Docker facilitates seamless deployment across various environments, enhances scalability, and promotes consistency in distributed systems, making it a valuable tool for building and managing containerized applications in distributed architectures.

# Steps

##Setting Up the Couchbase Environment

We began by pulling the Couchbase server image from the Docker hub using the command docker pull couchbase. Subsequently, we launched multiple Couchbase nodes as Docker containers, each mapped to unique host ports to avoid conflicts and ensure proper node communication. The nodes are named from couchbase node1 to couchbase node6 with docker run command. After the containers were running, we retrieved their IP addresses using docker inspect, which is crucial for cluster configuration and inter-node communication.

## Cluster Configuration

With the nodes up and running, we proceeded to create two clusters, each comprising three nodes. This was achieved through the Couchbase web console, where we configured the clusters and ensured they were functioning correctly.

## Data Management

Once the clusters were operational, we focused on data management tasks. We created a bucket in the first cluster and populated it with documents, amounting to a data chunk of 60 MB, to simulate a realistic data distribution scenario. An empty bucket was then created in the second cluster to prepare for data replication.

## Replication Process

We established a replication reference from Cluster 1 to Cluster 2 to initiate one-way data replication\cite{xdcr-docs}. This was verified by observing the data presence in Cluster 2's bucket post-replication. Further, we set up a bi-directional replication reference, allowing data to be replicated back to Cluster 1 when new data was introduced into Cluster 2.

## Data Sharding

In the scenario of fetching data from a Wikipedia document stored in Couchbase, which comprises roughly half a gigabyte, the utilization of data sharding sharding-docs in distributed system design is evident. By distributing all Wikipedia items equally among the nodes in Cluster 1 while occupying comparable disk space on each node, data sharding is implemented effectively. This approach involves breaking down the large data set of Wikipedia items into smaller, manageable shards, and then distributing these shards evenly across multiple nodes within Cluster 1. This strategy not only enhances performance by allowing parallel processing of queries across distributed shards but also ensures scalability as the system can accommodate larger volumes of data. Furthermore, in Cluster 2, where only a single node exists, the data from all the Wikipedia items is replicated onto this lone node, illustrating the principle of replication in data distribution. This replication strategy ensures fault tolerance and high availability of data, even in scenarios where only a single node is present, showcasing the flexibility and adaptability inherent in the design of distributed systems using data sharding techniques.

## Fault Tolerance

In the context of Couchbase's distributed system design, the implementation of fail over illustrates a fundamental aspect of fault tolerance and data redundancy. When a node within Cluster 1 fails, employing fail over mechanisms ensures continuity and data availability by redistributing the data originally stored on the failed node evenly among the remaining operational nodes. This redistribution process prevents any loss of data and maintains query accessibility even in the event of node failure. Subsequently, employing the Add Back-Full Recovery process to recover and rebalance the previously failed node facilitates the restoration of its functionality within the cluster. During this recovery phase, data from other nodes is replicated back to the recovered node, decreasing the data load on the source nodes while reintegrating the recovered node into the cluster's data distribution scheme. This fail over and recovery mechanism in Couchbase exemplifies the resilience and adaptability of distributed systems by ensuring data availability, load balancing, and system stability even in the face of node failures or disruptions.

## References

[1] Couchbase, Inc.CouchbaseDocumentation.Availableat: https://docs. couchbase.com/ 
[2] Couchbase, Inc, Cross Data Center Replication (XDCR) Avaiable at: https://docs.couchbase.com/server/current/learn/ clusters-and-availability/xdcr-overview.html 
[3] Couchbase, Inc, Cross Data Center Replication (XDCR) Avaiable at: https://couchbase.com/resources/concepts/ what-is-database-sharding/ 
[4]Docker, Inc.DockerDocumentation.Availableat: https://docs.docker. com/




