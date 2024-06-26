## List all available security-group ids
     ec2 describe-security-groups 
## create new security group
    aws ec2 describe-vpcs
    aws ec2 create-security-group --group-name my-sg --description "My security group" --vpc-id
    vpc-1a2b3c4d
## this will give output of created my-sg with its id, so we can do:
    aws ec2 describe-security-groups --group-ids sg-903004f8
## add firewall rule to the group for port 22
    aws ec2 authorize-security-group-ingress --group-id sg-903004f8 --protocol tcp --port 22 --cidr 203.0.113.0/24
    aws ec2 describe-security-groups --group-ids sg-903004f8
# Use an existing key-value pair or if you want, create and use a new key-pair. 'KeyMaterial' gives
    us an unencrypted PEM encoded RSA private key.
    aws ec2 create-key-pair --key-name MyKeyPair --query 'KeyMaterial' --output text > MyKeyPair.pem
# launch ec2 instance in the specified subnet of a VPC
    aws ec2 describe-subnets
    aws ec2 describe-instances -> will give us ami-imageid, we will use the same one
    aws ec2 run-instances
    --image-id ami-xxxxxxxx
    --count 1
    --instance-type t2.micro
    --key-name MyKeyPair
    --security-group-ids sg-903004f8
    --subnet-id subnet-6e7f829e
# ssh into the ec2 instance with the new key pem after creating it - public IP will be returned as
    json, so query it
    aws ec2 describe-instances --instance-ids {instance-id}
    chmod 400 MyKeyPair.pem
    ssh -i MyKeyPair.pem ec2-user@public-ip
# check UI for all the components that got created
# describe-instances - with filter and query
    --filter is for picking some instances. --query is for picking certain info about those instances