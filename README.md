# aws-step-functions-lambda-s
# Executando Tarefas Automatizadas com AWS Lambda e S3

## Descrição do Desafio
Este repositório foi criado para documentar o desafio “Executando Tarefas Automatizadas com Lambda Function e S3”, proposto pela Digital Innovation One (DIO).  
O objetivo é consolidar conhecimentos em **automação de tarefas** utilizando a **AWS Lambda**, em conjunto com o **Amazon S3**, explorando conceitos de computação serverless.

---

## Objetivos de Aprendizagem
Ao concluir este desafio, fui capaz de:
- Criar e configurar funções Lambda na AWS;
- Integrar eventos do S3 para disparar funções automaticamente;
- Entender o conceito de computação sem servidor (serverless);
- Documentar tecnicamente processos de automação em nuvem.

---

## O que é o AWS Lambda
O **AWS Lambda** é um serviço de **computação serverless** que executa seu código **automaticamente em resposta a eventos**, sem precisar gerenciar servidores.  
Você apenas envia o código — e o Lambda cuida do resto: escala, executa, monitora e cobra **somente pelo tempo de execução**.

---

## Integração com o Amazon S3
O **Amazon S3** (Simple Storage Service) é um serviço de armazenamento de objetos que pode gerar **eventos automáticos** sempre que algo acontece em um bucket, como:
- Upload de um novo arquivo,
- Exclusão de um objeto,
- Criação de versão de arquivo.

Esses eventos podem ser configurados para **disparar uma função Lambda automaticamente**, criando fluxos totalmente automatizados, como:
- Processamento de imagens enviadas;
- Conversão de arquivos;
- Geração de logs ou relatórios.

---

## Exemplo Prático

### Cenário
Quando um arquivo é enviado para o bucket `meu-bucket-dio`, a função Lambda é executada automaticamente para registrar o evento e mover o arquivo para outra pasta dentro do mesmo bucket.

### Exemplo de Código (Python)
```python
import json
import boto3

def lambda_handler(event, context):
    s3 = boto3.client('s3')
    
    # Obtém informações do evento
    bucket = event['Records'][0]['s3']['bucket']['name']
    arquivo = event['Records'][0]['s3']['object']['key']

    print(f"Arquivo '{arquivo}' foi enviado para o bucket '{bucket}'")

    # Exemplo de ação automatizada: copiar o arquivo para uma pasta 'processados'
    destino = f"processados/{arquivo}"
    s3.copy_object(Bucket=bucket, CopySource={'Bucket': bucket, 'Key': arquivo}, Key=destino)
    
    return {
        'statusCode': 200,
        'body': json.dumps('Arquivo processado com sucesso!')
    }
