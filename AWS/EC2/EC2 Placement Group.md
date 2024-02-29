Placement group:
EC2 instances placement strategy, the strategy is defined by using placement group

1. Cluster: same rack same AZ, if the rack crashed, the whole thing crashed but it fast
2. Spread: different AZ, different hardware, limited to 7 instances per placement group
3. Partition: 1 partition can have many EC2 instances and in different AZ, each partition represents a rack in AWS, up to 7 partitions per AZ, the instances in a partition do not share racks with other instances in other partitions, EC2 instances get access to the partition information as metadata