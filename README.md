# Revisão

---

## 1. Redes de Computadores e Sistemas Distribuídos

> **Analogia Central:** A rede funciona como o serviço de correios: o IP é o endereço do prédio (cidade/rua), a Porta TCP/UDP é o número do apartamento de quem vai receber, e o MAC é o CPF físico de quem mora lá.

### Modelos de Camadas (OSI vs. TCP/IP)

<pre>
Modelo OSI (7 Camadas)             Modelo TCP/IP (4/5 Camadas)     Unidade de Dados (PDU)
[ 7. Aplicação    ] ──┐
[ 6. Apresentação ] ──┼──────────> [ 4. Aplicação           ] ──> Dados
[ 5. Sessão       ] ──┘
[ 4. Transporte   ] ─────────────> [ 3. Transporte          ] ──> Segmento (TCP) / Datagrama (UDP)
[ 3. Rede         ] ─────────────> [ 2. Rede / Internet     ] ──> Pacote
[ 2. Enlace       ] ─────────────> [ 1. Enlace / Interface  ] ──> Quadro (Frame)
[ 1. Física       ] ─────────────> [    Física              ] ──> Bits
</pre>

* **Mnemônico Camadas OSI:** **A**lguém **A**visou **S**obre o **T**ransporte da **R**ede do **E**difício **F**rancês.
* **Equipamentos de Interconexão:**
  * **Hub (Camada 1):** Repetidor burro; gera 1 único domínio de colisão e broadcast.
  * **Switch (Camada 2/3):** Comuta por endereço **MAC**; quebra domínios de colisão, mantém 1 domínio de broadcast por VLAN.
  * **Roteador (Camada 3):** Encaminha por **IP**; delimita domínios de broadcast.

### Tabela de Portas e Protocolos Mais Cobrados

| Protocolo | Porta | Camada | Transporte | Função Principal |
|---|:---:|:---:|:---:|---|
| **SSH** / **Telnet** | 22 / 23 | Aplicação | TCP | Acesso remoto seguro (22) vs. texto claro inseguro (23). |
| **SMTP** | 25 / 587 | Aplicação | TCP | Envio (*push*) de e-mails entre servidores. |
| **DNS** | 53 | Aplicação | UDP (consultas) / TCP (zonas) | Resolução de nomes em IPs. |
| **DHCP** | 67 (servidor) / 68 (cliente) | Aplicação | UDP | Atribuição dinâmica de IP, máscara e gateway. |
| **HTTP** / **HTTPS** | 80 / 443 | Aplicação | TCP | Transferência hipertexto (443 usa TLS/criptografia). |
| **SNMP** | 161 / 162 (Trap) | Aplicação | UDP | Monitoramento e gerência de ativos de rede. |
| **LDAP** | 389 | Aplicação | TCP | Consulta a diretórios centralizados de autenticação. |
| **NFS** | 2049 | Aplicação | TCP/UDP | Compartilhamento de arquivos em rede Unix/Linux. |

### Segurança de Rede & IPv6
* **IPSec (Camada 3):** 
  * **AH (*Authentication Header*):** Garante integridade e autenticidade; **não** provê confidencialidade.
  * **ESP (*Encapsulating Security Payload*):** Garante confidencialidade (cifra), integridade e autenticação.
* **NAT:** Mapeia endereços IPv4 privados (RFC 1918: `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`) em IPs públicos.
* **IPv6:** Endereço de **128 bits** (hexadecimal). Elimina NAT, descarta broadcast (usa **Multicast** e **Anycast**) e permite autoconfiguração stateless via **SLAAC**.

---

## 2. Ambientes Linux e Windows 11

### Administração e Rede

| Recurso | Linux (Ubuntu/Debian) | Windows Server / 11 |
|---|---|---|
| **Configuração de Rede** | `ip addr`, `ip route`, Netplan (`/etc/netplan/*.yaml`) | `Get-NetIPAddress`, `ncpa.cpl`, `ipconfig` |
| **Servidor DNS / DHCP** | **BIND9** (`named.conf`) / **Kea** ou **ISC-DHCP** | Função **DNS Server** e **DHCP Server** no Server Manager |
| **Servidor Web** | **Apache** (`httpd.conf`/`sites-available`, `.htaccess`) | **IIS** (`web.config`, Application Pools) |
| **Acesso Remoto** | SSH (porta 22) | **RDP / Terminal Services** (porta 3389) |
| **Automação** | **Bash**: manipulação de streams de texto puro (`grep`, `awk`, `sed`) | **PowerShell**: pipeline de objetos estruturados (`cmdlets`) |

### Integração de Sistemas e Active Directory (AD)

<pre>
 [ Active Directory (Floresta / Domínio / OUs) ]
           │
┌──────────┴──────────┐
▼ (LDAP - Porta 389)  ▼ (Kerberos - Porta 88)
Consultas de Diretório   Autenticação por Tickets (TGT/TGS)
▲                     ▲
└──────────┬──────────┘
│ (SSSD / Winbind / Samba-CIFS)
[ Clientes Linux ]
</pre>

* **Samba (CIFS/SMB):** Permite interoperabilidade nativa de arquivos e impressoras entre Linux e Windows.
* **NFS:** Focado em rede de arquivos nativa Unix-to-Unix.
* **Kerberos (Porta 88):** Protocolo de autenticação baseado em tickets temporários (evita trafegar senhas na rede).

---

## 3. Gerência de Projetos (Tradicional vs. Ágil)

### Ciclo Tradicional (PMBOK / Cascata)

<pre>
 [ Requisitos ] ──> [ Projeto Lógico ] ──> [ Projeto Físico ] ──> [ Testes ] ──> [ Implantação ] ──> [ Fechamento ]
</pre>

* **Termo de Abertura (*Project Charter*):** Documento formal que autoriza o início do projeto e concede autoridade ao gerente.
* **EAP / WBS (*Work Breakdown Structure*):** Decomposição hierárquica orientada a entregas do trabalho do projeto. **Não contém cronograma nem datas**, apenas o escopo decomposto em pacotes de trabalho.
* **Método do Caminho Crítico (CPM):** Sequência de atividades com **folga total igual a zero** ($Folga = FimMaisTarde - FimMaisCedo = 0$). É o menor tempo possível para concluir o projeto. Se qualquer atividade do caminho crítico atrasar 1 dia, o projeto inteiro atrasa 1 dia.

### Comparativo: Preditivo vs. Adaptativo

| Dimensão | Tradicional (Preditivo / Cascata) | Ágil (Adaptativo / Scrum / Kanban) |
|---|---|---|
| **Escopo** | Fixo (controlado rigorosamente via linha de base) | Flexível (replanejado a cada Sprint) |
| **Custo e Tempo** | Estimados conforme o escopo fixo | Fixos por iteração (Sprint de 2 a 4 semanas) |
| **Feedback** | Tardio (na entrega final) | Contínuo (a cada incremento funcional) |
| **Papéis Scrum** | Gerente de Projetos centraliza decisões | **Product Owner** (Prioriza o Backlog), **Scrum Master** (Facilitador), **Dev Team** |

---

## 4. Segurança da Informação e Defesa Cibernética

> **Analogia de Defesa:** O Firewall é o porteiro do prédio (filtra crachás); o WAF inspeciona as encomendas suspeitas abrindo os pacotes (Camada 7); o SIEM é a central de monitoramento gravando as câmeras; e o EDR é o segurança dentro de cada apartamento.

### Arquitetura de Defesa e Ferramentas

<pre>
 Tráfego Externo ──> [ Firewall (L3/L4) ] ──> [ WAF (L7) ] ──> [ Servidor Web / API ]
│                      │
└───────► [ SIEM ] ◄───┘ (Correlação de Logs)
▲
[ Endpoints / Hosts ] ──(Agente EDR)─────┘
</pre>

* **Firewall Tradicional vs. WAF:** Firewall atua nas camadas 3/4 (IP/Porta); WAF atua na camada 7 protegendo aplicações web contra falhas da OWASP (ex: SQL Injection, XSS).
* **IDS vs. IPS:** IDS apenas detecta e gera alerta (*passivo*); IPS detecta e descarta pacotes maliciosos em tempo real (*ativo/in-line*).
* **Testes de Aplicação:**
  * **SAST (*Static Application Security Testing*):** White-box; analisa o código-fonte estático sem executá-lo.
  * **DAST (*Dynamic Application Security Testing*):** Black-box; testa a aplicação em tempo de execução pela interface externa.
  * **IAST (*Interactive*):** Híbrido; roda agentes dentro da aplicação em execução.

### Principais Ataques Web

* **SQL Injection (SQLi):** Injeção de trechos SQL em inputs não sanitizados (ex: `' OR '1'='1`). *Defesa:* Queries parametrizadas (*Prepared Statements*).
* **XSS (*Cross-Site Scripting*):** Injeção de scripts maliciosos (JavaScript) executados no navegador de outros clientes.
* **CSRF (*Cross-Site Request Forgery*):** Força o navegador de um usuário já autenticado a executar ações não intencionais. *Defesa:* Tokens anti-CSRF aleatórios por sessão.
* **Controle de Acesso:** **RBAC** (acesso baseado na função/cargo do usuário) vs. **ABAC** (acesso baseado em atributos dinâmicos: hora, localização, tipo de dispositivo).

---

## 5. Storage, Virtualização, Containers e DevOps

### Storage: SAN vs. NAS vs. DAS

| Característica | DAS (*Direct-Attached*) | NAS (*Network-Attached*) | SAN (*Storage Area Network*) |
|---|---|---|---|
| **Acesso a Dados** | Nível de Bloco | **Nível de Arquivo** (*File-level*) | **Nível de Bloco** (*Block-level*) |
| **Protocolos** | SATA, SAS, NVMe | **NFS, CIFS/SMB** | **Fibre Channel (FC), iSCSI** |
| **Rede de Transporte** | Cabo local direto | Rede Ethernet comum | Rede dedicada de altíssima velocidade |
| **Caso de Uso** | Disco local de PC/Servidor | Compartilhamento de arquivos em rede | Grandes Bancos de Dados, Clusters de Virtualização |

### Virtualização (Hipervisores) vs. Containers

<pre>
 [ Hipervisor Tipo 1 (Bare-Metal) ]                [ Hipervisor Tipo 2 (Hosted) ]                [ Contêineres (Docker / Podman) ]

┌──────────────────────────────────────┐     ┌──────────────────────────────────────┐     ┌──────────────────────────────────────┐
│ App A (SO Guest) │ App B (SO Guest)  │     │ App A (SO Guest) │ App B (SO Guest)  │     │ App A (Libs/Deps) │ App B (Libs/Deps)│
├──────────────────┴───────────────────┤     ├──────────────────┴───────────────────┤     ├──────────────────────────────────────┤
│ Hypervisor (ESXi, KVM, Hyper-V)      │     │ Hypervisor (VirtualBox, Workstation) │     │ Container Engine (Docker / Podman)   │
├──────────────────────────────────────┤     ├──────────────────────────────────────┤     ├──────────────────────────────────────┤
│ Hardware Físico                      │     │ Sistema Operacional Host (Windows/Linux)│  │ Kernel do SO Host (Compartilhado)    │
└──────────────────────────────────────┘     ├──────────────────────────────────────┤     ├──────────────────────────────────────┤
│ Hardware Físico                      │     │ Hardware Físico                      │
└──────────────────────────────────────┘     └──────────────────────────────────────┘

</pre>
* **Diferença Crítica de Container:** Contêineres **não virtualizam hardware** nem sobem um kernel próprio; eles usam recursos de isolamento do kernel host (*namespaces* e *cgroups*).
* **Infraestrutura como Código (IaC):** Garantia de **Idempotência** (executar o script 1 ou 100 vezes gera exatamente o mesmo estado final).
  * **Terraform:** Provisionamento declarativo de infraestrutura.
  * **Ansible:** Gerenciamento de configuração e automação agentless (via SSH).

---

## 6. Computação em Nuvem e Gerenciamento de TI (ITIL / COBIT)

### Modelos de Serviço em Nuvem

| Modelo | O Provedor gerencia | O Cliente gerencia | Exemplo |
|---|---|---|---|
| **IaaS** | Hardware, Virtualização, Rede, Storage | SO, Middleware, Runtime, Dados, Aplicação | AWS EC2, Azure VM, GCP Compute Engine |
| **PaaS** | IaaS + SO, Runtime, Middleware | Código da Aplicação e Dados | AWS Elastic Beanstalk, Heroku |
| **SaaS** | Tudo (fim a fim) | Apenas configurações de usuário e consumo | Google Workspace, Microsoft 365 |

* **Elasticidade vs. Escalabilidade:**
  * **Escalabilidade:** Capacidade do sistema de suportar aumento de carga (Vertical/Scale-Up: aumentar CPU/RAM; Horizontal/Scale-Out: adicionar mais nós).
  * **Elasticidade:** Capacidade de **alocar e desalocar recursos dinamicamente e de forma automática** conforme a demanda oscila.

### ITIL 4 & COBIT 4.1

<pre>
 [ Sistema de Valor de Serviço (SVS) - ITIL 4 ]

┌────────────────────────────────────────────────────────────────────────┐
│  Oportunidade / Demanda ──> [ Cadeia de Valor de Serviço ] ──> Valor   │
│                                                                        │
│  4 Dimensões:                                                          │
│  1. Organizações e Pessoas          3. Parceiros e Fornecedores        │
│  2. Informação e Tecnologia         4. Fluxos de Valor e Processos     │
└────────────────────────────────────────────────────────────────────────┘
</pre>

* **Práticas ITIL Fundamentais:**
  * **Incidente:** Restabelecer a operação normal do serviço o mais rápido possível (foco no sintoma imediato).
  * **Problema:** Identificar e gerenciar a **causa raiz** subjacente de um ou mais incidentes (foco na causa).
  * **Habilitação de Mudança:** Maximizar o número de mudanças bem-sucedidas avaliando riscos.
* **COBIT 4.1:** Domínio **DS (Entregar e Suportar / *Deliver and Support*)**: Focado na entrega técnica, níveis de serviço (**SLAs**), segurança, continuidade de operações e gestão de custos.

---

## 7. Governança, Normas e Resposta a Incidentes

### NIST SP 800-61 (Ciclo de Vida de Resposta a Incidentes)

<pre>
 [ 1. Preparação ] ──> [ 2. Detecção e Análise ] ──> [ 3. Contenção, Erradicação e Recuperação ] ──> [ 4. Pós-Incidente (Lições Aprendidas) ]
</pre>

1. **Preparação:** Políticas, ferramentas, equipe treinada.
2. **Detecção e Análise:** Identificar vetores e validar se o alerta é real.
3. **Contenção, Erradicação e Recuperação:** Isolar máquinas afetadas, remover artefatos maliciosos e restaurar serviços limpos.
4. **Atividade Pós-Incidente:** Relatório de lições aprendidas para retroalimentar a preparação.

### Modelagem de Ameaças STRIDE & Normas ISO

* **STRIDE:**
  * **S**poofing (Falsificação de identidade) $\rightarrow$ *Viola Autenticidade*
  * **T**ampering (Adulteração de dados) $\rightarrow$ *Viola Integridade*
  * **R**epudiation (Repúdio/Negação de autoria) $\rightarrow$ *Viola Não-Repúdio*
  * **I**nformation Disclosure (Vazamento de dados) $\rightarrow$ *Viola Confidencialidade*
  * **D**enial of Service (Ataque de negação de serviço) $\rightarrow$ *Viola Disponibilidade*
  * **E**levation of Privilege (Elevação de privilégio) $\rightarrow$ *Viola Autorização*
* **ISO 27001:** Requisitos para o SGSI (Sistema de Gestão de Segurança da Informação).
* **ISO 27002:** Código de práticas e guia de controles de segurança.
* **ISO 31000:** Gestão de Riscos corporativos.
* **ISO 22301:** Gestão de Continuidade de Negócios (SGCN).

---

## 8. Banco de Dados

### Modelo Relacional e Transações ACID

* **Atomicidade:** A transação é executada por inteiro ou é desfeita (*commit* ou *rollback* — "tudo ou nada").
* **Consistência:** A transação leva o banco de um estado válido a outro estado válido, respeitando restrições (chaves, tipos, checks).
* **Isolamento:** Transações simultâneas não interferem no resultado umas das outras.
* **Durabilidade:** Após o *commit*, os dados persistirão mesmo em caso de falha de energia ou colapso do sistema.

### Comandos SQL por Categoria

| Categoria | Definição | Comandos Principais |
|---|---|---|
| **DDL** (*Data Definition Language*) | Define estruturas e esquemas das tabelas | `CREATE`, `ALTER`, `DROP`, `TRUNCATE` |
| **DML** (*Data Manipulation Language*) | Manipula e consulta os dados | `SELECT`, `INSERT`, `UPDATE`, `DELETE` |
| **DCL** (*Data Control Language*) | Controla permissões e acessos | `GRANT`, `REVOKE` |
| **TCL** (*Transaction Control Language*) | Controla o fluxo de transações | `COMMIT`, `ROLLBACK`, `SAVEPOINT` |

* **Stored Procedure:** Bloco de código procedural executado sob demanda no servidor.
* **Trigger (Gatilho):** Procedimento invocado **automaticamente** pelo SGBD em resposta a eventos DML (`INSERT`, `UPDATE`, `DELETE`).

---

## 9. Programação e Engenharia de Software

### Análise de Complexidade de Algoritmos (Big-O)

Excelente: O(1)  ──> Busca em Hash Table
Bom:       O(log n) ──> Busca Binária em Árvore Balanceada
Aceitável: O(n)  ──> Busca Linear em Lista Encadeada
Médio:     O(n log n) ──> MergeSort, QuickSort (médio)
Ruim:      O(n²) ──> BubbleSort, InsertionSort (dois loops aninhados)
Péssimo:   O(2ⁿ) ou O(n!) ──> Algoritmos de força bruta exponencial

### Estruturas de Dados Fundamentais

* **Pilha (*Stack*):** LIFO (*Last-In, First-Out*). Operações: `push` (inserir) e `pop` (remover) sempre no topo.
* **Fila (*Queue*):** FIFO (*First-In, First-Out*). Inserção no fim (`enqueue`), remoção no início (`dequeue`).
* **Tabela Hash:** Mapeamento Chave $\rightarrow$ Valor via função de dispersão (*hashing*). Acesso médio em tempo constante $O(1)$.

---

### Pilares de POO & Java

[ Abstração ]     ──> Isolar o essencial ignorando detalhes desnecessários.
[ Encapsulamento ]──> Ocultar o estado interno via modificadores (private) e métodos (getters/setters).
[ Herança ]       ──> Reutilização de atributos e métodos de uma superclasse (extends).
[ Polimorfismo ]  ──> Capacidade de responder à mesma chamada de método de formas diferentes.

* **Sobrecarga (*Overload*):** Métodos com o **mesmo nome** na **mesma classe**, porém com **assinaturas/parâmetros diferentes** (resolvido em tempo de compilação).
* **Sobrescrita (*Override*):** Classe filha redefine o comportamento do método da classe pai mantendo a **mesma assinatura** (`@Override`, resolvido dinamicamente em tempo de execução).

#### Java EE Corporativo
* **Servlets:** Classes Java que interceptam e processam requisições HTTP, gerando respostas dinâmicas no lado do servidor.
* **JSP (*JavaServer Pages*):** Tecnologia de templates com código Java embutido em tags dentro do HTML para camada de apresentação (*View*).
* **EJB (*Enterprise JavaBeans*):** Componentes de arquitetura para regras de negócio com suporte nativo a transações distribuídas, segurança declarativa e pool de instâncias.

---

## 10. Raciocínio Lógico e Lógica Sentencial

### Tabela-Verdade Unificada

| $P$ | $Q$ | Conjunção ($P \land Q$) | Disjunção ($P \lor Q$) | Disjunção Exclusiva ($P \underline{\lor} Q$) | Condicional ($P \rightarrow Q$) | Bicondicional ($P \leftrightarrow Q$) |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **V** | **V** | **V** | **V** | F | **V** | **V** |
| **V** | **F** | F | **V** | **V** | **F** *(Vera Fischer)* | F |
| **F** | **V** | F | **V** | **V** | **V** | F |
| **F** | **F** | F | F | F | **V** | **V** |

---

### Regras de Ouro e Equivalências Essenciais

#### 1. Condicional ($P \rightarrow Q$)
* **Equivalência da Contrapositiva:** 
  $$P \rightarrow Q \equiv \neg Q \rightarrow \neg P$$
* **Equivalência com Disjunção (Mnemônico NE-MA):** 
  $$P \rightarrow Q \equiv \neg P \lor Q$$
  *(**NE**ga a primeira **OU** **MA**ntém a segunda)*
* **Negação da Condicional (Mnemônico MA-NÉ):** 
  $$\neg(P \rightarrow Q) \equiv P \land \neg Q$$
  *(**MA**ntém a primeira **E** **NE**ga a segunda)*

#### 2. Leis de De Morgan (Negação de Proposições Compostas)
* $\neg(P \land Q) \equiv \neg P \lor \neg Q$
* $\neg(P \lor Q) \equiv \neg P \land \neg Q$

#### 3. Classificação das Sentenças
* **Tautologia:** O resultado da tabela-verdade é **sempre Verdadeiro (V)**, independentemente dos valores das proposições.
* **Contradição:** O resultado da tabela-verdade é **sempre Falso (F)**.
* **Contingência:** O resultado da tabela-verdade apresenta **ao menos um V e ao menos um F**.

#### 4. Lógica de Predicados (Negação de Quantificadores)
* **Negar "Todo" ($\forall$):** Vira "Existe ao menos um que NÃO é" ($\exists \neg$)
  $$\neg(\forall x P(x)) \equiv \exists x \neg P(x)$$
* **Negar "Existe" ($\exists$):** Vira "Nenhum é" / "Todo não é" ($\forall \neg$)
  $$\neg(\exists x P(x)) \equiv \forall x \neg P(x)$$

