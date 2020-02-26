# aws_study
Aws study


## Conexao SSH
- ssh -i file.pem ubuntu@3.220.79.244
- ssh -i "file.pem" ubuntu@ec2-54-209-154-181.compute-1.amazonaws.com## aws Configure

## Transferencia de arquivos Windows/EC2
- pscp -i file.ppk  -r alura\html\* ubuntu@ec2-54-209-154-181.compute-1.amazonaws.com:/var/www/html

Para o endereco de destino, eh necessario dar acesso 0775 para o usuario Ubuntu.
- sudo chmod -R 0775 /var/www/

Pode ser que seja necessario usar o WinSCP ou Filezilla, mas eles tamb�m usarao chaves ppk
- https://www.youtube.com/watch?v=e9BDvg42-JI



## aws Configure
- aws configure
    - forneca o AWS Access Key ID
    - forneca o AWS Secrete Access KEY
    - Defina a regiao: us-east-1
    - Defina o formato de saida: json

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

## visualizar informacoes de uma instance especifica
- aws ec2 describe-instances --instance-id i-04c712e488f2a0b9c

## inicializar instancia
- aws ec2 start-instances --instance-ids i-04c712e488f2a0b9c

## parar uma instancia
- aws ec2 stop-instances --instance-ids i-04c712e488f2a0b9c