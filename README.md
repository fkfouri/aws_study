# aws_study
Aws study


## aws ec2

- aws ec2 describe-instances --query Reservations[*].Instances[*].{Instance:[InstancesId,InstanceType,State]}

```json
[
    [
        {
            "Instance": [
                null,
                "t2.micro",
                {
                    "Code": 16,
                    "Name": "running"
                }
            ]
        }
    ],
    [
        {
            "Instance": [
                null,
                "t2.micro",
                {
                    "Code": 16,
                    "Name": "running"
                }
            ]
        }
    ]
]
```