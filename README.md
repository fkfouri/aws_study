# aws_study
Aws study


## aws ec2

- aws ec2 describe-instances --query Reservations[*].Instances[*].{Instance:[InstanceId,InstanceType,State]}

```json
[
    [
        {
            "Instance": [
                "i-04c712e488f2a0b9c",
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
                "i-0e9c06cbc93af2768",
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