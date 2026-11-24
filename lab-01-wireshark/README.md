# Lab 01 — Análise de Tráfego HTTP sobre IPv6 (Wireshark)

## 1. Objetivo
Realizar a análise inicial de tráfego de rede (nível SOC N1) no arquivo de captura `v6-http.cap` para identificar os equipamentos envolvidos, filtrar requisições web e ler os cabeçalhos da troca de mensagens entre cliente e servidor.

## 2. Ferramentas e Ambiente
- **Arquivo analisado:** `v6-http.cap` (captura de exemplo IPv6 / SixXS)
- **Ferramenta:** Wireshark
- **Referência de horário:** UTC (`2007-08-05 19:16:44 UTC`)

## 3. Passo a Passo e Descobertas
1. **Identificação dos equipamentos (`Statistics > Conversations > IPv6`):**
   - **Cliente (quem pediu a página):** `2001:6f8:102d:0:2d0:9ff:fee3:e8de`
   - **Servidor (quem respondeu):** `2001:6f8:900:7c0::2`
2. **Filtro de requisições web (`http.request`):**
   - Localizado o **Pacote 49**, realizando um pedido `GET / HTTP/1.0` (solicitação da página inicial).
3. **Leitura da conversa completa (`Follow > HTTP Stream`):**
   - **Site acessado (`Host`):** `cl-1985.ham-01.de.sixxs.net`
   - **Programa usado (`User-Agent`):** `Lynx/2.8.6rel.2 libwww-FM/2.14 SSL-MM/1.4.1 OpenSSL/0.9.8b` (navegador em modo texto usado via terminal)
   - **Resposta do servidor:** `HTTP/1.1 200 OK` (conexão bem-sucedida, servidor `Apache`, entregando uma página `text/html` de `2121` bytes)

## 4. Visão de Triagem SOC N1
- **O que isso significa:** Houve uma requisição web comum (`GET`) via protocolo HTTP (sem criptografia) feita por um navegador em modo texto (`Lynx`), que recebeu resposta positiva (`200 OK`) do servidor.
- **Conduta N1:** Em um ambiente corporativo real, tráfego HTTP em texto claro e `User-Agent` de ferramentas de terminal merecem verificação rápida para confirmar se o computador de origem pertence à equipe autorizada e se o acesso faz parte da rotina esperada.

## 5. Evidências
- `01-filtro-http.png` — Tela do Wireshark com o filtro `http.request` aplicado.
- `02-http-stream.png` — Janela do *Follow HTTP Stream* com a requisição e a resposta do servidor.