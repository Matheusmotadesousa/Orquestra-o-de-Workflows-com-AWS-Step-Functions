## **Sobre o Projeto**  
Este repositório documenta minha jornada no desafio de AWS Step Functions da DIO, onde explorei a orquestração de serviços serverless para criar fluxos de trabalho automatizados, resilientes e escaláveis.  

Aqui você encontrará não apenas o código e as definições das máquinas de estados, mas também insights, aprendizados e boas práticas que adquiri durante esta imersão prática no universo da computação serverless.

## **O que é AWS Step Functions?**  
O AWS Step Functions é um serviço de orquestração serverless que permite coordenar múltiplos serviços AWS em fluxos de trabalho visualmente estruturados. Ele utiliza Máquinas de Estados definidas em Amazon States Language (ASL) para criar pipelines robustos com tratamento de erros, execuções paralelas e integração nativa com mais de 220 serviços AWS.  

## **Integrações Poderosas**  
O que torna o Step Functions realmente poderoso é sua capacidade de se integrar nativamente com:

* AWS Lambda - Funções serverless para lógica de negócio.

* Amazon DynamoDB - Operações CRUD em banco NoSQL.

* Amazon SNS/SQS - Mensageria e notificações.

* AWS Batch - Processamento de dados em larga escala.

* Amazon SageMaker - Pipelines de machine learning.

## **Minha Jornada no Desafio**  
Fase 1: Configuração Inicial  
* Criação da Máquina de Estados  
* Acessei o console AWS Step Functions  
* Utilizei o Workflow Studio para projetar visualmente o workflow  
* Defini os estados e transições usando ASL  
* Permissões IAM  
* Configurei a role IAM necessária para o Step Functions executar as tarefas  
* Garanti princípio de menor privilégio  

## **Fase 2: Definindo o Workflow**  
{
  "Comment": "Exemplo de workflow com Step Functions",
  "StartAt": "Processar Dados",
  "States": {
    "Processar Dados": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:REGION:ACCOUNT_ID:function:processar-dados",
      "Next": "Validar Resultado"
    },
    "Validar Resultado": {
      "Type": "Choice",
      "Choices": [
        {
          "Variable": "$.status",
          "StringEquals": "SUCESSO",
          "Next": "Finalizar Sucesso"
        },
        {
          "Variable": "$.status",
          "StringEquals": "ERRO",
          "Next": "Tratar Erro"
        }
      ],
      "Default": "Aguardar"
    },
    "Finalizar Sucesso": {
      "Type": "Pass",
      "Result": "Workflow concluído com sucesso!",
      "End": true
    },
    "Tratar Erro": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:REGION:ACCOUNT_ID:function:tratar-erro",
      "End": true
    },
    "Aguardar": {
      "Type": "Wait",
      "Seconds": 10,
      "Next": "Finalizar Sucesso"
    }
  }
}  
## **Fase 3: Execução e Testes**  
* Executei o workflow com diferentes cenários  
* Monitorei as execuções no console  
* Analisei os logs e métricas no CloudWatch  

## **Fase 4: Desafios e Soluções**
Desafio 1: Tratamento de Erros

Problema: Exceções não tratadas interrompiam o fluxo

Solução: Implementei Catch e Retry nos estados Task

Desafio 2: Passagem de Dados  

Problema: Dados não eram propagados corretamente entre estados

Solução: Utilizei InputPath, OutputPath e ResultPath para controlar o fluxo de dados  

O que aprendi.
Orquestração Serverless é poderosa para automatizar processos complexos

O Amazon States Language é uma linguagem expressiva e flexível

Tratamento de erros é crucial para workflows robustos

Monitoramento via CloudWatch é essencial para produção

O Workflow Studio acelera significativamente o desenvolvimento

Para Próximos Passos.  
 Explorar Map State para processamento paralelo de arrays
□ Integrar com SNS para notificações em tempo real
□ Implementar Callback Pattern para interações humanas no workflow
□ Criar pipelines de CI/CD com AWS CodePipeline
