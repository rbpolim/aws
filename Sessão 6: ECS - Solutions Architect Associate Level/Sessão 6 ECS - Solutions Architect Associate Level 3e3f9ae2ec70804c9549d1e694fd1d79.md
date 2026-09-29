# Sessão 6: ECS - Solutions Architect Associate Level

## Aula 48 – IP Público, IP Privado e Elastic IP

### IPv4 e IPv6

Um endereço IP identifica um dispositivo dentro de uma rede.

Os dois principais padrões são:

- **IPv4** → formato mais comum, como `192.168.1.10`.
- **IPv6** → possui um espaço de endereçamento muito maior e utiliza uma representação mais extensa, como `2001:db8::1`.

O curso trabalha principalmente com **IPv4**, pois ele ainda é amplamente utilizado.

No IPv4, cada número pode variar de **0 a 255**:

```
192.168.1.10
 ↑    ↑  ↑ ↑
 0-255
```

Isso permite aproximadamente **4,3 bilhões de endereços IPv4**.

---

### IP Público

O **IP público** é utilizado para identificar uma máquina na Internet.

Características:

- Pode ser acessado pela Internet, desde que as regras de rede permitam.
- Deve ser **globalmente único** enquanto estiver em uso.
- É utilizado para comunicação entre uma máquina e a Internet.

Exemplo:

```
Internet
    │
    ▼
IP Público
    │
    ▼
Servidor EC2
```

Se uma instância EC2 possui um IP público, podemos utilizá-lo para acessá-la pela Internet, por exemplo, através de *SSH* ou HTTP.

---

### IP Privado

O **IP privado** é utilizado dentro de uma rede privada.

Exemplo:

```
Rede privada
│
├── Servidor A → 10.0.0.10
├── Servidor B → 10.0.0.20
└── Servidor C → 10.0.0.30
```

Essas máquinas conseguem se comunicar utilizando seus IPs privados.

Uma característica importante é que **IP privado não precisa ser globalmente único**.

Por exemplo:

```
Empresa A
Servidor → 10.0.0.10

Empresa B
Servidor → 10.0.0.10
```

Isso é perfeitamente válido porque são redes privadas diferentes.

---

### IP Público x IP Privado

| Característica | IP Público | IP Privado |
| --- | --- | --- |
| Visível na Internet | Sim | Não diretamente |
| Escopo | Internet | Rede privada |
| Deve ser globalmente único | Sim | Não |
| Comunicação entre máquinas da rede | Sim | Sim |
| Exemplo | `18.123.45.10` | `10.0.1.10` |

> **Analogia:** pense em um condomínio. O **IP privado** seria o número do apartamento, utilizado pelas pessoas dentro do condomínio. O **IP público** seria o endereço do condomínio, utilizado para encontrá-lo a partir da rua.
> 

---

### Como uma máquina privada acessa a Internet?

Uma máquina com IP privado não precisa possuir um IP público próprio para acessar a Internet.

Ela pode utilizar um **Internet Gateway** e mecanismos de tradução de endereço, como o NAT.

De forma simplificada:

```
EC2
IP privado
10.0.1.10
   │
   ▼
NAT / Internet Gateway
   │
   ▼
Internet
```

Isso permite que recursos dentro de uma rede privada se comuniquem com a Internet sem necessariamente serem diretamente acessíveis pela Internet.

---

### IP público da instância EC2

Por padrão, uma instância EC2 pode possuir:

- **IP privado** → utilizado dentro da rede.
- **IP público** → utilizado para comunicação externa.

Um detalhe muito importante é que o **IP público pode mudar**.

Por exemplo:

```
EC2 iniciada
→ IP público: 18.10.20.30

EC2 parada

EC2 iniciada novamente
→ IP público: 52.40.50.60
```

Por isso, não é recomendado depender diretamente do IP público de uma instância para identificar permanentemente um servidor.

---

### Elastic IP

O **Elastic IP** é um endereço IPv4 público estático que você pode associar a uma instância EC2.

Diferentemente do IP público comum, ele permanece associado à sua conta até que você o libere.

Exemplo:

```
Elastic IP
18.10.20.30
     │
     ▼
   EC2 A
```

Se a instância apresentar um problema, o Elastic IP pode ser associado a outra instância:

```
Elastic IP
18.10.20.30
     │
     ▼
   EC2 B
```

Isso permite manter o mesmo endereço público mesmo alterando a instância.

---

### Por que evitar Elastic IP?

Embora seja útil em situações específicas, o instrutor recomenda evitar o uso excessivo de Elastic IPs.

A arquitetura normalmente deve preferir mecanismos como:

- *DNS*
- *Route 53*
- *Load Balancer*

Em vez de depender diretamente de um endereço IP fixo.

Por exemplo:

```
Usuário
   │
   ▼
DNS
app.exemplo.com
   │
   ▼
Load Balancer
   │
   ├── EC2 A
   └── EC2 B
```

Dessa forma, o usuário não precisa conhecer o IP das instâncias.

---

### Elastic IP não é a mesma coisa que IP público

Essa diferença é importante:

**IP público comum:**

```
EC2 → IP público temporário
```

Pode mudar quando a instância é parada e iniciada novamente.

**Elastic IP:**

```
EC2 → Elastic IP
```

O endereço pode permanecer o mesmo enquanto estiver associado à sua conta.

---

### Acesso à EC2

Se você estiver fora da rede privada e **não estiver utilizando VPN**, normalmente utilizará o **IP público** para acessar a instância.

```
Seu computador
      │
      │ Internet
      ▼
 IP público da EC2
      │
      ▼
 IP privado da EC2
```

Já dentro da mesma rede privada, ou através de uma conexão como VPN, é possível utilizar o **IP privado**.

---

### Resumo para o exame

- **IPv4** → formato como `192.168.1.10`.
- **IPv6** → possui um espaço de endereçamento muito maior.
- **IP público** → identifica a máquina na Internet.
- **IP privado** → identifica a máquina dentro de uma rede privada.
- IPs privados **podem se repetir em redes diferentes**.
- Uma EC2 pode possuir **IP privado e IP público**.
- O **IP público padrão pode mudar** quando a EC2 é parada e iniciada novamente.
- **Elastic IP** → IPv4 público estático que pode ser associado a uma instância.
- É geralmente preferível utilizar **DNS e Load Balancers** em arquiteturas escaláveis em vez de depender diretamente de Elastic IPs.
- Para acessar uma EC2 pela Internet sem VPN, normalmente utilizamos seu **IP público**.
- Para comunicação interna entre recursos da AWS, normalmente utilizamos os **IPs privados**.

---

## Aula 49 – Comportamento do IP Público e Elastic IP

### IP público x IP privado na prática

Uma instância *EC2* normalmente possui um **IP privado** e pode possuir um **IP público**.

Quando você está fora da AWS, conectado apenas à Internet, utiliza o **IP público** para acessar a instância.

```
Seu computador
      │
   Internet
      │
      ▼
 IP público da EC2
      │
      ▼
 IP privado da EC2
```

O **IP privado** só pode ser utilizado diretamente por recursos que tenham acesso à rede privada onde a EC2 está localizada, como outra instância na mesma VPC ou uma conexão via VPN.

### Analogia simples

Imagine uma empresa:

- **IP público** → endereço da empresa na rua. Qualquer pessoa pode chegar até o prédio.
- **IP privado** → número de uma sala dentro do prédio. Para chegar nela, primeiro você precisa estar dentro do prédio.

### Caso de uso

Imagine que você tenha uma API rodando em uma EC2:

```
API → EC2
IP público: 54.123.10.20
IP privado: 10.0.1.15
```

Seu computador pode acessar:

```
http://54.123.10.20
```

Mas não consegue acessar diretamente:

```
http://10.0.1.15
```

a menos que esteja conectado à rede privada, por exemplo, através de uma VPN.

---

### O IP público muda quando a EC2 é parada?

**Sim.**

Quando uma instância *EC2* possui um IP público automático, esse endereço pode mudar quando a instância é **parada e iniciada novamente**.

Exemplo:

```
Antes:

EC2
IP público → 54.10.20.30

Stop + Start

Depois:

EC2
IP público → 18.50.60.70
```

Isso significa que, se você utilizava o IP antigo para fazer *SSH*, precisará utilizar o novo IP.

### Atenção: Stop/Start ≠ Reboot

O comportamento apresentado na aula ocorre quando fazemos:

**Stop → Start**

Não é o mesmo que simplesmente fazer um **Reboot**.

O ponto importante para o exame é:

> **Stop + Start pode alterar o IP público automático da EC2.**
> 

---

### O IP privado muda?

Normalmente, **não** nesse cenário.

O IP privado continua associado à interface de rede da instância.

Exemplo:

```
Antes:
IP privado → 10.0.1.15
IP público → 54.10.20.30

Stop + Start

Depois:
IP privado → 10.0.1.15
IP público → 18.50.60.70
```

Essa diferença é importante:

| Endereço | Stop + Start |
| --- | --- |
| IP público automático | 🔄 Pode mudar |
| IP privado | ✅ Permanece |
| Elastic IP | ✅ Permanece |

---

### Elastic IP

Quando precisamos de um **IPv4 público estático**, podemos utilizar um **Elastic IP**.

Ele é um endereço IPv4 público que fica associado à sua conta AWS e pode ser vinculado a uma instância *EC2*.

```
Elastic IP
54.10.20.30
      │
      ▼
     EC2
```

Se a instância for parada e iniciada novamente:

```
Elastic IP
54.10.20.30
      │
      ▼
     EC2
```

O endereço continua o mesmo.

### Analogia simples

Imagine que o IP público automático seja um **telefone descartável**:

> Toda vez que você troca de aparelho, pode receber outro número.
> 

O *Elastic IP* seria seu **número de telefone fixo**:

> Você pode trocar o aparelho, mas mantém o mesmo número.
> 

---

### Caso de uso do Elastic IP

Imagine que você tenha um servidor que precisa ser acessado por um sistema externo através de um IP específico.

```
Sistema externo
      │
      ▼
Elastic IP
54.10.20.30
      │
      ▼
EC2
```

Se a EC2 precisar ser substituída:

```
              ┌── EC2 antiga
              │
Elastic IP ───┤
              │
              └── EC2 nova
```

Você pode associar o mesmo *Elastic IP* à nova instância.

Assim, o sistema externo continua utilizando:

```
54.10.20.30
```

sem precisar ser reconfigurado.

---

### Elastic IP não deve ser usado indiscriminadamente

Embora seja útil, a arquitetura moderna geralmente tenta **evitar depender diretamente de IPs fixos**.

Em aplicações maiores, é comum utilizar:

```
Usuário
   │
   ▼
DNS
   │
   ▼
Load Balancer
   │
   ├── EC2
   ├── EC2
   └── EC2
```

Nesse modelo, o usuário não precisa conhecer o IP das instâncias.

Isso facilita:

- Escalabilidade.
- Alta disponibilidade.
- Substituição de instâncias.
- Distribuição de tráfego.

---

### Custo dos endereços IPv4

A aula destaca que endereços **IPv4 públicos e Elastic IPs possuem cobrança** na AWS.

Portanto, é importante não deixar recursos desnecessários ativos.

Especialmente durante os estudos:

- Termine as instâncias *EC2* que não estiver utilizando.
- Libere *Elastic IPs* que não são mais necessários.

> **Importante:** os valores e franquias de cobrança de IPv4 podem mudar com o tempo. Para valores atuais, consulte a página de preços da AWS.
> 

---

### Associando um Elastic IP

O processo apresentado na aula é basicamente:

```
1. Criar Elastic IP
       ↓
2. Selecionar o Elastic IP
       ↓
3. Associar à instância EC2
       ↓
4. Escolher a interface/IP privado
       ↓
5. EC2 passa a utilizar o Elastic IP
```

Depois disso, o Elastic IP aparece como o **IPv4 público** da instância.

---

### Liberar o Elastic IP

Quando você não precisa mais dele, existem duas ações diferentes:

**Desassociar:**

```
Elastic IP
    X
   EC2
```

Remove o endereço da instância.

**Liberar:**

```
Elastic IP
    ↓
AWS
```

Devolve o endereço para a AWS.

> **Cuidado:** apenas desassociar não significa necessariamente que você deixou de possuir o Elastic IP. Para evitar cobrança, é necessário **liberá-lo quando não for mais necessário**.
> 

---

### Fluxo completo da aula

```
EC2 criada
   │
   ├── IP privado → permanece
   │
   └── IP público automático
             │
             ▼
        Stop + Start
             │
             ▼
       IP público muda
```

Com Elastic IP:

```
EC2
 │
 ├── IP privado → permanece
 │
 └── Elastic IP → permanece
                    │
                    ▼
              Stop + Start
                    │
                    ▼
             mesmo endereço
```

---

### Resumo para o exame

- **IP privado** → utilizado dentro da rede privada.
- **IP público** → utilizado para comunicação através da Internet.
- Para acessar uma EC2 pela Internet, normalmente utilizamos seu **IP público**.
- O **IP privado** não é diretamente acessível pela Internet.
- **Stop + Start** pode alterar o IP público automático.
- O IP privado permanece associado à interface de rede.
- **Elastic IP** fornece um IPv4 público estático.
- O Elastic IP permanece o mesmo mesmo após um **Stop + Start**.
- É possível mover um Elastic IP de uma instância para outra.
- Quando não precisar mais de um Elastic IP, **libere-o** para evitar cobranças.
- Em arquiteturas escaláveis, normalmente é preferível utilizar **DNS + Load Balancer** em vez de depender diretamente de IPs fixos.
- ***Prints que eu realizei os testes junto a aula***
    
    ![image.png](Sess%C3%A3o%206%20ECS%20-%20Solutions%20Architect%20Associate%20Level/image.png)
    
    ![image.png](Sess%C3%A3o%206%20ECS%20-%20Solutions%20Architect%20Associate%20Level/image%201.png)
    
    ![image.png](Sess%C3%A3o%206%20ECS%20-%20Solutions%20Architect%20Associate%20Level/image%202.png)
    
    ![image.png](Sess%C3%A3o%206%20ECS%20-%20Solutions%20Architect%20Associate%20Level/image%203.png)
    
    ![image.png](Sess%C3%A3o%206%20ECS%20-%20Solutions%20Architect%20Associate%20Level/image%204.png)
    

---

## Aula 50 — EC2 Placement Groups

### O que são *Placement Groups*?

Os **Placement Groups** permitem definir para a AWS **como as instâncias EC2 devem ser posicionadas fisicamente na infraestrutura**.

Você não escolhe diretamente o servidor físico ou rack, mas informa à AWS qual estratégia de posicionamento deseja.

Existem **3 estratégias**:

| Estratégia | Objetivo | Principal característica |
| --- | --- | --- |
| **Cluster** | Desempenho | Instâncias próximas fisicamente |
| **Spread** | Alta disponibilidade | Instâncias em hardware separado |
| **Partition** | Escalabilidade + isolamento | Instâncias distribuídas entre diferentes racks |

---

### *Cluster Placement Group*

No **Cluster**, as instâncias ficam **próximas umas das outras dentro de uma única AZ**.

Isso proporciona:

- **Baixa latência**
- **Alta largura de banda**
- Comunicação rápida entre as instâncias
- Alto desempenho computacional
- Instâncias na **mesma Availability Zone**

A ideia é colocar as máquinas próximas fisicamente para que elas consigam trocar dados rapidamente.

**Desvantagem:** existe um maior risco de impacto simultâneo. Se ocorrer um problema grave naquela *Availability Zone*, todas as instâncias do grupo podem ser afetadas.

**Casos de uso:**

- *Big Data*
- Processamento distribuído
- Aplicações que precisam de comunicação muito rápida entre servidores
- Workloads de alta performance

**Analogia:** imagine uma equipe trabalhando em um escritório. Todos ficam **na mesma sala**, então conseguem conversar rapidamente, mas se houver um problema nessa sala, todos são afetados.

**Exemplo:**

```
AZ us-east-1a

┌─────────────────────────────┐
│       Cluster Group         │
│                             │
│  EC2 ── EC2 ── EC2 ── EC2   │
│       baixa latência        │
└─────────────────────────────┘
```

---

### *Spread Placement Group*

No **Spread**, a prioridade é **reduzir o risco de falhas simultâneas**.

As instâncias são colocadas em **hardware diferente** e podem ser distribuídas entre várias AZs.

```
AZ 1                  AZ 2

Hardware 1 → EC2      Hardware 4 → EC2
Hardware 2 → EC2      Hardware 5 → EC2
Hardware 3 → EC2      Hardware 6 → EC2
```

Se um hardware apresentar problema, as outras instâncias estarão em hardwares separados.

**Principal característica:** isolamento de falhas.

**Limitação importante:** o *Spread Placement Group* possui uma quantidade limitada de instâncias por AZ (**até 7 instâncias por AZ por grupo**).

**Casos de uso:**

- Aplicações críticas
- Instâncias que não podem compartilhar o mesmo hardware
- Sistemas que precisam minimizar o impacto de uma falha física

**Analogia:** em vez de colocar seis pessoas no mesmo prédio, você coloca cada uma em **prédios diferentes**. Se um prédio tiver um problema, as outras pessoas continuam seguras.

---

### *Partition Placement Group*

O **Partition** também busca isolamento de falhas, mas foi projetado para **workloads grandes e distribuídos**.

As instâncias são distribuídas entre diferentes **partições**, e cada partição corresponde a um conjunto diferente de racks de hardware.

```
AZ 1
┌─────────────┐   ┌─────────────┐
│ Partition 1 │   │ Partition 2 │
│ EC2 EC2 EC2 │   │ EC2 EC2 EC2 │
│   Rack A    │   │   Rack B    │
└─────────────┘   └─────────────┘

AZ 2
┌─────────────┐
│ Partition 3 │
│ EC2 EC2 EC2 │
│   Rack C    │
└─────────────┘
```

Uma característica importante é que **instâncias de partições diferentes não compartilham o mesmo rack físico**.

Assim, uma falha em um rack pode afetar uma partição, mas as outras partições permanecem isoladas.

É possível ter **até 7 partições por AZ** e suportar **centenas de instâncias EC2**.

### Por que *Partition* é diferente de *Spread*?

A diferença principal está na **escala**:

- **Spread:** isolamento individual de instâncias → quantidade limitada.
- **Partition:** isolamento por grupos/racks → permite trabalhar com **centenas de instâncias**.

Além disso, a aplicação pode descobrir **em qual partição uma instância está** através dos metadados da instância.

**Casos de uso:**

- *Apache Kafka*
- *Apache Cassandra*
- *HDFS*
- *HBase*
- Aplicações de *Big Data* distribuídas

**Analogia:** imagine um depósito dividido em vários galpões. Dentro de cada galpão existem várias máquinas. Se um galpão pegar fogo, as máquinas dos outros galpões continuam funcionando.

---

### Comparação para memorizar

```
CLUSTER
→ Quero VELOCIDADE
→ Instâncias próximas
→ 1 AZ
→ Baixa latência / alta rede

SPREAD
→ Quero ISOLAMENTO
→ Cada instância em hardware diferente
→ Até 7 instâncias por AZ
→ Aplicações críticas

PARTITION
→ Quero ESCALA + ISOLAMENTO
→ Instâncias separadas por racks/partições
→ Centenas de instâncias
→ Kafka, Cassandra, HDFS, HBase
```

### 🧠 Como lembrar na prova

Pense em **C-S-P**:

**C → Cluster → Close**

Instâncias **próximas** para obter performance.

**S → Spread → Separate**

Instâncias **separadas** para reduzir falhas.

**P → Partition → Partes**

Instâncias divididas em **partições/racks** para workloads distribuídos em grande escala.

### Caso de uso real

Imagine uma empresa executando **Apache Kafka** com centenas de instâncias EC2.

Colocar todas as instâncias próximas poderia aumentar o risco de uma falha física afetar uma grande parte do cluster. O *Partition Placement Group* permite distribuir os brokers entre diferentes partições/racks, reduzindo a possibilidade de uma única falha física derrubar vários componentes do sistema.

Já para um processamento científico que exige que várias EC2 troquem enormes quantidades de dados com **baixa latência**, um *Cluster Placement Group* pode ser mais adequado.

### Resumo para estudos

- **Placement Group** → controla como as EC2 são posicionadas na infraestrutura da AWS.
- **Cluster** → **performance**.
    - Mesma AZ.
    - Baixa latência.
    - Alta taxa de transferência.
    - Maior risco de falha conjunta.
- **Spread** → **alta disponibilidade/isolamento**.
    - Hardware separado.
    - Pode utilizar várias AZs.
    - Até **7 instâncias por AZ por grupo**.
- **Partition** → **escala + isolamento**.
    - Separação por partições/racks.
    - Até **7 partições por AZ**.
    - Pode suportar **centenas de EC2**.
    - Ideal para *Kafka*, *Cassandra*, *HDFS* e *HBase*.

> **Regra mental:**
> 
> 
> **Cluster = rápido** ⚡
> 
> **Spread = separado** 🛡️
> 
> **Partition = distribuído em grande escala** 📦
> 

---

## Aula 51 — Criando e associando *Placement Groups*

### Criando um *Placement Group*

No console da AWS, os *Placement Groups* podem ser encontrados em:

**EC2 → Network & Security → Placement Groups**

A partir daí, podemos criar grupos utilizando as três estratégias estudadas anteriormente.

---

### Associando uma EC2 ao *Placement Group*

Depois de criar o grupo, ao iniciar uma instância EC2:

**Launch Instance → Advanced Details → Placement Group**

Nesse campo, é possível selecionar o *Placement Group* desejado:

- `my-high-performance-group` → *Cluster*
- `my-critical-group` → *Spread*
- `my-distributed-group` → *Partition*

A instância será então criada utilizando a estratégia de posicionamento definida pelo grupo.

### 🖼️ Prints

- Prints que realizei na AWS
    
    ![image.png](Sess%C3%A3o%206%20ECS%20-%20Solutions%20Architect%20Associate%20Level/image%205.png)
    
    ![image.png](Sess%C3%A3o%206%20ECS%20-%20Solutions%20Architect%20Associate%20Level/image%206.png)
    
    ![image.png](Sess%C3%A3o%206%20ECS%20-%20Solutions%20Architect%20Associate%20Level/image%207.png)
    
    ![image.png](Sess%C3%A3o%206%20ECS%20-%20Solutions%20Architect%20Associate%20Level/image%208.png)
    

---

## Aula 52 — *Elastic Network Interfaces (ENI)*

<aside>
💡

- **Failover** → mecanismo de **troca automática ou manual para um recurso de backup** quando o principal falha.
- **VPC** → **rede virtual privada** dentro da AWS, onde ficam recursos como EC2, subnets e ENIs.
</aside>

### O que é uma *ENI*?

**ENI (*Elastic Network Interface*)** é uma **placa de rede virtual** dentro de uma *VPC*.

Ela é responsável por fornecer conectividade de rede para uma instância *EC2*.

Podemos pensar na ENI como a **placa de rede física de um computador**, mas virtualizada dentro da AWS.

```
EC2
 │
 └── ENI (placa de rede virtual)
       ├── IP privado
       ├── IPs privados secundários
       ├── IP público / Elastic IP
       ├── Security Groups
       └── MAC Address
```

A interface principal normalmente aparece como **`eth0`**.

Se adicionarmos outra ENI à instância, ela poderá aparecer como **`eth1`**.

🔬 Estudar mais sobre:
[https://aws.amazon.com/blogs/aws/new-elastic-network-interfaces-in-the-virtual-private-cloud/](https://aws.amazon.com/blogs/aws/new-elastic-network-interfaces-in-the-virtual-private-cloud/)

---

### Principais atributos de uma *ENI*

Uma ENI pode possuir:

- **IPv4 privado primário**
- Um ou mais **IPv4 privados secundários**
- **Elastic IP** associado a um IPv4 privado
- Um ou mais **IPv4 públicos**, conforme a configuração
- Um ou mais *Security Groups*
- **MAC Address**

Por exemplo:

```
ENI
├── Private IPv4: 10.0.1.10
├── Secondary IPv4: 10.0.1.20
├── Public IPv4: 54.x.x.x
├── Security Groups
└── MAC Address
```

---

### Uma EC2 pode ter múltiplas ENIs

Uma instância *EC2* pode possuir mais de uma interface de rede.

```
EC2
├── eth0 → ENI principal → 10.0.1.10
└── eth1 → ENI secundária → 10.0.1.20
```

Isso pode ser útil quando queremos separar diferentes tipos de tráfego ou quando precisamos mover uma interface entre instâncias.

---

### ENI é independente da EC2

Um ponto importante é que a **ENI pode existir independentemente de uma instância EC2**.

Podemos:

1. Criar uma ENI.
2. Associá-la a uma EC2.
3. Desassociá-la.
4. Associá-la a outra EC2.

Isso permite manter os atributos da interface, como o **IP privado**, durante a movimentação.

---

### ENI pertence a uma *Availability Zone*

Uma ENI está vinculada a uma **Availability Zone específica**.

Por exemplo:

```
us-east-1a
   │
   ├── EC2
   └── ENI
```

Essa ENI não pode simplesmente ser movida para uma instância localizada em outra AZ.

> **Regra importante:**
> 
> 
> **ENI é específica de uma AZ.**
> 

---

### ENI e *Failover*

Uma das utilizações mais interessantes das ENIs é implementar **failover**.

Imagine duas instâncias:

```
EC2-A
  │
  └── ENI
       └── 10.0.1.50

EC2-B
```

A aplicação utiliza o IP privado `10.0.1.50`.

Se a **EC2-A** apresentar problema, podemos desanexar a ENI e anexá-la à **EC2-B**:

```
Antes:

EC2-A ── ENI ── 10.0.1.50

Depois:

EC2-B ── ENI ── 10.0.1.50
```

O endereço IP privado continua associado à ENI, então a aplicação pode continuar utilizando o mesmo endereço.

### Analogia simples

Imagine que a ENI é uma **placa de identificação com um número de telefone**.

Você tem dois funcionários:

```
Funcionário A → telefone 5555
Funcionário B
```

Se o funcionário A ficar indisponível, você pode entregar o mesmo telefone `5555` para o funcionário B.

O número não mudou; apenas mudou **quem está utilizando a interface**.

Essa é a ideia do failover utilizando ENI.

---

### Caso de uso real

Imagine uma aplicação que precisa manter um **IP privado fixo** dentro da *VPC*.

Você possui:

- `EC2-A` → servidor principal
- `EC2-B` → servidor de backup
- ENI → `10.0.1.50`

A aplicação utiliza `10.0.1.50` para acessar o servidor.

Se `EC2-A` apresentar uma falha, a ENI pode ser associada à `EC2-B`, mantendo o mesmo IP privado.

Isso evita que outros componentes precisem ser configurados novamente para apontar para um novo endereço.

---

### 🧠 Resumo para estudos

**ENI = placa de rede virtual da AWS.**

- Pertence a uma **VPC**.
- Fornece conectividade de rede para a *EC2*.
- Pode ter **IPv4 privado primário**.
- Pode ter **IPv4 privados secundários**.
- Pode estar associada a **IP público / Elastic IP**.
- Pode possuir *Security Groups*.
- Possui **MAC Address**.
- Pode ser criada independentemente da EC2.
- Pode ser anexada/desanexada de instâncias.
- É vinculada a uma **Availability Zone**.
- Pode ser utilizada para implementar **failover**.

> **Para memorizar:**
> 
> 
> **ENI = placa de rede virtual + IPs + Security Groups + possibilidade de movimentação entre EC2s.**
> 

---

## Aula 53 — *Elastic Network Interfaces (ENI) - Hands On*

- Criado um novo EC2 com duas instâncias
    
    ![image.png](Sess%C3%A3o%206%20ECS%20-%20Solutions%20Architect%20Associate%20Level/image%209.png)
    
    ![image.png](Sess%C3%A3o%206%20ECS%20-%20Solutions%20Architect%20Associate%20Level/image%2010.png)
    
- Criando minha própria Network Interface (ENI)
    
    ![image.png](Sess%C3%A3o%206%20ECS%20-%20Solutions%20Architect%20Associate%20Level/image%2011.png)
    
- Note que aparece apenas uma Network Interface (ENI) anexada a ela
    
    ![image.png](Sess%C3%A3o%206%20ECS%20-%20Solutions%20Architect%20Associate%20Level/image%2012.png)
    
- Anexando a ENI criada em um das nossas instâncias.
    
    ![image.png](Sess%C3%A3o%206%20ECS%20-%20Solutions%20Architect%20Associate%20Level/image%2013.png)
    
    ![image.png](Sess%C3%A3o%206%20ECS%20-%20Solutions%20Architect%20Associate%20Level/image%2014.png)
    
- Veja que agora temos duas Network Interfaces (ENI) anexadas a minha instância
    
    ![image.png](Sess%C3%A3o%206%20ECS%20-%20Solutions%20Architect%20Associate%20Level/image%2015.png)
    

---

## Aula 55 — EC2 *Hibernate*

### O que é *Hibernate*?

O **EC2 Hibernate** permite **pausar uma instância mantendo o estado da RAM**.

Diferente do *Stop*, que reinicia o sistema operacional quando a EC2 volta, o *Hibernate* salva o conteúdo da RAM no **volume EBS** e, ao iniciar novamente, restaura esse estado.

### Diferença entre *Stop* e *Hibernate*

- **Stop** → RAM é perdida → sistema operacional inicia novamente.
- **Hibernate** → RAM é salva no EBS → estado da máquina é restaurado.

```
Hibernate:

RAM
 ↓
salva no EBS
 ↓
EC2 parada
 ↓
EC2 inicia
 ↓
RAM restaurada
 ↓
continua de onde parou
```

### Analogia simples

Imagine que você está trabalhando em um documento com vários programas abertos.

- **Stop:** você fecha tudo e depois precisa abrir novamente.
- **Hibernate:** você tira uma **foto do estado atual** e, quando volta, tudo é restaurado como estava.

### Por que usar?

O principal benefício é **inicialização rápida**, especialmente quando a aplicação demora para inicializar.

Casos de uso:

- Processos de longa duração.
- Aplicações que possuem um estado importante na memória.
- Serviços que demoram muito para inicializar.
- Cenários em que queremos pausar temporariamente uma EC2 sem perder o estado da aplicação.

### Requisitos importantes

Para o *Hibernate* funcionar:

- A **RAM precisa ser salva no EBS**.
- O **volume raiz EBS deve ser criptografado**.
- O EBS precisa ter espaço suficiente para armazenar o conteúdo da RAM.
- Não funciona em instâncias **bare metal**.
- Suporta Linux e Windows.
- Existem limites de tamanho de memória e duração da hibernação que podem variar conforme a AWS.

### 🧠 Resumo para prova

> **EC2 Hibernate = Stop + preservar RAM.**
> 

**Stop:**

`RAM ❌ → EBS mantém dados do disco`

**Hibernate:**

`RAM → EBS → EC2 parada → RAM restaurada`

**Palavra-chave:** **preservar o estado da memória para voltar mais rapidamente.**

---

## Aula 56 — EC2 *Hibernate* — Hands On

### Configuração da instância

Foi criada uma instância *EC2* com:

- **AMI:** Amazon Linux 2
- **Tipo:** `t2.micro`
- **RAM:** 1 GB
- **Volume raiz EBS:** 8 GB
- **EBS:** criptografado
- **Hibernate:** habilitado

O volume EBS precisa ser **criptografado** e possuir espaço suficiente para armazenar o conteúdo da RAM.

### Testando o *Hibernate*

Após iniciar a instância, foi utilizado o **EC2 Instance Connect** para acessar o servidor.

O comando:

```bash
uptime
```

mostra **há quanto tempo o sistema operacional está em execução desde a última inicialização**.

O teste foi realizado da seguinte maneira:

1. Instância iniciada.
2. Acesso através do *EC2 Instance Connect*.
3. Execução do `uptime`.
4. Instância colocada em **Hibernate**.
5. Instância iniciada novamente.
6. Novo `uptime`.

O resultado mostrou que o tempo de atividade **não voltou para zero**.

Isso demonstra que, durante o *Hibernate*, o estado da memória foi preservado e restaurado quando a instância voltou.

### 🧠 O que foi comprovado?

```
EC2 em execução
      ↓
   Hibernate
      ↓
RAM → EBS
      ↓
EC2 parada
      ↓
Iniciar EC2
      ↓
EBS → RAM
      ↓
Estado restaurado
```

### Caso de uso

Uma aplicação demora vários minutos para inicializar porque precisa carregar dados e aquecer caches.

Com *Hibernate*, podemos pausar a EC2 preservando seu estado na RAM. Ao retornar, a aplicação pode continuar de onde estava, evitando todo o processo de inicialização novamente.

### Resumo para estudos

- Habilitar **Hibernate** durante a criação da EC2.
- O **EBS raiz deve ser criptografado**.
- O EBS precisa ter espaço suficiente para a **RAM**.
- `uptime` pode ser utilizado para verificar se o sistema operacional foi reiniciado.
- **Stop:** perde o conteúdo da RAM.
- **Hibernate:** preserva o conteúdo da RAM no EBS.
- No *Hands On*, a instância foi hibernada e iniciada novamente, mantendo o tempo de atividade do sistema operacional.

### 🖨️ Prints da aula prática

- Prints
    
    ![image.png](Sess%C3%A3o%206%20ECS%20-%20Solutions%20Architect%20Associate%20Level/image%2016.png)
    
    ![image.png](Sess%C3%A3o%206%20ECS%20-%20Solutions%20Architect%20Associate%20Level/image%2017.png)
    
    ![image.png](Sess%C3%A3o%206%20ECS%20-%20Solutions%20Architect%20Associate%20Level/image%2018.png)
    
- Após conectado, a gente executa o comando `uptime`  
Esse comando diz quanto tempo a nossa instância está ligada, desde a última reinicialização
    
    ![image.png](Sess%C3%A3o%206%20ECS%20-%20Solutions%20Architect%20Associate%20Level/image%2019.png)
    
- Agora podemos colocar a nossa **instância para Hibernar**
    
    ![image.png](Sess%C3%A3o%206%20ECS%20-%20Solutions%20Architect%20Associate%20Level/image%2020.png)
    
    ![image.png](Sess%C3%A3o%206%20ECS%20-%20Solutions%20Architect%20Associate%20Level/image%2021.png)
    
- Depois a gente sobe/conecta novamente a nossa instância e executa o comando uptime 
E verifica pelo tempo que a nossa i**nstância hibernou com sucesso**
    
    ![image.png](Sess%C3%A3o%206%20ECS%20-%20Solutions%20Architect%20Associate%20Level/image%2022.png)