# Ultimate AWS Certified Cloud Practitioner

Anotações do curso do Stephane Maarek

# What is Cloud Computing?

*Entrega sob demanda de recursos como computação, banco de dados e aplicações*

## Deployment Models

* **Private Cloud:** não exposta na internet, aplicações sensíveis, atender necessidades específicas
* **Public Cloud:** exposta na internet, infraestrutura pertence e é mantida por terceiros
* **Hybrid Cloud:** manter parte dos servidores on premises e conectá-los a recursos em cloud

## Vantagens da Cloud

1. troca de investimento em capital (CAPEX) por investimento operacional (OPEX)
   - pagamento por demanda
   - redução do Total Cost of Ownership (TCO) e Operational Expense(OPEX)
2. grande economia em escala
   - quanto mais usuários, mais eficiente a AWS disponibiliza os recursos
3. não é necessário estimar capacidade
4. aumento de velocidade e agilidade
5. sem gastos com operação e manutenção de data centers
6. global em minutos

## Tipos de Cloud Computing

Com infra on premises, é necessário gerenciar networking, storage, servers, virtualization, OS, middleware, runtime, data e applications

* **IaaS - Infrastructure as a Service:** necessário gerenciar a partir do OS (EC2)
* **Pass - Plataform as a Service:** necessário gerenciar data e applications somente (Beanstalk)
* **Saas - Software as a Service:** não é necessário gerenciar nada (Rekognition)

## Princípios do Modelo pay as you go

* **compute:** pagamento por tempo de uso
* **storage:** pagamento por dados armazenados
* **data transfer:** pagamento somente para saída de informações da cloud
  * entrada de dados é gratuita

## Global Infrastructure

* **Regions:** grupo de 3 a 6 Availability Zones
  * regiões têm nomes **~>** region de Sydney tem nome ap-southeast-2
  * muitos serviços são regionais, mudar de região é como iniciar novamente do zero
  * serviços globais: IAM, Route 53, CloudFront e WAF
  * considerar na escolha: compliance, latência, serviços disponíveis e preço
* **Availability Zones:** um ou mais data centers
  * AZs têm letras **~>** ap-southeast-2**a**, ap-southeast-2**b** e ap-southeast-2**c**
  * separadas fisicamente contra desastres
* **Edge Location:** entrega de conteúdo com baixa latência
  * também conhecidas como Points of Presence



# IAM - Identity and Access Management

* Global
* grupos só podem conter usuários (não podem conter outros grupos)
* usuários podem pertencer a nenhum grupo, um ou mais
* políticas são aplicadas a usuários ou a grupos
* **least privilege principle:** dar o mínimo de permissões necessárias
* **políticas de senha:** tamanho, exigir caracteres especiais, tempo de expiração
* **MFA:** virtual (App), Universal 2nd Factor (USB), Hardware Key Fob Device (display)
* na CLI, se um usuário perde permissões, ao tentar usar um recurso a saída é em branco
* na CloudShell, arquivos criados são persistentes
* **roles:** permissões para recursos em vez de usuários
  * usos mais comuns são de EC2 e Lambda

* **Credential Report:** csv com informações dos usuários **~>** a nível de CONTA
* **Access Advisor:** lista de serviços/permissões com último uso ou nunca usados **~>** nível de USUÁRIO



# EC2 - Elastic Compute Cloud

* **Bootstrap:** rodar comandos (como sudo) na primeira vez que uma instância inicia
  * colocar código no campop User Data


## Security Group

* vinculado à região/VPC: precisa criar outro se trocar de região ou de VPC
* porta 21 **FTP**
* porta 22 **SSH** e **SFTP**
* porta 80 **HTTP**
* porta 443 **HTTPS**
* porta 3389 **RDP**

## SSH

**Bad Permissions** no windows: propriedades o arquivo **~>** segurança **~>** avançado

* Quebrar herança
* Adicionar seu usuário como owner. full control e único usuário

## Saving plans

* comprometimento de uso (em dolares por mês)
* uso a mais é cobrado como On Demand

## Capacity Reservations

* define a capacidade em uma AZ
* cobrado como On Demand independente do uso

# EC2 Instance Storage

## EBS - Elastic Block Storage

* atachado a uma única instância da mesma AZ
  * se fizer snapshot é possível restaurar em outra AZ
* atachado via network **~>** pode ser feito a quente
* necessário definir capacidade e velocidade (IOPS) no provisionamento
* **Delete on termination:** atributo que pode ser desmarcado
  * por padrão, o primeiro é marcado para ser deletado e os demais não

* **snapshots:**
  * é possível copiar de uma região para outra
  * é possível ativar a lixeira e manter os snapshots apagados
  * é possível mover o snapshot para a classe **Snapshot Archive** que é 75% mais barata

## AMI - Amazon Machine Image

* pode ser usada para instalar seus próprios programas
* pose der usada pra usar suas própias licenças
* Para criar uma AMI
  1. Iniciar uma instância e instalar os programas necessários
  2. Para a instância para manter integridade dos dados
  3. Criar a AMI (cria também snapshots)
  4. Iniciar uma instância com a AMI

## EC2 Image Builder

* automação para criação de vms ou containers

## EC2 Instance Store

* marmazenamento efêmero **~>** perdido quando a instância é encerrada
* alta performance
* buffer, cache e conteúdo temporário

## EFS - Elastic File System

* é um Network File System
* pode ser atachado a 100 instâncias simultaneamente
* somente linux
* tem a classe **Infrequent Access** para otimizar custos

## Shared Responsability Model 

* **AWS:** replicação de EBS e EFS, troca de hardware defeituoso, proteger os dados de serem acessados pelos colaboradores
* **Usuário:** backup, snapshots, encriptação, responsável pelos dados armazenados, entender os riscos de se usar Instance Store

## FSx

* file system de alta performance
* serviço de terceiros
* 2 principais
  * **Windows File Server:** para instâncias com Windows ou para máquinas, integrado ao AD
  * **Lustre:** para HPC (High Performance Computing), para Linux

# Elastic Load Balancing e Auto Scaling Groups

* escalabilidade vertical somente para aplicações não distribuídas **~>** banco de dados
* high availability **~>** pelo menos 2 AZs
* **escalabilidade:** capacidade do sistema de acomodar aumento de carga por aumento de hardware (vertical) ou por aumento de nós (horizontal)
* **elasticidade:** havendo escalabilidade, o sistema é capaz de auto escalar, se ajustando com as variações de carga
* **agilide:** disponibilidade de recursos a poucos cliques e minutos na nuvem, comparado a semanas on premises

## ELB - Elastic Load Balancer

* o Load Balancer que fica exposto à internet
* responsável por distribuir o tráfego entre instâncias (back end)
* concentra um único DNS para a aplicação
* caso uma instância falhe, ela é ignorada e o tráfego encaminhado a outra **~>** usuário não percebe a falha
* providencia certificado SSL
* AWS cuida de manutenções e garante a alta disponibilidade
* tipos:
  * **Application:** http, https e gRPC (camada 7)
    * roteamentos http e DNS estático
  * **Network:** alta performance, TCP/UDP (camada 4)
    * IP estático
  * **Gateway:** protocolo Geneve (camada 3)
    * detecção de intrusão, direciona o tráfego para firewall do usuário em EC2
  * **Classic:** depreciado (camada 4 e 7)

## ASG - Auto Scaling Groups

* trabalha junto com o Load Balancer
* necessário criar um **Lauch Template** que é então vinculado ao LB
* pode escalar manualmente
* escalar dinamicamente baseado em:
  * triggers do CloudWatch de uso de CPU
  * carga específica **~>** manter CPU em torno de 40%
  * agendado **~>** horários/dias em que se sabe que haverá maior/menor consumo
  * preditivo (Machine Learning)

# S3 - Simple Storage Service

* buckets são **regionais**
* arquivos armazenados como objetos
  * key: caminho completo até o arquivo
  * 5tb no máximo por objeto
* o nome do bucket deve ser único em toda AWS
* versionamento é configurado a nível do bucket
  * rollback:
    1. ativar a a chave show versions
    2. deletar o item (irreversível)
* arquivos deletados recebem uma flag
  * são mostrados quando ativada a chave show versions
  * para recuperar deve selecionar o arquivo com a flag e deletá-lo (irá deletar a flag)
* todo arquivo é encriptado na AWS (server side)
  * além dessa encriptação, o usuário também pode encriptar antes de fazer o upload (client side)


## Segurança

* baseada em usuários do IAM
* baseada em Bucket Policies **~>** permite acesso de outras contas
* baseada em Object ACLs
* baseada em Bucket ACLs

## Replicação

* ativar versionamento nos dois buckets
* Cross Region Replication ou Same Region Replication
* podem ser buckets da mesma conta ou de contas diferentes

## Classes

* **durabilidade:** capacidade de não perder arquivos
  * para um armazenamento de 10 000 000 arquivos, espera-se a perda de 1 arquivo a cada 10 000 anos
  * todas as classes tem a mesma durabilidade

* **disponibilidade:** capacidade de acessar o arquivo
  * depende da classe (por exemplo Standand fica indisponível 53 min por ano)

### Standard

* 99.99% disponível
* dados frequentemente acessados
* baixa latência e alta transferência
* suporta falha em 2 instalações simultâneas
* Big Data, games, distribuição de conteúdo

### Standard Infrequent Access

* 99.9% disponível
* dados pouco acessados, porém com acesso rápido
* mais barata que Standard
* Recuperação de desastres, backups

### One-Zone Infrequent Access

* 99.5% disponível
* perde-se os dados se a AZ falhar
* segunda cópia de backup, dados facilmente recriados

### Glacier Instant Retrieval

* custos por armazenamento e restauração
* restauração em milisegundos
* mínimo de 90 dias de armazenamento

### Glacier Flexibel Retrieval

* mínimo de 90 dias de armazenamento
* 3 classes internas de recuperação:
  * expedited **~>** 1 a 5 minutos
  * standard **~>** 3 a 5 horas
  * bulk **~>** 5 a 12 horas **sem custo**

### Glacier Deep Archive

* mínimo de 180 dias de armazenamento
* classe mais barata
* 2 classes internas de recuperação:
  * standard **~>** 12 horas
  * bulk **~>** 48 horas

### Intelligent Teiring

* move automaticamente os objetos entre as classes
* baseado no acesso
* não há custo pela recuperação
* funcionamento:
  * todos arquivos ficam no Frequent Access Tier
  * sem acesso por 30 dias **~>** Infrequent Access Tier
  * sem acesso por 90 dias **~>**  Archive Instant Access Tier
  * opcional configurável por 90 a 700+ dias **~>** Achive Access Tier
  * opcional configurável por 180 a 700+ dias **~>** Deep Archive Access Tier
* podem ser definidas **Lifecycle Rules** para transitar arquivos entre as classes
  * essa configuração é feita independente da classe Intelligent Tiering


## Shared Responsability Model

* **AWS:** infraestrutura, configuração e análise de vulnerabilidades, compliance
* **Usuário:** versionamento, políticas, replicação, monitoramento e encriptação

## Storage Gateway

* ponte entre on premises e cloud
* disponibilização de arquivos, volumes ou fitas
* "allow on premises servers to **seamlessly** use cloud"

# Snow Family

* **OpsHub:** software instalado nas máquinas locais para controlar os dispositivos **Snow**
* opções de pagamento por demanda (dia), por mês, adiantado por 1 ou 3 anos 

## Migração de Dados

* dispositivos offline
* custos por dados transferidos
* armazenamento em blocos ou objetos (compatível com S3)
* após a transferência par ao S3 os dispositivos são apagados
* **Snowcone:** até 8 TB de HDD
  * compacto, resistente a intempéries
  * armazenamento offline ou online (já tem o DataSync instalado)
  * recomendado para migrações até 24 TB
* **Snowcone SSD:** até 14 TB de SSD
* **Snowball Edge Storage Optmized:** até 80 TB de HDD
  * armazenamento offline
  * arranjo de até 15 nós
  * recomendado para migrações de Petabytes
* **Snowball Edge Compute Optimized:** até 28 TB de SSD NVMe
* **Snowmobile:** até 100 PB
  * caminhão monitorado 24/7 via vídeo e GPS
  * temperatura controlada
  * recomendado para migrações de Exabytes

## Edge Computing

* computação em áreas remotas, como caminhões, navios e minas
* internet limitada ou sem internet
* pré processamento de dados, machine learning, transcrição de mídia
* pode ter os dados migrados para o S3 posteriormente
* todos podem rodar EC2 e Lambda através do IoT Greengrass
* **Snowcone:** 2CPUs, 4GB ram, alimentado por usb ou bateria
* **Snowball Edge Compute Optimized:** 104 vCPUs, 416 GB ram, GPU opcional
* **Snowball Edge Storage Optimized:** 40 vCPUs, 80 GB ram

# Databases & Analytics

## RDS - Relational Database Service

* SQL **~>** Postgres, MySQL, MariaDB, Oracle, SQL Server ou Aurora 
* gerenciado pela AWS
  * SO atualizado
  * backups automáticos **Point in Time**
  * multi AZ
  * Read Replicas
    * leituras simlutâneas mais rápidas
    * até 15 réplicas (na mesma região ou em outra)
  * escalável (vertical e horizontal)
  * monitoramento via dashboard
  * NÃO É POSSÍVEL ACESSAR A INSTÂNCIA VIA SSH

## Aurora

* gerenciado pela AWS
* Postgres ou MySQL
* Otimizado para Cloud
  * 5x mais rápido que o MySQL no RDS
  * 3x mais rápido que o Postgres no RDS
* armazenamento escalável automaticamente a cada 10gb (até 128tb)
* mais caro que RDS (20%)

## DocumentDB

* MongoDB **~>** NoSQL
* banco de documentos JSON
* gerenciado pela AWS
* otimizado para Cloud
* armazenamento escalável automaticamente a cada 10gb (até 64 tb)
* milhões de requisições por segundo

## ElasticCache

*  in-memory database **~>** alta performance, baixa latência
* reduz a carga de leitura no banco
* Redis e Memcached
* gerenciado pela AWS

## DynamoDB

* NoSQL **~>** par chave-valor
* "single digit millisecond latency" **~>** altíssima performance
* milhões de requisições por segundo, trilhões e linhas
* gerenciado pela AWS e serveless (o banco já existe, o usuário cria as tabelas)
* escalável
* classes de tabelas Standard e Infrequent Access para redução de custos
* **DAX - DynamoDB Accelerator:** in-memory cache para o DynamoDB
* **Global Table:** torna a tabela acessível de várias regiões a baixa latência

## Redshift

* postgres
* data warehousing
* **OLAP:** online analytical processing **~>** analystics and data warehousing
* carrega dados a cada hora
* armazenamento em colunas
* integração com Quicksight, Tableua e outras ferramentas de BI

## EMR - Elastic MapReduce

* Hadoop clusters: união de instâncias EC2 para processamento de dados
* big data e machine learning

## Athena

* serveless
* analytics nos dados armazenados no S3
* SQL

## QuickSight

* criação de dashboards com uso de machine learning
* analytics visualizações
* integração com RDS, Aurora, Athena, Redshift, S3

## Neptune

* banco de grafos
* gerenciado pela AWS
* alta disponibilidade
* pode armazenar bilhões de relações entre os nós
* mile segundos de latência
* artigos relacionados (wikipedia), detecção de fraude, engines de recomendações, redes sociais

## QLDB

* Quantum Ledger Database
* armazenamento de transações financeiras CENTRALIZADO
* serverless de alta disponibilidade
* usa SQL
* immutable journal
* verificado com criptografia

## Managed Blockchain

* decentralizado
* usado para:
  *  se juntar a uma rede pública de blockchain
  * criar uma rede privada
* compatível com Hyperledger Fabric e Ethereum

## Glue

* gerencia a extração, transformação e carregamento de dados
* dados podem ser extraídos do S3 e RDS, transformados e jogados no Redshift para análise
* usado para preparar dados para serem analisados

## DMS - Database Migration Service

* migração de bancos
* banco de origem continua disponível durante a migração
* migração homogênea: Oracle para Oracle
* migração heterogênea: SQL Server para Aurora

# Outros Serviços de Computação

## ECS - Elastic Container Service

* o usuário provisiona instâncias EC2 para rodar os containers

## Fargate

* serverless
* o usuário não precisa provisionar instâncias
* **ECR - Elastic Container Registry:** registro privado para armazenar imagens de containers

## Lambda

* Function as a Service
  * thumbnail de imagens do S3, cron job
* serverless
* compatível com várias linguagens
* roda funções curtas sob demanda **~>** limite de tempo
* escalável automaticamente
* custos por número de vezes invocadas e gb.s de computação
* event-driven: uma função é invocada mediante um evento/trigger
  * um evento pode ser um arquivo sendo carregado no S3
  * um trigger pode ser definido no EventBridge

## API Gateway

* criação, manutenção e monitoramento de APIs serverless
* expôe funções Lambda como APIs HTTP

## Batch

* semelhante ao Lambda
* sem limite de tempo, mas com tempo definido
* roda imagens de docker
* usa EBS/instance store para armazenamento

## Lightsail

* virtual servers, banco de dados,
* baixo custo e previsibilidade

# Deployments e Infraestrutura em Escala

## CloudFormation

* Infrastructure as Code
* JSON e YAML
* templates
* usado quando se precisa repetir ambientes em diferentes regiões/contas
* estimador de custos baseado no template
* deletando a stack deleta também todos os recursos que ela criou
* serviço gratuito, paga somente pelos recursos que criar

## CDK - Cloud Development Kit

* uso de linguagens de programação para Infrastructure as code
* o código é transformado em um template de CloudFormation

## Elastic Beanstalk

* Plataform as a Service
  * usa CloudFormation por trás dos panos
* centro de deploy com as ferramentas necessárias para um DEV
* dashboard de monitoramento
* serviço gratuito, paga somente pelos recursos que criar

## CodeDeploy

* deploy/upgrade de versão automático
* funciona com EC2 e on premises **~>** serviço HÍBRIDO

## CodeCommit

* repositório de código
* compatível com git

## CodeBuild

* compila, testa e gera pacotes para deploy
* CodeCommit **~>** CodeBuild **~>** Deploy

## CodePipeline

* CICD
* compatível com CodeCommit, CodeBuild, CodeDeploy, GirHub

## CodeArtifact

* armazenamento de dependências
* compatível com  Maven, Graddle, npm, yarn, twine, pip e NuGet

## CodeStar

* UI unificada para desenvolvimento **~>** orchestrate
* "quick way to get started"
* permite a edição de código na nuvem com Cloud9

## Cloud9

* IDE em nuvem
* permite colaboração simultânea na edição de código

## SSM - System Manager

* shell de gerenciamento de EC2 e on premises **~>** serviço HÍBRIDO
  * fecha a porta 22 para ssh **~>** mais seguro
  * necessário instalar agente
  * Amazon Linux e Ubuntu AMI já vêm com ele instalado
* suíte com mais de 10 produtos
  * patch automático
  * rodar comandos em todos os servidores
  * salvar parâmetros com SSM Parameter Store
* compatível com Windows, Linux, MacOs e Raspbian
* "get operational insight of your infrastructure"

## OpsWorks

* alternativa ao SSM
* Chef e Puppet
* configuração automática de servidores
* tarefas repetitivas
* EC2 e on premises
  * instâncias, bancos, load balancer, volumes EBS

# Infraestrutura Global

* menor latência
* recuperação de desastre
* preteção contra ataques

## Route 53

* gerenciador de DNS
* registros:
  * **A:** hostname para IPv4
  * **AAAA:** hostname para IPv6
  * **CNAME:** hostname para hostname
  * **alias:** hostname para um recurso da AWS

* Routing Policies:
  * **Simple Routing Policy:** uma única instância EC2
  * **Weighted Routing Policy:** várias instâncias com porcentagens (pesos) diferentes de tráfego
  * **Latency Routing Policy:** várias instâncias, tráfego direcionado para a instância mais próxima (menor latência)
    * criar um registro para cada instância
  * **Failover Routing Policy:** 2 instâncias (principal e failover), checa a saúde da principal e envia para a failover em caso de falha

## CloudFront

* serviço global
* **CDN:** Conten Delivery Network
* faz cache de conteúdo nas **Edge Location** (ou Points of Presence) **~>** melhora a performance
* a distribuição ajuda conta ataques DDoS
  * junto com Shield e Web Application Firewall
* pode apontar para S3 (para cache ou para upload de arquivos)

|              CloudFront               |       S3 Cross Region Replication        |
| :-----------------------------------: | :--------------------------------------: |
|                  CND                  | Precisa ser configurado para cada região |
|     Cache temporário de arquivos      |   Arquivos upados quase em tempo real    |
| Conteúdo estático para qualquer lugar |  Conteúdo dinâmico para algumas regiões  |

## S3 Transfer Acceleration

* upload para uma Edge Location e dela para o bucket
* a conexão entre a Edge Location e o bucket é privada da AWS **~>** mais rápida

## Global Accelerator

* cliente se conecta na Edge Location e dela na aplicação
* a conexão entre a Edge Location e a aplicação é privada da AWS **~>** 60% mais rápida

## Outposts

* servidores da AWS on premises **~>** serviço híbrido
* baixa latência e facilidade para migração

## WaveLength

* baixíssima latência aos serviços através de rede 5G
* cidades inteligentes, AR/VR, veículos inteligentes

## Local Zones

* "extension of an AWS Region"
* expande a vpc para outras regiões
* a reigão precisa ter Local Zones disponíveis para serem habilitadas

## Global Applications Architecture

* **Single Region, Single AZ:** sem alta disponibilidade, alta latência global
* **Single Region, Multi AZ:** alta disponibilidade, alta latência global

* **Active-Passive:** região principal (active) com leitura e escrita, região replicada (passive) somente leitura
  * alta disponibilidade, baixa latência global para leitura
* **Active-Active:** todas as regiões permitem escrita
  *  alta disponibilidade, baixa latência global

# Cloud Integration

## SQS - Simple Queue Service

* serveless
* decoupled applications
* mensagens compartilhadas
* pull-based system
* a fila pode ser ordenada (FIFO, First In First Out)

## SNS - Simple Notification Service

* decoupled applications
* "pub/sub"
* publisher só mandam mensagem para um tópico
* cada subscriber recebe todas as mensagens
* email, Lambda, SQS, mobile

## MQ

* serviços de mensagem e fila on premises
* compatível com RabbitMQ e ActiveMQ
* usado em migrações que usam esses protocolos

## Kinesis

* real time big data streaming

# Cloud Monitoring

## CloudWatch

* métricas de recursos
* possível criar dashboards com as métricas  desejadas
* possível criar alarmes para notificar quando uma métrica atingir determinado valor
  * para EC2, os alarmes podem ser criados na página da instância
* EC2: cpu, network (não tem monitoramento de RAM)
* Billing Alarm disponível somente no Norte da Virgínia (us-east-1)
* log agent pode ser instalado em uma instância ou on premises

## EventBridge

* **Schedule:** cron jobs, agendamentos
* **Event Patterns:** reação à ação de algum serviço
  * exemplo: quando o root fazer login enviar um email pelo SNS
* pode ser associado a eventos de terceiros, como Zendesk e Datadog

# CloudTrail

* registro de todas as ações que acontecem na conta: console, sdk, cli
* ativado por padrão

# X-Ray

* troubleshooting de performance e dependências
* visualização com grafos

# CodeGuru

* **Reviewer:** revisão de código
* **Profiler:** monitoramento de performance
* usa Machine Learning

# Health Dashboard

* **Service History:** mostra todas as regiões e serviços, eventuais problemas
* **Your Account:** mostra alertas e recomendações quando eventos podem causar impacto na sua infra

# VPC & Networking

* **Virtual Private Cloud:** rede privada pra implantação de recursos (restrita a uma região)
* **Subnets:** partição da rede (restrita a uma AZ)
* **Internet Gateway:** conecta a VPC à internet
  * subnets públicas já possuem rota por padrão
* **NAT Gateway:** conecta VPC privada a internet mantendo-a privada

## NACL & Security Groups

* **Network Access Control List:** firewall a nível de subnet
  * regras de negação e permissão
  * somente IPs
  * stateless: tráfego de retorno precisa ser liberado
* **Security Groups:** firewall a nível de instância ou ENI (Elastic Network Interface)
  * somente regras de permissão
  * IPs ou outros security groups
  * stateful: tráfego de retorno é liberado automaticamente

## VPC Flow Log

* faz log de informações da VPC

* desativado por padrão

## VPC Peering 

* conecta 2 VPCs usando conexão da AWS
* conectar A com B e B com C não faz A se conectar com C automaticamente

## VPC Endpoing

* conecta dois serviços da AWS através de uma rede privada
* **Endpoint Gateway:** S3 e DynamoDB
* **Endpoint Interface:** demais serviços

## PrivateLink

* conecta uma VPC a outra de um vendedor do Market Place de forma segura
* no vendedor é criado um Network Load Balancer
* no cliente é criada uma Elastic Network Interface

## Site to Site VPN

* VPN entre infra on premises e a sua VPC
  * on premises é criado um Costumer Gateway (CGW)
  * na VPC é criado um Virtual Private Gateway (VGW)
* encriptada
* conecta pela rede pública

## DX - Direct Connect

* conexão física da infra on premises com a VPC
* mais segura e mais rápida

## Client VPN

* conecta um computador à instância EC2 pelo IP privado
* usa OpenVPN

## Transit Gateway

* concentrador de conexões (milhares, incluindo on premises)
* topologia em estrela ou hub-to-spoke

# Security & Compliance

## Shared Responsibility Model

* **AWS:** segurança da nuvem
  * proteger a infra, hardware, software, instalações e rede
  * gerenciar serviços como S3, DynamoDB, RDS...
* **cliente:** segurança na nuvem
  * em EC2, gerenciamento do SO, firewall e rede
  * usuário e roles do IAM
  * encriptação de dados
* **Controle Compartilhado**
  * gerenciamento de patchs, configuração de gerenciamento, vigilância e treinamento

## Proteção contra DDoS

* Shield
  * sua ÚNICA FUNÇÃO é proteger de DDoS
  * Standard está disponível para todos sem custo adicional **~>** ataques mais simples
  * Advanced para proteção mais sofisticada 24/7
  
* Web Application Firewall
  * camada 7 (HTTP)
  * regras podem bloquear iIP, cabeçalho ou corpo de HTTP e URI
  * protege contra SQL injection e Cross-Site Scripting
  * bloqueio de países
  * bloqueio por limite de taxa de requisição
* CloudFront, Route53 e Auto Scaling
  * atuam na disponibilidade e distribuição de carga

## Network Firewall

* protege a VPC inteira
* da camada 3 a 7
* entrada e saída

## Penetration Testing

* pode ser feito teste de penetração sem aprovação prévia para 8 serviços
* **NÃO** pode ser feito ataque DDoS ou flooding (que também são penetration test)

## Encriptação

* at rest: dados armazenados
* in transit: dados em transferência
* são usada chaves de encriptação
* **KMS - Key Management Service:** AWS gerencia as chaves
* **CloudHSM:** o usuário gerencia as chaves, hardware específico

## ACM - AWS Certificate Manager

* criação de certificado SSL/TLS
* compatível com TLS privados e públicos 
* gratuito para TLS público

## Secrets Manager

* armazenamento e gerenciamento de senhas
* programação para rotação de senhas
* geração de senhas com lambda
* integração com RDS

## Artifact

* relatórios de segurança e compliance auditados por terceiros como ISO, PCI e SOC
* acordos como BAA e HIPAA
* relatório e acordos são disponibilizados para download

## GuardDuty

* descoberta de ameaças usando Machine Learning
* protege contra CryptoCurrency attacks
* principais features: VPC Flow Logs, CloudTrail Logs e DNS Logs
* integração com EventBridge

## Inspector

* testes automatizados de vulnerabilidades
* integração com Security Hub e EventBridge

## Config

* salva as alterações feitas nos recursos com o tempo
* esses dados podem ser armazenados no S3 e analisados no Athena
* pode se integrar ao SNS para notificar alterações
* verifica o estado (compliant/noncompliant) dos seviços baseado em regras de segurança definidas pelo usuário

## Macie

* busca por informações pessoais no S3
* Personal Identifiable Information (PII)
* usa Machine Learning

## Security Hub

* concentra as ferramentas de segurança em um dashboard
* gerenciamento de segurança para várias contas
* checagens automáticas de segurança

## Detective

* análise profunda da causa de falhas de segurança
* "root of issues"
* usa Machine Learning

## Abuse

* reportar a AWS IPs ou recursos que estão realizando coisas ilegais ou abusivas

## Privilégios do usuário Root

* não usar root constantemente
* somente o root pode:
  * alterar configurações de conta (nome, email, etc)
  * fechar a conta da AWS
  * restaurar permissões de IAM
  * alterar ou cancelar plano de suporte
  * registrar como vendedor no Reserved Instance Marketplace

## IAM Access Analyzer

* localiza serviços compartilhados externamente
* define uma Zone of Trust

# Machine Learning

## Rekognition

* localiza e identifica em fotos e vídeos
* objetos, texto, pessoas e cenas

## Transcribe

* converte voz para texto
* usa Deep Learning Automatic Speech Recognition (ASR)
* remove automaticamente informações pessoais (PII)

## Polly

* converte texto em áudio

* usa Deep Learning

## Translate

* traduz grandes volumes de texto
* ideal para sites e aplicações

## Lex

* tecnologia por trás da Alexa
* usa ASR
* usado para criar chatbots e call center bots

## Connect

* contact center in cloud
* pode se integrar a outros CRMs

## Comprehend

* Natural Language Processing (NPL)
* serverless
* identifica e organiza textos por tópicos
* usado para analizar emails de clientes e organizar artigos

## SageMaker

* criação de modelos de Machine Learning

## Forecast

* fazer previsões
* vendas, planejamento financeiro

## Kendra

* serviço de pesquisa em documentos
* extração de respostas a partir de pdf, doc, html, power point entre outros

## Personalize

* serviço de recomendações personalizadas
* usados em lojas de varejo, mídia e entretenimento

## Textract

* extração de texto a partir de imagens escaneadas
* documentos, textos escritos a mão

# Account Management, Billing & Support

## Organizations

* serviço global
* gerenciamento de várias contas (filhas) em uma só (master)
* consolidação de contas **~>** faz somente o pagamento da master
* agregação de recursos **~>** redução de custos por volume
* reservas de EC2 podem ser usadas por qualquer conta filha
* **SCP - Service Control Policies:** restrição de recursos das contas filhas

## Control Tower

* serviço para automatizar a criação de Organizations
* poucos cliques
* usa boas práticas

## RAM - Resource Access Manager

* compartilhar recursos entre contas

* é possível compartilhar VPC, EC2, Aurora, Transite Gateway, Route 53 entre outros

* Resource Groups usam TAGs para definir os grupos

## Service Catalog

* criação de templates para disponibilização de recursos
* o admin cria os templates e atribui aos usuários

## Pricing Models

* pay as you go
  * custos de EC2 com Linux por SEGUNDO

* save when you reserve
* pay less by using more
* pay less as AWS grows
* volumes EBS: custos por tamanho do volume (independente do uso), por snapshots
* CloudFront: valores diferentes por região, custos por http/https requests
* serviços gratuitos: IAM, VPC, Consolidated Billing, Elastic Beanstalk, entre outros

 ## Savings Plan

* comprometimento em dinheiro por hora por 1 ou 3 anos
* mais fácil definir em dinheiro do que em recursos
* definido na página do Cost Explorer
* tipos:
  * EC2 **~>** 72% de economia
  * Compute: EC2, Fargate e Lamba **~>** 66% de economia
  * Machine Learning

## Compute Optimizer

* recomendações baseadas em Machine Learning
* redução de custos e melhora de performance
* compatível com EC2, ASG, EBS e Lambda

## Pricing Calculator

* usado para decidir migração para Cloud
* estimativa de custos mensais e anuais
* baseado em recursos selecionados

## Billing Dashboard

* distribuição de custos por recurso
* previsão do mês

## Free Tier Dashboard

* mostra % de uso de cada recurso de Free Tier

## Cost Allocation Tags

* definir tags para recursos
* é possível exportar uma planilha com os custos separados por tags

## Cost and Usage Reports

* relatórios detalhados de custos
* é a forma mais detalhada de exibir os custos
* podem ser integrados a Athena, Redshift ou QuickSight

## Cost Explorer

* visualização de custos e uso por tempo
* criação de relatórios personalizados
* previsão de uso até 12 meses (baseado em uso prévio)
* nessa página que é definido o Savings Plan

## Budget

* criação de alarmes
* alarmes de uso e de previsão de uso
* **tipos:** Cost, Usage, Savings Plan e Reservation

## Cost Anomaly Detection

* monitoramento contínuo com Machine Learning
* envia relatórios com a causa raiz da anormalidade

## Trusted Advisor

* analisa a conta e faz recomendações
* categorias:
  * **Cost Optimization**
  * **Performance**
  * **Security**
  * **Fault Tolerance**
  * **Service Limits**
* **Support Plan:** Basic e Developer **~>** 7 checks
  1. permissões nos buckets do S3
  2. portas do security groups
  3. mínimo de uma conta IAM
  4. MFA na conta root
  5. snapshots públicas no EBS
  6. snapshots públicas no RDS
  7. Limite de serviços
* **Support Plan:** Business e Enterprise **~>** Todos os checks
  * pode habilitar alarmes do CloudWatch quando atingir limites
  * AWS Support API

## Support Plans

* **Basic**: acesso 24x7 a customer service, documentação, papers e fóruns 

* **Developer:** Basic + 
  * Cloud Support Associates (email)
  * abertura de tickets ilimitados (1 por vez)
  * tempo de resposta de 24h (geral) ou 12h (infra prejudicada)
* **Business:**  production
  * Full Trusted Advisor + API
  * 24x7 telefone, email ou chat com Cloud Support Engineers
  * abertura de tickets ilimitados (sem limite de tickers simultâneos)
  * acesso Infrastructure Event Management (custo adicional)
  * tempo de resposta: 
    * geral **~>** 24h
    * infra prejudicada **~>** 12h
    * produção prejudicada **~>** 4h
    * produção parada **~>** 1h

* **Enterprise On Ramp:** production or business critical
  * acesso a pool de Technical Account Managers (TAM)
  * acesso ao Concierge Support Team para boas práticas de conta e custos
  * Infrasctructure Event Management, Well-Architected & Operations Reviews
  * tempo de resposta:
    * produção prejudicada **~>** 4h
    * produção parada **~>** 1h
    * business critical system down **~>** 30 minutos
* **Enterprise:** mission critical
  * acesso a um TAM designado
  * tempo de resposta business critical de 15 minutos

# Advanced Identity

## STS - Security Token Service

* criação de credenciais temporárias e limitadas
* o usuário/serviço assume uma role com essas credenciais

## Cognito

* dar uma identidade aos usuários de aplicações (capacidade de milhões de usuários)
  * só criar IAM para pessoas da sua empresa
* criação de usuários e senha para aplicações Web e Mobile
* integração com redes sociais para login

## Directory Services

* **Simple AD:** é um AD na nuvem, sem integração
* **AD Connector:** direciona ao AD on premises, suporta MFA
* **Managed Microsoft AD:** sistema de confiança entre o AD on premises e em nuvem, suporta MFA

## IAM Identity Center

* sucessor do Single Sign-On
* um único login para vários recursos:
  * contas dentro da Organizations
  * aplicações como Office 365, Salesforce
  * instâncias Windows

# Other Services

## WorkSpaces

* Desktop as a Service
* Windows e Linux
* usado para substitui TS em on premises
* integrado com KMS
* pode ser provisionado em várias regiões para reduzir latência

## AppStream 2.0

* acessa as aplicações direto do navegador
* Desktop Application Stream Service
* usa aplicações sem provisionar infra (blender, libreoffice..)

## IoT Core

* conecta dispositivos inteligentes a AWS
* capacidade de conectar bilhões de dispositivos

## Elastic Transcoder

* converte arquivos de mídia armazenados no S3
* custo por tempo de processamento

## AppSync

* backend para aplicações web e mobile
* usa GraphQL
* integrado com DynamoDB e Lambda

## Amplify

* conjunto de ferramentas para aplicações full stack
* cria o backend baseado na stack definida

## Device Farm

* serviço para testar aplicações web e mobile
* faz teste com navegadores e diferentes dispositivos (celulares, tablets)

## Backup

* backups por demanda ou agendados
* suporta Point in Time Recovery
* pode ser feito em múltiplas regiões e múltiplas contas dentro da Organizations

## Disaster Recovery Strategies

* **Backup and Restore:** **~>** a mais barata
* **Pilot Light:** somente as funções principais do serviço prontas, sem escalabilidade
* **Warm Standby:** todas as funções prontas, sem escalabilidade

* **Multi-site/Hot-site:** todas as funções e escalabilidade

## DRS - Elastic Disaster Recovery

* replicação contínua de ambiente on premises na AWS
* criado ambiente de staging com instâncias mínimas
* no caso de desastre, ativa o failover e é criado ambiente de produção com instâncias maiores

## DataSync

* mover ou replicar grande quantidade de dados de on premises para AWS
* armazenamento em S3, EFS ou FSx for Windows
* replicação passa a ser **incremental** após full load

## Application Discovery Service

* verifica o ambiente on premises para definir as dependências para migração
* pode ser feito com agente (mais completo) ou sem
* resultados no Migration Hub

## MGN - Application Migration Service

* serviço de rehost **~>** Lift-and-Shift
* converte servidores (físicos, virtuais ou em outras clouds) para rodarem nativamente na AWS

## Migration Evaluator

* coleta de dados para definir o plano de migração

## FIS - Fault Injection Simulation

* injection experiments based on Chaos Engineering
* simulação de excesso de uso do processador, memória, falha no banco...
* usado para descobrir bugs e gargalos de performance

## Step Functions

* criação visual de workflows usando grafos
* compatível com Lambda, EC2, ECS, on premises entre outros

## Ground Station

* comunicação de dados com satélites em órbita

## Pinpoint

* campanhas de marketing
* envio de emails, sms e push notifications
* criação de templates e agendamentos

# Architecting & Ecosystem

## Well Architecting Framework

* não estimar capacidade **~>** usar auto scaling
* testar sistemas em produção
* automatizar arquitetura (Infrastructure as Code)
* servidores devem ser descartáveis e fáceis de configurar
  * não configurar demais um sevidor e automatizar sempre
* usar componentes desacoplados
* pense em serviços, não servidores
* 6 pilares

## 1 - Operational Excellence

* *habilidade de rodar e monitorar sistemas para entrega de valor e continuamente melhorar*
* **Infrasctructure as code ~> ** CloudFromation e CI/CD
* documentação
* fazer mudanças constantes, pequenas e reversíveis
* refinar processos
* antecipar falhas e aprender com elas

## 2 - Security

* *habilidade de proteger informações e sistemas para entrega de valor através de testes de risco e estratégias de mitigação*
* centralizar controle e identificar as pessoas/serviços e seus acessos **~>** IAM
* princípio do mínimo privilégio
* aplicar segurança em todas as camadas
* ter acesso ao log de tudo
* automatizar a segurança
* proteger dados in transit e at rest
* simular eventos de segurança e automatizar detecção, investigação e recuperação

## 3 - Reliability

* *habilidade de o sistema se recuperar de uma falha, dinamicamente adquirir mais recursos computacionais para atender às demandas e mitigar falhas*
* testar procedimento de recuperação e automatizá-los
* usar Auto Scaling para não precisar adivinhar o uso de recursos

## 4 - Performance Efficiency

* *habilidade de usar recursos de computação eficientemente para atender às demandas e manter essa eficiência enquanto as demandas e tecnologia avançam*
* usar serviços **serverless**
* go global in minutes
* experimentar novos serviços com frequência
* avaliar logs e performance no CloudWatch

## 5 - Cost Optimization

* *habilidade de rodar sistemas para entrega de valor com o custo mínimo*
* usar serviços Pay for what you use
* medir eficiência com CloudWatch
* não gastar dinheiro com operações em data centers **~>** migração para cloud
* usar tags para identificar Return of Investment (ROI)
* usar serviços gerenciados pela AWS

## 6 - Sustainability

* *habilidade de minimizar os impactos ambientais usando recursos em cloud*
* medir eficiência
* maximizar utilização de recursos
* adotar novos hardwares
* usar serviços gerenciados pela AWS

## Well-Architected Tool

* serviço para avaliar a infra a partir dos 6 pilares
* avalia as respostas às perguntas dos serviços e indica pontos de melhoria

## CAF - Cloud Adoption Framework

* não é um serviço, é um whitepaper
* 6 perspectivas, 2 capacidades e 4 domínios de transformação
* **Business Capabilities**
  * **Business:** garantir que os investimentos em cloud aceleram os retornos
  * **People:** garantir a cultura de evolução contínua, liderança e força de trabalho
    * *serve como uma ponte entre a tecnologia e o negócio*
  * **Governance:** ajuda a orquestrar as iniciativas em cloud para maximizar os benefícios e minimizar os riscos
* **Technical Capabilities**
  * **Plataform:** garantir uma infra escalável, modernizar cargas e implementar recursos cloud-native
  * **Security:** garantir a confidencialidade, integridade e disponibilidade dos dados
  * **Operations:** garantir que os recursos de cloud atendam às demandas
* **Transformation Domains**
  * **Technology:** migrar e modernizar infra, aplicações, dados e análises
  * **Process:** digitalizar, automatizar e otimizar operações
  * **Organization:** reorganizar o modelo de operação e a equipe para evolução rápida
  * **Product:** reorganizar o modelo de negócios, novos produtos e serviços

## Right Sizing

* escolher a menor instância que atenda às necessidades **~>** menor custo
* ter em mente que escalar é muit fácil e rápidp **~>** começar pequeno
* 2 momentos:
  * antes de migrar pra cloud **~>** otimizar a infra para não ter recursos desperdiçados e levar essa estrutura não otimizada para cloud
  * depois de migrar **~>** constantemente avaliar a infra em busca de redução de recursos sem comprometer as aplicações

## Ecosystem

* **Free Resources**: blogs, Forums, Whitepapers & Guides, Quick Start, Solutions
* **Support:** tratado anteriormente
* **Marketplace:** softwares, containers, AMIs
* **Training:** Digital, Classroom, Private (para empresas), Academy (universidades), Training and Certification para governo dos EUA e para Enterprise
* **Professional Services:** time de profissionais que atuam junto a equipe interna

* **Knowledge Center:** portal com FAQs e guias de boas práticas nos principais tópicos da AWS
* **IQ:** plataforma para contratar free lancers para prestar consultoria
* **re:Post:** fórum, não indicado para perguntas time-sensitve ou com informações proprietárias

## AMS - Managed Services

* time de suporte para operar a infra
* cuidam da infra para o cliente focar nos negócios
