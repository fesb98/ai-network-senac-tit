**Documento Técnico Padronizado - Ubuntu Server (Docker)**

_Arquivo gerado automaticamente a partir das anotações coletadas no servidor._

**Informações Gerais do Servidor**

| Categoria | Descrição | Configuração |
|-----------|-----------|--------------|
| 🖥️ Hostname | Nome do host | ctlinux01 |
| 🧾 Sistema Operacional | Versão Ubuntu | Ubuntu 24.04.4 LTS (noble) |
| 🧩 Kernel | Versão do kernel Linux | 6.8.0-107-generic |
| ⏱️ Uptime | Tempo desde o último boot | up 2:10 (hora registrada: 23:40:16) |
| 🏷️ Architecture | Arquitetura do Sistema | x86_64 |
| 🧭 Virtualization | Tipo de virtualização | VirtualBox (innotek GmbH) |

Explicação (nível não técnico):

"Este servidor está executando Ubuntu 24.04 LTS com kernel 6.8. Ele é uma máquina virtual (VirtualBox) chamada `ctlinux01`. O tempo de atividade atual é curto (aprox. 2 horas), ou seja, o sistema foi reiniciado recentemente."

**Informações de Hardware do Servidor**

| Categoria | Descrição | Configuração |
|-----------|-----------|--------------|
| 💾 Memória RAM | Total / Disponível | 3915 MB total — 3298 MB available |
| 🔁 Swap | Tamanho Swap | 3914 MB (0 usado) |
| 🧱 Disco (LVM) | Particionamento e volumes | sda: 50G (sda2 2G /boot, sda3 48G LVM => `/` 47G) |
| 📂 Uso de Disco | Uso do FS principal | `/` 47G total — 8.5G usado (19%) |

Explicação (nível não técnico):

"O servidor tem cerca de 4GB de memória RAM e um disco virtual de 50GB (com ~37GB livres). A memória disponível é suficiente para cargas leves; se houver muitos containers em produção, pode haver necessidade de mais RAM."

**Informações de Rede do Servidor**

| Categoria | Descrição | Configuração |
|-----------|-----------|--------------|
| 🌐 Interface principal | Nome e MAC | `enp0s3` — 08:00:27:42:90:a5 |
| 🌍 Endereço IPv4 | IPv4 / máscara | `10.24.82.200/24` (scope global) |
| 🔀 Gateway padrão | Rota padrão | `default via 10.24.82.1 dev enp0s3` |
| 🧭 Rotas | Redes conhecidas | `10.24.82.0/24` e `172.17.0.0/16` (docker0) |
| 🐋 Rede Docker | Bridge default | `docker0` — 172.17.0.1/16 |
| 📡 DNS | Servidores configurados | 8.8.8.8, 8.8.4.4 (resolv.conf em modo stub) |
| 🔒 Loopback | Endereço local | 127.0.0.1/8 |

Explicação (nível não técnico):

"O servidor utiliza o endereço IP 10.24.82.200 na rede local com gateway 10.24.82.1. O Docker cria uma rede interna (`docker0`) separada (172.17.0.0/16) para os containers. DNS público (Google) está configurado."

**Informações de Serviços e Processos**

| Categoria | Descrição | Configuração |
|-----------|-----------|--------------|
| ⚙️ Serviços em execução | Serviços systemd | `docker.service`, `containerd.service`, `portainer.service`, `ssh.service`, `rsyslog.service`, `systemd-networkd.service`, `systemd-resolved.service`, `unattended-upgrades.service`, entre outros. |
| 🔎 Portas TCP | Listening (ss -tln) | `0.0.0.0:22` (SSH), `0.0.0.0:9000` (Portainer), `127.0.0.54:53` e `127.0.0.53:53` (DNS locais/resolv) |
| 🐳 Containers / Orquestração | Indicação Docker/Portainer | Docker-CE ativo + Portainer container em execução (Portainer expõe UI na porta 9000) |

Explicação (nível não técnico):

"Serviços essenciais estão ativos: SSH para administração remota e Portainer para gestão de containers via interface web (porta 9000). O Docker e seus componentes (containerd) estão rodando. Logs do sistema são coletados pelo `rsyslog`."

**Informações de Softwares e Atualização**

| Categoria | Descrição | Configuração |
|-----------|-----------|--------------|
| 📦 Gerenciador de pacotes | Ferramenta padrão | `apt` (com `unattended-upgrades` ativo) |
| 🏷️ Versões e Identificação | Sistema e kernel | Ubuntu 24.04.4 LTS — Kernel 6.8.0-107-generic |
| 🔄 Atualizações automáticas | Serviço | `unattended-upgrades.service` — ativo |
| ❓ Upgradability check | Resultado disponível nas notas | Comando `app list --upgradable` não presente no sistema (comando inválido); não há lista de pacotes upgradáveis nas anotações fornecidas. |

Explicação (nível não técnico):

"O servidor usa `apt` para gerenciar pacotes e tem atualizações automáticas ativadas (`unattended-upgrades`), o que ajuda a manter o sistema seguro. Não há, nas anotações fornecidas, uma lista de pacotes pendentes de atualização."

**Observações e Recomendações Rápidas**

- **Backups / Snapshots**: Não há informações sobre snapshots ou backups nas anotações — confirmar estratégia de backup.
- **Recursos**: Considerar aumento de memória se for executar múltiplos containers de produção simultaneamente.
- **Segurança**: Revisar regras de firewall e acesso SSH (chaves, portas, fail2ban) já que há acesso remoto habilitado.

**Metadados**

- Arquivo gerado a partir de: `labs/Unbuntu-docker.txt`
- Data da extração: 2026-06-01

