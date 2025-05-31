## HTTP/3: Estrutura, Localização no Modelo TCP/IP e Integração no Linux

Este repositório tem como objetivo apresentar uma introdução técnica ao **HTTP/3**, destacando sua história, localização no modelo TCP/IP e sua correlação com chamadas de sistema operacional no **Linux**.

---

## História e Evolução do HTTP/3 📚

O `HTTP/3` é a terceira grande revisão do protocolo Hypertext Transfer Protocol, que rege a forma como os dados são transmitidos na web. Sua história está profundamente ligada à necessidade de superar limitações dos protocolos anteriores, principalmente o HTTP/1.1 e o HTTP/2.

`HTTP/1.1` (1997): Utiliza TCP (Transmission Control Protocol) como base. Ele é confiável, mas introduz latência devido à necessidade de handshake e ao problema de head-of-line blocking (bloqueio de linha de frente), onde o atraso de um pacote afeta todos os demais na fila.

`HTTP/2` (2015): Introduziu multiplexação, mas ainda usava TCP, mantendo o problema de head-of-line blocking em níveis mais baixos.

`HTTP/3` (finalizado pelo IETF em 2022): Adota o QUIC (Quick UDP Internet Connections), um protocolo desenvolvido pelo Google que roda sobre UDP (User Datagram Protocol), oferecendo melhorias como:

- Redução de latência com conexões mais rápidas.

- Eliminação do head-of-line blocking em nível de transporte.

- Criptografia embutida (com TLS 1.3 integrado diretamente no transporte).

---
## 🌐 O que é HTTP/3?

**HTTP/3** é a terceira geração do protocolo de transferência de hipertexto utilizado na Web. Ele traz mudanças significativas em relação aos seus antecessores (HTTP/1.1 e HTTP/2) ao abandonar o protocolo **TCP** em favor do novo protocolo **QUIC**, que opera sobre **UDP**.

### 🔄 Evolução Histórica

| Versão | Protocolo de Transporte | Características |
|--------|--------------------------|------------------|
| HTTP/1.1 | TCP | Comunicação sequencial, latência alta |
| HTTP/2   | TCP | Multiplexação, mas ainda afetado por *head-of-line blocking* |
| **HTTP/3** | **QUIC (via UDP)** | Multiplexação eficiente, menor latência, criptografia embutida |

---

## 🚀 Por que o QUIC?

O **QUIC** foi desenvolvido para **melhorar o desempenho do HTTP**, especialmente em redes com **alta latência** e **perda de pacotes**. Ele foi projetado como uma alternativa moderna ao TCP, oferecendo diversas vantagens:

### 🔄 Multiplexação
Permite que **várias requisições e respostas** sejam enviadas simultaneamente em uma única conexão, **reduzindo a latência** e acelerando o carregamento das páginas.

### 📶 Controle de Congestionamento Adaptativo
Adapta-se dinamicamente às condições da rede de forma **mais eficiente do que o TCP**, melhorando o desempenho em redes instáveis ou congestionadas.

### 📲 Suporte à Migração de Rede
Permite que uma conexão ativa **migre entre diferentes redes** (como entre Wi-Fi e rede móvel) **sem interrupção**, algo impossível com conexões TCP tradicionais.

### 🔐 Segurança (TLS 1.3)
O QUIC integra **TLS 1.3 nativamente**, garantindo **criptografia moderna e segura** em todas as conexões, desde o início. Simplifica o processo de handshake, reduzindo o tempo necessário para estabelecer uma conexão segura. Isso resulta em menos latência e maior eficiência

### 🌐 Travessia de NAT e Firewalls
Por utilizar **UDP**, o QUIC tem **maior facilidade para atravessar NATs e firewalls**, que frequentemente impõem restrições ao tráfego TCP.

### ⚡ Conexão Mais Rápida
O QUIC pode **estabelecer conexões mais rapidamente** do que o TCP, especialmente em conexões repetidas, graças à eliminação de handshakes redundantes e à reutilização de sessões criptográficas.

---
## Camada no modelo TCP/IP para o QUIC/UDP
![pilha-http2-versus-pilha-http3](https://github.com/user-attachments/assets/59e569c4-098c-4567-aed0-bb74d4099ba3)

HTTP/3 mantém-se na **camada de aplicação**, mas muda o transporte para **QUIC**, que utiliza **UDP** como base.

---

## 🧪 Monitorando Conexões HTTP/3 no Firefox e vendo na prática.
🧰 Verificação com about:networking
- Digite `about:networking` na barra de endereços.
- Vá para a aba `HTTP`.
- Você verá uma lista de conexões ativas. A coluna "Protocol Version" pode mostrar h3 para conexões `HTTP/3`.
![Screenshot 2025-05-28 at 12-43-26 About Networking](https://github.com/user-attachments/assets/ad730445-8f78-45a8-9c1f-b35b9b79045f)

---
## 🧪 Ativando/Desativando HTTP/3 (opcional)

- Digite `about:config` na barra de endereços.
- Procure: `network.http.http3.enabled`

`true`: HTTP/3 está ativado.

`false`: HTTP/3 está desativado.
![Screenshot 2025-05-28 at 12-43-40 Advanced Preferences](https://github.com/user-attachments/assets/c913c5ac-5c4b-42c7-902c-59c62b0deec7)

---
## 🖥️ Monitorando a Conexão através de um Terminal Linux

## Usando tcpdump ou Wireshark (fora do navegador)

- Se quiser monitorar externamente: 
`sudo tcpdump -i any udp port 443`

- QUIC (e, por consequência, HTTP/3) utiliza UDP, normalmente na porta 443. Você verá pacotes QUIC com esse comando.




## Usando `nmap` para mapear o tráfico através do endereço IPv4 de sua rede e escutar através da porta 443 e da porta 80.

- Usando o comando `nmap -p 80,443` + endereço IPv4 da sua rede.

![Screenshot from 2025-05-28 12-04-16](https://github.com/user-attachments/assets/7354ee7e-b039-4aa6-8f1f-9b895aa9f46f)

## Usando `nmap` para fazer um Scan stealth de um HTTP Header ( cabeçalho onde são campos que fazem parte das mensagens HTTP (requisições e respostas) e transmitem informações adicionais sobre a requisição ou resposta, como o tipo de conteúdo, as credenciais de autenticação, e outras configurações ). 

- Usando o comando `nmap -sS -sV -p 80,433 -script http-enum <endereço IPv4>`

![Screenshot from 2025-05-28 12-03-49](https://github.com/user-attachments/assets/6b8602f4-f50c-470e-852f-bde7d2bcea39)







