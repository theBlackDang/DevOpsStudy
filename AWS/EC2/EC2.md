














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

EC2 spot instance request
can get up to 90% compared to On-demand

define max spot price and the instance while current spot price is lower than your max

. the hourly spot price varies based on offer and capacity 

. if the current spot price is bigger than your max, we can choose to stop or terminate EC2

the strategy is spot block

. block spot instance during a specific time frame without interruptions

. in rare situations, the instance can be reclaimed

not great for databases uses or cirtical jobs

terminate spot instance 

1. cancel spot request to make sure that no new instances will be launched by AWS
2. Terminate instances

spot fleet 

spot fleets = set of spot instances + optional on-demand instances

the spot fleet will try to meet the target capacity with price constraints

+ define possible launch pools: instance type, OS, AZ

+ multiple launch pools, so that the fleet can choose

+ spot fleet stops launching instances when reaching capacity or max cost

strategies to allocate spot instances:

+ lowestprice: get instances from the pool with lowest price

+ diversified: distributed accross all pools (great for availability, short workload)

+ capacityOptimized: pool with optimal capacity for the number of instances

+ priceCapacityOptimized (recommended): poolls with highest capacity available, then select the pool with the lowest price (best choice for most workloads)

- Spot fleet allows us to automatically request spot instances with the lowest price 

burstable instances:

+ When the machine wants to execute something it requires a spike in load, it can burst and CPU can be very good because it will use burst credits, when all credits are gone, the CPU becomes bad. When the machine stop bursting, credits are accumulated overtime

+ burstable instances can be amazing to handle unexpected traffic and getting the insurance that it will be handled correctly

+ if your instance consistently runs low on credit, you need to move to a different kind of non-burstable instance

+ when the credit are exhausted the measured CPU ultilization drops

+ it is possible to have an "un-limited burst credit balance"

+ we pay extra cost if we overuse our credit balance but we don't lose our performance

+ if average CPU usage over 24-hour period exceeds the baseline, the instance is billed for addtional usage per vCPU per hour

+ be watchful for CPU health

elastic IP

+ when we stop and start Ec2 instance, it changes public IP

+ if we need to have a fixed public IP, we will need and elastic IP

+ an elastic IP is a public IPv4 you own as long as you don't delete it

+ attach to 1 instance at a time

+ ability to remap accross instances

+ we don't pay for the elastic IP if it attached to a server

+ with an elastic IP address, we can mask failure by rapidly remapping to another instance

+ we can only have 5 elastic IP in your account (we can ask AWS to increase that)

How to avoid using EI

+ random public IP and assign DNS name to it

+ use load balancer with a static hostname

Cloudwatch metrics for EC2

+ AWS provided metrics (AWS pushed them): basic monitoring (metrics are collected at a 5 minutes interval), detailed (paid, metrics are collected at a 1 minute interval), includeds CPU, network, disk and status check metrics

+ custom metrics (yours to push): basic resolution (1 min resolution), high resolution (1 sec resolution), include RAM, application level metrics, make sure the IAM permissions on the EC2 instance role are correct

EC2 included metrics:

+ CPU: CPU ultilization + credit usage / balance

+ network: in and out

+ status check: instance status (EC2 VM), system status (check the underlying hardwae)

+ disk: read / write for OPS / Bytes (only for instance store)

+ RAM is not included in the EC2 metrics

Unified cloudwatch agent 

+ for VMs

+ collect additional system-level metrics such as RAM, processes, used disk space, etc

+ collect logs to send to cloudwatch logs, no logs from inside your Ec2 instance will be sent to cloudwatch logs without using an agent

+ centralize configuration using ssm

+ iam permissions are correct

unified cloudwatch agent - procstat plugin

+ collect metrics and monitor system utilization of individual processes

+ support both Linux and Windows servers

ex: amount of time the process uses CPU, amount of mem the process uses

+ select which processes to monitor by

pid_file: name of process identification number (PID) files they create

+ select which processes to monitor by

pid_file: name of process identification number (PID) files they created

exe: process name that match string you specify (RegEx)

pattern: command lines used to start the processes (RegEx)

+ Metric collected by procstat plugin begins with "procstat" prefix (procstat_cpu_time...)

Status check:

automated checks to identify hardware and software issues

+ System status checks:

. Monitor problems with AWS systems (software/hardware issues on the physical host, loss of the system power...)

. Check personal health dashboard for any scheduled critical maintenance by AWS to your instance's host

. resolution: stop and start the instance (instance migrated to a new host)

+ Instance status checks

. Monitors software/netwok configuration of your instance (invalid network configuration, exhausted mem...)

. resolution: reboot the instance or change instance configuration

Status checks - CW metrics and recovery

+ Cloudwatch metrics ( 1 min interval)

. StatusCheckFailed_System

. StatusCheckFailed_Instance

. StatusCheckFailed (for both)

+ Option 1: Cloudwtch Alarm 

. recover EC2 instance with the same private/public IP, EIP, metadata and placement group

. send notifications using SNS

+ Option 2: Auto Scaling Group

. Set min/max/desired to recover an instance but won't keep the same private and elastic IP

EC2 Instance Status Checks - MUST KNOW
These can be the focus of 2 to 3 questions at the exam, therefore know the differences:
 
SYSTEM status checks
System status checks monitor the AWS systems on which your instance runs
Problem with the underlying host. Example: 
Loss of network connectivity
Loss of system power
Software issues on the physical host
Hardware issues on the physical host that impact network reachability
Either wait for AWS to fix the host, OR
Move the EC2 instance to a new host = STOP & START the instance (if EBS backed)
 
INSTANCE status checks
Instance status checks monitor the software and network configuration of your individual instance
Example of issues
Incorrect networking or startup configuration
Exhausted memory
Corrupted file system
Incompatible kernel
Requires your involvement to fix
Restart the EC2 instance, OR
Change the EC2 instance configuration

EC2 Hibernate

1. Stop (data on disk is kept intact in the next start), terminate (any EBS volumes (root) also set-up to be destroyed is lost)

2. Start process: the OS boots and the EC2 user data script is run, application starts, caches get warmed up and may take time

EC2 Hibernate

. the in-memory aka RAM is preserved

. the instance is fast because the OS is not stopped / refreshed

. under the hood: the RAM state is written to a file in the root EBCS volume

. the root EBS volume must be encrypted

use cases: long-running processing, saving RAM state, services that take time to initialize

Supported instance families: C3, C4, I3, M3...

Instance RAM size: must less than 150 GB

instance size: not supported for bare metal instances

AMI: Linux, CentOS, Windows...

root volume: must be EBC, encrypted, not instance store and large

available for On-demand, reserved and spot instances

an instance can not be hibernated more than 60 days