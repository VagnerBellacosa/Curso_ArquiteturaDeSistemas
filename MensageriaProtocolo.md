# Mensageria e Protocolos

- “Mensageria é um conceito que define que sistemas distribuídos, possam se comunicar por meio de troca de mensagens (evento), sendo estas mensagens “gerenciadas” por um Message Broker (servidor/módulo de mensagens).”

## O que é um Message Broker?

Imagine que na organização na qual você trabalha, surge a necessidade de realizar uma integração com algum sistema parceiro que agrega valor ao seu negócio. Parece ser algo simples e você logo pensa:

- “Bom só preciso me preocupar, em desenvolver uma comunicação com a Web API do sistema que desejo integrar e está tudo certo!”

De certa forma, é basicamente isso! Porém, vamos analisar alguns pontos importantes no cenário apresentado, mas antes vamos alinhar alguns detalhes para deixar claro o cenário de exemplo aqui apresentado:

Você precisa integrar dados de pessoas (Pessoa Física e Jurídica), qualquer ação que envolva a modificação destas entidades, são chamadas de eventos, sendo assim podemos afirmar que um evento é uma mensagem.

O sistema parceiro já possui uma Web API Rest pública pronta para ser consumida.

Seu sistema não pode ser impactado, caso o sistema parceiro esteja indisponível.

Vamos voltar para análise do problema cenário…

Como devo tratar os eventos gerados na minha aplicação e realizar a integração com o sistema parceiro?

Em um primeiro momento, você deve imaginar algo parecido com:

- “Ao realizar a persistência do meu registro, faço uma requisição na API Rest do sistema parceiro enviando o meu dado a ser integrado! E já era!”
Ao começar desenvolver você começa a se perguntar:“…e se ao fazer a requisição na API Rest do sistema parceiro ela estiver indisponível? O que devo fazer?”

Bom, você pode fazer várias coisas, ifs que analisam o HTTP status code (não é uma boa), usar algum framework de retry ou até quem sabe registrar em banco de dados SQL que o registro não foi integrado e posteriormente por meio de um serviço de backend, realizar uma nova tentativa. Há várias maneiras, mencionei algumas mais comuns que já vi acontecer nas trincheiras.

Porém, há uma outra alternativa que seria por meio de mensageria. Basicamente, ao realizar alguma inclusão/alteração do registro e garantir que isso de fato foi persistido, um evento (mensagem) seria publicado em um Message Broker. Voltamos para a pergunta:

## Que raios é um Message Broker?

Vamos lá, chegou a hora de saber o que esse cara faz!
Um Message Broker nada mais é que um servidor de mensagens, responsável por garantir que a mensagem seja enfileirada e armazenada em disco (opcional), garantindo que ela fique lá enquanto necessário até que alguém (consumidor) a retire de lá.

Em outras palavras, é como se fosse uma caixa de correio, e as mensagens são as cartas que serão depositadas(publicadas) ali e retiradas(consumidas) por alguém que tenha interesse em ler essas cartas.

Pra ficar mais claro, de uma olhada na imagem abaixo, ela representa a comunicação via mensagem ponto a ponto entre a Aplicação A e a Aplicação B, sendo a Aplicação B responsável em obter a mensagem enfileirada no Message Broker e enviar para a API Rest do sistema parceiro.

Toda a lógica de comunicação com a API Rest, fica na Aplicação B desta forma não há um acoplamento direto entre Aplicação A e B.

## Representação de uma comunicação ponto a ponto por meio de mensagem.

- **Event:** um evento é a mensagem em si. Pode ser um JSON, XML ou qualquer tipo de formato em bytes.
- **Producer:** é a aplicação que envia uma mensagem para uma queue do Message Broker.
- **Queue:** é uma fila que recebe as mensagens geradas por um producer. As mensagens ficarão dentro da fila até que alguma aplicação consumidora (consumer) retire a mensagem da fila.
- **Consumer:** é a aplicação que consumirá as mensagens que estão presentes na fila.
- **Exchange:** você não viu nada sobre exchange na imagem, mas tenha certeza que isso é muito importante e que de certa forma ela está lá. Nesse momento, você apenas tem que entender, que o exchange é apenas um roteador. Fará sentido mais adiante, nos exemplos que este artigo apresentará. Só para adiantar, há quatros tipos de exchanges sendo elas direct, fanout, topic e headers, cada uma delas possui sua importância e definição de funcionamento. Criaremos exemplos práticos, para melhor compreender.
- **AMQP:** Advanced Message Queuing Protocol, é um protocolo especifico para comunicação baseada em mensagens.

Então basicamente um Message Broker é composto de:

- Exchange
- Queues
- Producers
- Consumers
- Bindings
- AMQP (não é uma regra, mas atualmente os principais message brokers implementam este protocolo)

### Para finalizar o exemplo do nosso cenário, as vantagens em utilizar uma solução em mensageria são:

A Aplicação A não precisar se preocupar se a Aplicação B vai estar disponível no momento em que ela enviar o evento.

Os eventos gerados podem ser persistidos em disco pelo Message Broker. Isso é opcional e você define no momento da criação da queue, mas com certeza você vai configurar para que isso aconteça, afinal você não quer que nenhuma mensagem seja perdida caso o Message Broker seja reiniciado.

Caso o consumer não consiga confirmar a leitura da mensagem (ack) a mesma continuará enfileirada, até que em outro momento ele consiga confirmar a leitura.

#### Baixo acoplamento.
Note que a Aplicação A, nem sabe como a Aplicação B foi desenvolvida, se foi com C#, JavaScript, Ruby ou seja lá qual for a linguagem. A única responsabilidade da Aplicação A é comunicar que houve um evento e que mesmo deve ser integrado quando possível.