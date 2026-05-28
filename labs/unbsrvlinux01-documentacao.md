# 📋 Documentação do Servidor — `wslinux01`

> **Para quem é este documento?**
> Este guia foi escrito para pessoas que não são da área de TI. Aqui você vai encontrar, de forma simples e visual, tudo sobre como este servidor está configurado, quais serviços ele oferece e qual é o estado atual do sistema.

---

## 🖥️ Identificação do Servidor

| Campo            | Informação                          |
|------------------|-------------------------------------|
| **Nome**         | `wslinux01`                         |
| **Usuário**      | `senac`                             |
| **Sistema**      | Ubuntu Linux 24.04.4 LTS            |
| **Tipo**         | Servidor virtual (Máquina Virtual)  |
| **Plataforma**   | KVM (virtualização completa)        |
| **Arquitetura**  | x86_64 (64 bits)                    |

> 💡 **O que é uma Máquina Virtual?**
> Imagine um computador "dentro" de outro computador. O servidor físico real (host) hospeda este servidor virtual (wslinux01), que funciona de forma independente.

---

## ⚙️ Hardware (Recursos de Processamento)

### 🧠 Processador (CPU)

| Detalhe            | Valor                          |
|--------------------|--------------------------------|
| **Modelo**         | Intel Core i7-14700K           |
| **Núcleos (vCPU)** | 2 núcleos virtuais             |
| **Tipo**           | 32 e 64 bits                   |

```
┌─────────────────────────────┐
│         PROCESSADOR         │
│   Intel Core i7-14700K      │
│                             │
│  [ Núcleo 0 ] [ Núcleo 1 ] │
│                             │
│  2 CPUs virtuais ativas     │
└─────────────────────────────┘
```

> 💡 Os 2 núcleos virtuais permitem que o servidor execute múltiplas tarefas ao mesmo tempo, como atender várias pessoas simultaneamente.

---

### 💾 Memória RAM

| Tipo        | Total  | Em uso | Livre  | Disponível |
|-------------|--------|--------|--------|------------|
| **RAM**     | 3,8 GB | 1,3 GB | 1,5 GB | 2,5 GB     |
| **SWAP**    | 2,0 GB | 0 B    | 2,0 GB | —          |

```
RAM  [████████░░░░░░░░░░░░░░░░░░░░]  34% usada
     |<-- 1,3 GB usado -->|<--- 2,5 GB disponível --->|

SWAP [░░░░░░░░░░░░░░░░░░░░░░░░░░░░]   0% usada
     (memória de emergência — não está sendo usada ✅)
```

> ✅ **Situação:** A memória RAM está saudável. Mais de 65% está disponível, o que indica que o servidor não está sobrecarregado.

> 💡 **O que é SWAP?** É um espaço no disco que serve como "memória extra de emergência". O fato de estar em 0% de uso é ótimo — significa que o servidor tem RAM suficiente.

---

### 💿 Armazenamento (Disco)

#### Estrutura do Disco

```
DISCO PRINCIPAL (sda) — 50 GB total
├── sda1 — 1 MB   (área de boot BIOS)
├── sda2 — 2 GB   → /boot  (arquivos de inicialização)
└── sda3 — 48 GB  → sistema principal (LVM)
         └── ubuntu-vg/ubuntu-lv — 48 GB → /
```

#### Uso do Espaço em Disco

| Partição / Área        | Tamanho | Usado | Livre | Uso%  | Função                    |
|------------------------|---------|-------|-------|-------|---------------------------|
| Sistema principal `/`  | 47 GB   | 11 GB | 35 GB | 24%   | 🏠 Tudo do servidor       |
| Boot `/boot`           | 2,0 GB  | 200 MB| 1,6 GB| 11%   | 🚀 Inicialização          |
| Memória temporária     | 392 MB  | 1,1 MB| 391 MB| 1%    | ⚡ Arquivos temporários   |

```
Disco Principal (/)
[█████░░░░░░░░░░░░░░░░░░░░░░░░░] 24% usado — 11 GB de 47 GB
                                  35 GB livres ✅
```

> ✅ **Situação:** Excelente. Apenas 24% do disco principal está em uso. Há bastante espaço disponível.

---

## 🌐 Rede — Como o Servidor se Conecta

### Endereços IP (Como o Servidor é Identificado na Rede)

> 💡 **O que é um endereço IP?** É como o "endereço residencial" do servidor na rede. Assim como uma casa tem um número, o servidor tem um IP para que outros computadores possam encontrá-lo.

| Interface   | Tipo           | Endereço IP        | Situação  |
|-------------|----------------|--------------------|-----------|
| `lo`        | Loopback       | 127.0.0.1          | Interno   |
| `enp0s3`    | Principal (fixo)| **10.24.82.215/24**| ✅ Ativo  |
| `enp0s3`    | Secundário (DHCP)| 10.24.82.47/24    | ✅ Ativo  |

> 💡 **Dois IPs?** Sim! O servidor tem dois endereços na mesma placa de rede:
> - **10.24.82.215** → configurado manualmente (fixo, permanente)
> - **10.24.82.47** → atribuído automaticamente pelo roteador (temporário)

### Roteamento (Como o Servidor Sai para a Internet)

| Destino         | Via (Gateway)  | Significado                              |
|-----------------|----------------|------------------------------------------|
| Internet (geral)| 10.24.82.1     | 🌐 Saída padrão para a internet          |
| Rede local      | direto         | 🏠 Computadores na mesma rede (10.24.82.x)|

```
SERVIDOR wslinux01
(10.24.82.215)
      │
      ▼
GATEWAY / ROTEADOR
(10.24.82.1)
      │
      ▼
🌐 INTERNET
```

### DNS — Sistema de Nomes (Como o Servidor Resolve Endereços)

> 💡 **O que é DNS?** Quando você digita "www.google.com", o DNS traduz esse nome para o endereço IP correspondente. É como uma agenda telefônica da internet.

| Servidor DNS       | Tipo          | Descrição                |
|--------------------|---------------|--------------------------|
| **8.8.8.8**        | Primário      | 🌐 Google DNS (principal)|
| **8.8.4.4**        | Secundário    | 🌐 Google DNS (backup)   |
| 10.24.40.190       | Interno       | 🏢 DNS da rede Senac     |
| 10.1.1.195         | Interno       | 🏢 DNS da rede Senac     |
| 10.1.1.242         | Interno       | 🏢 DNS da rede Senac     |

**Domínios de busca configurados:** `senac.br` | `senacsp.edu.br`

### Configuração de Rede (Netplan)

A placa de rede `enp0s3` está configurada com:
- ✅ DHCP ativado (recebe IP automático)
- ✅ IP fixo manual: `10.24.82.215/24`
- ✅ Gateway: `10.24.82.1`
- ✅ DNS: Google (`8.8.8.8`, `8.8.4.4`)

---

## 🚀 Serviços Ativos — O Que o Servidor Oferece

> 💡 **O que são serviços?** São programas que ficam "de plantão" esperando pedidos. Como um garçom que fica à disposição para atender os clientes.

### Serviços Principais em Execução

| Emoji | Serviço          | Porta(s)     | O que faz                                          | Status     |
|-------|------------------|--------------|----------------------------------------------------|------------|
| 🌐    | **Apache 2**     | 80, 8888     | Servidor web — publica páginas na internet/intranet | ✅ Rodando |
| ☕    | **Tomcat 11**    | 8080         | Servidor de aplicações Java                        | ✅ Rodando |
| 🗃️    | **MySQL**        | 3306, 33060  | Banco de dados — armazena informações              | ✅ Rodando |
| 📊    | **Grafana**      | 3000         | Painel visual de monitoramento                     | ✅ Rodando |
| 🔬    | **Prometheus**   | 9091         | Coleta métricas do servidor                        | ✅ Rodando |
| 📡    | **Node Exporter**| 9100         | Exporta dados do sistema para o Prometheus         | ✅ Rodando |
| 🔐    | **SSH (sshd)**   | 22           | Acesso remoto seguro ao servidor                   | ✅ Rodando |

### Mapa Visual de Portas

```
SERVIDOR wslinux01 — Portas abertas para conexão
═══════════════════════════════════════════════════

Porta 22    🔐 SSH       — Acesso remoto (administração)
Porta 80    🌐 HTTP      — Site/Web (Apache)
Porta 3000  📊 Grafana   — Painel de monitoramento
Porta 3306  🗃️  MySQL     — Banco de dados
Porta 8080  ☕ Tomcat    — Aplicações Java
Porta 8888  🌐 Apache    — Site alternativo
Porta 9091  🔬 Prometheus— Coleta de métricas
Porta 9100  📡 Node Exp. — Dados do sistema
Porta 33060 🗃️  MySQL X   — MySQL protocolo avançado
```

### Como os Serviços de Monitoramento se Comunicam

```
┌──────────────────────────────────────────────┐
│              MONITORAMENTO                   │
│                                              │
│  📡 Node Exporter    ──────►  🔬 Prometheus  │
│  (coleta dados do         (agrupa e          │
│   sistema: CPU,            armazena          │
│   memória, disco)          métricas)         │
│                                 │            │
│                                 ▼            │
│                          📊 Grafana          │
│                          (exibe gráficos     │
│                           e dashboards)      │
└──────────────────────────────────────────────┘
```

### Conexão SSH Ativa no Momento

```
🖥️  ADMINISTRADOR          🔐 SSH (porta 22)       💻 wslinux01
    10.24.82.33      ◄─────────────────────►    10.24.82.215
    (porta 50225)             (ATIVA)
```

---

## 📦 Serviços do Sistema (Serviços de Suporte)

Além dos serviços principais, o servidor conta com vários serviços de suporte rodando automaticamente:

| Serviço                  | O que faz                                           |
|--------------------------|-----------------------------------------------------|
| `cron`                   | ⏰ Agenda tarefas automáticas no servidor           |
| `rsyslog`                | 📝 Registra logs (histórico de eventos)             |
| `systemd-timesyncd`      | 🕐 Sincroniza o relógio do servidor                 |
| `systemd-networkd`       | 🌐 Gerencia as configurações de rede                |
| `systemd-resolved`       | 🔍 Resolve nomes de domínio (DNS)                   |
| `unattended-upgrades`    | 🔄 Instala atualizações de segurança automaticamente|
| `ufw`                    | 🛡️ Firewall — controla acessos ao servidor          |
| `apparmor`               | 🔒 Segurança adicional para processos               |
| `open-vm-tools`          | 🖥️ Integração com o hipervisor VMware/KVM           |

---

## 🔄 Atualizações do Sistema

### Situação Atual

O sistema passou por uma verificação de atualizações disponíveis. O resultado foi:

| Pacote         | Situação                     |
|----------------|------------------------------|
| `vim`          | ⬆️ Atualização disponível     |
| `vim-common`   | ⬆️ Atualização disponível     |
| `vim-runtime`  | ⬆️ Atualização disponível     |
| `vim-tiny`     | ⬆️ Atualização disponível     |
| `xxd`          | ⬆️ Atualização disponível     |
| `libgcrypt20`  | ⬆️ Atualização de segurança   |

> ⚠️ **Atenção:** O pacote `libgcrypt20` é uma **atualização de segurança** importante. Recomenda-se aplicar essa atualização o quanto antes.

### Como aplicar as atualizações

Para aplicar as atualizações pendentes, um administrador deve executar:

```bash
sudo apt update && sudo apt upgrade -y
```

### Política de Atualizações Automáticas

O servidor possui `unattended-upgrades` ativado, que aplica automaticamente atualizações de segurança críticas da Ubuntu. As atualizações de origem terceira (como Grafana e Node.js) **não** são aplicadas automaticamente.

---

## 🛡️ Segurança — Resumo das Vulnerabilidades

> 💡 As "vulnerabilidades" listadas são análises do processador sobre falhas de segurança conhecidas em processadores Intel. A maioria está com mitigações (proteções) ativas.

| Vulnerabilidade       | Situação          | O que significa                         |
|-----------------------|-------------------|-----------------------------------------|
| Meltdown              | ✅ Não afetado    | Falha clássica de processadores Intel   |
| Spectre v1            | 🛡️ Mitigado       | Proteção ativa via software             |
| Spectre v2            | 🛡️ Mitigado       | Proteção ativa via software avançado    |
| Spec Store Bypass     | ⚠️ Vulnerável     | Sem mitigação total — risco baixo       |
| Retbleed              | 🛡️ Mitigado       | Proteção IBRS ativa                     |
| MDS / L1TF / Meltdown | ✅ Não afetado    | Geração de CPU não afetada              |

> ⚠️ **Spec Store Bypass** aparece como "vulnerável", mas em ambientes virtuais controlados o risco é geralmente baixo. Consulte o administrador de sistemas para avaliação específica.

---

## 📡 Diagrama Geral do Servidor

```
                        🌐 INTERNET
                             │
                        10.24.82.1
                        (GATEWAY)
                             │
                    ┌────────┴────────┐
                    │   wslinux01     │
                    │ 10.24.82.215    │
                    │ 10.24.82.47     │
                    │                 │
                    │  ┌───────────┐  │
                    │  │ SERVIÇOS  │  │
                    │  │           │  │
                    │  │ 🌐 :80    │  │
                    │  │ 🌐 :8888  │  │
                    │  │ ☕ :8080  │  │
                    │  │ 🗃️  :3306  │  │
                    │  │ 📊 :3000  │  │
                    │  │ 🔬 :9091  │  │
                    │  │ 📡 :9100  │  │
                    │  │ 🔐 :22    │  │
                    │  └───────────┘  │
                    │                 │
                    │  💾 RAM: 3.8GB  │
                    │  💿 DISCO: 50GB │
                    │  🧠 CPU: 2 vCPU │
                    └─────────────────┘
                             │
                    ─────────┴─────────
                    Rede: 10.24.82.0/24
```

---

## ✅ Resumo de Saúde do Servidor

| Área              | Status  | Observação                                      |
|-------------------|---------|-------------------------------------------------|
| 🧠 CPU            | ✅ OK   | 2 vCPUs ativas, sem sobrecarga                  |
| 💾 RAM            | ✅ OK   | 34% em uso, 2,5 GB disponíveis                  |
| 💿 Disco          | ✅ OK   | 24% em uso, 35 GB livres                        |
| 🌐 Rede           | ✅ OK   | Interface ativa com 2 IPs                       |
| 🚀 Serviços       | ✅ OK   | Todos os serviços principais rodando            |
| 🔄 Atualizações   | ⚠️ AÇÃO | 6 pacotes pendentes (1 é de segurança urgente)  |
| 🛡️ Segurança      | ⚠️ NOTA | Spec Store Bypass sem mitigação total           |

---

*Documentação gerada automaticamente com base nos dados coletados do servidor `wslinux01` · Ubuntu 24.04.4 LTS*
