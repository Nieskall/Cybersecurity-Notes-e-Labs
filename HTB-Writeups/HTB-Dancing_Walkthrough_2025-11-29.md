# 📝 Hack The Box - Dancing (Resumo Completo e Write-up)

Este documento detalha o processo de enumeração e exploração de serviços (SMB e Redis) e as lições aprendidas em teoria de rede e web, culminando na obtenção da flag do desafio.

## 1. Fundamentos de Rede (FTP e HTTP/S)

* **SFTP vs. FTP:** SFTP (SSH File Transfer Protocol) é o acrônimo para o protocolo seguro que usa SSH, enquanto FTP transfere dados em texto claro (sem criptografia).
* **Login Anônimo (FTP):** O nome de usuário para login sem conta é **anonymous**.
* **Ajuda do Cliente FTP:** Use `ftp -h` para ver o resumo de uso.
* **Porta Padrão HTTPS:** A porta padrão para o protocolo HTTPS (seguro) é a **443**.

## 2. Enumeração e Exploração do Serviço SMB (Server Message Block)

### 2.1. Descoberta de Compartilhamentos
* **Comando:** `smbclient -L //IP_do_Servidor`
* **Lição Crucial:** O switch de listagem é **-L** (maiúsculo). O erro `NT_STATUS_NOT_FOUND` foi resolvido usando o **endereço IP** do servidor (`//10.129.1.12`) em vez do nome do host (`Dancing`).
* **Compartilhamentos Listados (4):** ADMIN$, C$, IPC$, e **WorkShares**.

### 2.2. Acesso e Navegação
* **Comando:** `smbclient //10.129.1.12/WorkShares`
* **Comandos Internos:** `ls` ou `dir` para listar conteúdo; `cd <diretório>` para navegar; `cd ..` para voltar um diretório.
* **Objetivo:** Explorar os diretórios de usuários (ex: `Amy.J`, `James.P`) para encontrar a flag.

## 3. Exploração e Sucesso no Serviço Redis

### 3.1. Conexão e Contexto
* **Comando de Conexão:** `redis-cli -h IP_do_Servidor`
* **Lição de Rede:** O erro `No route to host` é um problema de conectividade de rede (ping, firewall), não do `redis-cli`.
* **Comando Essencial:** `SELECT <número>` (Ex: `SELECT 0`)
    * **Lição Crucial (Sucesso da Flag):** O erro foi tentar comandos de extração (como `GET flag`) no **DB 2** ou outro DB, enquanto as chaves (incluindo `flag`) estavam no **DB 0**. Mudar o contexto para o DB correto é vital.

### 3.2. Extração de Chaves
* **Listar Chaves:** `KEYS *` (Encontradas 4 chaves no DB 0: `numb`, `flag`, `temp`, `stor`).
* **Verificar Tipo:** `TYPE <chave>` (Essencial para usar o comando de extração correto, ex: `GET` para String, `HGETALL` para Hash).
* **Recuperação da Flag:** Após o `SELECT 0`, a flag foi recuperada com sucesso usando o comando de extração apropriado ao tipo de dado da chave.

## 4. Enumeração Web e Teoria de Vulnerabilidades

* **Nmap (Varredura de Serviços):** O comando `sudo nmap -sV -p 80 <IP>` identificou o serviço **Apache httpd 2.4.38 ((Debian))** na porta 80.
* **Acesso HTTP pelo Terminal:** O comando `curl` (Client URL) é usado para fazer requisições HTTP (`curl http://exemplo.com`).
* **Terminologia Web:** Uma 'pasta' em terminologia de web-application é chamada **Directory** (Diretório).
* **Erro HTTP "Not Found":** O código de resposta é **404**.
* **Gobuster (Brute Force de Diretórios):** O switch usado para especificar a busca por diretórios é **dir**.
* **Comentário MySQL:** O caractere único para comentar o restante de uma linha em MySQL é **#**.
* **Vulnerabilidade (OWASP):** A classificação do OWASP Top 10 de 2021 para **SQL Injection** é **A03:2021 – Injection** (Nota: O desafio HTB pode esperar a resposta de 2017: **A1:2017 – Injection** devido a sistemas de correção desatualizados).
