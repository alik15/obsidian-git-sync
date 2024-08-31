
## KillerCoda
Has simple CKAD and CKA exercises(very close to actual exam)

[https://killercoda.com/](https://killercoda.com/)

*runs in your web browser and is free to access*

## Github exercises
Really really good for getting fast at the commands and has some things that killer coda misses.

There isn't an equivalent version for CKA that is as useful.

[https://github.com/dgkanatsios/CKAD-exercises](https://github.com/dgkanatsios/CKAD-exercises)

*Has to be run locally or in other free online environments like kodekloud or killercoda*
  


## Mumshad Mannambeth's Udemy Course

Focus more on the kodekloud labs and mock exams and just go over udemy’s syllabus once for CKAD. For CKA, if you have taken CKAD before, you can skip the parts that were in CKAD
and move on to the uncovered materials. Also provides you with kodekloud labs for practice.

_has to be bought_ 

## CKAD

focuses more on the application part of Kubernetes  which means that objects will be will be more on objects related to applications for example

1. Pods
	- Running commands inside pods
	- exposing pods

1. Deployments
    - Scaling 
    - Modifying
2. Jobs
    
4. Cronjobs
    
5. Ingress

7. Persistent Volumes

9. Services

  

Most of the exam is deploying these objects with given specifications. Make sure you know how to check if an object creation is successful or not.

Note: there are other parts of the exam, this is just the focus 

  
  

## CKA
 - Focuses more on the administrative part of Kubernetes. Which means you will also have to focus on parts like roles, service accounts including things you learning in CKAD.
 - Another important CKA part is troubleshooting. This requires knowing how to access and use logs and logging tools.

  
  
## General exam tips

  

- Imperative commands arent just helpful, they are necessary for the exam. If there is an imperative command you can use to solve a problem, you need to use it. 



- Identify what the general questions for each object would be.
	
	For example:
	
	-  create a pod that runs a single command
	- scale a deployment 
	- create a cronjob
	- create service accounts
	- create roles and rolebindings
	    

- Get fast at the commands, you should be able to create simple pods, deployments, config-maps as much as possible through the command line and then modifying them later to save time.

- Make sure you know what the `YAML` for objects looks like

- For example if asked to add a container port to the `YAML`, do not go to the documentation page, you should be able to do it yourself

	You only need to go to the documentation for a few objects written below, since you wont be able to create them through the `kubectl` cli tool.
	
	1. Persistent Volumes and Persistent Volume Claims 
	2. Network Policies

- Create a process for troubleshooting since you will be asked to correct incorrect configurations, for that you need to be familiar with logs, log locations, object logs, services, end-points, linux logs etc.

- Develop a process of troubleshooting/checks for each object: 
	for a deployment:
	 with an exposed service you will need to check the service, the endpoint, the deployment and finally all objects related to the deployment such as config maps, persistent volumes, secrets etc.
	
	for a persistent volume:
		Check the persistent volume logs and the `Bound` status, check the pod logs. 
  
- Make use of `--dry-run=client`, bash variable exports etc to save time


- Remember the way to pass the exam isn't to learn everything rather it is an exercise in time management and confidence on the cli. This will also mean learning vim and other linux utilities such as `grep`, `cat`, `pipes(|)`.


You will be ready for the exam once:

1. You can do all the killercoda exercises and github exercises without looking at the documentation expect for the mentioned objects/scenarios.
2. Complete the lighting labs, mock exams and killer.sh exercises in half the allotted time.