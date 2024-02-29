If you want to access EC2, run command as below:
ssh -i (keypair.pem) (username)@(Public EC2 IP)

If we want to change instance type (it only works on EBS backed instance), 
Stop the instance
Change instance type
Start instance

Shutdown behaviors: there are 2 options, stop and terminate

Termination protection: enabled or disabled