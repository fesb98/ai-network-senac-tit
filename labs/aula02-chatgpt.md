# Guia Prático — Importação da Máquina Virtual Ubuntu Server 22.04.4 LTS no Oracle VirtualBox 7.2

## Objetivo
Importar a imagem virtual `UbuntuServer-OnPremises.ova` no Oracle VirtualBox 7.2, configurar a rede em **Modo Bridge (Ponte)** utilizando a rede cabeada do laboratório e iniciar a máquina virtual para acesso remoto via SSH.

---

# 🖥️ Ambiente do Laboratório

| Item | Especificação |
|---|---|
| Sistema Operacional | Microsoft Windows 11 |
| Virtualizador | Oracle VirtualBox 7.2 |
| Processador | Intel Core i7-14700K |
| Memória RAM | 32GB |
| Armazenamento | 1TB |
| Rede do Laboratório | 10.24.82.0/24 |
| Arquivo da VM | `UbuntuServer-OnPremises.ova` |

---

# 📥 PASSO 1 — Localizar a Imagem da Máquina Virtual

1. Faça login no computador do laboratório.
2. Abra o **Explorador de Arquivos**.
3. Acesse a pasta:

```powershell
Downloads
```

4. Verifique se o arquivo abaixo está disponível:

```powershell
UbuntuServer-OnPremises.ova
```

---

# 🧩 PASSO 2 — Abrir o Oracle VirtualBox

1. Clique no menu **Iniciar**.
2. Procure por:

```powershell
Oracle VirtualBox
```

3. Abra o programa.

---

# 📦 PASSO 3 — Importar a Máquina Virtual (.OVA)

1. No VirtualBox clique em:

## 📂 Arquivo → Importar Appliance

2. Clique no ícone da pasta 📁.
3. Navegue até:

```powershell
Downloads\UbuntuServer-OnPremises.ova
```

4. Selecione o arquivo `.ova`.
5. Clique em:

```powershell
Avançar
```

---

# ⚙️ PASSO 4 — Revisar as Configurações da Importação

Na tela de importação:

✅ Verifique:
- Nome da VM
- Quantidade de RAM
- Disco virtual
- Processador

⚠️ NÃO altere nenhuma configuração sem orientação do professor.

Clique em:

```powershell
Finalizar
```

⏳ Aguarde a importação da VM.

---

# 🌐 PASSO 5 — Configurar Rede em Modo Bridge (Ponte)

Após finalizar a importação:

1. Selecione a VM:

```powershell
UbuntuServer-OnPremises
```

2. Clique em:

## ⚙️ Configurações

3. Vá até:

## 🌐 Rede

---

## Configuração do Adaptador

### Adaptador 1

Configure exatamente assim:

| Opção | Valor |
|---|---|
| Habilitar Placa de Rede | ✅ Marcado |
| Conectado a | Adaptador em Ponte |
| Nome | Placa de Rede Cabeada do Laboratório |
| Modo Promíscuo | Permitir Tudo |
| Cabo Conectado | ✅ Marcado |

---

# 🔍 PASSO 6 — Identificar a Interface de Rede Correta

No campo:

```powershell
Nome:
```

Selecione a placa de rede cabeada.

Exemplos comuns:

- Intel Ethernet
- Realtek PCIe
- Ethernet
- Intel(R) Ethernet Controller

⚠️ NÃO selecionar:
- Wi-Fi
- Bluetooth
- VPN

---

# 💾 PASSO 7 — Salvar Configurações

Clique em:

```powershell
OK
```

---

# ▶️ PASSO 8 — Iniciar a Máquina Virtual

1. Selecione a VM.
2. Clique em:

## ▶️ Iniciar

⏳ Aguarde o Ubuntu Server inicializar.

---

# 🔑 PASSO 9 — Fazer Login no Ubuntu Server

Na tela do terminal:

Digite o usuário e senha fornecidos pelo professor.

Exemplo:

```bash
login: suporte
password: ********
```

---

# 🌍 PASSO 10 — Verificar o Endereço IP da Máquina Virtual

Após login, execute:

```bash
ip addr
```

ou

```bash
hostname -I
```

---

## Exemplo de Resultado

```bash
10.24.82.150
```

⚠️ O IP deve estar na rede do laboratório:

```powershell
10.24.82.0/24
```

---

# 🔐 PASSO 11 — Testar o Serviço SSH

Verifique se o SSH está ativo:

```bash
sudo systemctl status ssh
```

Resultado esperado:

```bash
active (running)
```

---

# 💻 PASSO 12 — Acessar a VM via SSH

No Windows 11:

1. Abra o:

## 💻 PowerShell

2. Execute:

```powershell
ssh usuario@IP_DA_VM
```

---

## Exemplo

```powershell
ssh suporte@10.24.82.150
```

3. Digite:

```powershell
yes
```

4. Informe a senha do usuário.

---

# ✅ PASSO 13 — Verificar Acesso Remoto

Se tudo estiver correto, aparecerá algo semelhante:

```bash
suporte@ubuntu:~$
```

✅ SSH funcionando com sucesso.

---

# 🧠 Resumo Final

| Etapa | Status |
|---|---|
| Importar arquivo OVA | ✅ |
| Configurar rede Bridge | ✅ |
| Iniciar VM | ✅ |
| Obter IP da rede local | ✅ |
| Validar SSH | ✅ |
| Acesso remoto funcionando | ✅ |

---

# 📌 Observações Importantes

- Sempre utilizar a placa de rede cabeada do laboratório.
- O modo Bridge permite que a VM receba IP diretamente da rede do SENAC.
- Caso não receba IP:
  - Reinicie a VM
  - Verifique o cabo conectado
  - Confirme o adaptador correto no VirtualBox
- Nunca alterar configurações avançadas sem autorização do professor.

---

# 🚀 Ambiente Preparado para as Aulas Práticas de IA

A máquina virtual Ubuntu Server está pronta para utilização nas atividades práticas de:
- Inteligência Artificial
- Redes de Computadores
- Automação
- DevOps
- Infraestrutura Linux
- Serviços de Rede
