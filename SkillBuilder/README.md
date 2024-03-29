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
  * CodeDeploy e OpsWorks podem ser usados
  
* **Híbrido**
  * conectar recursos em cloud com a infraestrutura on premises
  * integrar recursos em cloud com aplicações legadas
  * Route 53 e Virtual Private Gateway podem ser usados

## Benefícios da Computação em Nuvem

* troca de despesas iniciais por variáveis
* redução de custos com manutenção
* capacidade de escalar quando precisar, sem precisar prever a necessidade
* agilidade pra escalar e testar novos recursos e aplicações
* disponibilidade global com baixa latência
* o uso de muitas empresas faz com que a AWS economize, gerando um custo menor ao cliente

# Compute in the Cloud

* provisionar capacidade para picos de carga
* alta disponibilidade **~>** mínimo de 2 AZs
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

## AS - Auto Scaling

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

## ELB - Elastic Load Balancing

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
  * serverless: o cliente não cuida do sevidor **~>** maior performance
  * roda código a partir de triggers (máximo de 15 min por execução)
  * pagamento por tempo de execução

* **ECS - Elastic Container Service**
  * orquestrador de containers compatível com Docker
* **EKS -Elastic Kubernetes Service**
  * serviço que permite a execução de Kubernetes
* **Fargate**
  * solução serverless para containers
  * compatível com **ECS** e **EKS**

# Global Infraestructure and Reliability

* **Route 53** e **CloudFront** são recursos globais
* Deve-se escolher a região baseado em
* **compliance:** regulações governamentais relacionadas aos dados
  
* **latency:** quanto mais próximo dos clientes menor a latência
  
* **services:** novos serviços costumam estar disponíveis somente em poucas regiões
  
* **pricing:** cada região tem um preço


* **Availability Zone**
  * um ou mais data centers dentro de uma **Region**
  * separadas a 100 km para manter a baixa latência e evitar que desastres atinjam mais de uma
  * boa prática: rodar aplicações em pelo menos 2 instância em **AZs** diferentes
* **Outposts:** equipamentos da AWS dentro do datacenter do cliente

## Edge Location

* local separado das **Regions**
* cria um cache da aplicação usando o **CloudFront** para entregar mais rápido aos clientes (CDN)
* também roda o **Route53** (DNS Web Service)
  * requisição do cliente **~>** Route 53 resolve o nome e devolve ao cliente o IP **~>** CloudFront envia a requisição a Edge Location mais próxima e se conecta ao Load Balancer **~>** Load Balancer envia a requisição às instâncias EC2

* um aplicação pode ser cacheada em várias **Edge Locations**

## How to Provisioning Resources

* tudo na AWS é uma **API Request** que pode ser feita via:

  * **Management Console:** interface web ou mobile **~>** uso do mouse

  * **CLI:** ferramenta via terminal (usando chave SSH) compatível com windows, linux e mac **~>** scripts
  * **SDKs:** ferramentas compatíveis com diversas linguagens de programação **~>** programas

* **Elastic Beanstalk**

  * automação para EC2 usando código

  * ajustes de capacidade, load balancing e monitoramento

* **CloudFormation**
  * Infrastructure as a Code
  * automação no provisionamento de ambientes completos

# Networking

* **Virtual Private Cloud**
  * isolamento de redes dos usuários da AWS
  * dentro de uma **VPC** podem ser criadas **subnets** para separar os recursos
    * os **Security Groups** também são criados no **VPC Dashboard**
  * para entrar ou sair em uma **VPC**, um pacote passa por verificação em **ACL** (Access Control List)
    * em **Default ACL** toda entrada/saída é **liberada**
    * em **Custom ACL** toda entrada/saída é **bloqueada** até que sejam criada regras
    * **ACL** é um filtro *stateless* **~>** não grava informações, verifica todos os pacotes 
  * para acessar uma instância **EC2**, um pacote passa por verificação em **Security Group**
    * toda entrada é **bloqueada** até que sejam criadas regras
    * toda saída é **liberada**
    * **Security Group** é um filtro *stateful* **~>** grava informações de entrada e não verifica novamente na saída
    * múltiplas instâncias **EC2** em uma mesma subnet podem usar o mesmo **Security Group** ou diferentes 
* **Internet Gateway:** interface entre a internet e a **VPC**
* **Virtual Private Gateway**
  * só permite conexões autenticadas **~>** cria uma **VPN**
* **Direct Connet:** conexão direta via fibra entre **VPC** e a rede do cliente

# Storage and Databases

## Instance Stores

* **Instance Store:** armazenamento temporário atachado na instância
  * os dados são perdidos quando a instância é encerrada ou parada e retomada
* **Block Storage:** arquivos são armazenados em blocos
  * quando um arquivo é atualizado, somente o bloco é atualizado
  * ver **Object Storage** na seção seguinte para comparar
* **EBS - Elastic Block Store:** armazenamento persistente atachado a uma instância da mesma AZ
  * armazenamento em bloco **~>** ótimo para mudanças pequenas em arquivos grandes
  * possibilita backup incrementais com snapshots
  * armazenamento limitado a 16 TB

## S3 - Simple Storage Service

* **Object Storage:** arquivo, metadados (informações) e chave (identificador)
  * quando um arquivo é atualizado, todo o objeto é atualizado
* armazenamento ilimitado
* Write Once, Read Many (WORM)
* 99,999999999% de durabilidade
* limite de 5TB por arquivo
* permite versionamento
* para escolher a classe, considerar:
  * frequência de acesso/recuperação
  * disponibilidade dos arquivos

### Standard

* acesso frequente
* armazena os arquivos em no mínimo 3 AZs **~>** alta disponibilidade
* classe mais cara
* websites, distribuição de conteúdo e análise de dados

### Standard Infrequent Access

* acesso não frequente
* armazena os arquivos em no mínimo 3 AZs **~>** alta disponibilidade
* armazenamento mais barato que a Standard
* recuperação mais cara que a Standard

### One Zone Infrequent Access

* acesso não frequente
* armazena os aquivos em somente uma AZ
* armazenamento mais barato que o Standard IA
* indicado para dados que podem ser recriados no caso de falha na AZ

### Intelligent Tiering

* cobra taxa de monitoramento por objeto
* automatiza a mudança de classe baseado nos acessos (Standard e Standard IA)

### Glacier Instant Retrieval

* arquivamento de dados
* recuperação imediata

### Glacier Flexible Retrieval

* arquivamento de baixo custo
* recuperação em minutos ou horas
* dados de clientes ou fotos e vídeos antigos

### Glacier Deep Archive

* classe mais barata de todas
* armazena os arquivos em no mínimo 3 AZs **~>** alta disponibilidade
* recuperação em entre 12 e 48 horas
* acesso 1 ou 2 vezes ao ano

### Outposts

* cria buckets nos armazenamentos da AWS internos a empresa

## EFS - Elastic File System

* servidor de arquivos com linux (não é só um HD)
* disponível em toda a região
* autoescalável
* replicação automática
* ideal para muitos acesso simultâneos

## RDS - Relational Database Service

* automatiza escalabilidade, patches e backups
* compatível com PostgreSQL, MySQL, MariaDB, Oracle e SQL Server

## Amazon Aurora

* compatível com MySQL e PostgreSQL
* mais ráido
* reduz custos otimizando I/O
* realiza 6 réplicas em 3 AZs e backup automático

## DynamoDB

* noSQL **~>** cada tabela contém um item (key) e seus atributos (values)
* serverless **~>** não precisa instalar, atualizar nem manter nenhum sistema
* autoescalável
* resposta em milisegundos em qualquer escala

## Redshift

* data warehousing
* big data e Business Intelligence

## DMS - Database Migration Service

* SQL e noSQL
* durante a migração a origem permanece operacional
* origens possíveis: on premises, EC2 e RDS
* destinos possíveis: EC2 e RDS
* pode ser usado para:
  * testar aplicações no banco de produção sem afetar os dados/disponibilidade
  * combinar vários bancos **~>** consolidar
  * replicação contínua

## Additional Databases Services

* **DocumentDB:** banco de documentos compatível com MongoDB
* **Neptune:** banco de grafos usado para redes sociais, detecção de fraudes e recomendação de engines
* **Quantum Ledger Database:** banco de registro usado para verificar o histórico de mudanças nos dados
* **Managed Blockchain:** criação e manutenção de redes de blockchain
* **ElastiCache:** acelerador que adiciona um cache para aumentar a velocidade de leituras recorrentes em bancos (compatível com Redis e Memcached)
*  **DynamoDB Accelerator (DAX):** adiciona cache em memória para o DynamoDB (reduz o tempo de resposta de milisegundos para microsegundos)

# Security

* para realizar pentest é necessário solitiar à AWS e esperar pela aprovação

## Shared Responsability Model

* **AWS:** segurança **da** nuvem
  * hardware, rede, virtualização
* **Cliente:** segurança **na** nuvem
  * acesso, criptografia, SO e aplicações

## IAM - Identity and Access Management

* não usar o usuário root constantemente
  * criar um usuário com privilégios de criar outros usuários e usar ele
  * usar a conta de root somente em extrema necessidade, como trocar email da conta ou o plano de suporte
* **usuário:** identidade que representa uma pessoa ou aplicação que interage com os serviços ou recursos
  * um novo usuário não tem nenhuma permissão até que lhe seja atribuiída
  * criar um usuário para cada pessoa que acessa a AWS e atribuir aos grupos
* **políticas:** documento que permite ou proibe o uso de serviços e recursos
  * aplicar as políticas aos grupos é mais fácil de organizar do que se aplicar aos usuários
  * **least privilege principle:** fornecer o mínimo de permissões necessárias
* **roles:** ou funções, são permissões temporárias as recursos e serviços
  * o usuário/aplicação/serviço que assumir uma role perde todas as permissões anteriores enquanto estiver assumindo a role
  * o usuário precisa ter permissão para assumir uma role

## Organizations

* serviço para centralizar várias contas da AWS
  * máximo de 4 contas por padrão, mas é possível acionar a AWS para solicitar mais

* consolida cobrança **~>** descontos
* **Services Control Policies (SCPs):** controle de acesso e recursos por conta ou **OU**
* **Organizational Units (OU):** grupos de contas da AWS

## AWS Artifact

* serviço que fornece acordos e relatórios de segurança e conformidade (realizados por terceiros)

## Costumer Compliance Center

* serviço que fornece artigos e documentos relacionados a conformidade
* estudos de caso
* **auditor learning path:** documentos voltados para clientes em cargos de auditoria

## AWS Shield

* serviço de proteção a ataques de **Distributed Denial of Service (DDoS)**
* **standard:** sem custo, protege todos os usuários
* **advanced:** pago, capaz de previnir ataques mais sofisticados e se integra a outros serviços

## Demais serviços de Segurança

* **Key Management Service (KMS):** gerenciador de chaves de criptografia
* **Web Application Firewall (WAF):** monitoramento e filtro de tráfego usando ACLs
* **Amazon Inspector:** realiza testes de segurança e lista pontos de melhoria
* **GuardDuty:** monitora atividades da conta e da rede em busca de ameaças

# Monitoring and Analytics

## CloudWatch

* monitoramento de métricas em tempo real
* criação de alarmes baseados em métricas (uso de CPU, memória)
* alarmes podem executar ações (desligar uma instância, por exemplo)

## CloudTrail

* registra todas as API calls (toda ação em qualquer recurso da AWS)
  * quem fez a ação, IP, hora, resposta da solicitação
* **CloudTrail Insights:** serviço extra que detecta atividades suspeitas (iniciar mais instâncias que o usual, por exemplo)

## Trusted Advisor

* avalia a infraestrutura e recomenda mudanças em 5 categorias
  * custo, performance, segurança, tolerância à falha e limite de serviço
* para cada categoria
  * **green check:** itens com nenhum problema
  * **orange triangle:** itens a serem investigados
  * **red circle:** itens com ação recomendada

# Pricing and Support

## Free Tier

* **Always Free:** não expira e está disponível a todos os usuários
  * Lambda permite 1 milhão de execuções por mês
* **12 Months Free:** a partir da criação da conta
  * S3, EC2 e CloudFront (todos com limite de uso mensal)
* **Trials:** teste por um período
  * Inspector por 90 dias, Lightsail por 750 horas em 30 dias

## Pricing Concepts

* **Pay for what you use:** pagamento pela exata quantidade de recursos utlizados, sem contratos ou licenças
* **Pay less when you reserve:** alguns serviços permitem reserva (EC2) **~>** economia
* **Pay less when you use more:** alguns serviços custam menos quanto maior o uso (S3) **~>** economia

## Pricing Calculator

* cria estimativas de gastos
* estimativas podem ser separadas em grupos
* gera um link para ser compartilhado

## Billing & Cost Management Dashboard

* monitoramento de gastos
* pagamentos
* compra de Saving Plans
* criação de Budgets

## Consolidated Billing

* centraliza as contas a serem pagas
* disponível para Organizations (várias contas gerenciadas por uma root)
* acúmulo de descontos por maior uso

## Budgets

* definição de orçamentos (somente referência, não limita os gastos)
* criação de alertas baseados em porcentagem de custos

## Cost Explorer

* análise de custos atuais e passados
* distribuídos por serviços, regiões ou tags

## Support Plans

* **Basic:** gratuito para todos, inclui documentação, artigos e fórum
  * Trusted Advisor com recursos limitados
  * pode contatar a AWS para dúvidas no custos ou aumentar limite de serviços
  * **Personal Health Dashboard:** ferramenta que alerta sobre eventos da AWS que podem afetar o usuário
* **Developer:** recomendações de boas práticas, diagnósticos e identificação de oportunidades de melhoria

* **Business:** Trusted Advisor completo, suporte limitado a softwaras de terceiros em instâncias
* **Enterprise:** Technical Account Manager (TAM), resposta em 15 minutos a chamados críticos

## Marketplace

* catálogo de softwares de terceiros que rodam nos serviços da AWS
* alguns produtos permitem a utilização de uma licença previamente adquirida
* grande parte dos produtos oferece pagamento por demanda

# Migration and Innovation

## CAF - Cloud Adoption Framework

* **Business Perspective:** estratégias e objetivos do negócio
* **People Perspective:** análise e treinamento de equipe
* **Governance Perspective:** estrutura e governança
* **Plataform Perspective:** modelos e arquiteturas de migração
* **Security Perspective:** padrões de segurança e controle
* **Operations Perspective:** operações e procedimentos

## Migration Strategies

1. **Rehosting:** mover as aplicações sem alterações (lift-and-shift)
   - ideal para transições iniciais que serão revisitadas futuramente
   - não aproveita todos os benefícios da nuvem
2. **Replataforming:** leves otimizações para o ambiente em nuvem (lift, tinker, and shift)
3. **Refactoring:** mudanças grandes na arquitetura da aplicação
   - uso de features da nuvem
4. **Repurchasing:** atualizar licenças legadas e utilizar soluções Software as a Service
5. **Retaining:** manter a aplicação on premises
   - aplicações críticas ou que necessitam de muitas mudanças para migrar
6. **Retiring:** encerrar aplicações que não têm mais uso

## Snow Family

* dispositivos intermediários para migração
* são instalados on premises para receber os dados e então enviados a AWS
* AWS sobe os dados para a conta do cliente

1. **Snowcone:** 2 CPUs, 4 GB de ram e 8 TB de armazenamento
2. **Snowball Storage Optmized:** 40 vCPUs, 80 GB de ram, 80 TB (HDD) e 1 TB (SSD)
3. **Snowball Edge Compute Optimized:** 52 vCPUs, 208 GB de ram, GPU Tesla, 42 TB (HDD) e 8 TB (NVME)
4. **Snowmobile:** 100 PB

## Innovation

* **serverless:** rodar código sem provisionar ambiente
* **AI**
  * **Transcribe:** converte texto para voz
  * **Comprehend:** localiza padrões em textos
  * **Fraud Detector:** identifica atividades potencialmente fraudulentas
  * **Lex:** criação de chatbots com texto e voz
* **SageMaker:** ferramenta de Machine Learning 

# The Cloud Journey

## Well-Architected Framework

- 6 pilares

1. **Operational Excellence:** monitoramento, automação, documentação, prevenção à falha
2. **Security:** proteção de dados armazenados e em trânsito, automação
3. **Reliability:** recuperação de pequenos problemas e desastres, escalabilidade
4. **Performance Efficiency:** serverless, ser capaz de disponibilizar globalmente em minutos
5. **Cost Optimization:** usar seviços gerenciados (menor custo)
6. **Sustainability:** maximar utilização, redução de impacto
