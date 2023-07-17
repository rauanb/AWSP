# AWS-Practitioner
Conteúdo para a Certificação AWS Practitioner

# Conceitos de Cloud

## Vantagens de um ambiente Cloud

* velocidade para implantar soluções
* atualizações/integrações sem interromper o serviço
* menor custo
* backup e segurança de dados
* escalabilidade sem interromper o serviço

## Tipos de Cloud

* **Infrastructure as a Service:** usuário cuida do SO pra cima **~>** EC2
* **Plataform as a Service:** usuário cuida somente dos dados e da aplicação **~>** Beanstalk
* **Software as a Service:** provedor cuida de tudo **~>** Office 365

## Público, Privado  e Híbrido

* **público:** disponibilização de servidores compartilhados ao público geral para utilizar os serviços
  * mantém a segurança de cada cliente
* **privado:** disponibilização de servidores exclusivos para o cliente
  * maior segurança para dados sensíveis (bancos, governos)
* **híbrido:** utiliza os dois tipos
  * normalmente público para aplicação e privado para banco de dados

## Serviços AWS

* 237 seviços atualmente e aumentando
* **EC2:** servidores virtuais
* **route53:** domínios de sites

## Modelo de Responsabilidade Compartilhada

* **AWS:** segurança **da** nuvem 
  * hardware dos servidores, armazenamento, redes e instalações
  * softwares e SO do host
  * regiões e zonas de disponibilidades
* **Cliente:** segurança **na** nuvem
  * depende do serviço contratado
  * **EC2:** software e SO do guest
  * **S3:** gerenciamento dos dados e criptografia

## CLI e CloudShell

* **cli:** é instalada na máquina 
* **cloudshell:** terminal no próprio navegador

# Infraestrutura Global

* os serviços são disponibilizados por região **~>** verificar disponibilidade

## Regiões

* **representadas por:** sa-east-1

* **us:** Estados Unidos
* **sa:** Brasil
* **eu:** Europa

## AZ - Zonas de Disponibilidade

* **representadas por:** sa-east-1**a**

* grandes datacenters pertencentes aquela região
* são interligados por fibras de alta velocidade
* uma **az** é backup da outra **~>** não para nenhum serviço no caso de desastre em uma
* fisicamente distantes, mas não mais que 100km

## LZ - Zonas Locais

* pequenos datacenters
* são ligados às AZs por fibras de alta velocidade
* são mais próximos do cliente final
* baixa latência **~>** ideal para serviços de stream, jogos em tempo real e machine learning

## Wavelength

* voltado para dispositivos móveis 5g preferencialmente**~>** latência inferior a 10 ms
* equipamentos da AWS no  provedor de telefonia
* equipamentos conectados às AZs com fibra de alta velocidade

## Outspot

* disponibilidade de equipamentos da AWS em datacenters de terceiros/clientes

# IAM - Identity Acces Management

* todo controle dos serviços passa pelo **IAM** através de uma API
* **acessos via:** console, cli ou API
* **roles:** permissões para serviços
* **policies:** permissões/bloqueios de usuários/grupos
* **novo usuário:** inicialmente sem nenhum privilégio e não associado a nenhuma região
  * **access key:** acesso direto ao serviço
  * **password:** acesso ao console
* **MFA:** usa 2 códigos seguidos
* **password policy:** definição de requisitos de senha

# EC2 - Elastic Compute Cloud

* **user data:** automação
  * comandos executados quando a instância é criada
  * arquivos de até 16kb em bash, batch ou powershell

* **acesso ao S3:** através de roles

## Batch

* execução de scripts em lote

## Lightsail

* EC2 simplificado

## ECS - Elastic Container Service

* **task:** container
* **registry:** imagem
* **ECS EC2 Cluster:** o usuário controla
* **ECS FARGATE:** serverless

# Storage

* diversos serviços: EFS, FSx, S3, S3 Glacier e Storage Gateway

* diversas categorias: block, file e objetc storage
* **Block Storage:** blocos
  * armazenado em volumes, como HD interno ou externo (DAS)
  * baixa latência
* **File Storage:** dados compartilhados
  * compartilhados com 1 ou mais usuários
  * se comporta como um NAS (Network Attached Storage)
* **Object Storage:** objetos
  * id
  * flat address (URL)
  * metadados

## EBS - Elastic Block Store

* armazenamento de instâncias EC2
* precisa estar na mesma AZ
  * para transferir, é criado um snapshot e então restaurado na outra instância
* **volumes gp2 e gp3:** SSD de baixo custo
  * uso geral: vms, database, boot, aplicações sensíveis à latência
  * de 1 GB a 16 TB
  * 250 MB/s (gp2) e 1 GB/s (gp3)
* **volumes io1 e io2:** SSD de alta performance
  * database de fluxo intenso
  * de 4 GB a 64 TB
  * 1GB/s (io1 )
* **volumes st1:** HDD
  * big data, processamento de logs
  * de 125 GB a 16 TB
  * 250 MB/s
* **volumes sc1:** Cold HDD
  * dados pouco acessados
  * de 125 GB a 16 TB
  * 250 MB/s

## S3 - Simple Storage Service

* sem limite de armazenamento total
* limite de 5 TB por objeto (arquivo)
* trabalha com objetos
* **bucket:** diretório
  * nome único e universal
* **durabilidade:** garantia de que o arquivo não será corrompido **~>** 99.999999999 %
* **disponibilidade:** garantia de acesso ao arquivo **~>** 99.95 %
* **snapshot:** cópia de um volume
* **AMI:** cópia da imagem de uma instância
  * na hora de iniciar uma nova instância, a AMI criada aparece em **My AMIs**

### Classes

* a classe é por arquivo carregado **~>** mesmo bucket pode ter arquivos de várias classes

* **Standard:** uso geral de dados acessados com frequência
  * baixa latência e alto troughput
  * resiliente contra desastres em uma AZ inteira
  * podem ser usadas políticas de ciclo de vida para mudar automaticamente de classe
  * recuperação ilimitada
* **Intelligent Tiering:** uso geral
  * monitora o acesso (pequena cobrança extra) para mudar de categoria
  * acesso frequente: baixa latência e alto troughput
    * arquivos de até 128 KB sempre são armazenados nessa categoria e sem monitoramento
  * acesso não frequente: baixa latência e alto troughput **~>** até 40% de economia
  * arquivos raramente acessados **~>** até 68% de economia
    * o arquivo precisa ser restaurado antes de ser acessado
  * recuperação ilimitada
  * resiliente contra desastres em uma AZ inteira
* **Standard Infrequent Access (IA):** armazenamentos de longa duração, backups
  * pouco acesso, mas acesso rápido
  * baixa latência e alto troughput
  * resiliente contra desastres em uma AZ inteira
  * recuperação por GB
* **One Zone Infrequent Access:** cópias de backup ou réplicas de regiões
  * somente uma AZ **~>** economia de até 20%
* **Glacier Instant Retrieval**
  * acesso até uma vez por trimestre
  * aceso em milissegundos
  * economia de até 68% em relação ao Standard IA
  * recuperação por GB
  * resiliente contra desastres em uma AZ inteira
* **Glacier Flexible Retrieval**
  * acesso até 2 vezes ao ano
  * recuperação assíncrona
  * economia de até 10% em relação ao Instant Retrieval
  * recuperação ilimitada
  * resiliente contra desastres em uma AZ inteira
* **Glacier Deep Archive:** arquivamento de dados legais, registros
  * acesso até 2 vezes ao ano
  * recuperação em até 12h
  * alternativa à fitas magnéticas
  * resiliente contra desastres em uma AZ inteira
  * **Object Lock** ou **Vault Lock:** arquivo não pode ser alterado/excluído
* **Outposts:** no local
  * não é armazenado em nuvem
  * autenticação usando IAM
  * transferência de dados para outras regiões

### Preço

* https://calculator.aws/#/
* levar em conta armazenamento e recuperação (para as classes específicas)
* levar em conta mudança de classe

### Versionamento

* cada versão tem um ID diferente
* atenção ao acúmulo de espaço com muitas versões
* é possível configurar o número máximo de versões

### Página Estática

1. criar um bucket com ACLs e desbloqueio público
2. adicionar arquivo index.html
3. nas permissões do bucket ativar página estática
4. nas permissões do arquivo ir em object actions **~>** Make public
5. o link estará no final das permissões do bucket

### Ciclo de Vida

* regras que são aplicadas com o passar do tempo
* podem ser por grupo de arquivos ou por bucket
* regra de mudança de classe baseada em acesso
* regra de exclusão de versão antiga (dias ou número de versões)

### Replicação

* ir em Management **~>** Replication Rules
* atenção à taxa de replicação + armazenamento dobrado
* necessário ter o versionamento ativado
* pode ser por grupo de arquivos ou por bucket

### Regras de Acesso

* definida em json
* pode ser via:
  * **IAM:** aplicada ao usuário, grupo ou role
  * **Bucket Policies:** aplicada ao bucket

### Storage Gateway

* appliance da AWS na empresa (on promises)
* faz a comunicação da infraestrutura interna com o S3
* tem cache interno **~>** baixa latência
* ideal para backup ou para transição local-cloud

# DNS - Domain Name System

* **Route 53**
  * políticas de disponibilidade, latência e geolocalização

# Auto Scaling

* **Scaling Up:** aumentar os recursos de uma única instância
* **Scaling Out:** aumentar o número de instâncias
* **Auto Scaling Group:** grupo de subnets que serão auto escaladas
* **Cloud Watch:** gerencia a inicialização/desligamento de instâncias
  * baseado em regras de CPU entre outras
  * instâncias cópias da **Launch Tamplate**
* **Target Tracking:** 
  * sobe 1 instância ao atingir o limite definido nela 
  * para tráfegos muito intensos pode não atender
* **Step/Simple Scaling:**
  * sobre mais de uma instância a depender do número de requisições

# Load Balancer

* é uma feature do **EC2**
* faz a distribuição de requisições entre as instâncias
* trabalhar muito bem com o auto scaling

## ALB - Application Load Balancer

* mais detalhado e inteligente
* camada 7 **~>** Aplicação
* roteamento baseado em PATH (/contato, /eventos) ou em host (subdomínios)
* pode apontar para instâncias, containers, lambda e targets

## NLB - Network Load Balancer

* mais rápido
* camada 4 **~>** TCP/IP
* roteamento baseado em IP ou TCP/UDP

# Billing

* cobraça baseada em compute, storage e transfer out
* **pay as you go:** a conta começa com $0 e com o passar dos dias no mês vai acumulando
* pagamento por demanda **~>** em segundos a partir de 60s
* **pay less as you use more:** recursos mais performáticos têm melhor custo-benefício
* grandes descontos para serviços reservados
* **CAPEX vs OPEX**
  * **Capital Expenditure:** investimento inicial alto **~>** pré pago
  * **Operational Expenditure:** investimento crescente conforme o uso **~>** pós pago
* **Budget:** definição do orçamento base e alertas
* **Cost Explorer:** relatórios de gastos

## Planos

* **Basic:** gratuito, sem suporte (somente self service)
* **Developer:** email horário comercial, 1 ticket aberto por vez, 12 horas de tempo de resposta
* **Business:** email, chat ou phone 24/7, sem limite de tickets abertos, 1 hora de tempo de resposta
* **Enterprise:** Technical Account Manager (TAM)

## Organizations

* semelhante ao AD do Windows (Organization Unit, Accounts, Resources e Policies)
* separa os recursos e gastos por setor ou grupo

# Segurança

* **compliance:** conformidade
* Shared Responsability Model

* **Web Application Firewall (WAF):** identifica tráfegos maliciosos na camada 7

* **Shield:** identifica e inibe ataques de **DDoS** 
  * **Distributed Denial of Service:** tráfego altímissimo que impede o serviço de atender as requisições
* **Inspector:** agente que identifica vulnerabilidades em instâncias EC2
* **Trusted Advisor:** analisa em tempo real toda a estrutura e indica melhorias
* **Cloud Trail:** monitoramento de comandos via APIs e console
* **Athena:** busca informações no S3 usando SQL
* **Macie:** busca informações no S3 usando Machine Learning e Natural Language Processing
  * indicado para dados sensíveis, como de transações financeiras

# CloudFormation

* cria a estrutura a partir de um template (yaml ou json)
* controle de versões
* visualizar as mudanças antes de aplicá-las
* previsão de gastos
* automatização para destruir em períodos sem uso e recriar

# Lambda + Python

1. Criar par de chaves
2. Criar a função no Lambda
3. Criar o script

