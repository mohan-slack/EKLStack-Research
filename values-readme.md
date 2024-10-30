#1. Namespace and Replica Counts

    `namespace: sgprho-prd-az1-elastic-search-rjfw4e`

  o	This sets the namespace for the Elasticsearch deployment.

      `masterreplicaCount: 3`

  o	Number of master node replicas.

      `clientreplicaCount: 1`

  o	Number of client node replicas.

      `datareplicaCount: 12`

  o	Number of data node replicas.
  
2. Master Service
•	masterService: "elasticsearch-master"
o	Name of the master service.
3. Security Configuration
•	security:
o	certificate: prudential-elasticsearch.jks
o	password: quantexa-prudential
o	Specifies the security certificate and password.
•	secret:
o	KEYSTORE_PASSWORD: quantexa-prudential
o	keystorePassword: elastic
o	Defines the keystore password.
4. Configuration
•	config:
o	SECURITY_CERTIFICATE: prudential-elasticsearch.jks
o	KEYSTORE_PASSWORD: quantexa-prudential
o	Specifies security-related configurations.
5. Elasticsearch Image
•	esimage:
o	repository: elasticsearch
o	pullPolicy: IfNotPresent
o	tag: 7.16.2
o	master: name: elasticsearch-master
o	client: name: elasticsearch-client
o	data: name: elasticsearch-data
o	Details about the Elasticsearch Docker image.
6. Storage Configuration
•	esstorage:
o	name: storage
o	master: name: prudential-aks-master-pvc
o	client: name: prudential-aks-client-pvc
o	data: name: prudential-aks-data-pvc
o	storageclassname: prudential-aks-managed-disk
o	capacity: storage: 512Gi
o	hostPath: path: /data/prod/1
o	Defines storage settings, including persistent volume claims and capacity.
7. Deployment Configuration
•	esdeployment:
o	master: name: elasticsearch-master
o	client: name: elasticsearch-client
o	data: name: elasticsearch-data
o	Names for master, client, and data deployments.
8. Resource Limits and Requests
•	esresources:
o	limits: cpu: 4, memory: 32Gi
o	requests: cpu: 4, memory: 32Gi
•	esdataresources:
o	limits: cpu: 8, memory: 64Gi
o	requests: cpu: 8, memory: 64Gi
o	CPU and memory limits and requests for Elasticsearch nodes.
9. Environment Variables
•	env:
o	javaoptions: "-Xms16g -Xmx16g"
o	Java options for Elasticsearch.
10. Labels
•	eslabels:
o	app: elasticsearch
o	env: prd
o	master: role: master
o	client: role: client
o	data: role: data
o	Labels for identifying Elasticsearch components.
11. ConfigMaps
•	esconfigmap:
o	master: name: elasticsearch-master-config
o	client: name: elasticsearch-client-config
o	data: name: elasticsearch-data-config
•	esconfigmap2:
o	master: name: elasticsearch-master-config2
o	client: name: elasticsearch-client-config2
o	data: name: elasticsearch-data-config2
o	ConfigMaps for master, client, and data nodes.
12. Secrets
•	essecret:
o	master: name: elasticsearch-master-secret
o	client: name: elasticsearch-client-secret
o	data: name: elasticsearch-data-secret
o	Secrets for master, client, and data nodes.
13. Service Configuration
•	esservice:
o	type: ClusterIP
o	clientport: 9200
o	transport: 9300
o	master: name: elasticsearch-master
o	client: name: elasticsearch-client
o	data: name: elasticsearch-data
o	Service type and ports for client and transport.
14. Pod Disruption Budget
•	espdb:
o	enabled: true
o	minAvailable: 1
o	master: name: elasticsearch-master
o	client: name: elasticsearch-client
o	data: name: elasticsearch-data
o	Ensures a minimum number of available pods.
15. Autoscaling
•	esautoscaling:
o	enabled: false
o	minReplicas: 1
o	maxReplicas: 100
o	targetCPUUtilizationPercentage: 75
o	targetMemoryUtilizationPercentage: 75
o	master: name: elasticsearch-master, deploymentname: es-master-hpa-deployment
o	client: name: elasticsearch-client, deploymentname: es-client-hpa-deployment
o	data: name: elasticsearch-data, deploymentname: es-data-hpa-deployment
o	Autoscaling settings, including min and max replicas and target utilization percentages.
16. Ingress Configuration
•	esingress:
o	enabled: true
o	name: elasticsearch
o	host: elastic-prd-sgprho-prd-az1-elastic-search-rjfw4e.aks-lb1-sgprho-prd-az1-y9g5l0.pru.intranet.asia
o	port: 9200
o	annotations:
	kubernetes.io/ingress.class: "nginx"
	nginx.org/client-max-body-size: "0"
	nginx.ingress.kubernetes.io/proxy-body-size: "0"
	nginx.ingress.kubernetes.io/proxy-connect-timeout: "3600"
	nginx.ingress.kubernetes.io/proxy-read-timeout: "3600"
	nginx.ingress.kubernetes.io/proxy-send-timeout: "3600"
o	Ingress settings, including host, port, and annotations for NGINX.
17. Secret and Config Folders
•	secretFolder:
o	enabled: true
o	mountPath: /usr/share/elasticsearch/config/
o	master: name: elasticsearch-master-config
o	client: name: elasticsearch-client-config
o	data: name: elasticsearch-data-config
•	esconfigFolder:
o	enabled: true
o	mountPath: /usr/share/elasticsearch/configs
o	master: name: elasticsearch-master-configfolder
o	client: name: elasticsearch-client-configfolder
o	data: name: elasticsearch-data-configfolder
o	Mount paths for secrets and configuration files.
18. Global Tolerations
•	global:
o	applications:
	tolerations:
	enabled: true
•	tolerations:
o	enabled: true
o	key: "workload"
o	operator: "Equal"
o	effect: "NoSchedule"
o	value: "elasticapps"
o	Tolerations for scheduling pods on specific nodes.
This configuration file is designed to deploy a robust and secure Elasticsearch cluster with detailed settings for replicas, security, storage, resources, and deployment. It ensures proper resource allocation, security measures, and scalability options for the Elasticsearch nodes.
==================================
===================================
Application Overview
![image](https://github.com/user-attachments/assets/91b7e115-1191-4db3-babe-c202b278ae61)
![image](https://github.com/user-attachments/assets/e4650745-431b-4d63-8823-9d982e573e4a)

data ingestion to app
![image](https://github.com/user-attachments/assets/4f488b2a-a233-4431-9b03-defce30f5882)
Quantexa Multi Branch CI/CD Jenkins Pipeline
![image](https://github.com/user-attachments/assets/68c435f4-5d78-46c1-a346-8aaa221d267b)




