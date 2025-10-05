# aws-lambda-s3-automation

# 🚀 Desafio DIO — Executando Tarefas Automatizadas com AWS Lambda e S3

Este repositório contém a documentação e os arquivos desenvolvidos durante o desafio **"Executando Tarefas Automatizadas com Lambda Function e S3"** da **Digital Innovation One (DIO)**.  
O objetivo é demonstrar como configurar uma automação simples na AWS usando **Lambda** e **S3**, aplicando os conceitos aprendidos em aula.

---

## 🎯 Objetivos do Projeto

- Criar uma função **AWS Lambda** para executar uma tarefa automatizada.  
- Integrar a função com um **bucket S3**, utilizando eventos (`ObjectCreated`) como gatilho.  
- Documentar o processo técnico de forma clara e estruturada.  
- Utilizar o **GitHub** como ferramenta de documentação e portfólio técnico.

---

## 🧩 Arquitetura da Solução

1. O usuário envia um arquivo para o bucket **S3**.  
2. O evento `ObjectCreated` aciona automaticamente a **função Lambda**.  
3. A função registra o evento e cria um arquivo de log em outro bucket.  

Fluxo básico da automação:

Usuário → S3 Bucket → Evento → Lambda Function → Log em outro Bucket


---

## ⚙️ Tecnologias e Serviços Utilizados

- **AWS Lambda**
- **Amazon S3**
- **AWS IAM (permissões)**
- **AWS CloudFormation** *(opcional)*
- **Python 3.x**
- **Git e GitHub**

---

## 💻 Código da Função — `lambda_function.py`

```python
import json
import boto3

def lambda_handler(event, context):
    s3 = boto3.client('s3')

    # Obtém informações do arquivo enviado
    bucket_name = event['Records'][0]['s3']['bucket']['name']
    file_key = event['Records'][0]['s3']['object']['key']

    print(f"Novo arquivo detectado: {file_key} em {bucket_name}")

    # Cria um log simples em outro bucket
    log_bucket = "meu-bucket-de-logs"  # Altere para o nome do seu bucket de logs
    log_message = f"Arquivo '{file_key}' foi enviado para o bucket '{bucket_name}'"

    s3.put_object(
        Bucket=log_bucket,
        Key=f"logs/{file_key}.txt",
        Body=log_message.encode('utf-8')
    )

    return {
        'statusCode': 200,
        'body': json.dumps('Processamento concluído com sucesso!')
    }
