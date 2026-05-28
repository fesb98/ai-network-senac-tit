# 🖥️ Documentação do Servidor `unbsrvlinux01`
### Ubuntu Server 24.04 LTS — Guia para Usuários Não Técnicos

> 👋 **Este documento foi feito para VOCÊ** — mesmo que não seja da área de TI!
> Aqui você vai entender, de forma simples e visual, **o que é, como funciona e qual é o estado atual** do servidor da sua instituição.

---

## 📋 Índice Rápido

| Seção | O que você vai encontrar |
|---|---|
| [🏷️ Identificação](#identificação) | Nome, sistema operacional, tipo de servidor |
| [⚙️ Hardware](#hardware) | Processador, memória e disco |
| [🌐 Rede e Topologia](#rede-e-topologia) | Como o servidor se conecta |
| [🚀 Serviços](#serviços) | O que o servidor oferece |
| [🔄 Atualizações](#atualizações) | O que precisa ser atualizado |
| [🛡️ Segurança](#segurança) | Status de proteção |
| [✅ Resumo de Saúde](#resumo-de-saúde) | Visão geral rápida |

---

## 🏷️ Identificação

| Campo | Informação |
|---|---|
| **Nome do servidor** | `unbsrvlinux01` |
| **Usuário padrão** | `senac` |
| **Sistema operacional** | Ubuntu Linux 24.04.4 LTS |
| **Tipo** | Máquina Virtual (VM) |
| **Plataforma de virtualização** | KVM |
| **Arquitetura** | x86_64 (64 bits) |

> 💡 **O que é uma Máquina Virtual (VM)?**
> Imagine um **computador dentro de outro computador**. O computador físico real (chamado de "host") hospeda este servidor virtual, que funciona de forma completamente independente — como um apartamento dentro de um prédio.

---

## ⚙️ Hardware

### 🧠 Processador (CPU)

| Detalhe | Valor |
|---|---|
| **Modelo físico** | Intel Core i7-14700K |
| **Núcleos virtuais disponíveis** | 2 vCPUs |
| **Compatibilidade** | 32 e 64 bits |

```
╔══════════════════════════════════╗
║         PROCESSADOR              ║
║    Intel Core i7-14700K          ║
║                                  ║
║   ┌─────────┐   ┌─────────┐     ║
║   │ Núcleo  │   │ Núcleo  │     ║
║   │    0    │   │    1    │     ║
║   └─────────┘   └─────────┘     ║
║                                  ║
║   2 núcleos virtuais ativos ✅   ║
╚══════════════════════════════════╝
```

> 💡 Os 2 núcleos virtuais permitem que o servidor atenda **várias pessoas ao mesmo tempo**, sem travar — como um caixa de banco com dois atendentes.

---

### 💾 Memória RAM

| Tipo | Total | Em uso | Disponível | Status |
|---|---|---|---|---|
| **RAM** | 3,8 GB | 1,3 GB | 2,5 GB | ✅ Saudável |
| **SWAP** | 2,0 GB | 0 B | 2,0 GB | ✅ Não utilizada |

```
MEMÓRIA RAM
████████░░░░░░░░░░░░░░░░░░░░░░  34% em uso
│◄ 1,3 GB usado ►│◄──── 2,5 GB disponível ────►│

SWAP (memória de emergência)
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░   0% — ótimo! ✅
```

> ✅ **Situação excelente!** Mais de 65% da memória está livre. O servidor está tranquilo.

> 💡 **O que é SWAP?** É um "colchão de emergência" no disco, usado quando a RAM estiver cheia. Estar em 0% significa que a RAM está suficiente para tudo que está rodando.

---

### 💿 Armazenamento (Disco)

```
DISCO PRINCIPAL (50 GB total)
├── 🟡 Área de boot BIOS   → 1 MB
├── 🚀 Inicialização /boot → 2 GB
└── 🏠 Sistema principal   → 48 GB
         └── Partição ativa → 48 GB (montada em /)
```

| Área | Tamanho | Usado | Livre | Uso | Função |
|---|---|---|---|---|---|
| 🏠 Sistema `/` | 47 GB | 11 GB | 35 GB | 24% | Tudo do servidor |
| 🚀 Boot `/boot` | 2,0 GB | 200 MB | 1,6 GB | 11% | Inicialização |
| ⚡ Temporário | 392 MB | 1 MB | 391 MB | 1% | Arquivos temporários |

```
Disco Principal (/)
█████░░░░░░░░░░░░░░░░░░░░░░░░░  24% usado
│◄ 11 GB ►│◄──────────── 35 GB livres ────────────►│
```

> ✅ **Excelente!** Apenas 24% do disco está em uso. Há muito espaço para crescer.

---

## 🌐 Rede e Topologia

### 🗺️ Topologia Física — Como tudo está conectado fisicamente

```
┌─────────────────────────────────────────────────────────────────┐
│                     LABORATÓRIO / ESCRITÓRIO                    │
│                                                                 │
│  ┌──────────────────────────┐                                   │
│  │   💻 Desktop Windows 11  │                                   │
│  │   ┌──────────────────┐   │                                   │
│  │   │   VirtualBox     │   │                                   │
│  │   │  ┌────────────┐  │   │                                   │
│  │   │  │ 🐧 Ubuntu  │  │   │◄──── Cabo Ethernet               │
│  │   │  │   Server   │  │   │           │                       │
│  │   │  │  (VM)      │  │   │     ┌─────┴─────┐                │
│  │   │  └────────────┘  │   │     │  Switch   │                │
│  │   └──────────────────┘   │     │  de Rede  │                │
│  └──────────────────────────┘     └─────┬─────┘                │
│                                         │                       │
│                                   ┌─────┴─────┐                │
│                                   │ Roteador  │                │
│                                   │ /Firewall │                │
│                                   └─────┬─────┘                │
└─────────────────────────────────────────┼───────────────────────┘
                                          │
                                    🌐 INTERNET
```

> 💡 **Modo Bridge:** A VM do Ubuntu Server se comporta como se fosse um computador físico real na rede — ela recebe um endereço de rede próprio e pode ser acessada por outros computadores do laboratório diretamente.

---

### 🗺️ Topologia Lógica — Como os endereços estão organizados

```
╔══════════════════════════════════════════════════════════════╗
║          REDE DO LABORATÓRIO — 10.24.82.0/24                 ║
║                                                              ║
║   💻 Windows 11          🐧 Ubuntu Server (VM)              ║
║   IP: 10.24.82.x         IP: 10.24.82.215 (fixo)           ║
║                           IP: 10.24.82.47  (automático)     ║
║                                    │                         ║
║                             ───────┴────────                 ║
║                             Switch de Rede                   ║
║                             ───────┬────────                 ║
║                                    │                         ║
║                           🔒 Roteador/Firewall               ║
║                           IP: 10.24.82.1 (Gateway)          ║
╚══════════════════════════════════════════════════════════════╝
                                    │
                              🌐 INTERNET
                                    │
                         👤 Cliente SSH Externo
                          (acesso remoto via SSH)
```

---

### 📍 Endereços IP do Servidor

> 💡 **O que é um endereço IP?** É como o "número da casa" do servidor na rede. Outros computadores usam esse número para encontrá-lo e se comunicar com ele.

| Interface | Tipo | Endereço IP | Situação |
|---|---|---|---|
| `lo` | Loopback (interno) | 127.0.0.1 | 🔁 Uso interno |
| `enp0s3` | Principal — fixo | **10.24.82.215/24** | ✅ Ativo |
| `enp0s3` | Secundário — automático | 10.24.82.47/24 | ✅ Ativo |

> 💡 **Por que dois IPs?**
> - `10.24.82.215` → configurado manualmente pelo administrador (permanente, nunca muda)
> - `10.24.82.47` → atribuído automaticamente pelo roteador (pode mudar)
>
> É como ter um endereço fixo **e** um apartamento temporário — o fixo é o principal.

---

### 🚦 Roteamento — Como o servidor sai para a internet

```
🖥️ unbsrvlinux01           🔀 Gateway              🌐 Internet
  10.24.82.215    ──────►  10.24.82.1    ──────►   (qualquer site)

🖥️ unbsrvlinux01           🏠 Rede local
  10.24.82.215    ──────►  10.24.82.x    (direto, sem passar pelo gateway)
```

---

### 🔍 DNS — Como o servidor resolve nomes de sites

> 💡 **O que é DNS?** Quando você digita "www.google.com", o DNS é o serviço que traduz esse nome para o endereço IP correspondente. É como uma **agenda telefônica da internet**.

| Servidor DNS | Tipo | Descrição |
|---|---|---|
| **8.8.8.8** | 🌐 Primário | Google DNS (principal) |
| **8.8.4.4** | 🌐 Secundário | Google DNS (backup) |
| 10.24.40.190 | 🏢 Interno | DNS da rede Senac |
| 10.1.1.195 | 🏢 Interno | DNS da rede Senac |
| 10.1.1.242 | 🏢 Interno | DNS da rede Senac |

**Domínios de busca:** `senac.br` · `senacsp.edu.br`

---

## 🚀 Serviços

> 💡 **O que são serviços?** São programas que ficam **"de plantão"** no servidor, esperando por pedidos. Como garçons em um restaurante — cada um atende um tipo de pedido diferente, pela "janela" (porta) certa.

### 📋 Tabela de Serviços Ativos

| Serviço | Porta | O que faz | Status |
|---|---|---|---|
| 🌐 **Apache 2** | 80, 8888 | Servidor web — publica páginas e sites | ✅ Rodando |
| ☕ **Tomcat 11** | 8080 | Servidor para aplicações Java | ✅ Rodando |
| 🗃️ **MySQL** | 3306, 33060 | Banco de dados — armazena informações | ✅ Rodando |
| 📊 **Grafana** | 3000 | Painel visual de monitoramento | ✅ Rodando |
| 🔬 **Prometheus** | 9091 | Coleta métricas do servidor | ✅ Rodando |
| 📡 **Node Exporter** | 9100 | Exporta dados do sistema para o Prometheus | ✅ Rodando |
| 🔐 **SSH** | 22 | Acesso remoto seguro ao servidor | ✅ Rodando |

---

### 🚪 Mapa de Portas — As "janelas" do servidor

```
╔═══════════════════════════════════════════════════════╗
║         SERVIDOR unbsrvlinux01 — PORTAS ABERTAS       ║
║                                                       ║
║  Porta  22  🔐  SSH        → Acesso remoto seguro     ║
║  Porta  80  🌐  HTTP       → Site / Página Web        ║
║  Porta 3000 📊  Grafana    → Painel de monitoramento  ║
║  Porta 3306 🗃️   MySQL      → Banco de dados          ║
║  Porta 8080 ☕  Tomcat     → Aplicações Java          ║
║  Porta 8888 🌐  Apache     → Site alternativo         ║
║  Porta 9091 🔬  Prometheus → Coleta de métricas       ║
║  Porta 9100 📡  Node Exp.  → Dados do sistema         ║
║  Porta33060 🗃️   MySQL X    → MySQL protocolo avançado║
╚═══════════════════════════════════════════════════════╝
```

> 💡 **O que é uma "porta"?** Imagine o servidor como um prédio com várias janelas numeradas. Cada serviço atende por uma janela específica. Quem quiser falar com o site web, bate na janela 80. Quem quiser acessar o banco de dados, bate na 3306. E assim por diante.

---

### 📈 Como os serviços de monitoramento trabalham juntos

```
┌─────────────────────────────────────────────────────┐
│                  MONITORAMENTO                      │
│                                                     │
│  📡 Node Exporter              🔬 Prometheus        │
│  (Coleta dados brutos)  ────►  (Organiza e          │
│   CPU, memória, disco,          armazena os         │
│   rede, processos...            dados históricos)   │
│                                        │            │
│                                        ▼            │
│                                 📊 Grafana          │
│                                 (Exibe gráficos     │
│                                  e dashboards       │
│                                  bonitos para       │
│                                  o usuário)         │
└─────────────────────────────────────────────────────┘
```

> 💡 **Analogia:** O Node Exporter é o **termômetro**, o Prometheus é o **prontuário médico** e o Grafana é o **médico** que interpreta tudo de forma visual e compreensível.

---

### 🔐 Conexão SSH Ativa

```
👤 ADMINISTRADOR               🔐 SSH (porta 22)      🖥️ unbsrvlinux01
   10.24.82.33       ◄────────────────────────────►   10.24.82.215
   (porta 50225)                  ATIVA ✅
```

---

### ⚙️ Serviços de Suporte (funcionam nos bastidores)

| Serviço | O que faz |
|---|---|
| `cron` | ⏰ Agenda tarefas automáticas |
| `rsyslog` | 📝 Registra o histórico de tudo que acontece |
| `systemd-timesyncd` | 🕐 Mantém o relógio do servidor correto |
| `systemd-networkd` | 🌐 Gerencia as configurações de rede |
| `systemd-resolved` | 🔍 Resolve nomes de sites (DNS) |
| `unattended-upgrades` | 🔄 Instala atualizações de segurança automaticamente |
| `ufw` | 🛡️ Firewall — controla quem pode acessar o servidor |
| `apparmor` | 🔒 Segurança extra para os programas |
| `open-vm-tools` | 🖥️ Integração com o ambiente de virtualização |

---

## 🔄 Atualizações

### 📦 Pacotes com atualização disponível

| Pacote | Tipo | Urgência |
|---|---|---|
| `vim` | Atualização geral | 🟡 Normal |
| `vim-common` | Atualização geral | 🟡 Normal |
| `vim-runtime` | Atualização geral | 🟡 Normal |
| `vim-tiny` | Atualização geral | 🟡 Normal |
| `xxd` | Atualização geral | 🟡 Normal |
| `libgcrypt20` | **Atualização de segurança** | 🔴 **Urgente** |

> ⚠️ **Atenção:** O pacote `libgcrypt20` é uma **atualização de segurança crítica**. Recomenda-se aplicá-la o quanto antes. Fale com o administrador do sistema.

### 🛠️ Como aplicar as atualizações (para o administrador)

```bash
sudo apt update && sudo apt upgrade -y
```

### 🤖 Política de atualização automática

```
✅ Atualizações de segurança Ubuntu  →  AUTOMÁTICAS (unattended-upgrades)
⚠️ Atualizações de terceiros         →  MANUAIS (Grafana, Node.js, etc.)
```

---

## 🛡️ Segurança

> 💡 As vulnerabilidades listadas abaixo são falhas conhecidas em processadores Intel. A maioria já tem **proteções (mitigações) ativas** instaladas automaticamente pelo sistema operacional.

| Vulnerabilidade | Status | O que significa |
|---|---|---|
| Meltdown | ✅ Não afetado | Esta geração de CPU não é vulnerável |
| Spectre v1 | 🛡️ Protegido | Proteção ativa via software |
| Spectre v2 | 🛡️ Protegido | Proteção avançada ativa |
| Spec Store Bypass | ⚠️ Vulnerável | Sem mitigação total — risco baixo |
| Retbleed | 🛡️ Protegido | Proteção IBRS ativa |
| MDS / L1TF | ✅ Não afetado | Esta geração de CPU não é vulnerável |

> ⚠️ O item **Spec Store Bypass** aparece como vulnerável, mas em ambientes virtuais controlados o risco é geralmente baixo. Consulte o administrador para avaliação específica do seu ambiente.

---

## 📡 Diagrama Geral do Servidor

```
                          🌐 INTERNET
                               │
                         10.24.82.1
                      🔀 Gateway/Roteador
                               │
              ┌────────────────┴────────────────┐
              │          🐧 unbsrvlinux01        │
              │    IPs: 10.24.82.215 (fixo)      │
              │         10.24.82.47  (dhcp)       │
              │                                  │
              │  ┌──────────────────────────┐    │
              │  │       SERVIÇOS           │    │
              │  │  🔐 SSH       :22        │    │
              │  │  🌐 Apache    :80 :8888  │    │
              │  │  ☕ Tomcat    :8080      │    │
              │  │  🗃️  MySQL     :3306      │    │
              │  │  📊 Grafana   :3000      │    │
              │  │  🔬 Prometheus :9091     │    │
              │  │  📡 Node Exp. :9100      │    │
              │  └──────────────────────────┘    │
              │                                  │
              │  🧠 CPU: 2 vCPUs (i7-14700K)     │
              │  💾 RAM: 3,8 GB (34% em uso)     │
              │  💿 DISCO: 50 GB (24% em uso)    │
              │  🐧 Ubuntu Server 24.04.4 LTS    │
              └──────────────────────────────────┘
                               │
                   ────────────┴────────────
                   Rede local: 10.24.82.0/24
                   ────────────────────────
                   💻 Windows 11: 10.24.82.x
                   💻 Outros dispositivos
```

---

## ✅ Resumo de Saúde do Servidor

| Área | Status | Observação |
|---|---|---|
| 🧠 **CPU** | ✅ OK | 2 vCPUs ativas, sem sobrecarga |
| 💾 **RAM** | ✅ OK | 34% em uso — folgada |
| 💿 **Disco** | ✅ OK | 24% em uso — bastante espaço livre |
| 🌐 **Rede** | ✅ OK | Interface ativa com 2 IPs configurados |
| 🚀 **Serviços** | ✅ OK | Todos os serviços principais rodando |
| 🔄 **Atualizações** | ⚠️ AÇÃO NECESSÁRIA | 6 pacotes pendentes — 1 é de segurança urgente |
| 🛡️ **Segurança** | ⚠️ ATENÇÃO | Spec Store Bypass sem mitigação total |

### 🎯 O que precisa ser feito agora?

```
🔴 URGENTE   → Aplicar atualização de segurança do pacote libgcrypt20
🟡 RECOMENDADO → Aplicar as demais 5 atualizações pendentes
🟢 OK        → Todo o resto está funcionando normalmente
```

---

> 📄 *Documentação gerada com base nos dados coletados do servidor `unbsrvlinux01` · Ubuntu 24.04.4 LTS*
> 🏫 *Rede: Senac — 10.24.82.0/24*
