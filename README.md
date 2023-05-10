# AWS Cloud Practitioner Essentials

Anotações do curso disponível em AWS Skill Builder

# Introduction to AWS

* Pay for what you need
* **Computação em nuvem:** entrega sob demanda de recursos de TI pela internet com pagamento por uso
* AWS foca em fornecer os recursos que não diferenciam os negócios de seus clientes (infra, trabalhos repetitivos) para que eles foquem seus esforços no que os diferencia de seus concorrentes

## Modelos de Implementação

* **Cloud**
  * executar todas as partes de uma aplicação em cloud
  * migrar aplicações existentes para cloud
  * desenvolver aplicações em cloud

* **On Premises**
  * também conhecido como nuvem privada
  * implementar recursos usando virtualização
  * aumentar o uso de recursos com virtualização

* **Híbrido**
  * conectar recursos em cloud com a infraestrutura on premises
  * integrar recursos em cloud com aplicações legadas

## Benefícios da Computação em Nuvem

* troca de despesas iniciais por variáveis
* redução de custos com manutenção
* capacidade de escalar quando precisar, sem precisar prever a necessidade
* agilidade pra escalar e testar novos recursos e aplicações
* disponibilidade global com baixa latência
* o uso de muitas empresas faz com que a AWS economize, gerando um custo menor ao cliente

# Compute in the Cloud

* multitenancy: distribuição de recursos de hardware entre múltiplas vms
* vertical scaling: aumentar os recursos de uma instância (cpu, memória)

## Tipos de Instâncias

* **General Purpose** 
  * recursos balanceados
  * bancos de dados de pequeno e médio porte
  * servidor de aplicativos ou games
* **Compute Optimized**
  * CPUs de alta performance
  * processamento de dados com muitas transações
  * servidores de aplicativos e games com alta performance
* **Memory Optimized**
  * RAM de alta performance
  * banco de dados de alta performance
  * aplicações em tempo real
* **Accelerated Computing**
  * aceleradores de hardware ou coprocessadores
  * cálculos com ponto flutuante
  * processamento gráfico e streaming
* **Storage Optimized**
  * Armazenamento de alta performance (leitura e escrita)
  * sistemas de arquivos distribuídos
  * data warehouse

## Custos

* **On Demand**
  * custo por uso em horas ou segundos
  * sem custos iniciais ou compromisso a longo prazo
  * ideal para aplciações de curto prazo, ambiente de teste ou com carga imprevisível
  * pode servir para estimar o uso médio dos recursos
* **Saving Plans**
  * planos de 1 ou 3 anos
  * até 72% de economia em relação a On Demand
  * carga previsível e constante
* **Reserved Instances**
  * planos de 1 ou 3 anos
  * até 75% de economia em relação a On-Demand
  * opções de pagamento adiantado integral, parcial ou nenhum
  * opções:
    * Standard Reserved Instances: regime constante
    * Convertible Reserved Instances: atributos flexíveis
    * Schedule Reserved Instances: agendamento programado para eventos, feriados, semanas ou um mês
* **Spot Instance**
  * até 90% de economia em relação a On-Demand
  * aplicações com disponibilidade flexível (que pode ser interrompida sem prejuízos)
  * mecanismo:
    * usuário define o preço máximo por hora que deseja pagar
    * preço da instância varia conforme a disponibilidade
    * se o preço da instância for menor que o definido pelo usuário, a instância fica disponível
* **Dedicated Hosts**
  * podem ser usadas licenças do cliente para Windows Server, SQL Server ou Oracle
  * pagamento por uso (horas) ou reserva (até 70% de economia)
  * indicados para atender requisitos de compliance (regulações governamentais)

## Auto Scaling

* problemas com on premises
  * se investe pouco para não ter hardware parado **~>** não atende aos picos de demanda
  * se investe muito para atender os picos de demanda **~>** maior parte do tempo em pouco uso
* auto scaling permite aumentar ou diminuir instâncias baseando-se em
  * mudanças na demanda **~>** Dynamic Scaling
  * previsão de demanda **~>** Predictive Scaling
* scaling up **~>** aumentar os recursos de uma instância
* scaling out **~>** aumentar o número de instâncias
* Auto Scaling Group
  * definição do mínimo, do desejado e do máximo de instâncias

## Elastic Load Balancing

* dispositivo regional **~>** automaticamente escalável
* atravessador que distribui a carga entre as instâncias
* processa cargas externas ou internas (do front para o back)

## Messaging and queuing

* **Simple Notification Service**
  * comunicação entre componentes da aplicação
  * ideal para arquitetura em micro serviços
* **Simple Queue Service**
  * cria e gerencia uma fila de comunicação entre os componentes da aplicação
  * ideal para trabalhar junto do **SNS** em arquitetura de micro serviços 

## Additional Compute Services

* **Lambda**
  * serverless: o cliente não cuida do sevidor
  * roda código a partir de triggers (máximo de 15 min por execução)
  * pagamento por tempo de execução

* **Elastic Container Service**
  * orquestrador de containers compatível com Docker
* **Elastic Kubernetes Service**
  * serviço que permite a execução de Kubernetes
* **Fargate**
  * solução serverless para containers
  * compatível com **ECS** e **EKS**

# Global Infraestructure and Reliability

Deve-se escolher a região baseado em

* **compliance:** regulações governamentais relacionadas aos dados
* **latency:** quanto mais próximo dos clientes menor a latência
* **services:** novos serviços costumam estar disponíveis somente em poucas regiões
* **pricing:** cada região tem um preço

# Networking

# Storage and Databases

# Security

# Monitoring and Analytics

# Pricing and Support

# Migration and Innovation

# The Cloud Journey

# AWS Certified Cloud Practitioner Basics



