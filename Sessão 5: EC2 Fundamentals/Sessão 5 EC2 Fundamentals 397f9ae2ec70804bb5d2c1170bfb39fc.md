# Sessão 5: EC2 Fundamentals

## Aula 31 — AWS Budget Setup

### Objetivo da aula

Configurar alertas de custos para evitar cobranças inesperadas durante o uso da AWS.

---

### 1. Acessando o Billing

O painel de faturamento fica em:

```
Billing and Cost Management
```

Por padrão, usuários IAM **não têm acesso** às informações de faturamento.

É necessário habilitar essa opção na conta **Root**.

---

### 2. Explorando os custos

No painel de Billing é possível visualizar:

- Gastos do mês
- Previsão de gastos
- Cobranças por serviço (EC2, S3, EBS, etc.)
- Uso da Free Tier

Isso ajuda a identificar rapidamente qual serviço está gerando custos.

---

### 3. AWS Budgets

O **AWS Budgets** envia alertas quando os gastos atingem um limite definido.

Exemplos:

- **Zero Spend Budget** → alerta ao gastar o primeiro centavo.
- **Monthly Cost Budget** → define um limite mensal (ex.: US$ 10).

Os alertas são enviados por e-mail.

---

### 4. Free Tier

O painel **Free Tier** mostra:

- Consumo atual
- Limites gratuitos
- Previsão de uso

Se algum recurso ultrapassar o limite gratuito, ele poderá gerar cobrança.

---

### Resumo final (para decorar)

```
Billing
→ Visualizar custos

Free Tier
→ Acompanhar uso gratuito

AWS Budgets
→ Receber alertas de gastos
```

**Analogia:**

- O AWS Budgets funciona como o **limite de gastos do cartão de crédito**: você define um valor e recebe um aviso antes que a conta fique alta.
    
    ![image.png](Sess%C3%A3o%205%20EC2%20Fundamentals/image.png)
    
    ![image.png](Sess%C3%A3o%205%20EC2%20Fundamentals/image%201.png)
    
    ![image.png](Sess%C3%A3o%205%20EC2%20Fundamentals/image%202.png)
    
    ![image.png](Sess%C3%A3o%205%20EC2%20Fundamentals/image%203.png)
    

---

## Aula 32 — Amazon EC2 Overview

### Objetivo da aula

Entender o que é o **Amazon EC2** e quais configurações podem ser escolhidas ao criar uma instância.

---

### 1. O que é o Amazon EC2?

O **Amazon EC2 (Elastic Compute Cloud)** permite criar **máquinas virtuais** na AWS sob demanda.

É um dos serviços mais utilizados da plataforma.

**Analogia:**

O EC2 é como **alugar um computador na nuvem**. Você escolhe a configuração e paga apenas pelo tempo de uso.

---

### 2. O que pode ser configurado?

Ao criar uma instância EC2, é possível escolher:

- Sistema operacional (Linux, Windows ou macOS)
- CPU
- Memória (RAM)
- Armazenamento
- Rede
- Regras de firewall (Security Group)

---

### 3. Serviços relacionados ao EC2

O EC2 trabalha em conjunto com outros serviços:

- **EBS** → armazenamento persistente
- **Elastic Load Balancer (ELB)** → distribui o tráfego
- **Auto Scaling Group (ASG)** → aumenta ou reduz instâncias automaticamente

---

### 4. EC2 User Data

O **User Data** é um script executado **apenas na primeira inicialização** da instância.

Usos comuns:

- Instalar atualizações
- Instalar softwares
- Baixar arquivos
- Configurar o servidor automaticamente

Os comandos são executados com permissões de **root**.

---

### Resumo final (para decorar)

```
EC2
→ Máquina virtual na AWS
```

```
Ao criar uma instância você define:

• Sistema Operacional
• CPU
• RAM
• Armazenamento
• Rede
• Firewall (Security Group)
```

```
User Data
→ Script executado apenas
na primeira inicialização.
```

---

## Aula 33 — EC2 Hands On

### Objetivo da aula

Criar a primeira **instância EC2**, executar um servidor web usando **User Data** e aprender o ciclo de vida da instância.

---

### 1. Criando uma instância EC2

Durante a criação foram definidos:

- Nome da instância
- AMI (**Amazon Linux 2**)
- Tipo da instância (**t2.micro** - Free Tier)
- Key Pair (SSH)
- Security Group
- Armazenamento (EBS)

---

### 2. Security Group

Foram liberadas as portas:

- **22 (SSH)** → acesso remoto à instância
- **80 (HTTP)** → acesso ao servidor web

---

### 3. EC2 User Data

Foi utilizado um script para configurar automaticamente a instância na primeira inicialização.

O script:

- Instala um servidor web (Apache/httpd)
- Cria uma página **Hello World**

> O **User Data é executado apenas uma vez**, na primeira inicialização da instância.
> 

---

### 4. Acessando o servidor

Após a instância iniciar, basta acessar:

```
http://<Public IPv4>
```

> Utilize **HTTP**, não HTTPS.
> 

---

### 5. Estados da instância

Principais ações:

- **Start** → inicia a instância
- **Stop** → para a instância (não cobra processamento)
- **Terminate** → exclui a instância definitivamente

---

### 6. IP Público x IP Privado

Ao reiniciar uma instância:

- **Public IPv4** → pode mudar
- **Private IPv4** → permanece o mesmo

**Analogia:**

O IP privado é como o número do apartamento: permanece igual.

O IP público é como uma vaga de estacionamento: pode mudar quando você sai e volta.

---

### Resumo final (para decorar)

```
Criar EC2

• Amazon Linux 2
• t2.micro
• Key Pair
• Security Group
• EBS
• User Data
```

```
Security Group

22 → SSH
80 → HTTP
```

```
User Data
→ Executa apenas
na primeira inicialização.
```

```
Start
→ Liga

Stop
→ Desliga (mantém dados)

Terminate
→ Exclui a instância
```

```
Public IP
→ Pode mudar

Private IP
→ Não muda
```

![image.png](Sess%C3%A3o%205%20EC2%20Fundamentals/image%204.png)

![image.png](Sess%C3%A3o%205%20EC2%20Fundamentals/image%205.png)

![image.png](Sess%C3%A3o%205%20EC2%20Fundamentals/image%206.png)

![image.png](Sess%C3%A3o%205%20EC2%20Fundamentals/image%207.png)

![image.png](Sess%C3%A3o%205%20EC2%20Fundamentals/image%208.png)

![image.png](Sess%C3%A3o%205%20EC2%20Fundamentals/image%209.png)

- Esse código abaixo será executado assim que a nossa instância for criada.
- Esse trecho de código faz parte da aula e ele foi disponibilizado como material complementar
    
    ![image.png](Sess%C3%A3o%205%20EC2%20Fundamentals/image%2010.png)
    
    ![image.png](Sess%C3%A3o%205%20EC2%20Fundamentals/image%2011.png)
    
- Abri a URL do IP público - [http://54.86.106.218](http://54.86.106.218/)
    
    ![image.png](Sess%C3%A3o%205%20EC2%20Fundamentals/bc38e4ab-76ec-4adf-af12-30b7bc9dbe31.png)
    
- Posso também “parar (stop)” minha estância
    
    ![image.png](Sess%C3%A3o%205%20EC2%20Fundamentals/image%2012.png)
    

---

## Aula 34 — Tipos de Instâncias do Amazon EC2

Ao criar uma máquina virtual no Amazon EC2, você precisa escolher um **tipo de instância**. Cada tipo foi projetado para um objetivo específico, como processamento intenso, grande quantidade de memória ou armazenamento de alta velocidade.

A escolha correta impacta diretamente o **desempenho** e o **custo** da aplicação.

---

### Convenção de nomenclatura das instâncias

Exemplo:

> **M5.2xlarge**
> 

Essa nomenclatura possui três partes:

- **M** → Classe da instância (família)
- **5** → Geração do hardware
- **2xlarge** → Tamanho da instância

#### Classe (Família)

Define o foco da instância.

Exemplos:

- **M** → Uso geral (General Purpose)
- **C** → Otimizada para computação (Compute Optimized)
- **R** → Otimizada para memória (Memory Optimized)

#### Geração

Representa a evolução do hardware.

Exemplo:

- M5 → geração 5
- M6 → geração mais recente
- M7 → geração ainda mais nova

Normalmente, gerações mais recentes oferecem:

- melhor desempenho;
- menor consumo de energia;
- melhor custo-benefício.

#### Tamanho

Indica a quantidade de recursos disponíveis.

Ordem crescente:

```
small
medium
large
xlarge
2xlarge
4xlarge
8xlarge
16xlarge
...
```

Quanto maior o tamanho:

- mais vCPUs;
- mais memória RAM;
- maior capacidade de rede;
- maior desempenho geral.

---

## Tipos de instâncias

### 1. General Purpose (Uso Geral)

Possuem um equilíbrio entre:

- CPU
- Memória
- Rede

São as mais utilizadas para aplicações comuns.

Casos de uso:

- servidores web;
- APIs;
- aplicações corporativas;
- repositórios Git;
- pequenos bancos de dados.

Famílias comuns:

- T (T2, T3, T4g)
- M (M5, M6)

Durante o curso será utilizada a:

> **t2.micro**
> 

Ela faz parte da camada gratuita (Free Tier).

---

### 2. Compute Optimized (Otimizadas para Computação)

São focadas em oferecer alto poder de processamento (CPU).

Indicadas quando o gargalo da aplicação é o processador.

Casos de uso:

- processamento em lote (Batch Processing);
- transcodificação de vídeos;
- servidores web de alta performance;
- Machine Learning;
- High Performance Computing (HPC);
- servidores de jogos.

Família:

- **C** (C5, C6...)

> **Dica para decorar:** **C = CPU**.
> 

---

### 3. Memory Optimized (Otimizadas para Memória)

Possuem grande quantidade de memória RAM.

São ideais para aplicações que mantêm muitos dados carregados em memória.

Casos de uso:

- bancos de dados relacionais de alta performance;
- bancos NoSQL;
- bancos de dados em memória;
- Redis / ElastiCache;
- Business Intelligence (BI);
- processamento de Big Data em tempo real.

Famílias:

- **R**
- X1
- High Memory
- Z1

> **Dica para decorar:** **R = RAM**.
> 

---

### 4. Storage Optimized (Otimizadas para Armazenamento)

Projetadas para aplicações que realizam muitas operações de leitura e escrita em disco local.

Casos de uso:

- sistemas OLTP (Online Transaction Processing);
- bancos relacionais;
- bancos NoSQL;
- Redis;
- sistemas de arquivos distribuídos;
- aplicações de Data Warehouse.

Famílias:

- I
- D
- H1

Essas instâncias priorizam alto desempenho de armazenamento.

---

## Comparando alguns exemplos

| Instância | Característica |
| --- | --- |
| **t2.micro** | 1 vCPU e 1 GB RAM. Indicada para pequenas aplicações e estudos. |
| **r5.16xlarge** | Grande quantidade de memória (512 GB RAM). Ideal para bancos de dados em memória. |
| **c5d.4xlarge** | Muitas vCPUs e menos memória proporcionalmente. Excelente para processamento pesado. |

Perceba que cada família distribui os recursos de forma diferente conforme seu objetivo.

---

## Analogia simples

Imagine que você precisa contratar um veículo para diferentes tarefas:

- **General Purpose (M/T)** → Um carro comum: serve para a maioria das situações.
- **Compute Optimized (C)** → Um carro esportivo: feito para velocidade (CPU).
- **Memory Optimized (R)** → Um caminhão de mudanças: transporta muita carga (RAM).
- **Storage Optimized (I/D/H)** → Um caminhão frigorífico: especializado em transportar grandes volumes de armazenamento rapidamente.

Cada veículo resolve um problema diferente.

---

## Ferramenta útil para comparar instâncias

Um site bastante utilizado para comparar todas as instâncias do EC2 é:

- [**EC2Instances.info](http://EC2Instances.info)**

Ele permite comparar:

- preço;
- quantidade de vCPUs;
- memória RAM;
- largura de banda;
- armazenamento;
- desempenho de rede;
- entre outras características.

É uma excelente ferramenta para escolher a instância mais adequada antes de criar uma máquina no EC2.

---

## O que costuma cair na prova

Você **não precisa decorar** todas as famílias (C5, R5, X1, H1 etc.).

O importante é saber identificar:

- **General Purpose** → equilíbrio entre CPU, memória e rede.
- **Compute Optimized** → foco em CPU.
- **Memory Optimized** → foco em RAM.
- **Storage Optimized** → foco em armazenamento e operações de disco.

Também é importante entender a nomenclatura:

- Classe (M, C, R...)
- Geração (5, 6, 7...)
- Tamanho (small, large, xlarge...)

---

## Resumo final (para decorar)

- **EC2 oferece diferentes famílias de instâncias**, cada uma otimizada para um tipo de carga de trabalho.
- A nomenclatura segue o padrão **Classe + Geração + Tamanho** (ex.: **M5.2xlarge**).
- **General Purpose (M/T)** → equilíbrio entre CPU, RAM e rede.
- **Compute Optimized (C)** → maior capacidade de processamento (CPU).
- **Memory Optimized (R)** → grande quantidade de memória RAM.
- **Storage Optimized (I/D/H)** → alto desempenho de armazenamento local.
- Quanto maior o tamanho da instância (**xlarge, 2xlarge, 4xlarge...**), maiores serão os recursos disponíveis (CPU, memória e rede).
- Para comparar instâncias e custos, uma ferramenta muito utilizada é o **EC2Instances.info**.

---

## Aula 35 - Security Groups & Classic Ports Overview

## Security Groups (Grupos de Segurança) no EC2

### O que são os *Security Groups*?

Os *Security Groups* são **firewalls virtuais** que protegem as instâncias *EC2*. Eles controlam **quais conexões podem entrar (Inbound)** e **quais podem sair (Outbound)** da instância.

Uma característica importante é que eles possuem **apenas regras de permissão (Allow)**. Não existem regras de negação (*Deny*). Se um tráfego não estiver explicitamente permitido, ele será automaticamente bloqueado.

> **Analogia:** imagine que um *Security Group* é o porteiro de um condomínio. Ele possui uma lista de pessoas autorizadas a entrar. Quem não estiver na lista simplesmente não entra.
> 

---

### Como funcionam as regras?

Cada regra de um *Security Group* possui quatro informações principais:

- **Tipo (Type):** serviço (SSH, HTTP, HTTPS, etc.)
- **Protocolo (Protocol):** normalmente TCP ou UDP.
- **Porta (Port):** porta utilizada pelo serviço.
- **Origem/Destino (Source/Destination):** endereço IP ou outro *Security Group* autorizado.

Exemplo:

| Tipo | Protocolo | Porta | Origem |
| --- | --- | --- | --- |
| SSH | TCP | 22 | Seu IP |
| HTTP | TCP | 80 | 0.0.0.0/0 |

---

### O que significa `0.0.0.0/0`?

Este é um dos valores mais importantes para lembrar.

- `0.0.0.0/0` significa **qualquer endereço IPv4 da Internet**.
- Ou seja, qualquer computador pode tentar acessar aquela porta.

Por exemplo:

- Porta **80** + `0.0.0.0/0` → qualquer pessoa pode acessar seu site.

---

### Tráfego de entrada e saída

#### Inbound (Entrada)

Controla quem pode acessar sua instância.

Exemplo:

- Seu computador pode acessar a porta 22 (SSH).
- Outro computador, com IP diferente, será bloqueado.

Quando isso acontece, normalmente ocorre um **timeout**, pois o firewall impede a conexão antes mesmo de chegar à instância.

---

#### Outbound (Saída)

Controla para onde a instância pode enviar dados.

Por padrão:

- ✅ Todo tráfego de saída é permitido.

Isso significa que a instância pode acessar a Internet normalmente (baixar atualizações, acessar APIs, etc.).

---

### Características importantes

Os *Security Groups* possuem algumas características importantes:

- Um mesmo *Security Group* pode ser associado a **várias instâncias EC2**.
- Uma instância EC2 pode possuir **mais de um Security Group**.
- São específicos de uma **VPC** e de uma **Região AWS**.
- Eles ficam **fora da instância EC2**, funcionando como um firewall externo.

Isso significa que, se uma conexão for bloqueada, **a instância nem chega a receber a requisição**.

---

### Timeout x Connection Refused

Esse é um detalhe bastante cobrado em provas e muito útil na prática.

| Erro | Significado |
| --- | --- |
| **Timeout** | O *Security Group* bloqueou o acesso. |
| **Connection Refused** | O firewall permitiu a conexão, mas o serviço na instância não estava disponível (aplicação parada, porta fechada, etc.). |

**Resumo:**

- **Timeout → problema no Security Group.**
- **Connection Refused → problema na aplicação ou serviço.**

---

### Boas práticas

Uma recomendação comum é criar um *Security Group* exclusivo para acesso **SSH**.

Isso facilita o gerenciamento e aumenta a segurança, pois o acesso administrativo fica isolado das regras utilizadas pela aplicação.

---

### Referenciando outros *Security Groups*

Além de liberar acesso por endereço IP, um *Security Group* pode autorizar conexões vindas de **outro Security Group**.

Exemplo:

```
EC2 A
└── Security Group A

EC2 B
└── Security Group B

Security Group A
Permite acesso do Security Group B
```

Assim:

- Qualquer instância que utilize o **Security Group B** poderá acessar a instância protegida pelo **Security Group A**.
- Não é necessário conhecer ou configurar os endereços IP das instâncias.

Esse recurso é muito utilizado com:

- *Load Balancers*
- Servidores de aplicação
- Bancos de dados
- Arquiteturas com múltiplas instâncias

---

### Comportamento padrão

Por padrão, todo novo *Security Group* possui:

- ❌ Todo tráfego de **entrada** bloqueado.
- ✅ Todo tráfego de **saída** permitido.

---

### Portas importantes para o exame

| Porta | Serviço | Utilização |
| --- | --- | --- |
| **22** | *SSH* | Acesso remoto seguro a instâncias Linux. |
| **21** | *FTP* | Transferência de arquivos (não seguro). |
| **22** | *SFTP* | Transferência segura de arquivos utilizando SSH. |
| **80** | *HTTP* | Sites sem criptografia. |
| **443** | *HTTPS* | Sites com criptografia SSL/TLS (padrão atual). |
| **3389** | *RDP* | Acesso remoto a instâncias Windows. |

---

### Resumo final (para decorar)

- *Security Groups* são os **firewalls das instâncias EC2**.
- Possuem apenas regras de **Allow** (não existe *Deny*).
- Controlam tráfego **Inbound** e **Outbound**.
- `0.0.0.0/0` significa **qualquer IP da Internet**.
- Por padrão:
    - Entrada: **bloqueada**
    - Saída: **permitida**
- **Timeout** indica bloqueio pelo *Security Group*.
- **Connection Refused** indica que a conexão chegou à instância, mas o serviço não respondeu.
- Um *Security Group* pode ser compartilhado por várias instâncias, e uma instância pode utilizar vários *Security Groups*.
- É possível permitir acesso utilizando **outros Security Groups**, sem depender de endereços IP.
- Memorize as portas:
    - **22 → SSH / SFTP**
    - **21 → FTP**
    - **80 → HTTP**
    - **443 → HTTPS**
    - **3389 → RDP (Windows)**

---

## Aula 36 - Security Groups Hands On

### Hands-on: Gerenciando *Security Groups* no Amazon EC2

### Visualizando os *Security Groups*

Após criar uma instância *EC2*, é possível visualizar os *Security Groups* de duas formas:

- Pela aba **Security** da própria instância, que exibe um resumo das regras.
- Pelo menu **Network & Security → Security Groups**, onde é possível gerenciar todas as configurações.

Cada *Security Group* possui:

- Um **ID único**.
- Regras de **entrada (Inbound Rules)**.
- Regras de **saída (Outbound Rules)**.

---

### Regras de entrada (*Inbound Rules*)

As regras de entrada controlam **quem pode acessar a instância**.

No exemplo da aula, o *Security Group* possuía duas regras:

| Serviço | Porta | Origem |
| --- | --- | --- |
| *SSH* | 22 | `0.0.0.0/0` |
| *HTTP* | 80 | `0.0.0.0/0` |

Isso significa:

- Porta **22** → permite acesso remoto via *SSH*.
- Porta **80** → permite que qualquer pessoa acesse o servidor web.

---

### O que acontece ao remover uma regra?

O instrutor removeu a regra da porta **80 (HTTP)**.

Resultado:

- O navegador ficou tentando carregar indefinidamente.
- Após algum tempo, ocorreu um **timeout**.

Isso aconteceu porque o *Security Group* bloqueou completamente a conexão antes mesmo que ela chegasse à instância.

> **Importante:** quando uma conexão apresenta **timeout**, a primeira suspeita deve ser sempre o *Security Group*.
> 

---

### Como corrigir um timeout

Para restaurar o acesso, basta adicionar novamente a regra:

| Serviço | Porta | Origem |
| --- | --- | --- |
| *HTTP* | 80 | `0.0.0.0/0` |

Após salvar as alterações, o acesso ao site volta a funcionar imediatamente.

Uma vantagem dos *Security Groups* é que as alterações são aplicadas **em tempo real**, sem necessidade de reiniciar a instância.

---

### Criando novas regras

Ao adicionar uma regra, é possível escolher:

- **Tipo do serviço** (*HTTP*, *HTTPS*, *SSH*, etc.).
- **Protocolo** (TCP, UDP, etc.).
- **Porta** ou intervalo de portas.
- **Origem** da conexão.

Por exemplo:

| Serviço | Porta |
| --- | --- |
| *HTTP* | 80 |
| *HTTPS* | 443 |
| *SSH* | 22 |

O console da AWS já preenche automaticamente a porta correta ao selecionar o tipo do serviço.

---

### Escolhendo a origem do acesso

Existem diferentes formas de definir quem poderá acessar a instância.

#### Qualquer lugar

```
0.0.0.0/0
```

Permite acesso de qualquer computador da Internet.

É comum para:

- Sites públicos (*HTTP* e *HTTPS*).

---

#### Meu IP (*My IP*)

A AWS detecta automaticamente o IP público do computador utilizado.

Essa opção é recomendada para:

- *SSH*
- *RDP*

Assim, apenas seu computador poderá acessar a instância.

**Atenção:** se seu provedor alterar seu IP público (IP dinâmico), será necessário atualizar essa regra; caso contrário, você receberá um **timeout**.

---

### Regras de saída (*Outbound Rules*)

Por padrão, todo *Security Group* possui uma regra semelhante a esta:

| Serviço | Destino |
| --- | --- |
| Todo tráfego | `0.0.0.0/0` |

Isso permite que a instância:

- Acesse a Internet.
- Baixe atualizações.
- Consuma APIs.
- Envie dados para outros serviços.

Normalmente essa configuração é mantida, exceto em ambientes com requisitos de segurança mais restritivos.

---

### Uma instância pode ter vários *Security Groups*

Um ponto importante demonstrado na aula é que:

- Uma instância *EC2* pode possuir **vários Security Groups** associados.
- Um mesmo *Security Group* pode ser utilizado por **várias instâncias EC2**.

As permissões são **somadas** (união das regras).

Exemplo:

```
Instância EC2

├── Security Group SSH
│     Porta 22
│
├── Security Group Web
│     Portas 80 e 443
│
└── Security Group Banco
      Porta 5432
```

A instância terá acesso permitido em **todas** essas portas, pois as regras dos grupos são combinadas.

---

### Dica prática

Uma boa prática é separar os *Security Groups* por finalidade, por exemplo:

- **SG-SSH** → acesso administrativo (*SSH*).
- **SG-Web** → *HTTP* e *HTTPS*.
- **SG-Database** → acesso ao banco de dados.
- **SG-Application** → comunicação entre servidores.

Essa organização facilita a manutenção, aumenta a reutilização e reduz erros de configuração.

---

### Resumo final (para decorar)

- Os *Security Groups* podem ser gerenciados pelo menu **Network & Security**.
- As **Inbound Rules** controlam quem pode acessar a instância.
- As **Outbound Rules** controlam para onde a instância pode enviar tráfego.
- Remover a regra da porta **80** torna um servidor web inacessível.
- **Timeout = quase sempre problema de Security Group.**
- As alterações nas regras são aplicadas imediatamente.
- `0.0.0.0/0` significa acesso de qualquer IP da Internet.
- A opção **My IP** restringe o acesso apenas ao seu computador.
- Uma instância pode utilizar vários *Security Groups* ao mesmo tempo.
- Um mesmo *Security Group* pode ser compartilhado entre diversas instâncias EC2.

---

## Aula 37 – Visão Geral do SSH (*SSH Overview*)

### O que é SSH?

*SSH* (**Secure Shell**) é um protocolo que permite acessar remotamente uma instância *EC2* Linux por meio de uma conexão segura e criptografada.

Após conectar-se via *SSH*, você pode:

- Executar comandos no servidor.
- Instalar programas.
- Editar arquivos.
- Reiniciar serviços.
- Realizar manutenção e administração da instância.

> **Analogia:** imagine que o *SSH* é como um "controle remoto" do servidor. Em vez de estar fisicamente na frente da máquina, você envia comandos pela Internet de forma segura.
> 

---

### Como acessar uma instância EC2?

O método de conexão depende do sistema operacional utilizado.

| Sistema Operacional | Ferramenta recomendada |
| --- | --- |
| macOS | *SSH* (Terminal) |
| Linux | *SSH* (Terminal) |
| Windows 10 ou superior | *SSH* (PowerShell ou Prompt de Comando) |
| Windows (qualquer versão) | *PuTTY* |
| Todos os sistemas | *EC2 Instance Connect* (Navegador) |

---

### Opção 1: *SSH* (Terminal)

O *SSH* é uma ferramenta de linha de comando disponível nativamente em:

- macOS
- Linux
- Windows 10 e superiores

Com ele, é possível abrir um terminal e conectar-se diretamente à instância *EC2*.

É o método mais utilizado por administradores de sistemas e engenheiros de nuvem.

---

### Opção 2: *PuTTY*

O *PuTTY* é um cliente de *SSH* bastante popular no Windows.

Ele oferece a mesma funcionalidade do *SSH*, mas com uma interface gráfica.

É indicado principalmente para:

- Windows 7
- Windows 8
- Versões antigas do Windows

Na prática, tanto *SSH* quanto *PuTTY* utilizam o mesmo protocolo para estabelecer a conexão segura.

---

### Opção 3: *EC2 Instance Connect*

O *EC2 Instance Connect* permite acessar uma instância diretamente pelo console da AWS, utilizando apenas o navegador.

Suas principais vantagens são:

- Não requer instalação de programas.
- Não exige configuração de terminal.
- Funciona em macOS, Linux e Windows.
- É uma excelente opção para iniciantes.

Durante o curso, o instrutor utiliza esse método por sua simplicidade.

> **Observação:** o *EC2 Instance Connect* depende da imagem (*AMI*) utilizada. No curso, ele funciona porque a instância foi criada com o *Amazon Linux 2*.
> 

---

### Qual método escolher?

Não existe um método "melhor" para todos os casos.

O importante é conseguir acessar a instância.

Se um dos métodos funcionar, ele já é suficiente para administrar o servidor.

---

### Problemas comuns com SSH

Segundo o instrutor, a conexão via *SSH* costuma ser a parte que mais gera dúvidas durante o aprendizado de AWS.

Os erros mais frequentes são:

- *Security Group* sem a porta **22** liberada.
- Digitação incorreta do comando *SSH*.
- Chave privada (*.pem*) errada ou sem permissão adequada.
- Usuário incorreto (por exemplo, utilizar `ubuntu` em vez de `ec2-user`).
- Endereço IP ou DNS incorreto da instância.

Na maioria dos casos, revisar cuidadosamente cada etapa resolve o problema.

---

### Dica do instrutor

Caso encontre dificuldades com *SSH*, experimente utilizar o *EC2 Instance Connect*.

Como ele funciona diretamente pelo navegador, elimina diversos problemas relacionados à configuração do computador local.

---

### Resumo final (para decorar)

- *SSH* é o protocolo utilizado para acessar instâncias Linux de forma segura.
- macOS, Linux e Windows 10 possuem cliente *SSH* nativo.
- O *PuTTY* é uma alternativa para usuários do Windows.
- O *EC2 Instance Connect* permite conectar-se diretamente pelo navegador, sem instalar ferramentas.
- O curso utiliza principalmente o *EC2 Instance Connect* por ser mais simples.
- Os problemas mais comuns com *SSH* estão relacionados ao *Security Group*, à chave de acesso (*.pem*) ou a erros de configuração.
- Se **qualquer método de conexão funcionar**, você já consegue administrar sua instância *EC2*.

---

## Aula 37 – Conectando à EC2 via SSH (Mac/Linux)

O **SSH (Secure Shell)** é o protocolo utilizado para acessar e controlar uma máquina Linux remotamente através do terminal. Na AWS, ele é o método mais comum para administrar instâncias EC2.

### Como funciona a conexão SSH

Quando sua instância EC2 é criada:

- Ela recebe um **IP público** (caso esteja em uma sub-rede pública).
- O **Security Group** deve permitir conexões na **porta 22 (SSH)**.
- Você baixa um **arquivo `.pem`**, que é sua chave privada para autenticação.

O fluxo da conexão é:

```
Seu Mac/Linux
      │
      │ SSH (porta 22)
      ▼
Internet
      │
      ▼
Security Group (porta 22 liberada)
      │
      ▼
Instância EC2 (Amazon Linux)
```

---

### Pré-requisitos

Antes de conectar, verifique:

- Você possui o arquivo `.pem` baixado durante a criação da instância.
- O Security Group possui uma regra de entrada:
    - **Type:** SSH
    - **Protocol:** TCP
    - **Port:** 22
    - **Source:** Seu IP (recomendado) ou `0.0.0.0/0` (apenas para testes).
- Sua instância possui um **Public IPv4** ou **Public DNS**.

---

### Organizando o arquivo `.pem`

É recomendado colocar o arquivo em uma pasta conhecida, por exemplo:

```bash
~/.ssh/
```

ou

```bash
~/aws-course/
```

Evite nomes com espaços.

Exemplo:

```
EC2Tutorial.pem
```

---

### Navegando até a pasta

Para verificar em qual diretório você está:

```bash
pwd
```

Para listar os arquivos:

```bash
ls
```

Para entrar em uma pasta:

```bash
cd aws-course
```

Se o arquivo estiver visível no `ls`, você está no diretório correto.

---

### Primeiro erro comum

Se você tentar conectar apenas com:

```bash
ssh ec2-user@54.123.45.67
```

receberá um erro semelhante a:

```
Permission denied (publickey)
```

Isso acontece porque o SSH ainda não sabe qual chave privada utilizar.

---

### Informando a chave privada

Utilize a opção `-i` para informar o arquivo `.pem`:

```bash
ssh -i EC2Tutorial.pem ec2-user@54.123.45.67
```

---

### Ajustando as permissões do arquivo

Antes da conexão, o macOS/Linux normalmente exibirá:

```
WARNING: UNPROTECTED PRIVATE KEY FILE!
```

Por segurança, apenas o proprietário pode acessar a chave privada.

Execute:

```bash
chmod 400 EC2Tutorial.pem
```

O significado do comando:

- `chmod` → altera permissões de arquivos.
- `400` →
    - proprietário: somente leitura;
    - grupo: nenhum acesso;
    - outros usuários: nenhum acesso.

Depois disso, tente novamente:

```bash
ssh -i EC2Tutorial.pem ec2-user@54.123.45.67
```

---

### Primeira conexão

Na primeira vez, aparecerá uma mensagem semelhante a:

```
The authenticity of host '54.123.45.67' can't be established.

Are you sure you want to continue connecting?
```

Digite:

```
yes
```

Essa confirmação será armazenada no arquivo `~/.ssh/known_hosts`.

---

### Conexão realizada

Ao conectar com sucesso, o prompt mudará para algo parecido com:

```
[ec2-user@ip-172-31-xx-xx ~]$
```

Isso significa que os comandos agora estão sendo executados **dentro da instância EC2**, e não mais no seu Mac.

---

### Comandos básicos

Verificar o usuário conectado:

```bash
whoami
```

Resultado:

```
ec2-user
```

Testar acesso à Internet:

```bash
ping google.com
```

---

### Encerrando a conexão

Para sair da instância:

```bash
exit
```

Você retornará ao terminal do seu Mac.

---

### Conectando novamente

Sempre que precisar acessar a instância novamente:

```bash
ssh -i EC2Tutorial.pem ec2-user@SEU_IP_PUBLICO
```

---

### Atenção ao IP público

Se a instância for **parada (Stop)** e iniciada novamente (**Start**), o **IP público pode mudar** (a menos que ela utilize um Elastic IP).

Antes de conectar novamente, confirme o novo endereço na Console da AWS.

---

## Resumo final (para decorar)

- **SSH** permite controlar uma instância EC2 pelo terminal.
- É necessário possuir o arquivo **`.pem`** correspondente ao Key Pair da instância.
- O **Security Group** deve permitir conexões na **porta 22**.
- No Amazon Linux, o usuário padrão é **`ec2-user`**.
- Antes de conectar, execute **`chmod 400 arquivo.pem`** para proteger a chave privada.
- O comando padrão de conexão é:

```bash
ssh -i arquivo.pem ec2-user@IP_PUBLICO
```

- Use **`exit`** para encerrar a sessão SSH.
- Se a instância for parada e iniciada novamente, verifique se o **IP público** mudou antes de tentar reconectar.

Prints que realizei o passo a passo junto a aula

- Algumas prints abaixo
    
    ![image.png](Sess%C3%A3o%205%20EC2%20Fundamentals/image%2013.png)
    
    ![image.png](Sess%C3%A3o%205%20EC2%20Fundamentals/image%2014.png)
    
    ![image.png](Sess%C3%A3o%205%20EC2%20Fundamentals/image%2015.png)
    
    ![image.png](Sess%C3%A3o%205%20EC2%20Fundamentals/image%2016.png)
    

---

## Aula 42 - EC2 Instance Connect

### O que é o *EC2 Instance Connect*?

O *EC2 Instance Connect* é uma alternativa ao acesso via *SSH* tradicional que permite conectar-se a uma instância *EC2* diretamente pelo navegador, sem precisar configurar um cliente SSH local ou utilizar um arquivo `.pem`.

Na prática, a AWS cria uma chave SSH temporária, envia essa chave para a instância e estabelece a conexão automaticamente.

> **Analogia:** imagine que, em vez de carregar uma chave física para abrir uma porta, a recepção do prédio gera uma chave temporária válida apenas durante sua visita. Após o uso, ela deixa de existir.
> 

---

### Como acessar uma instância usando o *EC2 Instance Connect*

1. Acesse o console da AWS.
2. Selecione sua instância *EC2*.
3. Clique em **Connect**.
4. Escolha a aba **EC2 Instance Connect**.
5. Verifique:
    - O endereço IP público da instância.
    - O usuário (para *Amazon Linux*, normalmente é `ec2-user`).
6. Clique em **Connect**.

Após alguns segundos, uma nova aba será aberta com um terminal diretamente no navegador.

---

### O que pode ser feito no terminal?

Depois de conectado, você pode executar qualquer comando normalmente, por exemplo:

```bash
whoami
ping google.com
ls
pwd
```

A experiência é praticamente igual à de utilizar um terminal SSH tradicional.

---

### Vantagens do *EC2 Instance Connect*

- Não exige instalação de cliente SSH.
- Não requer gerenciamento de arquivos `.pem`.
- Funciona diretamente pelo navegador.
- A AWS gerencia automaticamente uma chave SSH temporária.
- Ideal para acessos rápidos e ocasionais.

---

### O *EC2 Instance Connect* ainda usa SSH?

Sim.

Embora a conexão aconteça pelo navegador, nos bastidores o serviço utiliza o protocolo *SSH*. A diferença é que a AWS cuida automaticamente da autenticação utilizando uma chave temporária.

---

### Requisitos para funcionar

Mesmo sem utilizar um arquivo `.pem`, a instância **precisa aceitar conexões SSH**.

Isso significa que:

- A porta **22 (SSH)** deve estar aberta no *Security Group*.
- A instância precisa possuir um endereço IP público (ou outra forma de ser acessada).

Se a porta 22 estiver fechada, a conexão falhará.

---

### Configuração do *Security Group*

É necessário existir uma regra de entrada semelhante a:

| Tipo | Porta | Origem |
| --- | --- | --- |
| SSH | 22 | Seu IP ou origem permitida |

Durante a demonstração, ao remover a regra da porta 22, o *EC2 Instance Connect* deixou de funcionar imediatamente.

Após adicionar novamente a regra SSH, a conexão voltou a funcionar.

---

### Atenção ao IPv6

Em alguns ambientes, apenas liberar IPv4 não é suficiente.

Caso a conexão continue falhando, pode ser necessário permitir também acesso via IPv6 na porta 22.

---

### Comparação: *SSH* tradicional × *EC2 Instance Connect*

| Característica | *SSH* tradicional | *EC2 Instance Connect* |
| --- | --- | --- |
| Arquivo `.pem` | Necessário | Não |
| Cliente SSH local | Necessário | Não |
| Funciona pelo navegador | Não | Sim |
| Utiliza SSH internamente | Sim | Sim |
| Requer porta 22 aberta | Sim | Sim |

---

### Quando utilizar cada opção?

**Use o *EC2 Instance Connect* quando:**

- desejar rapidez para acessar a instância;
- não quiser configurar chaves SSH;
- estiver utilizando o console da AWS.

**Use o *SSH* tradicional quando:**

- precisar automatizar conexões;
- utilizar ferramentas de desenvolvimento locais;
- trabalhar frequentemente via terminal.
    
    ![image.png](Sess%C3%A3o%205%20EC2%20Fundamentals/image%2017.png)
    
    ![image.png](Sess%C3%A3o%205%20EC2%20Fundamentals/image%2018.png)
    
    ![image.png](Sess%C3%A3o%205%20EC2%20Fundamentals/image%2019.png)
    

---

### Resumo final (para decorar)

- O *EC2 Instance Connect* permite acessar uma instância *EC2* diretamente pelo navegador.
- Não é necessário utilizar um arquivo `.pem`.
- A AWS cria uma chave SSH temporária para realizar a autenticação.
- Internamente, a conexão continua sendo feita via *SSH*.
- A porta **22** deve permanecer aberta no *Security Group*.
- Em alguns casos, também é necessário permitir conexões via **IPv6**.
- É uma forma prática e rápida de administrar instâncias sem configurar um cliente SSH local.

---

## Aula 43 - EC2 Instance Roles Demo

### Objetivo da aula

Nesta aula, o instrutor demonstra a maneira **correta e segura** de permitir que uma instância *EC2* acesse serviços da AWS utilizando uma *IAM Role*, sem armazenar credenciais na máquina.

Essa é uma das práticas de segurança mais importantes da AWS e frequentemente aparece no exame **AWS Certified Solutions Architect Associate**.

---

### Acesso à instância EC2

O acesso pode ser realizado por qualquer método:

- *EC2 Instance Connect* (navegador)
- *SSH* pelo terminal
- *PuTTY* (Windows)

Independentemente do método, todos chegam ao mesmo terminal Linux da instância.

Após conectado, é possível executar comandos normalmente:

```bash
whoami
ping google.com
clear
```

---

### A AWS CLI já vem instalada

A *Amazon Linux AMI* utilizada no curso já possui a *AWS CLI* instalada.

Isso permite executar comandos diretamente contra os serviços da AWS.

Exemplo:

```bash
aws iam list-users
```

No entanto, ao executar o comando pela primeira vez, o retorno é:

```
Unable to locate credentials
```

Isso significa que a instância ainda **não possui credenciais** para acessar a API da AWS.

![image.png](Sess%C3%A3o%205%20EC2%20Fundamentals/image%2020.png)

---

### O erro que muitos iniciantes cometem

Uma solução aparentemente simples seria executar:

```bash
aws configure
```

E informar:

- Access Key ID
- Secret Access Key
- Região

**Mas isso é uma péssima prática de segurança.**

---

### Por que nunca usar `aws configure` em uma EC2?

Ao executar `aws configure`, as credenciais ficam armazenadas na própria instância.

Isso cria diversos riscos:

- qualquer pessoa com acesso à instância poderá visualizar essas credenciais;
- as chaves poderão ser copiadas e utilizadas fora da AWS;
- caso a instância seja comprometida, as credenciais também serão.

> **Regra de ouro:** nunca armazene *Access Keys* de usuários *IAM* dentro de uma instância *EC2*.
> 

---

### A solução correta: IAM Roles

Em vez de utilizar chaves de acesso, a AWS permite anexar uma *IAM Role* diretamente à instância.

Essa função fornece **credenciais temporárias**, gerenciadas automaticamente pela AWS.

O processo é:

1. Criar uma *IAM Role*.
2. Anexar as permissões necessárias (políticas).
3. Associar essa função à instância *EC2*.

A partir desse momento, a própria instância recebe credenciais temporárias sem que nenhuma chave seja armazenada localmente.

---

### Exemplo da aula

Foi criada uma função chamada:

```
DemoRoleForEC2
```

Com a política:

```
IAMReadOnlyAccess
```

Essa política concede acesso somente leitura aos recursos do *IAM*.

---

### Como anexar uma IAM Role à EC2

No console da AWS:

```
EC2
→ Instâncias
→ Actions
→ Security
→ Modify IAM Role
```

![image.png](Sess%C3%A3o%205%20EC2%20Fundamentals/image%2021.png)

Selecionar:

```
DemoRoleForEC2
```

Salvar as alterações.

![image.png](Sess%C3%A3o%205%20EC2%20Fundamentals/image%2022.png)

Depois disso, a aba **Security** da instância passa a exibir a função anexada.

![image.png](Sess%C3%A3o%205%20EC2%20Fundamentals/image%2023.png)

---

### Resultado após anexar a Role

Agora o mesmo comando funciona normalmente:

```bash
aws iam list-users
```

A *AWS CLI* consegue acessar a API utilizando automaticamente as credenciais temporárias fornecidas pela *IAM Role*.

Nenhuma chave foi configurada manualmente.

![image.png](Sess%C3%A3o%205%20EC2%20Fundamentals/image%2024.png)

---

### O que acontece se remover a Role?

Quando a função é desassociada da instância, o mesmo comando retorna:

```
AccessDenied
```

Isso demonstra que:

- a instância perdeu suas permissões;
- a autenticação dependia exclusivamente da *IAM Role*.

---

### Propagação das permissões

Após adicionar ou remover políticas de uma *IAM Role*, as alterações podem não ser aplicadas imediatamente.

Pode levar alguns segundos até alguns minutos para a AWS propagar as mudanças.

Durante esse período, comandos podem continuar retornando:

```
AccessDenied
```

Após a propagação, o acesso passa a funcionar normalmente.

---

### Como a IAM Role funciona internamente?

Quando uma *IAM Role* é anexada à instância:

1. A *EC2* solicita credenciais temporárias ao serviço de metadados da AWS.
2. O serviço *AWS STS (Security Token Service)* gera credenciais temporárias.
3. A *AWS CLI* obtém essas credenciais automaticamente.
4. Os comandos são executados com as permissões da *IAM Role*.

Todo esse processo é transparente para o usuário.

> **Analogia:** em vez de entregar uma chave permanente para um funcionário, a empresa entrega um crachá temporário que funciona apenas enquanto ele está autorizado. Quando a autorização é removida, o crachá deixa de funcionar automaticamente.
> 

---

### Boas práticas

- Utilize sempre *IAM Roles* para instâncias *EC2*.
- Nunca armazene *Access Keys* em servidores.
- Conceda apenas as permissões necessárias (*Princípio do Menor Privilégio*).
- Prefira credenciais temporárias gerenciadas pela AWS.
- Lembre-se de que alterações em permissões podem levar alguns instantes para serem propagadas.

---

### Resumo final (para decorar)

- A *Amazon Linux AMI* já possui a *AWS CLI* instalada.
- Sem uma *IAM Role*, a instância não possui credenciais para acessar a AWS.
- **Nunca** utilize `aws configure` em uma instância *EC2* para armazenar *Access Keys*.
- A forma correta de conceder permissões é anexando uma *IAM Role* à instância.
- A AWS fornece credenciais temporárias automaticamente para a *EC2*.
- Ao remover a *IAM Role*, a instância perde imediatamente suas permissões.
- Alterações em políticas podem levar alguns instantes para serem propagadas.
- Para o exame, memorize: **Instâncias EC2 devem acessar serviços da AWS utilizando *IAM Roles*, nunca credenciais estáticas.**

---

## Aula 44 – Opções de Compra de Instâncias EC2

As instâncias *EC2* podem ser adquiridas de diferentes formas, cada uma otimizada para um cenário de uso. Escolher a opção correta pode reduzir significativamente os custos da infraestrutura.

---

### *On-Demand* (Sob Demanda)

É o modelo padrão e mais simples.

**Características:**

- Paga apenas pelo tempo de uso.
- Sem contrato ou compromisso de longo prazo.
- Cobrança por segundo (Linux e Windows, após o primeiro minuto).
- Maior custo entre as opções.

**Ideal para:**

- Projetos temporários.
- Ambientes de desenvolvimento e testes.
- Aplicações cujo consumo é imprevisível.

**Analogia:**
É como reservar um quarto de hotel na hora. Você entra quando quiser, sai quando quiser, mas paga o preço cheio.

---

### *Reserved Instances (RI)*

Você reserva uma instância por **1 ou 3 anos** em troca de grandes descontos.

**Desconto:**

- Até **72%** em relação ao *On-Demand*.

Durante a compra você escolhe:

- Tipo da instância.
- Região.
- Sistema operacional.
- Modelo de pagamento:
    - Sem pagamento antecipado.
    - Parcialmente antecipado.
    - Totalmente antecipado (maior desconto).

**Ideal para:**

- Bancos de dados.
- Servidores que permanecerão ligados durante muito tempo.
- Aplicações com uso constante.

---

### *Convertible Reserved Instances*

Funcionam como as *Reserved Instances*, porém com mais flexibilidade.

É possível alterar:

- Família da instância.
- Tipo.
- Sistema operacional.
- Região.
- Modelo de hospedagem (*tenancy*).

**Desconto:**

- Até **66%** (menor que a RI tradicional devido à flexibilidade).

**Ideal para:**
Quando você sabe que usará a AWS por muito tempo, mas não tem certeza de qual tipo de instância utilizará futuramente.

---

### *EC2 Savings Plans*

É a opção mais moderna da AWS.

Ao invés de reservar uma instância específica, você se compromete a gastar um determinado valor por hora durante **1 ou 3 anos**.

Exemplo:

> "Vou gastar US$ 10 por hora durante os próximos 3 anos."
> 

Enquanto seu consumo estiver dentro desse valor, você recebe descontos.

**Desconto:**

- Aproximadamente **70%**.

Você possui flexibilidade para alterar:

- Tamanho da instância.
- Sistema operacional.
- Modelo de hospedagem.

Mantendo apenas:

- A família da instância.
- A região.

**Ideal para:**
Empresas que possuem consumo constante, mas podem alterar o tamanho das máquinas ao longo do tempo.

**Analogia:**
É como contratar um plano mensal de academia. Você se compromete a gastar um valor fixo, independentemente de usar equipamentos diferentes.

---

### *Spot Instances*

São as instâncias mais baratas da AWS.

**Desconto:**

- Até **90%**.

Porém existe uma condição importante:

A AWS pode interromper sua instância a qualquer momento caso outro cliente esteja disposto a pagar mais pela capacidade disponível.

Isso significa que **não existe garantia de disponibilidade**.

**Ideal para:**

- Processamento em lote (*batch processing*).
- Big Data.
- Processamento de imagens e vídeos.
- Renderização.
- Machine Learning.
- Cargas distribuídas.
- Trabalhos que podem ser reiniciados.

**Não recomendado para:**

- Bancos de dados.
- Sistemas críticos.
- Aplicações que precisam ficar sempre disponíveis.

**Analogia:**
É como comprar uma passagem aérea em promoção de última hora. Você paga muito barato, mas pode perder a vaga se aparecer alguém pagando mais.

---

### *Dedicated Hosts*

Você reserva um **servidor físico inteiro** exclusivamente para sua empresa.

Isso oferece acesso ao hardware físico e atende requisitos específicos de licenciamento e conformidade.

É a opção mais cara.

**Ideal para:**

- Licenças "Bring Your Own License" (*BYOL*).
- Softwares licenciados por processador, núcleo ou servidor.
- Empresas com exigências regulatórias.

---

### *Dedicated Instances*

São diferentes dos *Dedicated Hosts*.

Aqui:

- O hardware é exclusivo para sua conta.
- Porém você **não controla** o servidor físico.
- Outras instâncias da sua própria conta podem compartilhar esse hardware.

**Diferença principal:**

| *Dedicated Instance* | *Dedicated Host* |
| --- | --- |
| Hardware dedicado | Servidor físico inteiro dedicado |
| Sem acesso ao hardware físico | Controle sobre o servidor físico |
| Menor custo | Maior custo |

**Resumo simples:**

- *Dedicated Instance* → você aluga um apartamento inteiro.
- *Dedicated Host* → você compra o prédio inteiro.

---

### *Capacity Reservations*

Não oferece desconto.

Seu objetivo é apenas garantir que haverá capacidade disponível em uma determinada *Availability Zone (AZ)*.

Você paga mesmo que a instância não esteja sendo utilizada.

**Ideal para:**
Aplicações críticas que precisam iniciar imediatamente em uma AZ específica.

- **AZ** significa **Availability Zone (Zona de Disponibilidade)**.
    
    Uma *Availability Zone* é um **datacenter físico (ou um conjunto de datacenters muito próximos)** dentro de uma região da AWS.
    
    Por exemplo:
    
    - Região: `us-east-1` (Norte da Virgínia)
        - AZs:
            - `us-east-1a`
            - `us-east-1b`
            - `us-east-1c`
            - `us-east-1d`
            - ...
    - Região: `sa-east-1` (São Paulo)
        - AZs:
            - `sa-east-1a`
            - `sa-east-1b`
            - `sa-east-1c`
    
    Cada AZ possui:
    
    - Energia independente.
    - Rede independente.
    - Infraestrutura física separada.
    - Baixa latência entre as demais AZs da mesma região.

Essa opção pode ser combinada com:

- *Reserved Instances*
- *Savings Plans*

Assim você obtém:

- Garantia de capacidade.
- Desconto no custo.

---

### Comparação rápida

| Opção | Economia | Flexibilidade | Melhor uso |
| --- | --- | --- | --- |
| *On-Demand* | ❌ | ⭐⭐⭐⭐⭐ | Testes, projetos temporários |
| *Reserved Instances* | ⭐⭐⭐⭐ | ⭐⭐ | Uso constante e previsível |
| *Convertible RI* | ⭐⭐⭐ | ⭐⭐⭐⭐ | Uso longo com mudanças futuras |
| *Savings Plans* | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | Uso constante com maior flexibilidade |
| *Spot* | ⭐⭐⭐⭐⭐ | ⭐ | Processamentos interrompíveis |
| *Dedicated Host* | ❌ | ⭐⭐ | Licenciamento e compliance |
| *Dedicated Instance* | ❌ | ⭐⭐⭐ | Hardware dedicado |
| *Capacity Reservation* | ❌ | ⭐⭐⭐ | Garantir disponibilidade |

---

### Como escolher?

Use esta regra prática:

- **Não sei quanto vou usar** → *On-Demand*.
- **Vou usar continuamente por anos** → *Reserved Instance*.
- **Vou usar continuamente, mas posso mudar o tamanho da máquina** → *Savings Plan*.
- **Posso perder a máquina sem problemas** → *Spot Instance*.
- **Preciso de um servidor físico exclusivo** → *Dedicated Host*.
- **Preciso apenas garantir capacidade em uma AZ** → *Capacity Reservation*.

---

### Resumo final (para decorar)

- **On-Demand** → flexível, sem compromisso, mais caro.
- **Reserved Instance** → uso previsível por 1 ou 3 anos, até **72%** de desconto.
- **Convertible RI** → semelhante à RI, porém com maior flexibilidade e menor desconto.
- **Savings Plans** → compromisso financeiro por hora, mais flexível que RI.
- **Spot** → até **90%** de desconto, mas pode ser interrompida pela AWS.
- **Dedicated Host** → servidor físico exclusivo para licenciamento e conformidade.
- **Dedicated Instance** → hardware exclusivo, sem acesso ao servidor físico.
- **Capacity Reservation** → garante disponibilidade de capacidade, sem desconto.

> **Dica para a prova:** sempre associe a opção de compra ao tipo de carga de trabalho:
> 
> - **Curta e imprevisível** → *On-Demand*.
> - **Longa e estável** → *Reserved Instances* ou *Savings Plans*.
> - **Interrompível** → *Spot Instances*.
> - **Compliance/licenciamento** → *Dedicated Hosts*.
> - **Garantia de capacidade** → *Capacity Reservations*.

---

# Aula 45 – Spot Instances & Spot Fleet (Instâncias Spot)

As **Spot Instances** permitem utilizar a capacidade ociosa da AWS com **descontos de até 90%** em relação às instâncias **On-Demand**. Em troca desse desconto, a AWS pode interromper sua instância quando precisar da capacidade.

---

### Como funcionam as Spot Instances?

Ao solicitar uma Spot Instance, você define um **preço máximo** que está disposto a pagar por hora.

- Se o **preço Spot atual** estiver **abaixo** do seu limite, sua instância é iniciada ou continua executando.
- Se o preço ultrapassar seu limite (ou a AWS precisar da capacidade), sua instância poderá ser interrompida ou encerrada.

Antes da interrupção, a AWS fornece um **aviso de aproximadamente 2 minutos**, permitindo que a aplicação:

- Salve dados importantes.
- Finalize tarefas em andamento.
- Pare de receber novas requisições.
- Seja encerrada de forma segura.

> **Analogia:** Imagine um hotel com quartos vagos. Enquanto houver quartos sobrando, você pode se hospedar pagando um preço muito baixo. Porém, se aparecerem hóspedes que pagam a tarifa cheia, o hotel pode pedir que você deixe o quarto.
> 

---

### Quando utilizar Spot Instances?

São ideais para cargas de trabalho que suportam interrupções, como:

- Processamento em lote (*batch processing*).
- Análise de dados.
- Simulações.
- Renderização.
- Outras tarefas tolerantes a falhas.

**Não são indicadas para:**

- Bancos de dados.
- Aplicações críticas.
- Sistemas que precisam permanecer disponíveis continuamente.

---

### Preço das Spot Instances

O preço varia constantemente de acordo com:

- Oferta de capacidade.
- Demanda.
- Região.
- Zona de Disponibilidade (*Availability Zone - AZ*).

Cada AZ pode possuir um preço diferente para a mesma instância.

Mesmo com essa variação, normalmente o custo permanece muito inferior ao das instâncias **On-Demand**.

---

## Spot Request (Solicitação Spot)

Ao criar uma solicitação Spot, você informa:

- Quantidade de instâncias.
- Preço máximo.
- Configuração da instância (AMI, tipo etc.).
- Período de validade da solicitação.
- Tipo da solicitação.

Existem dois tipos principais.

### Single Request (Solicitação única)

A solicitação é atendida apenas uma vez.

Depois que a instância é criada:

- O pedido é removido.
- Se a instância for interrompida, ela **não será recriada automaticamente**.

---

### Persistent Request (Solicitação persistente)

A solicitação permanece ativa enquanto estiver válida.

Se uma Spot Instance for interrompida, a AWS tentará iniciar outra automaticamente quando houver capacidade disponível novamente.

É uma forma de manter sempre a quantidade desejada de instâncias Spot.

---

## Como encerrar corretamente uma Spot Instance

A ordem é importante.

Primeiro:

1. Cancelar o **Spot Request**.

Depois:

1. Encerrar as **Spot Instances**.

Se fizer o contrário, o pedido persistente continuará ativo e a AWS poderá criar novas instâncias automaticamente.

> Esse detalhe é frequentemente cobrado na certificação.
> 

---

# Spot Fleet

O **Spot Fleet** permite solicitar várias instâncias Spot (e opcionalmente On-Demand) ao mesmo tempo.

Em vez de escolher apenas um tipo de instância, você informa diversas opções, como:

- Diferentes tipos de instâncias.
- Diferentes sistemas operacionais.
- Diferentes Availability Zones.

A AWS escolhe automaticamente a melhor combinação para atingir a capacidade desejada com o menor custo possível.

---

### Estratégias do Spot Fleet

#### Lowest Price

Escolhe sempre o conjunto com o menor preço.

- Maximiza a economia.
- Ideal para cargas curtas.

---

#### Diversified

Distribui as instâncias entre vários pools diferentes.

- Maior disponibilidade.
- Menor risco de perder todas as instâncias ao mesmo tempo.
- Indicado para cargas de longa duração.

---

#### Capacity Optimized

Prioriza os pools com maior capacidade disponível.

Objetivo:

- Reduzir as chances de interrupção das Spot Instances.

---

#### Price Capacity Optimized

Combina:

- Alta disponibilidade.
- Menor custo possível.

Segundo a aula, esta costuma ser a melhor escolha para a maioria das cargas de trabalho.

---

## Spot Instance × Spot Fleet

### Spot Instance

Você escolhe exatamente:

- O tipo da instância.
- A Availability Zone.
- A configuração desejada.

A AWS tenta fornecer exatamente essa instância.

---

### Spot Fleet

Você fornece várias possibilidades.

A AWS escolhe automaticamente a melhor combinação considerando:

- Preço.
- Capacidade disponível.
- Estratégia escolhida.

É uma solução mais inteligente para maximizar economia e disponibilidade.

---

# Resumo final (para decorar)

- **Spot Instances** oferecem até **90% de desconto** em relação às On-Demand.
- Podem ser interrompidas pela AWS quando necessário.
- Há um aviso de aproximadamente **2 minutos** antes da interrupção.
- São ideais para workloads tolerantes a falhas.
- Evite usá-las para bancos de dados e aplicações críticas.
- Existem dois tipos de solicitações:
    - **Single Request:** cria uma única vez.
    - **Persistent Request:** recria automaticamente quando possível.
- Para encerrar definitivamente:
    1. Cancele o **Spot Request**.
    2. Depois encerre as instâncias.
- **Spot Fleet** permite utilizar vários tipos de instâncias e AZs para obter a melhor combinação de custo e capacidade.
- Estratégias do Spot Fleet:
    - **Lowest Price:** menor preço.
    - **Diversified:** maior disponibilidade.
    - **Capacity Optimized:** menor chance de interrupção.
    - **Price Capacity Optimized:** equilíbrio entre custo e disponibilidade (recomendado para a maioria dos casos).

---

# Aula 46 — EC2 Instances Launch Types Hands On
Formas de lançar instâncias EC2

Nesta aula, o instrutor apresenta todas as principais maneiras de executar instâncias EC2 na AWS, explicando quando utilizar cada opção e suas principais características.

---

## Spot Requests

Uma **Spot Request** é uma solicitação para executar instâncias Spot aproveitando a capacidade ociosa da AWS.

Na criação da solicitação, é possível configurar:

- Modelo de lançamento (*Launch Template*) ou parâmetros manualmente.
- AMI.
- Par de chaves (*Key Pair*).
- Tipo da instância.
- Preço máximo que deseja pagar.
- Período de validade da solicitação.
- Quantidade de instâncias desejadas.

Também é possível definir se as instâncias devem:

- Ser encerradas.
- Ser paradas.
- Hibernar quando ocorrer uma interrupção.

---

### Capacidade alvo (Target Capacity)

Ao criar uma Spot Fleet, você pode definir sua necessidade de capacidade em diferentes unidades:

- Número de instâncias.
- Quantidade de vCPUs.
- Quantidade de memória (RAM).

Isso permite que a AWS escolha automaticamente as instâncias mais adequadas para atingir esse objetivo.

---

### Seleção dos tipos de instância

Existem duas formas de definir quais instâncias poderão ser utilizadas:

**Manual**

Você escolhe exatamente os tipos desejados.

Exemplo:

- c3.large
- c4.large
- c5.large

---

**Por atributos**

Você informa apenas os requisitos mínimos, como:

- Quantidade mínima e máxima de vCPUs.
- Quantidade mínima e máxima de memória.

A AWS procura automaticamente os tipos de instância que atendem a esses critérios.

> Quanto menos restrições forem impostas, maior será a quantidade de opções disponíveis e maiores tendem a ser as economias.
> 

---

### Estratégia de alocação

Durante a criação da Spot Fleet é possível escolher a estratégia de alocação.

As principais vistas na aula são:

- **Lowest Price** → prioriza o menor preço.
- **Capacity Optimized** → prioriza maior disponibilidade de capacidade.
- **Diversified** → distribui as instâncias entre vários tipos escolhidos.

---

## Criando uma Spot Instance diretamente

Também é possível criar uma Spot Instance durante o processo normal de criação de uma EC2.

Basta acessar:

**Launch Instance → Advanced Details → Request Spot Instance**

Por padrão:

- O preço máximo é limitado ao preço **On-Demand**.
- O tipo de solicitação é **Single Request**.

Se desejar, pode alterar:

- Preço máximo.
- Tipo da solicitação (*Single* ou *Persistent*).
- Data de expiração.
- Comportamento quando houver interrupção (Parar ou Hibernar).

---

## Reserved Instances

As **Reserved Instances (RI)** permitem reservar capacidade para um tipo específico de instância por um período determinado.

É possível escolher:

- Tipo da instância.
- Região.
- Duração (1 ou 3 anos).
- Forma de pagamento:
    - Total antecipado.
    - Parcialmente antecipado.
    - Sem pagamento antecipado.

Em troca desse compromisso, a AWS oferece descontos em relação às instâncias On-Demand.

> A aula comenta que as **Reserved Instances** vêm perdendo espaço para os **Savings Plans**, que oferecem maior flexibilidade.
> 

---

## Savings Plans

Os **Savings Plans** funcionam de maneira diferente das Reserved Instances.

Em vez de reservar um tipo específico de instância, você se compromete a gastar um determinado valor por hora durante um período de:

- 1 ano.
- 3 anos.

Em troca, recebe descontos mantendo maior flexibilidade para alterar:

- Tipo da instância.
- Família da instância.
- Availability Zone (AZ).

Segundo o instrutor, atualmente esta costuma ser a opção recomendada.

---

## Dedicated Hosts

Os **Dedicated Hosts** fornecem um servidor físico exclusivo para sua conta.

São utilizados principalmente quando há necessidade de:

- Aproveitar licenças de software específicas.
- Atender requisitos de conformidade.
- Ter controle sobre o hardware físico.

Durante a criação, é possível definir:

- Nome.
- Família de instâncias.
- Availability Zone.

Posteriormente, diversas instâncias EC2 podem ser executadas nesse host dedicado.

---

## Capacity Reservations

As **Capacity Reservations** garantem que determinada capacidade de instâncias EC2 estará disponível quando você precisar.

Exemplo:

Reservar quatro instâncias **m5.2xlarge** em uma região específica.

Mesmo que você ainda não esteja utilizando essas instâncias, a AWS mantém essa capacidade reservada.

> **Importante:** você paga pela reserva, independentemente de utilizar ou não essa capacidade.
> 

---

## Comparação rápida

| Opção | Melhor uso |
| --- | --- |
| **On-Demand** | Uso imediato, sem compromisso. |
| **Spot** | Máxima economia para cargas tolerantes a interrupções. |
| **Reserved Instance** | Uso previsível de uma instância específica por longo período. |
| **Savings Plan** | Uso contínuo com maior flexibilidade e desconto. |
| **Dedicated Host** | Licenciamento e requisitos de hardware dedicado. |
| **Capacity Reservation** | Garantir disponibilidade de capacidade quando necessário. |

---

# Resumo final (para decorar)

- Existem várias formas de executar instâncias EC2, cada uma voltada para um cenário diferente.
- **Spot Requests** permitem utilizar capacidade ociosa com grande desconto.
- A capacidade desejada pode ser definida por número de instâncias, vCPUs ou memória.
- É possível escolher tipos de instância manualmente ou apenas informar atributos mínimos.
- Uma Spot Instance também pode ser criada diretamente durante o lançamento de uma EC2.
- **Reserved Instances** oferecem desconto em troca do compromisso com uma instância específica.
- **Savings Plans** substituem grande parte dos casos de uso das Reserved Instances por oferecerem mais flexibilidade.
- **Dedicated Hosts** fornecem um servidor físico exclusivo, geralmente para requisitos de licenciamento e conformidade.
- **Capacity Reservations** garantem capacidade disponível para futuras execuções, mas são cobradas mesmo quando não estão sendo utilizadas.