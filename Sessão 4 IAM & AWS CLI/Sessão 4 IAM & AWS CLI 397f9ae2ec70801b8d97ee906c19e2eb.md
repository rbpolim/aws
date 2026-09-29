# Sessão 4: IAM & AWS CLI

## Aula 15 — IAM Policies (Hands On)

### Objetivo da aula

Entender **como permissões funcionam no IAM (Identity and Access Management)** e como elas podem ser atribuídas para usuários usando:

- **Grupos**
- **Políticas anexadas diretamente ao usuário**
- **Políticas customizadas (JSON ou editor visual)**

---

### 1. Permissões podem vir de grupos ou diretamente do usuário

No exemplo da aula:

Usuário: **Stephane**

Inicialmente ele fazia parte do grupo **Admin**.

Como consequência:

- Herdava a política **AdministratorAccess**
- Tinha acesso total à conta AWS

Fluxo:

```
Stephane
   ↓
Grupo: Admin
   ↓
Policy: AdministratorAccess
   ↓
Permissão: tudo na AWS
```

---

### 2. Remover usuário do grupo remove permissões

O instrutor removeu o usuário **Stephane** do grupo **Admin**.

Resultado:

- Ao atualizar o console IAM
- Recebeu **Access Denied**
- Erro: `iam:ListUsers`

Isso mostrou um conceito importante:

> Permissões não pertencem ao usuário automaticamente.
> 
> 
> Elas são concedidas por políticas.
> 

---

### 3. Políticas podem ser anexadas diretamente ao usuário

Para recuperar acesso, foi adicionada ao usuário a política:

**IAMReadOnlyAccess**

Essa política permite:

- Visualizar usuários
- Visualizar grupos
- Fazer consultas no IAM

Mas não permite:

- Criar grupos
- Alterar configurações

Exemplo:

✅ Ver usuários

✅ Ler configurações

❌ Criar grupo

❌ Alterar permissões

Conceito:

> **ReadOnly ≠ Full Access**
> 

---

### 4. Princípio do menor privilégio (Least Privilege)

A aula reforça uma prática muito importante na AWS:

> Conceda apenas as permissões necessárias.
> 

Exemplo:

- Usuário que só consulta → ReadOnly
- Usuário que administra → Full Access

Evite dar `AdministratorAccess` sem necessidade.

---

### 5. Usuário pode acumular permissões

Depois o instrutor deixou o usuário com:

1. **AdministratorAccess** → herdado do grupo Admin
2. **AlexaForBusiness** → herdado do grupo Developers
3. **IAMReadOnlyAccess** → anexado diretamente

Resultado:

As permissões finais do usuário são a **união de todas as políticas aplicadas**.

Visualmente:

```
Permissões finais =
Grupo Admin
+
Grupo Developers
+
Políticas diretas
```

---

### 6. Estrutura de uma IAM Policy (JSON)

#### AdministratorAccess

```
{
  "Effect":"Allow",
  "Action":"*",
  "Resource":"*"
}
```

Significado:

- `Allow` → permitir
- `Action: "*"` → qualquer ação
- `Resource: "*"` → qualquer recurso

Resultado:

→ **Acesso total**

---

#### IAMReadOnlyAccess

Exemplo simplificado:

```
{
  "Effect":"Allow",
  "Action": [
"iam:Get*",
"iam:List*"
  ],
  "Resource":"*"
}
```

Significado:

- `Get*` → qualquer operação que começa com Get
- `List*` → qualquer operação que começa com List

Exemplos permitidos:

```
iam:GetUser
iam:GetGroup
iam:ListUsers
iam:ListGroups
```

---

### 7. Criando políticas personalizadas

A AWS oferece duas formas:

#### Editor Visual

Seleciona:

- Serviço (IAM)
- Ações permitidas
- Recursos permitidos

Exemplo:

- `iam:ListUsers`
- `iam:GetUser`

---

#### Editor JSON

Escreve diretamente:

```
{
  "Effect":"Allow",
  "Action": [
"iam:ListUsers",
"iam:GetUser"
  ],
  "Resource":"*"
}
```

---

#### Resumo final (para decorar)

**IAM = quem pode fazer o quê na AWS**

- Usuários recebem permissões por **grupos** ou **políticas diretas**
- Remover do grupo → remove acesso
- Políticas definem:
    - `Effect` → Allow / Deny
    - `Action` → o que pode fazer
    - `Resource` → onde pode fazer
- = qualquer coisa
- Usuário acumula permissões de várias políticas
- Siga sempre o **Least Privilege**

**Fórmula mental:**

```
Usuário
→ Grupo(s)
→ Política(s)
→ Permissões finais
```

## Aula 16 — IAM MFA Overview

### Objetivo da aula

Aprender como proteger usuários IAM usando duas camadas de segurança:

1. **Password Policy** → fortalece senhas
2. **MFA (Multi-Factor Authentication)** → adiciona uma segunda verificação

---

### 1. Password Policy (Política de Senha)

A AWS permite definir regras para aumentar a segurança das senhas dos usuários IAM.

Configurações possíveis:

- Comprimento mínimo
- Exigir:
    - letra maiúscula
    - letra minúscula
    - número
    - caractere especial
- Permitir troca de senha pelo usuário
- Expirar senha (ex.: 90 dias)
- Impedir reutilização

Objetivo:

→ reduzir risco de **senha fraca** e **ataques de força bruta**

**Analogia:**

Uma senha forte é como colocar uma fechadura mais resistente na porta.

---

### 2. MFA (Multi-Factor Authentication)

MFA exige **duas formas de autenticação** para entrar.

Modelo:

```
Algo que você SABE
+
Algo que você POSSUI
```

Exemplo:

```
Senha
+
Código no celular
```

Benefício:

Mesmo que alguém descubra a senha, ainda precisará do dispositivo físico.

**Analogia:**

É como entrar em um condomínio:

- Saber o número do apartamento (senha)
- Ter o controle do portão (MFA)

Os dois são necessários.

---

### 3. Tipos de MFA na AWS

#### Virtual MFA (mais comum)

Aplicativos no celular:

- Google Authenticator
- Authy

Permitem gerar códigos temporários.

---

#### U2F Security Key

Exemplo:

- Chave física como YubiKey

Funciona conectando ou aproximando o dispositivo.

---

#### Hardware MFA

Dispositivos físicos dedicados para gerar códigos.

---

### Resumo final (para decorar)

```
Password Policy
→ Protege a SENHA

MFA
→ Protege o LOGIN

Segurança ideal:
Senha forte + MFA
```

Regra para prova:

```
Conta Root → MFA obrigatório
Usuários IAM → MFA recomendado
```

## Aula 17 — IAM MFA Hands On

### Objetivo da aula

Configurar na prática:

1. **Password Policy**
2. **MFA (Multi-Factor Authentication)** na conta Root

---

### 1. Configurando Password Policy

No IAM:

```
IAM → Account settings → Password policy
```

É possível:

- Definir tamanho mínimo da senha
- Exigir:
    - maiúscula
    - minúscula
    - número
    - caractere especial
- Expirar senha (ex.: 90 dias)
- Permitir usuário alterar senha
- Impedir reutilização

Objetivo:

→ aumentar a segurança sem depender apenas do comportamento do usuário.

---

### 2. Configurando MFA na conta Root

Caminho:

```
Conta → Security credentials → Multi-factor authentication (MFA)
```

A conta **Root** é a conta mais sensível da AWS.

Por isso, o ideal é ativar MFA nela primeiro.

Durante a configuração:

1. Dar um nome ao dispositivo
2. Escolher tipo do MFA
3. Vincular o dispositivo
4. Confirmar com códigos gerados

---

### 3. Tipos de MFA disponíveis

Opções apresentadas:

- Aplicativo autenticador (virtual)
- Chave de segurança física
- Token físico TOTP

Na aula foi usado:

- Authy

---

### 4. Como ativar usando aplicativo

Fluxo:

```
Abrir app
→ Escanear QR Code
→ Gerar código
→ Informar dois códigos consecutivos
→ Confirmar
```

A AWS pede **2 códigos** para validar que o dispositivo foi configurado corretamente.

---

### 5. Como funciona o login com MFA

Depois de ativado:

```
Login
→ Email + Senha
→ Código MFA
→ Acesso liberado
```

**Analogia:**

Senha é a chave da casa.

MFA é o porteiro conferindo se realmente é você.

Mesmo que alguém descubra sua senha, ainda precisará do dispositivo.

---

### Resumo final (para decorar)

```
Password Policy
→ Define regras para senhas

MFA
→ Adiciona segunda validação

Fluxo:
Senha + Código temporário
```

Regra prática:

```
Conta Root → Ative MFA primeiro
Guarde o dispositivo MFA com cuidado
```

## Aula 18 — AWS Access: Console, CLI e SDK

### Objetivo da aula

Entender as **3 formas principais de acessar a AWS** e como funciona a autenticação em cada uma.

---

### 1. Management Console (Console Web)

É a interface gráfica da AWS (navegador).

Proteção:

```
Usuário
+ Senha
+ MFA (opcional/recomendado)
```

Exemplo:

- Entrar no site da AWS
- Criar serviços
- Configurar recursos

É o método usado até agora no curso.

---

### 2. AWS CLI (Command Line Interface)

Permite acessar a AWS usando **terminal e comandos**.

Exemplo:

```
aws s3 ...
aws iam ...
```

Proteção:

```
Access Key ID
+
Secret Access Key
```

Essas chaves funcionam como credenciais para acessar APIs da AWS.

Objetivos do CLI:

- Executar comandos rapidamente
- Automatizar tarefas
- Criar scripts

**Analogia:**

Console é dirigir um carro usando volante e painel.

CLI é dirigir usando comandos rápidos e atalhos.

---

### 3. AWS SDK (Software Development Kit)

Usado quando sua **aplicação precisa conversar com a AWS via código**.

Em vez de executar comandos no terminal:

```
Aplicação → SDK → AWS API
```

Também usa:

```
Access Key ID
+
Secret Access Key
```

Exemplos de linguagens suportadas:

- JavaScript
- Python
- Java
- C#
- Go
- Node.js
- PHP

Exemplo citado:

O **AWS CLI foi construído usando o SDK Python (Boto)**.

---

### 4. Access Keys (Chaves de acesso)

Cada usuário IAM pode gerar suas próprias chaves.

Composição:

```
Access Key ID
+
Secret Access Key
```

Importante:

- Access Key ID → equivalente ao usuário
- Secret Access Key → equivalente à senha

Regra de segurança:

❌ Não compartilhar

❌ Não colocar no código

❌ Não enviar para colegas

**Analogia:**

Se o usuário é o cartão do banco, a Secret Key é a senha do cartão.

---

### Resumo final (para decorar)

```
Management Console
→ Interface Web

CLI
→ Terminal + automação

SDK
→ Aplicações acessando AWS
```

Autenticação:

```
Console → Usuário + Senha + MFA

CLI / SDK
→ Access Key + Secret Key
```

## Aula 22 — AWS CLI Hands On

### Objetivo da aula

Criar uma **Access Key**, configurar a **AWS CLI** e entender que ela respeita as mesmas permissões do usuário IAM.

---

### 1. Criando uma Access Key

Caminho:

```
IAM User → Security credentials → Create access key
```

Durante a criação, a AWS pergunta o uso da chave (CLI, aplicação, etc.) e exibe recomendações de segurança.

**Importante:**

A **Secret Access Key** é exibida **apenas uma vez**. Guarde-a em um local seguro.

---

### 2. Configurando a AWS CLI

Comando:

```
aws configure
```

Serão solicitados:

- Access Key ID
- Secret Access Key
- Região padrão (ex.: `eu-west-1`)
- Formato de saída (opcional)

Após isso, a CLI estará pronta para uso.

---

### 3. Testando a CLI

Exemplo:

```
aws iam list-users
```

O comando retorna os usuários IAM da conta, assim como o Console da AWS.

---

### 4. CLI usa as permissões do IAM

O instrutor removeu o usuário **Stephane** do grupo **Admins**.

Resultado:

- No **Console** → `Access Denied`
- Na **CLI** → o comando também foi negado

Ou seja, a CLI **não possui permissões próprias**.

**Analogia:**

A CLI é apenas outra porta de entrada para a AWS. Quem define o que pode ser feito é o usuário IAM.

---

### Resumo final (para decorar)

```
Criar Access Key
→ Configurar com aws configure
→ Executar comandos AWS
```

Lembre-se:

```
Console e CLI usam
as mesmas permissões do usuário IAM.
```

Se o usuário perder uma permissão no IAM, ela será perdida **tanto no Console quanto na CLI**.

- Alguns prints abaixo
    
    ![image.png](Sess%C3%A3o%204%20IAM%20&%20AWS%20CLI/image.png)
    
    ![image.png](Sess%C3%A3o%204%20IAM%20&%20AWS%20CLI/image%201.png)
    
    ![image.png](Sess%C3%A3o%204%20IAM%20&%20AWS%20CLI/image%202.png)
    
    ![image.png](Sess%C3%A3o%204%20IAM%20&%20AWS%20CLI/image%203.png)
    
    ![image.png](Sess%C3%A3o%204%20IAM%20&%20AWS%20CLI/image%204.png)
    
    ![image.png](Sess%C3%A3o%204%20IAM%20&%20AWS%20CLI/image%205.png)
    

## Aula 24 — AWS CloudShell

### Objetivo da aula

Conhecer o **AWS CloudShell**, um terminal integrado ao Console da AWS.

---

### 1. O que é o CloudShell?

É um **terminal na nuvem**, acessível diretamente pelo Console da AWS.

Vantagens:

- AWS CLI já instalada
- Não precisa configurar credenciais
- Gratuito

---

### 2. Como funciona?

Os comandos usam automaticamente:

- O usuário IAM logado
- A região atualmente selecionada no Console

Exemplo:

```
aws iam list-users
```

---

### 3. Recursos úteis

- Arquivos permanecem salvos entre sessões
- Upload e download de arquivos
- Múltiplas abas/terminais
- Personalização (tema e fonte)

---

### 4. Limitação

O CloudShell **não está disponível em todas as regiões da AWS**.

Se não estiver disponível, utilize a AWS CLI instalada no computador.

---

### Resumo final (para decorar)

```
CloudShell
→ Terminal dentro da AWS

Já possui:
✓ AWS CLI
✓ Credenciais
✓ Ambiente configurado
```

**Analogia:**

É como abrir um terminal remoto já preparado para usar a AWS, sem precisar instalar ou configurar nada.

![image.png](Sess%C3%A3o%204%20IAM%20&%20AWS%20CLI/image%206.png)

---

## Aula 25 — IAM Roles (Funções IAM)

### Objetivo da aula

Entender o que são **IAM Roles** e por que os serviços da AWS precisam delas.

---

### 1. O que é uma IAM Role?

Uma **IAM Role** é um conjunto de permissões usado por **serviços da AWS**, e não por usuários.

Enquanto um usuário IAM é utilizado por pessoas, uma Role é utilizada por serviços como EC2 e Lambda.

---

### 2. Por que ela existe?

Alguns serviços precisam acessar outros recursos da AWS.

Exemplo:

```
EC2 → Ler arquivo no S3
```

Para isso, a EC2 precisa de permissões, que são concedidas por uma **IAM Role**.

**Analogia:**

Uma IAM Role é como um **crachá de funcionário**. O serviço (EC2, Lambda, etc.) usa esse crachá para provar o que está autorizado a fazer.

---

### 3. Exemplos de uso

Serviços que normalmente utilizam IAM Roles:

- EC2
- Lambda
- CloudFormation

---

### Resumo final (para decorar)

```
Usuário IAM
→ Usado por pessoas

IAM Role
→ Usada por serviços AWS
```

```
Serviço AWS
→ Assume uma IAM Role
→ Recebe permissões
→ Acessa outros recursos da AWS
```

---

## Aula 26 — IAM Roles Hands On

### Objetivo da aula

Criar uma **IAM Role** para um serviço da AWS e anexar permissões a ela.

---

### 1. Criando uma IAM Role

No IAM:

```
IAM → Roles → Create role
```

Escolha:

- **Trusted entity:** AWS Service
- **Serviço:** EC2

---

### 2. Anexando permissões

Assim como um usuário, uma Role também precisa de políticas.

Na aula foi utilizada:

- **IAMReadOnlyAccess**

Isso permite que a instância EC2 faça consultas ao IAM.

---

### 3. Entidade confiável (Trusted Entity)

Ao criar a Role, é definido **quem pode utilizá-la**.

Neste exemplo:

```
Trusted Entity
→ Amazon EC2
```

Ou seja, somente uma instância EC2 poderá assumir essa Role.

**Analogia:**

É como emitir um crachá exclusivo para um cargo. Apenas quem ocupa aquele cargo (EC2) pode usá-lo.

---

### Resumo final (para decorar)

```
Criar Role
→ Escolher o serviço (EC2)
→ Anexar políticas
→ Definir quem pode assumir a Role
```

```
IAM Role
→ Permissões

Trusted Entity
→ Quem pode usar a Role
```

> **Importante:** A Role foi criada nesta aula, mas só será utilizada quando as instâncias **EC2** forem apresentadas nas próximas aulas.
> 

![image.png](Sess%C3%A3o%204%20IAM%20&%20AWS%20CLI/image%207.png)

![image.png](Sess%C3%A3o%204%20IAM%20&%20AWS%20CLI/image%208.png)

![image.png](Sess%C3%A3o%204%20IAM%20&%20AWS%20CLI/image%209.png)

![image.png](Sess%C3%A3o%204%20IAM%20&%20AWS%20CLI/image%2010.png)

---

## Aula 27 e 28 — IAM Security Tools

### Objetivo da aula

Conhecer duas ferramentas do IAM para **auditoria e segurança**:

- **Credential Report**
- **IAM Access Advisor**

---

### 1. Credential Report

Gera um relatório (.CSV) com informações de **todos os usuários da conta AWS**.

Exemplos de informações:

- Data de criação
- Última troca de senha
- MFA ativado
- Access Keys criadas
- Último uso das Access Keys

Objetivo:

→ identificar usuários com possíveis problemas de segurança.

---

### 2. ~~IAM Access Advisor~~ 
O serviço foi renomeado para “**Last Accessed**”

Mostra **quais serviços um usuário realmente utilizou** e quando foram acessados pela última vez.

Exemplo:

```
Usuário
→ EC2 ✅
→ IAM ✅
→ S3 ❌
→ Lambda ❌
```

Isso ajuda a descobrir permissões que nunca são utilizadas.

**Analogia:**

É como o histórico de acesso de um crachá em uma empresa. Você consegue ver quais portas a pessoa realmente passou.

---

### 3. Princípio do Menor Privilégio

Com o Access Advisor é possível remover permissões desnecessárias.

Fluxo:

```
Permissão concedida
→ Verificar uso
→ Remover o que não é utilizado
```

Isso reduz riscos e segue o princípio do **Least Privilege**.

---

### Resumo final (para decorar)

```
Credential Report
→ Visão geral da conta

Access Advisor
→ Visão de um usuário
```

```
Credential Report
→ "Como estão as credenciais?"

Access Advisor
→ "Quais permissões realmente são usadas?"
```

- Credentials reports
    
    ![image.png](Sess%C3%A3o%204%20IAM%20&%20AWS%20CLI/image%2011.png)
    
- “**Last Accessed**”
    
    ![image.png](Sess%C3%A3o%204%20IAM%20&%20AWS%20CLI/image%2012.png)
    

---

## Aula 29 — IAM Best Practices

### Objetivo da aula

Conhecer as principais **boas práticas de segurança** ao utilizar o IAM.

---

### 1. Utilize a conta Root apenas quando necessário

A conta **Root** deve ser usada apenas para configurações iniciais ou tarefas administrativas específicas.

No dia a dia, utilize um **usuário IAM**.

---

### 2. Um usuário IAM = uma pessoa

Cada pessoa deve possuir seu próprio usuário IAM.

❌ Nunca compartilhe credenciais.

---

### 3. Gerencie permissões com grupos

Em vez de conceder permissões para cada usuário individualmente:

```
Usuários
→ Grupos
→ Políticas
```

Isso facilita a administração.

---

### 4. Fortaleça a segurança

- Utilize senhas fortes
- Ative MFA sempre que possível

---

### 5. Use IAM Roles para serviços

Quando um serviço da AWS precisar acessar outro serviço, utilize uma **IAM Role**, e não Access Keys.

---

### 6. Proteja as Access Keys

As Access Keys são utilizadas pela **CLI** e pelos **SDKs**.

Trate-as como uma senha:

- Não compartilhe
- Mantenha em segredo

---

### 7. Revise permissões regularmente

Utilize:

- **Credential Report** → auditoria das credenciais
- **Access Advisor** → identificar permissões não utilizadas

---

### Resumo final (para decorar)

```
✓ Use Root apenas quando necessário
✓ Um usuário por pessoa
✓ Permissões via grupos
✓ Senha forte + MFA
✓ Serviços usam IAM Roles
✓ Nunca compartilhe Access Keys
✓ Revise permissões regularmente
```

---

## Aula 30 — Resumo do IAM

### Objetivo da aula

Revisar os principais conceitos aprendidos sobre **IAM (Identity and Access Management)**.

---

### Principais conceitos

- **Usuários IAM** → representam pessoas.
- **Grupos** → organizam usuários com as mesmas permissões.
- **Policies** → documentos JSON que definem permissões.
- **Roles** → concedem permissões para serviços da AWS (EC2, Lambda etc.).
- **MFA** e **Password Policy** → aumentam a segurança das contas.
- **CLI** e **SDK** → permitem acessar a AWS via terminal ou código.
- **Access Keys** → credenciais usadas pela CLI e SDK.
- **Credential Report** e **Access Advisor** → ferramentas para auditoria e revisão de permissões.

---

### Resumo final (para decorar)

```
IAM

Usuário → Pessoa
Grupo → Organiza usuários
Policy → Define permissões
Role → Permissões para serviços AWS

Segurança
• Senha forte
• MFA

Acesso
• Console
• CLI
• SDK

Credenciais
• Access Keys

Auditoria
• Credential Report
• Access Advisor
```

> **Mapa mental:** Pense no IAM como o sistema de controle de acesso da AWS: **quem é o usuário, o que ele pode fazer e como ele acessa os recursos**.
>