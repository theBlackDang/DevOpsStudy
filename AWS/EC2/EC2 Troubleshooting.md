# EC2 launch troubleshooting
## Instance limit exceed: if you get this error, it means that you have reached your limit of max number of vCPUs per region: 

   + On-demand instances limit are set on a per-region basis
   + example: if you run on-demand (A, C, D, H, I, M, R, T, Z) instance types you'll have 64 vCPUs (default)
   + Resolution: either launch the instance in a different in a different region or request AWS to increase your limit of the region
   + vCPU based limits only apply to run On-demand instances and spot instances
## Insufficient instance capacity: if we get this AWS doesn't have enough on demand resources for you to launch EC2 instances (AZ)

  + Wait for few minutes before requesting again
  + If the request is to big, break it down to create separated instances
  + If urgent, request for lower instance type and resize later

## Instance terminate immediately

+ Reach EBS limit 
+ EBS snapshot is corrupted
+ EBS volume is encrypted but you don't have KMS to decrypt

## EC2 SSH troubleshooting

+  key.pem file is get from ec2 creation
+  username is correct
+ chmod 400 permission for the key.pem file
+ Check for network
+ Overload check

