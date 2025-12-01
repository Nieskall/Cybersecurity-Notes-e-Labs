# 📝 HTB - Resumo Atualizado da Aula de Enumeração e Exploração (27/11/2025)

## 1. Enumeração de Protocolos de Rede (FTP/SFTP/SMB)

Este ponto de partida estabeleceu os fundamentos dos protocolos de transferência de arquivos.

* **FTP (File Transfer Protocol):** Transfere dados em **texto claro** (sem criptografia).
* **SFTP (SSH File Transfer Protocol):** Protocolo moderno que usa o **SSH** para fornecer as mesmas funcionalidades do FTP, mas de forma **segura e criptografada**.
* **Login Anônimo (FTP):** O nome de usuário para login sem uma conta específica é `anonymous`.
* **Ajuda do Cliente FTP:** Use `ftp -h` para ver o resumo de uso ou `man ftp` para ver o manual completo.

### Exploração de SMB (smbclient)

* **Listar Compartilhamentos:** Use `smbclient -L //IP_do_Servidor`.
    * **Lição:** O `switch` para listagem é `-L` (maiúsculo). Use o **IP** (Ex: `//10.129.1.12`) se o nome do host não for resolvido, evitando o erro `NT_STATUS_NOT_FOUND`.
* **Acessar Compartilhamento:** Use `smbclient //IP_do_Servidor/Nome_do_Share`.
* **Comandos Internos (smbclient shell):**
    * `ls` ou `dir`: Lista o conteúdo.
    * `cd <diretório>`: Navega para um subdiretório.
    * `cd ..`: Volta para o diretório pai.

## 2. Exploração e Sucesso no Serviço Redis (redis-cli)

O objetivo era conectar, navegar entre bancos de dados (DBs) e extrair o conteúdo da flag.

* **Conexão Remota:** Use `redis-cli -h IP_do_Servidor`.
    * **Lição:** A flag `-h` (host) é necessária. O erro `No route to host` indica um problema de rede/firewall, não do Redis.
* **Mudar de Banco de Dados:** Use `SELECT <número>` (Exemplo: `SELECT 0`).
    * **Lição Crucial (Resolvida):** O erro estava em tentar o comando `GET flag` no **DB 2**, onde a chave não existia, após ter listado as chaves no **DB 0**. A **atenção ao DB atual** (`SELECT`) é fundamental.
* **Listar Todas as Chaves no DB Atual:** Use `KEYS *`.
    * **Resultado:** Encontradas 4 chaves no DB 0: `numb`, `flag`, `temp`, `stor`.
* **Verificar o Tipo de Dado de uma Chave:** Use `TYPE <nome_da_chave>`.
    * **Função:** Essencial para saber como extrair o conteúdo.
* **Recuperar o Conteúdo:** A flag foi extraída com sucesso após voltar ao DB 0 e usar o comando de extração apropriado.

## 3. Lições Finais e Metodologia

* **Prioridade ao IP:** Sempre use o endereço IP quando o nome do host não for resolvido.
* **Sintaxe Precisa:** Comandos como `SELECT` e `KEYS` devem seguir a sintaxe exata do Redis (números para DBs, maiúsculas para comandos).
* **Atenção ao Contexto:** No Redis, sempre verifique o seu contexto (DB atual) antes de concluir que uma chave não existe.
* **Documentação:** Manter um registro de progresso e comandos (`cd ..`, `SELECT 0`, etc.) é essencial para revisar e encontrar erros de lógica e sintaxe.
