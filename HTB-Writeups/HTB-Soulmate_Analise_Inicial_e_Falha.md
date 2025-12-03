# 📝 HTB - Soulmate: Análise Inicial e Lições Aprendidas

Este documento registra a metodologia de enumeração inicial e os pontos de falha encontrados durante a tentativa de resolução do desafio/máquina HTB Soulmate. O objetivo é analisar os erros e documentar o caminho correto após a resolução.

## 1. Metodologia de Enumeração Inicial

A primeira fase consistiu na varredura de portas para identificar os serviços expostos pelo alvo.

* **Comando Utilizado:** `nmap -sC -sV <IP_OBJETIVO>`
* **Serviços Identificados:** Porta 22 (SSH) - OpenSSH | Porta 80 (HTTP) - Apache/Nginx.

### Lições do Nmap
A presença da Porta 80 (HTTP) indica que o principal vetor de ataque para a fase de User Flag será a aplicação Web, com o SSH sendo o vetor secundário (geralmente exigindo credenciais ou falhas de configuração).

## 2. Investigação do Serviço Principal (HTTP/Web)

### 2.1. Análise da Aplicação Web
* **Acesso ao Navegador:** A aplicação web inicial [Descrição Genérica: Ex: "Apresenta um formulário de login/registro e uma funcionalidade de busca/busca por ID"].
* **Tentativas de Entrada:** Testes básicos de Injeção SQL (`'` e `OR 1=1`) e XSS (`<script>`) foram aplicados sem sucesso imediato nos formulários visíveis.

### 2.2. Descoberta de Diretórios (Fuzzing)
* **Comando Utilizado:** `gobuster dir -u http://<IP_OBJETIVO> -w <caminho_wordlist>`
* **Resultados:** Foram encontrados diretórios comuns como `/assets` e `/css`, mas nenhum diretório crítico ou painel de administração (`/admin`) foi descoberto com a wordlist inicial.
* **Ponto de Falha / Bloqueio:** O Gobuster não retornou resultados que indicassem um caminho claro de vulnerabilidade ou um painel de administração oculto.

## 3. Análise da Falha (Onde o Progresso Parou)

O progresso foi interrompido na fase de **Análise da Aplicação Web** e **Descoberta de Diretórios**.

**Possíveis Causas da Falha (Para Investigação Futura):**

1.  **Vulnerabilidade Lógica de Negócio:** O ataque exigia a exploração de uma falha de lógica na aplicação (ex: redefinição de senha ou falha de autorização), e não uma injeção de código simples.
2.  **Wordlist Inadequada:** A wordlist utilizada para o GoBuster não era abrangente o suficiente para encontrar o diretório/arquivo oculto crucial para o próximo passo.
3.  **Vulnerabilidade em Porta Não Padrão:** O vetor de ataque pode estar em um serviço rodando em uma porta alta não comum.

---
### Próximo Passo
Após a conclusão do *walkthrough* ou solução correta, esta documentação será atualizada com o comando exato de enumeração (Nmap/Gobuster) que levou ao sucesso, corrigindo o erro metodológico identificado.
