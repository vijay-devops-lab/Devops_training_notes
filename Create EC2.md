(Elastic compute cloud) 
### What EC2 does

EC2 lets you:
- Create and run **virtual machines (VMs)**    
- Choose **CPU, RAM, storage, and OS**    
- Scale servers **up or down** as needed    
- Pay only for what you use   

---
### Key components of EC2

1. **Instance**    
    - A virtual server        
    - Example: `t2.micro`, `t3.medium`
        
2. **AMI (Amazon Machine Image)**    
    - Template for the instance        
    - Contains OS (Ubuntu, Amazon Linux, Windows) + preinstalled software
        
3. **Instance Types**    
    - Define hardware        
    - Example:        
        - `t` → general purpose            
        - `c` → compute optimized            
        - `m` → memory optimized
            
4. **Storage**    
    - **EBS** (Elastic Block Store) → like a hard disk        
    - **Instance store** → temporary storage
        
5. **Security**    
    - **Key pair** → SSH login        
    - **Security Groups** → firewall rules        

---

### What EC2 is used for

- Hosting **web applications**    
- Running **backend APIs**    
- Deploying **databases*    
- **DevOps tools** (Jenkins, Docker, Kubernetes nodes)    
- Machine learning workloads    

---

### Example

Instead of buying a physical server:
- You launch an EC2 instance    
- Install software (Nginx, Node.js, Java, etc.)    
- Access it via **SSH**    
- Stop or terminate it when done    

---

### EC2 vs Local Computer

| Local PC             | EC2                 |
| -------------------- | ------------------- |
| Fixed hardware       | Scalable hardware   |
| Must buy             | Pay per hour/second |
| Limited availability | 24/7 global access  |



## EC2 types
-> Free tier
-> ON-demand
-> reserved 
-> dedicated
-> spot

### Free tier :
 AWS give free credit we can use that and access the resource 

### ON-demand:
  A flexible, pay-as-you-go compute option in Amazon EC2 where you pay by the second or hour for virtual servers (instances) without long-term commitments or upfront costs.

### Reserved :
  we can reserve the instance for long term like 1 year or more than year.

### Dedicated:
   It is an Amazon EC2 virtual machine that runs on hardware physically isolated for a single customer

### Spot:
  it is used for testing or used for less time use and cost effictive



## To connect the instance 

- Windows - RDP - 3389 - .pem (privacy enhance mail)
- Linux - ssh - 22 - .pem or .ppk

