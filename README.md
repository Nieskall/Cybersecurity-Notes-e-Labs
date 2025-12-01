# 🛡️ Security Write-ups e Base de Conhecimento

Este repositório contém uma coleção de documentações técnicas, resumos de laboratórios (Hack The Box, CTFs) e notas metodológicas sobre enumeração, exploração e hardening de sistemas.

O objetivo é registrar o aprendizado sobre a postura de segurança e as vulnerabilidades de protocolos e serviços comuns em ambientes de rede.

---

## Conteúdo Principal

### 1. Laboratórios e Write-ups
* **HTB - Enumeração e Exploração (27-11-2025):** Análise detalhada da enumeração de protocolos de rede (FTP, SMB) e exploração de serviços (Redis).
    * [Link para o arquivo de resumo completo](./HTB-Writeups/HTB_27-11-2025_Exploracao.md)

### 2. Base de Conhecimento (Knowledge Base)
* **Protocolos de Rede:** Notas sobre as diferenças e vulnerabilidades de FTP vs. SFTP, e o uso de smbclient.
* **Serviços e Exploits:** Guia de comandos e lições aprendidas na interação com o redis-cli.

### 3. Metodologia
* **Lições Essenciais:** Foco na prioridade do IP, sintaxe precisa e atenção ao contexto (DBs no Redis) para a resolução de problemas.

---

## Metodologia e Lições Chave (Destaque)

As seguintes lições são cruciais para a análise de sistemas e infraestrutura:

* **Prioridade ao IP:** Sempre use o endereço IP quando o nome do host não for resolvido.
* **Atenção ao Contexto:** No Redis, sempre verifique o seu contexto (DB atual - `SELECT <número>`) antes de concluir que uma chave não existe.
* **Documentação é Essencial:** Manter um registro de progresso e comandos é vital para revisar e encontrar erros de lógica e sintaxe.
