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

## Transferencia do pem para dentro da maquina de gerencia de uma VPC
- scp -i "aws-estudo-ohio.pem" aws-estudo-ohio.pem ec2-user@ec2-3-15-189-114.us-east-2.compute.amazonaws.com:~/

<p style='color:red'>quando se usa SCP, deve-se usar o arquivo PEM, quando se usa o PSCP, deve-se usar o arquivo PPK.</p>


# AWS-CLI Configure
- aws configure
    - forneca o AWS Access Key ID
    - forneca o AWS Secrete Access KEY
    - Defina a regiao: us-east-1
    - Defina o formato de saida: json

A configuracao do AWS-CLI fica armazenada em <b>C:\Users\user\.aws</b>.


# aws ec2

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


#  AWS S3

- Help => aws s3 help

## Criar um bucket (make bucket)
- aws s3 mb s3://aws-estudo-fabio-images

## Exibir uma relação de buckets do S3
- aws s3 ls

## Remocao de bucket (remove bucket).
- aws s3 rb s3://aws-estudo-fabio-images
- aws s3 rb s3://aws-estudo-fabio-images --force (caso o bucket nao esteja vazio)

## Listando objetos de um bucket. So exibe se estiver o bucket estiver na mesma regiao configurada do AWS-CLI
- aws s3 ls s3://estudo-aws-fabio


## Copiar o conteudo de um Bucket para uma maquina local
- aws s3 cp s3://estudo-aws-fk/foto.jpg .

## Upload de objeto da maquina local para um Bucket
- aws s3 cp ./test.txt s3://estudo-aws-fk

## Remover um objeto do Bucket
- aws s3 rm s3://estudo-aws-fk/test.txt

## Renomear/Mover um objeto do bucket
- aws s3 mv s3://estudo-aws-fk/test.txt s3://estudo-aws-fk/test1.txt 

## Sincronizar do bucket com um repositorio local
- aws s3 sync s3://estudo-aws-fk/ .
- aws s3 sync . s3://estudo-aws-fk/

## Sincronizar do bucket com um repositorio local forçando o delete quando aplicavel
- aws s3 sync . s3://estudo-aws-fk/ --delete

















