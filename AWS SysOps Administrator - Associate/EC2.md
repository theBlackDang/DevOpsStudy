
The blueprint of the instance is defined by:
1. OS image
2. Instance type
3. Keypair
4. Network
5. Storage

If you want to access EC2, run command as below

ssh -i (keypair.pem) (username)@(Public EC2 IP)

If we want to change instance type (it only works on EBS backed instance), 
Stop the instance
Change instance type
Start instance


Network:
1. SR-IOV: higher bandwidth PPS (packet per second), lower latency. 2 options: Elastic Network Adapter (ENA) up to 100 Gbs, Legacy (Intel 82599 VF) upto 10 Gbs
2. Elastic Fabric Adapter (EFA): improve ENA for HPC, Linux only, great for inter-node communications, tightly coupled workloads, leverage message passing interface (MPI) standard, bypass the underlying Linux OS to provide low-latency, reliable transport
* Noted these network is defined by instance type


Placement group:
EC2 instances placement strategy, the strategy is defined by using placement group

1. Cluster: same rack same AZ, if the rack crashed, the whole thing crashed but it fast
2. Spread: different AZ, different hardware, limited to 7 instances per placement group
3. Partition: 1 partition can have many EC2 instances and in different AZ, each partition represents a rack in AWS, up to 7 partitions per AZ, the instances in a partition do not share racks with other instances in other partitions, EC2 instances get access to the partition information as metadata

Shutdown behaviors: there are 2 options, stop and terminate

Termination protection: enabled or disabled

EC2 launch troubleshooting
1. Instance limit exceed: if you get this error, it means that you have reached your limit of max number of vCPUs per region: 
   + On-demand instances limit are set on a per-region basis
   + example: if you run on-demand (A, C, D, H, I, M, R, T, Z) instance types you'll have 64 vCPUs (default)
   + Resolution: either launch the instance in a different in a different region or request AWS to increase your limit of the region
   + vCPU based limits only apply to run On-demand instances and spot instances
2. Insufficient instance capacity: if we get this AWS doesn't have enough on demand resources for you to launch EC2 instances (AZ)
  + Wait for few minutes before requesting again
  + If the request is to big, break it down to create separated instances
  + If urgent, request for lower instance type and resize later

3. Instance terminate immediately
+ Reach EBS limit 
+ EBS snapshot is corrupted
+ EBS volume is encrypted but you don't have KMS to decrypt

EC2 SSH troubleshooting
1. key.pem file is get from ec2 creation
2. username is correct
3. chmod 400 permission for the key.pem file
4. Check for network
5. Overload

EC2 connect not required key.pem is because
![[EC2 Instance connect.png]]

For EC2 instance connect, inbound rule must have ip prefix of that region so that these EC2 instances will be available for EC2 instance connect

EC2 instances purchasing options
on demand: pay per second, short workload, no long term commitment, highest cost but no upfront payment  (use whenever we like, pay for what we use)
reserved: long workloads, convertible reserved instances, discount a lot bigger in compare with on-demand (planning ahead and plan to stay for a long time we may get a good discount)
saving plans: (1-3 years): long workload, commitment to amount of usage (pay certain amount for certain period and use any tyoe)
spot instances: short workload, can lose instances, less reliable, we can lose c2 instances if our price is less than current spot price (allow people to bid for empty rooms, the highest bidder get to use, all the rest is being kicked out)
dedicated host: book entire physical server, control instance placement, the most expensive option  (book entire thing)
dedicated instances: no other customers will share your hardware, instances run on hardware dedicated to me 
capacity reservation: reserve capacity in a specific AZ for any duration, we will have access to EC2 capacity when we need it (book things with full price when we don't use it)