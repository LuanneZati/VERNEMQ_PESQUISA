# Pesquisa MQTT Broker VerneMQ

## **1. OBJETIVOS**

Esta pesquisa tem como objetivo apresentar, pesquisar e avaliar qual o melhor broker em questão de eficiência, segurança e implementação utilizando a plataforma esp32.

---

### **1. Apresentação da pesquisa**

- 1.1 Benefícios
- 1.2 Segurança
- 1.3 Eficiência

### **2. Implementação**

- 2.1 Vernemq na EC2
  - 2.1.1 Download
  - 2.1.2 Instalação
  - 2.1.3 Ativar nó
- 2.2 Configurações

### **3. Vernemq X Mosquitto**

- 3.1 Qual se sobresaiu nas pesquisas?

### **4. Conclusão**

### **5. Referências**

---

### **1. APRESENTAÇÃO DA APLICAÇÃO**

VerneMQ é um intermediário de mensagens MQTT distribuído de alto desempenho. Ele é dimensionado horizontal e verticalmente em hardware comum para oferecer suporte a um grande número de editores e consumidores simultâneos, mantendo baixa latência e tolerância a falhas. VerneMQ é o hub de mensagens confiável para sua plataforma IoT ou produtos inteligentes.

## **1.1 QUAIS OS BENEFÍCIOS DO VERNEMQ?**

O VerneMQ é um Broker MQTT feito em Erlang/OTP, o que o torna altamente escalável. Pode ser escalado horizontalmente e verticalmente em hardware comum para oferecer suporte a um grande número de editores e consumidores simultâneos, mantendo baixa latência e tolerância a falhas.
Possui suporte para monitoramento, administração, alteração de autenticação e suporte para vários bancos. É possível configurá-lo em webhook, ou seja, você pode fazer com que os eventos que chegam no broker sejam encaminhados para um endpoint.

VerneMQ implementa as especificações MQTT 3.1, 3.1.1 e 5.0. Atualmente os seguintes recursos são implementados e entregues como parte do VerneMQ:

QoS 0, QoS 1, QoS 2
Autenticação e autorização básicas
Suporte de ponte
Árvore $SYS para monitoramento e relatórios
Criptografia TLS (SSL)
Suporte a Websockets
Suporte a clusters
Registro (Console, Arquivos, Syslog)
Reporte ao Grafite
Arquitetura de plug-in extensível
Várias sessões por ClientId
Balanceamento de sessão
Assinaturas compartilhadas
Regulamento de carregamento de mensagens
Descarte de carga de mensagem (para proteção do sistema)
Armazenamento de mensagens offline (baseado em LevelDB)
A fila pode manipular mensagens no estilo FIFO ou LIFO.
Autenticação e integração do MongoDB
Autenticação e integração do Redis
Autenticação e integração do MySQL
Autenticação e integração do PostgreSQL
Autenticação e integração do CockroachDB
Integração com Memcached
Webhooks HTTP
Protocolo PROXY v2
API HTTP de administração
Rastreamento de sessão MQTT em tempo real
Multilocação completa
Página da web de status do cluster
Os recursos a seguir também se aplicam a clientes MQTT 5.0:

Esquemas de autenticação avançada (AUTH)
Expiração da mensagem
Última vontade e atraso do testamento
Assinaturas compartilhadas
Fluxo de solicitação/resposta
Alias ​​de tópicos
Controle de fluxo
Sinalizadores de assinatura (Reter como publicado, Não local, Reter tratamento)
Identificadores de assinante
Todos os tipos de propriedade são suportados: propriedades do usuário, strings de razão, tipos de conteúdo etc.

## **1.2 SEGURANÇA**

Seu protocolo de segurança é bem completo e tudo pode ser configurado no seu documento de configuração. O broker ofereçe o ambiente íntegro de segurança : Usuário e senha, ACL, criptografia e validação de certificados. Pode-se configurar endereços IP padrão e portas.

## **1.3 EFICIÊNCIA**

O VerneMQ é um broker bem eficiente, na fase de implementação pode se constatar que as mensagens são enviadas instantaneamente e apenas no tópico em que o usuário é configurado para publicar, esse é um padrão de segurança importante e eficiente.

## **2. IMPLEMENTAÇÃO**

Esta sessão é destinada a descrever o processo de implementação do Mqtt Broker, pode ser instalado nos sistemas operacionais: Linux (Amazon Linux 2 - Ubuntu - Debian - redhat - opensuse) e Mac OS, não é suportado em windows.

## **2.1 VERNEMQ NA EC2**

Primeiro passo é ter um sistema operacional citado acima, essa aplicação será feita dentro de uma EC2 na AWS.

### **2.1.1 Download**

Primeiro baixe o pacote binario, escontado na [Vernemq Downloads](https://vernemq.com/downloads/)

Terminal:

```js
sudo wget <Link download sua versão>
```

### **2.1.2 Instalação**

Para a instalação

```js
sudo dpkg -i vernemq-<VERSION>.bionic.x86_64.deb
```

Verifique sua instalação:

```js
dpkg -s vernemq | grep Status
```

Se foi instalado com sucesso: install ok installed é retornado.

### **2.1.3 Ativando o Nó**

Para ativar o nó:

```js
service vernemq start
```

## **2.2 CONFIGURAÇÃO VERNEMQ**

No documento de configuração, geralmente localizado em: `/etc/vernemq/vernemq.conf`
Ele funciona como 'chave = valor'

Antes de tudo é necessário aceitar a eula: `sudo vernemq chkconfig`

O vernemq foi configurado com valores:

`allow_anonymous = on`

`listener.http.default = ip_maquina:8888`

`listener.tcp.default = ip_maquina:1883`

`plugins.vmq_passwd = on`

`plugins.vmq_acl = on`

Para adicionar usuário e senha de acesso:

`vmq-passwd -c /etc/vernemq/vmq.passwd name`

Para definir quais tópicos são permitidos para cada usuário acesse `/etc/vernemq/vmq.acl`

```js
# ACL for user 'name'
user name
topic foo
topic read baz
topic write open_to_all
```

Ou seja, name pode se inscrever e publicar no tópico foo, se inscrever no tópico baz e publicar no tópico open_to_all

## **3 VERNEMQ X MOSQUITTO**

### **3.1 Qual se sobresaiu nas pesquisas?**

Uma comparação feita pelo site [Stack Share](https://stackshare.io/stackups/mosquitto-vs-vernemq), mostra algumas diferença em relação ao Mosquitto MQTT e o VerneMQ.
Os desenvolvedores descrevem o Mosquitto como " Um corretor de mensagens de código aberto que implementa o protocolo MQTT ". É leve e adequado para uso em todos os dispositivos, desde computadores de placa única de baixo consumo até servidores completos. O protocolo MQTT fornece um método leve de realizar mensagens usando um modelo de publicação/assinatura. Isso o torna adequado para mensagens da Internet das Coisas, como sensores de baixa potência ou dispositivos móveis, como telefones, computadores incorporados ou microcontroladores. Por outro lado, VerneMQ é detalhado como " VerneMQ é um intermediário de mensagens IoT/MQTT distribuído". VerneMQ é implementado em Erlang/OTP É de código aberto e licenciado em Apache 2. O VerneMQ implementa as especificações MQTT 3.1, 3.1.1 e 5.0.

Mosquitto e VerneMQ podem ser classificados principalmente como ferramentas "Message Queue" .

"Depois de verificar esses e outros corretores que ficaram de fora da corrida ainda mais cedo, optamos pelo VerneMQ.
VerneMQ é uma ferramenta de código aberto com 1,76K estrelas do GitHub e 189 forks do GitHub."

| Prós do Mosquitto | Prós do VerneMQ                                       |
| ----------------- | ----------------------------------------------------- |
| Simples e leve    | Clustering de código totalmente aberto                |
| Atuação           | Suporte ao protocolo proxy                            |
|                   | Sistema de plug-ins de código aberto                  |
|                   | Mensagem de código aberto e persistência de metadados |
|                   | Implementação do MQTT v5                              |
|                   | Assinaturas compartilhadas de código aberto           |
|                   | Fácil de configurar                                   |
|                   | Ótimo suporte a JSON                                  |

No site, também consta que as pessoas usam mais o broker Mosquitto do que o VerneMQ, isso se da pelo fato de que o mosquitto é um corretor leve e normalmente quem entra no mundo IoT tem como sua primeira opção o Mosquitto, mas isso não significa que ele seja o melhor, até porque foi constatado a eficiência e outros inúmeros recursos que o VerneMQ oferece.

Em um experimento feito pelo site [AutSoft](https://autsoft.net/choosing-an-mqtt-broker-for-your-iot-project/) foi relatado que "O VerneMQ pode ser facilmente estendido com scripts Lua, possui uma documentação relativamente boa e uma equipe de desenvolvedores muito útil no GitHub . Executamos alguns testes de desempenho nele e ele pode facilmente lidar com 400 clientes/s de entrada enviando 3.000 mensagens/s em um servidor de 2 núcleos de 2 GHz com 8 gigabytes de ram. No nível 1 de QoS no lado do consumidor e do produtor."
Já com o Mosquitto, "Um problema interessante que tivemos com ele quando começamos a carregar o teste é que acima de 10 conexões de cliente/s por TLS, as tentativas de conexão começaram a falhar devido a alguns erroscom o tratamento de conexões SSL usando OpenSSL."

## **5. CONCLUSÃO**

O objetivo da pesquisa foi atingido. Os resultados foram positivos em relação aos brokers e percebe-se benefícios e malefícios de cada um.
O VerneMQ é um broker mais robusto, melhor adequado para projetos empresariais e mais elaborados. Possui melhores configurações e uma capacidade maior para suportar dispositivos conectados.
Já o Mosquitto, é também um broker bom, porém, na escolha do ideal é necessário levar em consideração aspectos como, "Seu sistema é de um nível grande?", "Seu broker precisa suportar várias conexões ao mesmo tempo?", "Você precisará de um broker com clusetring?", se as respostas foram 'sim', o mosquitto não seria o Broker ideal.

A pesquisa sobre Mqtt Broker foi finalizada com sucesso, e para esta aplicação, o VerneMQ foi o mais indicado.

## 3.1 REFERÊNCIAS

Github: <https://github.com/vernemq/vernemq>  
Thought works: <https://www.thoughtworks.com/pt-br/radar/platforms/vernemq>  
Stack share: <https://stackshare.io/stackups/mosquitto-vs-vernemq>  
AutSoft: <https://autsoft.net/choosing-an-mqtt-broker-for-your-iot-project/>  
VerneMQ: <https://docs.vernemq.com/>
