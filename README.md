
# VPN Site-to-Site – Azure ↔ FortiGate 7.4

Este repositório documenta um ambiente de **VPN Site‑to‑Site** entre o **Azure VPN Gateway** e um firewall **FortiGate 7.4**, utilizando IKEv2 e políticas de IPsec alinhadas entre os dois lados.

O objetivo deste laboratório é demonstrar como configurar corretamente ambos os lados (Azure e FortiGate), validar a conexão e realizar testes de conectividade.

As imagens utilizadas estão armazenadas em **vpn/imagens**.

---

## 📡 Arquitetura do Ambiente

A topologia criada inclui:

- **Azure VPN Gateway**
- **FortiGate 7.4** (porta WAN com IP público)
- Rede local no FortiGate: `172.16.10.0/24`
- Rede Azure (remota): `10.0.0.0/24`
- Túnel IPsec IKEv2 com criptografia AES256/SHA256

---

## 🏗 1. Configuração no Azure

### 🔹 1.1 – Conexão do Gateway IPsec

![azure_conexao](vpn/imagens/azure conexao.png)

Status deve aparecer como **Conectado**.

### 🔹 1.2 – Detalhes adicionais da conexão

![azure_conexao2](vpn/imagens/azure conexao2.png)

Aqui é possível visualizar:

- Gateway de rede virtual  
- Gateway de rede local (FortiGate)  
- Dados enviados/recebidos  
- Localização  

---

## ⚙️ 2. Configurações do Azure – Política IPsec/IKE

No Azure, a conexão IPsec está configurada com criptografia personalizada:

![azure_fase1](vpn/imagens/azure fase 1 local network.png)

Parâmetros:

**Fase 1 (IKE):**
- Criptografia: **AES256**
- Integridade: **SHA256**
- Grupo DH: **14**
- IKEv2

**Fase 2 (IPsec):**
- Criptografia: **AES256**
- Autoridade/PRF: **SHA256**
- Grupo PFS: **2048**
- Vida do SA: **27000 segundos**

---

## 🔥 3. Configuração no FortiGate 7.4

### 🔹 3.1 – Configurações Gerais do Túnel

![fortigate_fase1](vpn/imagens/fortigate fase 1.png)

Parâmetros utilizados:

- Gateway remoto: IP público do Azure
- Interface: WAN do FortiGate
- Autenticação: PSK (chave pré‑compartilhada)
- IKEv2

---

### 🔹 3.2 – Configurações da Fase 1

![fortigate_fase1_2](vpn/imagens/fortigate fase 1.2.png)

- Criptografia: **AES256-SHA256**
- DH Group: **14**

---

### 🔹 3.3 – Configurações da Fase 2

![fortigate_fase2](vpn/imagens/fortigate fase 2.png)

Seletores configurados:

- Local: `172.16.10.0/24`
- Remoto: `10.0.0.0/24`

Com Perfect Forward Secrecy habilitado e DH Group 14.

---

## 🔒 4. Túnel Ativo

No painel do FortiGate, o túnel aparece como ativo:

![fortigate_vpn](vpn/imagens/Fortigate vpn.png)

---

## 📡 5. Logs e Monitoramento do Túnel

O status do túnel também pode ser visualizado em:

![fortigate_vpn2](vpn/imagens/Fortigate vpn.2.png)

---

## 🧪 6. Teste de Conectividade

Um teste de ping foi realizado da rede local para a rede remota `10.0.0.0/24`:

![ping_teste](vpn/imagens/maquina local.png)

Resultado:

- Respostas consistentes  
- Latência estável  
- Conectividade validada pelo túnel IPsec  

---

## 🎯 Conclusão

Este laboratório demonstra uma configuração funcional de:

- Azure VPN Gateway  
- FortiGate 7.4  
- Túnel IPsec IKEv2  
- Criptografia alinhada nas fases 1 e 2  
- Testes bem-sucedidos entre redes distintas  

Serve como guia para ambientes de produção, estudos, certificações e práticas profissionais de infraestrutura e segurança.

---


