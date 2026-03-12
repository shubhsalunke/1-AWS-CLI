Here are the **complete commands in English, step-by-step** to create an **S3 bucket and an EC2 instance using AWS CLI from Windows CMD**.

---

# 1. Check AWS CLI Installation

```cmd
aws --version
```

---

# 2. Configure AWS CLI

```cmd
aws configure
```

Example input:

```cmd
AWS Access Key ID [None]: YOUR_ACCESS_KEY
AWS Secret Access Key [None]: YOUR_SECRET_KEY
Default region name [None]: ap-south-1
Default output format [None]: json
```

---

# 3. Verify Configuration

```cmd
aws configure list
```

---

# 4. Check Existing S3 Buckets

```cmd
aws s3 ls
```

---

# 5. Create a New S3 Bucket

```cmd
aws s3api create-bucket --bucket user-demo-bucket-2026 --region ap-south-1 --create-bucket-configuration LocationConstraint=ap-south-1
```

---

# 6. Verify the Bucket

```cmd
aws s3 ls
```

---

# 7. Create a Key Pair for EC2

```cmd
aws ec2 create-key-pair --key-name key --query "KeyMaterial" --output text > key.pem
```

---

# 8. Verify the PEM File

```cmd
dir key.pem
```

---

# 9. Create a Security Group

```cmd
aws ec2 create-security-group --group-name shubh-sg --description "My EC2 security group" --region ap-south-1
```

Example output will contain a **Security Group ID**, such as:

```
sg-0c754*******b12
```

---

# 10. Open SSH Port (22)

```cmd
aws ec2 authorize-security-group-ingress --group-id sg-0c7*******24b12 --protocol tcp --port 22 --cidr 0.0.0.0/0 --region ap-south-1
```

---

# 11. Check Available Subnets

```cmd
aws ec2 describe-subnets --region ap-south-1 --query "Subnets[*].[SubnetId,AvailabilityZone,VpcId,MapPublicIpOnLaunch]" --output table
```

Choose one subnet ID.

Example:

```
subnet-07ff8b4*********1b4
```

---

# 12. Find Latest Amazon Linux AMI

```cmd
aws ec2 describe-images --owners amazon --filters "Name=name,Values=al2023-ami-*" "Name=architecture,Values=x86_64" "Name=root-device-type,Values=ebs" --query "reverse(sort_by(Images,&CreationDate))[:5].[ImageId,CreationDate,Name]" --output table --region ap-south-1
```

Example AMI:

```
ami-05957b*********d1d
```

---

# 13. Check Free-Tier Eligible Instance Types

```cmd
aws ec2 describe-instance-types --region ap-south-1 --filters Name=free-tier-eligible,Values=true --query "InstanceTypes[*].InstanceType" --output table
```

Example result:

```
t3.micro
t3.small
t4g.micro
```

---

# 14. Launch EC2 Instance

```cmd
aws ec2 run-instances --image-id ami-059******de7afd1d --instance-type t3.micro --key-name key --security-group-ids sg-0c75*******4124b12 --subnet-id subnet-07ff8b********1b4 --count 1 --associate-public-ip-address --region ap-south-1
```

---

# 15. Verify the EC2 Instance

```cmd
aws ec2 describe-instances --region ap-south-1 --query "Reservations[*].Instances[*].[InstanceId,State.Name,PublicIpAddress,InstanceType,KeyName]" --output table
```

Example output:

```
InstanceId            State    PublicIpAddress
i-xxxxxxxxxxxxxxxxx   running  15.xxx.xxx.xxx
```

---

# 16. Connect to EC2 Using SSH

```cmd
ssh -i shubh-key.pem ec2-user@15.207.72.2
```
