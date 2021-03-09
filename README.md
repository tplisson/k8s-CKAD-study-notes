# Certified Kubernetes Application Developer (CKAD) Study Guide  
This is my study guide for the Certified Kubernetes Application Developer (CKAD) exam.  
https://www.cncf.io/certification/ckad/  

<p align="center">
  <img src="CKAD-logo.png">
</p>
<br/>

**2Hrs | Cost $300 | [Online Exam](https://training.linuxfoundation.org/certification/certified-kubernetes-administrator-ckad/)**
**K8s version 1.20 (Jan 22, 2021)**
 
<br/>

## CKAD Curriculum  

Updated exam curriculum for v1.20  
https://github.com/cncf/curriculum/


**Domain**	| **Weight**
------- | -------------
**1. Core Concepts** | 13%
• Understand Kubernetes API primitives |  
• Create and configure basic Pods |  
**2. Multi-Container Pods** | 10%  
• Understand Multi-Container Pod design patterns (e .g. ambassador, adapter, sidecar) |  
**3. Pod Design** | 20%
• Understand Deployments and how to perform rolling updates |  
• Understand Deployments and how to perform rollbacks |  
• Understand Jobs and CronJobs |  
• Understand how to use Labels, Selectors, and Annotations || 
**4. State Persistence** | 8%  
• Understand PersistentVolumeClaims for storage |  
**5. Configuration** | 18%
• Understand ConfigMaps |  
• Understand SecurityContexts |  
• Define an application’s resource requirements |  
• Create & consume Secrets |  
• Understand ServiceAccounts |  
**6. Observability** |  18%
• Understand LivenessProbes and ReadinessProbes |  
• Understand container logging |  
• Understand how to monitor applications in Kubernetes |  
• Understand debugging in Kubernetes |  
**7. Services & Networking** |  13%
• Understand Services |  
• Demonstrate basic understanding of NetworkPolicies |  

<br/>


## Kubernetes Documentation
- Main Documentation page:  
https://kubernetes.io/docs/  
  
- Cheat Sheet:  
https://kubernetes.io/docs/reference/kubectl/cheatsheet/  

- `kubectl` Command Reference
https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands  
  
- One-page API Reference for Kubernetes v1.20  
https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.20/  
  

<br/>

## CKAD Training  

- Great course from "a Cloud Guru":  
https://learn.acloud.guru/course/certified-kubernetes-application-developer/  
by William Boyd

## CKAD Lab Exercises

- killer.sh_ 
  https://killer.sh/ckad
  - not free but worth it
  - 29.99€ for 2 CKA sessions of the same mock exam  
  
- Dimitris-Ilias Gkanatsios  
https://github.com/dgkanatsios/CKAD-exercises

- Benjamin Muschko
https://github.com/bmuschko/ckad-prep

- stretchcloud  
https://github.com/stretchcloud/cka-lab-practice

<br/>

## CKAD Exam Tips
- **Time management is paramount!**
  - You have 15-20 performance-based tasks to perform in 2 hours, so an average of 6-8 min per task. Some tasks are easy but some others will take much more time.
  - Get all questions done: 
    - Make sure to go through all 15-20 tasks
    - Get all the easiest tasks done first!
  - Never get stuck: 
    - If a task is seems difficult or it's taking you more than 2-3 min, then flag it and keep moving to get any easy task done first. 
    - Then go back to each of the flagged tasks afterwards.
<br/>

- Setup kubectl autocomplete 
  - ```source <(kubectl completion bash)```
  - ```alias k=kubectl```
  - ```complete -F __start_kubectl k```
<br/>  

- Setup an shell variable to easily generate resource specs in YAML format:
  - ```export do="--dry-run=client -o yaml"```
  - then we can run:
  - ```k run pod1 --image=nginx $do```
<br/>  

- Use `kubectl` [shortnames](https://kubernetes.io/docs/reference/kubectl/overview/#resource-types): 
  - `no` `po` `ns` `deploy` `svc` `ing` `ds` `netpol` `pv` `pvc` `sa` `cm` `ep` `sc` ...
<br/>

- Use [imperative commands](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create) whenever possible:
  - E.g. use: `k run -h`, `k create deploy -h`, `k expose -h`...
  - instead of: `k apply -f <filename.yaml>`
<br/>  

- Use `kubectl explain` to [list the fields of supported API resources](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#explain)
<br/>

- Use `kubectl replace` to [replace an existing resource with some updated specs](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#replace):
  - This saves you time compared to using `k delete` and `k apply -f <filename>`
<br/>

- Use `-w` (i.e. `--watch=true`) to  start watching updates to a particular object.
  - `k get po -w` to watch your pods
<br/>