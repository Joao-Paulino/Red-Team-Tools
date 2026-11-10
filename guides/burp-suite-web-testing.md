# Teste de Segurança em Aplicações Web com Burp Suite / Web Application Security Testing with Burp Suite

> **AVISO / WARNING**
> Executar apenas em laboratório isolado e com autorização. O uso em sistemas de terceiros sem permissão é crime.
> Run only in an isolated lab with authorization. Using it on third-party systems without permission is a crime.

## PORTUGUÊS

### 1. Objetivo
Utilizar o Burp Suite para intercetar, analisar e manipular tráfego HTTP/S entre o navegador e uma aplicação web de laboratório, explorando os módulos Proxy, Intruder, Repeater, Decoder e Comparer.

### 2. Ferramentas
- Burp Suite Community Edition (versão atual 2026.x)
- Navegador com extensão de proxy (FoxyProxy ou configuração manual)
- Aplicação web vulnerável de laboratório (DVWA, OWASP Juice Shop ou similar)
- Java Runtime Environment (JRE) 17 ou superior

### 3. Introdução
O Burp Suite é uma plataforma integrada de teste de segurança em aplicações web, desenvolvida em Java, que funciona como um proxy de interceção. Permite inspecionar e alterar pedidos e respostas HTTP/S, automatizar testes e analisar vulnerabilidades. A versão Community é gratuita, mas o Intruder tem limitação de velocidade e o Scanner não está disponível.

### 4. Instalação
1. Descarregar o instalador da versão Community em: https://portswigger.net/burp/communitydownload
2. Instalar o JRE 17 ou superior.
3. Executar o instalador e seguir as instruções.
4. Na primeira execução, aceitar os termos e criar um projeto temporário.

### 5. Configuração do proxy
1. No Burp, ir a Proxy > Options e confirmar que o listener está ativo em 127.0.0.1:8080.
2. No navegador, configurar o proxy manual para 127.0.0.1 porta 8080 (ou usar FoxyProxy).
3. Aceder a http://burp no navegador e descarregar o certificado CA do Burp.
4. Importar o certificado CA no navegador como autoridade de confiança.
5. Confirmar que o tráfego HTTPS é intercetado sem erros.

### 6. Módulos principais
- Proxy: interceta pedidos e respostas entre navegador e servidor.
- Spider: enumera conteúdo e funcionalidades da aplicação.
- Scanner: disponível apenas na versão Professional.
- Intruder: automatiza pedidos com payloads configuráveis (limitado na versão Community).
- Repeater: manipula manualmente pedidos e analisa respostas.
- Sequencer: analisa a qualidade de tokens e dados imprevisíveis.
- Decoder: traduz e codifica dados em vários formatos.
- Comparer: compara dois fluxos de dados (pedidos ou respostas).

### 7. Tarefas

**Tarefa 1 — Interceção com Proxy**
1. Ativar a interceção no Burp (Proxy > Intercept is on).
2. No navegador, aceder à aplicação de laboratório.
3. Observar o pedido intercetado e usar Forward para continuar.
4. Identificar cabeçalhos, cookies e parâmetros.

**Tarefa 2 — Análise com Repeater**
1. Com um pedido de login intercetado, clicar com o botão direito e escolher Send to Repeater.
2. No Repeater, alterar parâmetros e enviar.
3. Comparar respostas para diferentes entradas.
4. Registar diferenças de comportamento.

**Tarefa 3 — Teste de dicionário com Intruder**
1. Enviar um pedido de login para o Intruder (Send to Intruder).
2. Na aba Positions, limpar seleções e marcar apenas o campo da palavra-passe.
3. Escolher o tipo de ataque Cluster bomb.
4. Na aba Payloads, carregar uma lista de palavras-passe de laboratório.
5. Iniciar o ataque e analisar os resultados (tamanho e código de resposta).
6. Nota: na versão Community, o Intruder tem limitação de velocidade; usar listas pequenas.

**Tarefa 4 — Decodificação e comparação**
1. Usar Decoder para descodificar valores Base64 ou URL.
2. Usar Comparer para comparar duas respostas e identificar diferenças.

**Tarefa 5 — Relatório**
1. Documentar os achados de cada tarefa.
2. Identificar riscos e propor medidas de mitigação.

### 8. Medidas de mitigação
- Utilizar HTTPS com certificados válidos e HSTS.
- Implementar limitação de tentativas de login e bloqueio temporário.
- Usar tokens CSRF e cookies com flags Secure e HttpOnly.
- Validar e sanitizar todas as entradas no servidor.
- Monitorizar e registar tentativas de autenticação falhadas.

### 9. Referências
- https://portswigger.net/burp/communitydownload
- https://portswigger.net/burp/documentation
- https://portswigger.net/web-security
- https://owasp.org/www-project-juice-shop/
- https://dvwa.co.uk/

---

## ENGLISH

### 1. Objectioe
Use Burp Suite to intercept, analyze, and manipulate HTTP/S traffic between a browser and a lab web application, exploring the Proxy, Intruder, Repeater, Decoder, and Comparer modules.

### 2. Tools
- Burp Suite Community Edition (current version 2026.x)
- Browser with proxy extension (FoxyProxy or manual configuration)
- Vulnerable lab web application (DVWA, OWASP Juice Shop, or similar)
- Java Runtime Environment (JRE) 17 or later

### 3. Introduction
Burp Suite is an integrated web application security testing platform, developed in Java, that acts as an intercepting proxy. It allows inspecting and modifying HTTP/S requests and responses, automating tests, and analyzing vulnerabilities. The Community edition is free, but Intruder is rate-limited and Scanner is not available.

### 4. Installation
1. Download the Community installer from: https://portswigger.net/burp/communitydownload
2. Install JRE 17 or later.
3. Run the installer and follow the instructions.
4. On first launch, accept the terms and create a temporary project.

### 5. Proxy configuration
1. In Burp, go to Proxy > Options and confirm the listener is active on 127.0.0.1:8080.
2. In the browser, set the manual proxy to 127.0.0.1 port 8080 (or use FoxyProxy).
3. Visit http://burp in the browser and download the Burp CA certificate.
4. Import the CA certificate into the browser as a trusted authority.
5. Confirm HTTPS traffic is intercepted without errors.

### 6. Main modules
- Proxy: intercepts requests and responses between browser and server.
- Spider: enumerates application content and functionality.
- Scanner: available only in the Professional edition.
- Intruder: automates requests with configurable payloads (rate-limited in Community).
- Repeater: manually manipulates requests and analyzes responses.
- Sequencer: analyzes the quality of tokens and unpredictable data.
- Decoder: translates and encodes data in various formats.
- Comparer: compares two data streams (requests or responses).

### 7. Tasks

**Task 1 — Proxy interception**
1. Enable interception in Burp (Proxy > Intercept is on).
2. In the browser, access the lab application.
3. Observe the intercepted request and use Forward to continue.
4. Identify headers, cookies, and parameters.

**Task 2 — Repeater analysis**
1. With an intercepted login request, right-click and choose Send to Repeater.
2. In Repeater, modify parameters and send.
3. Compare responses for different inputs.
4. Record behavioral differences.

**Task 3 — Dictionary test with Intruder**
1. Send a login request to Intruder (Send to Intruder).
2. In the Positions tab, clear selections and mark only the password field.
3. Choose the Cluster bomb attack type.
4. In the Payloads tab, load a lab password list.
5. Start the attack and analyze results (response length and status code).
6. Note: in the Community edition, Intruder is rate-limited; use small lists.

**Task 4 — Decoding and comparison**
1. Use Decoder to decode Base64 or URL values.
2. Use Comparer to compare two responses and identify differences.

**Task 5 — Report**
1. Document findings from each task.
2. Identify risks and propose mitigation measures.

### 8. Mitigation measures
- Use HTTPS with valid certificates and HSTS.
- Implement login attempt rate limiting and temporary lockout.
- Use CSRF tokens and cookies with Secure and HttpOnly flags.
- Validate and sanitize all inputs on the server side.
- Monitor and log failed authentication attempts.

### 9. References
- https://portswigger.net/burp/communitydownload
- https://portswigger.net/burp/documentation
- https://portswigger.net/web-security
- https://owasp.org/www-project-juice-shop/
- https://dvwa.co.uk/
