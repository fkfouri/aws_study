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

Mais um exemplo
scp -r -i estudo_aws.pem aws_study\Route53\site ubuntu@ec2-3-231-156-36.compute-1.amazonaws.com:~    

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

esses dados sao criados no modulo IAM da AWS


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



# AWS ECS

## Exibicao dos clusters
- aws ecs list-cluesters

## Exibicao a lista de serviços de um cluster
- aws ecs list-services --cluster default

## Exebicao da lista de tarefas em execucao
- aws ecs list-tasks


## Definindo o numero de tarefas do meu serviço.
- aws ecs update-service --service sample-app-service --desired-count 2

## Descricao dos clusters
- aws ecs describe-clusters

```
{
    "clusters": [
        {
            "clusterArn": "arn:aws:ecs:us-east-1:032594213725:cluster/default",
            "clusterName": "default",
            "status": "ACTIVE",
            "registeredContainerInstancesCount": 0,
            "runningTasksCount": 2,
            "pendingTasksCount": 0,
            "activeServicesCount": 1,
            "statistics": [],
            "tags": [],
            "settings": [
                {
                    "name": "containerInsights",
                    "value": "disabled"
                }
            ],
            "capacityProviders": [],
            "defaultCapacityProviderStrategy": []
        }
    ],
    "failures": []
}
```

## Executando uma query
- aws ecs describe-clusters --query clusters[*].[clusterName,runningTasksCount]

```
[
    [
        "default",
        2
    ]
]
```


## Descrevendo dados de uma tarefa
- aws ecs describe-tasks --cluster default --tasks 6d45fd6a-d963-408a-bfbe-b7cd21edc6c7

```
{
    "tasks": [
        {
            "attachments": [
                {
                    "id": "4fe290e8-8656-46a4-96ac-c556d080c46b",
                    "type": "ElasticNetworkInterface",
                    "status": "ATTACHED",
                    "details": [
                        {
                            "name": "subnetId",
                            "value": "subnet-099286433d8be43e1"
                        },
                        {
                            "name": "networkInterfaceId",
                            "value": "eni-043e66382a6cb01a1"
                        },
                        {
                            "name": "macAddress",
                            "value": "02:42:11:ec:13:73"
                        },
                        {
                            "name": "privateIPv4Address",
                            "value": "10.0.0.199"
                        }
                    ]
                }
            ],
            "availabilityZone": "us-east-1a",
            "clusterArn": "arn:aws:ecs:us-east-1:032594213725:cluster/default",
            "connectivity": "CONNECTED",
            "connectivityAt": 1585853813.554,
            "containers": [
                {
                    "containerArn": "arn:aws:ecs:us-east-1:032594213725:container/6ef8baae-064f-4d80-a775-efb3ff4eb02c",
                    "taskArn": "arn:aws:ecs:us-east-1:032594213725:task/6d45fd6a-d963-408a-bfbe-b7cd21edc6c7",
                    "name": "sample-app",
                    "image": "httpd:2.4",
                    "runtimeId": "7cf49fd70c6c7c6c802b630b8720f71baa9beaf380f1df17f4d42172b976b9dc",
                    "lastStatus": "RUNNING",
                    "networkBindings": [],
                    "networkInterfaces": [
                        {
                            "attachmentId": "4fe290e8-8656-46a4-96ac-c556d080c46b",
                            "privateIpv4Address": "10.0.0.199"
                        }
                    ],
                    "healthStatus": "UNKNOWN",
                    "cpu": "256",
                    "memoryReservation": "512"
                }
            ],
            "cpu": "256",
            "createdAt": 1585853809.265,
            "desiredStatus": "RUNNING",
            "group": "service:sample-app-service",
            "healthStatus": "UNKNOWN",
            "lastStatus": "RUNNING",
            "launchType": "FARGATE",
            "memory": "512",
            "overrides": {
                "containerOverrides": [
                    {
                        "name": "sample-app"
                    }
                ],
                "inferenceAcceleratorOverrides": []
            },
            "platformVersion": "1.3.0",
            "pullStartedAt": 1585853830.37,
            "pullStoppedAt": 1585853848.37,
            "startedAt": 1585853851.37,
            "startedBy": "ecs-svc/1499716321643581408",
            "tags": [],
            "taskArn": "arn:aws:ecs:us-east-1:032594213725:task/6d45fd6a-d963-408a-bfbe-b7cd21edc6c7",
            "taskDefinitionArn": "arn:aws:ecs:us-east-1:032594213725:task-definition/first-run-task-definition:1",
            "version": 3
        }
    ],
    "failures": []
}
```

## Descrevendo daso de uma tarefa mas aplicando uma query
- aws ecs describe-tasks --cluster default --tasks 6d45fd6a-d963-408a-bfbe-b7cd21edc6c7 --query tasks[*].{cpu:cpu,memoria:memory}


## Visualizando autoscaling de uma ECS do tipo EC2
- aws autoscaling describe-auto-scaling-groups

## Definindo a capacidade desejada (tamanho do cluster)
- aws autoscaling set-desired-capacity -- auto-scaling-group-name EC2ContainerService-ecs-api-EcsInstanceAsg-1ASP5U9ZAX6TZ -- desired-capacity 2

## Parando uma task
- aws ecs stop-task --task ID --cluster ecs-api

## Exibe todas as task-definitions
- aws ecs list-task-definitions

## inicializando uma task
- aws ecs run-task --cluster ecs-api --task-definition api-monolitica:1

## inicializando multi tasks
- aws ecs run-task --cluster ecs-api --task-definition api-monolitica:1 --count 3



















