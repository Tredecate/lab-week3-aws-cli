## Lab 3

#### Clone the repository into your debian environment:
`https://gitlab.com/cit_4640/4640-w3-lab-start-w25`

We have three scripts were must make changes to:
  1. import-key
  2. create-bucket
  3. create-ec2

### 1. Change the key-name to `bcitkey`
Find this line at the bottom of the script: aws ec2 import-key-pair --key-name "bcitkey" --public-key-material fileb://${public_key_file} > key_data

------

### 2. Complete the create-bucket script

references:
- https://docs.aws.amazon.com/cli/latest/reference/s3api/create-bucket.html
- https://docs.aws.amazon.com/cli/v1/userguide/cli-services-ec2-instances.html

```bash
else
    aws s3api create-bucket \
        --bucket "$bucket_name" \
        --region us-west-2 \
        --create-bucket-configuration LocationConstraint=us-west-2;
fi
```

-----
### 3. Complete the create-ec2 script.
References: 
https://docs.aws.amazon.com/cli/latest/reference/ec2/run-instances.html

**Make the following changes to the script:**

```bash
instance_id=$(aws ec2 run-instances \
    --image-id "$debian_ami" \
    --instance-type t3.micro \
    --key-name "$key_name" \
    --subnet-id "$subnet_id" \
    --security-group-ids "$security_group_id" \
    --associate-public-ip-address \
    --region "$region" \
    --query "Instances[0].InstanceId" \
    --output text)
    

`
#Get the public IP address of the EC2 instance
public_ip=$(aws ec2 describe-instances \
    --instance-ids "$instance_id" \
    --query Reservations[0].Instances[0].PublicIpAddress \
    --output text)

#Write instance data to a file
echo "Public IP: $public_ip"
echo "$public_ip" > instance_data
```

    


