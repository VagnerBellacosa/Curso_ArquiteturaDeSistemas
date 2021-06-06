# Complexidade Ciclomatica

## Complexidade Ciclomática: Uma breve definição

A **complexidade ciclomática** é uma importante métrica de software. Refere-se ao número de caminhos de execução possíveis dentro de um determinado código - por exemplo, uma função. Quanto mais estruturas de decisão você usar, mais ramificações possíveis existem para seu código.

A complexidade ciclomática é especialmente importante quando se trata de testes. Ao calcular a complexidade ciclomática de uma função, por exemplo, você sabe o número mínimo de casos de teste que você precisará para alcançar a cobertura de ramificação completa dessa função. Portanto, podemos dizer que a complexidade ciclomática pode ser um preditor de quão difícil é testar um determinado código.

Um Exemplo de Complexidade Ciclomática Super Simples
Considere a seguinte função escrita em pseudocódigo:

void sayHello(name) {
    print("Hello, ${name}!");
}

Como tem uma única declaração, é fácil ver sua complexidade ciclomática é 1. Agora, vamos mudar um pouco as coisas:

void sayHello(name, sayGoodbye = false) {
    print("Hello, ${name}!");
    if (sayGoodbye) {
        print("Goodbye, ${name}!");
    }
}

A segunda versão da função tem um ramo nela. O chamador da função pode passar true como o valor do parâmetro sayGoodbye, mesmo que o valor padrão seja false. Se isso acontecer, a função imprimirá uma mensagem de adeus após dizer olá. Por outro lado, se o interlocutor não fornecer um valor para o parâmetro ou escolher false, a mensagem de adeus não será exibida.

Portanto, a função tem dois ramos de execução possíveis, o que é o mesmo que dizer que sua complexidade ciclomática tem um valor de 2.

## Por que a Complexidade Ciclomática é Ruim?

A complexidade ciclomática não é intrinsecamente ruim. Por exemplo, você pode ter um pedaço de código com um valor complexo ciclomático um pouco alto que é super fácil de ler e entender. Entretanto, de modo geral, podemos dizer que ter uma alta complexidade ciclomática é ou um sintoma de problemas com a base de código ou uma causa potencial de problemas futuros. Vamos cobrir algumas das razões pelas quais você gostaria de reduzi-la com mais detalhes.

### A Complexidade Ciclomática Pode Contribuir para a Complexidade Cognitiva

A complexidade cognitiva refere-se ao quanto é difícil entender um determinado código. Embora nem sempre seja esse o caso, a complexidade ciclomática pode ser um dos fatores que impulsionam a complexidade cognitiva. Quanto maior a complexidade cognitiva de um pedaço de código, mais difícil é navegar e manter.

### A Complexidade Ciclomática torna o código mais difícil de ser testado

Como já mencionamos, valores mais altos de complexidade ciclomática resultam na necessidade de um maior número de casos de teste para testar de forma abrangente um bloco de código - e.g., uma função. Portanto, se você quer facilitar sua vida ao escrever testes, provavelmente quer reduzir a complexidade ciclomática de seu código.

### A Complexidade Ciclomática Contribui para Maior Risco de Defeitos

É mais provável que você introduza defeitos em uma área da base de código que você muda muito do que a uma que você raramente toca. Além disso, quanto mais complexo for um determinado pedaço de código, mais provável é que você o entenda mal e introduza um defeito a ele.

Portanto, código complexo que sofre muito churn- mudanças freqüentes pela equipe - representa mais risco de defeitos. Ao reduzir a complexidade ciclomática - e, idealmente, também a rotatividade do código - você estará mitigando esses riscos.

#### Como Reduzir a Complexidade Ciclomática: 6 Maneiras Práticas

Agora vamos rever algumas dicas práticas que você pode usar para garantir que a complexidade ciclomática de seu código seja a mais baixa possível.

##### 1. Preferir funções menores

**O que fazer?**
Sendo todas as outras iguais, funções menores são mais fáceis de ler e entender. Elas também são menos propensas a conter bugs em virtude de seu comprimento. Se você não tem muito código, você não tem muitas oportunidades para o código de bugs. O mesmo raciocínio se aplica à complexidade ciclomática: é menos provável que você tenha um código complexo se tiver menos período de código. Portanto, o conselho aqui é preferir funções menores.

**Como fazer isso?**
Para cada função, identifique sua principal responsabilidade. Extraia o que sobra para suas próprias funções e módulos. Fazer isso também facilita a reutilização do código, o que é um ponto que revisaremos em breve.

##### 2.Evitar Argumentos de Bandeira em Funções

**O que fazer?**
Os argumentos da bandeira são parâmetros booleanos que você acrescenta a uma função. As pessoas normalmente os utilizam quando precisam mudar o funcionamento de uma função, preservando ao mesmo tempo o antigo comportamento.

**Como fazer?**
O que usar ao invés de parâmetros de bandeira? Em poucas palavras, você pode usar estratégias que alcançam o mesmo resultado sem incorrer em alta complexidade. Por exemplo, você poderia criar uma nova função, mantendo a antiga como está e extraindo as partes comuns em sua própria função privada.

Se o parâmetro da bandeira estiver sendo usado para melhorar ou melhorar de alguma forma o comportamento da função original, você pode querer aproveitar o [padrão decorator] (https://en.wikipedia.org/wiki/Decorator_pattern) para alcançar o mesmo fim.

##### 3.Reduzir o número de estruturas de decisão
**O que fazer?**
Você pode considerar este um “não-cérebro”. Se as estruturas de decisão - especialmente se-else e mudar de caso são o que causa mais ramos no código, é lógico que você deve reduzi-los se quiser manter a complexidade ciclomática à distância.

**Como fazer isso?**
Algumas das táticas que acabamos de ver podem contribuir para reduzir o número de se declarações em seu código. Por exemplo, em vez de usar argumentos de bandeira e depois usar uma declaração if* para verificar, você pode usar o padrão decorador. Em vez de usar um caso de troca para rever muitas possibilidades e decidir qual delas o código executará, você pode aproveitar o design pattern strategy. Claro que, em algum ponto do código, você ainda precisará de um estojo de troca. Afinal de contas, alguém tem que decidir qual a implementação real a ser usada. Entretanto, esse ponto se torna o único ponto no código que precisa dessa estrutura de decisão.

##### 4.Get Rid of Duplicated Code (Livre-se do código duplicado)

**O que fazer?**
Às vezes, você tem funções/métodos que fazem quase a mesma coisa. Manter ambos aumentam a complexidade ciclomática total de sua classe ou módulo. Se você pode limitar suas duplicatas, você pode limitar a complexidade.

**Como fazer?**
Remova as duplicatas de código por:

extraindo os bits de código comuns para seus próprios métodos/funções dedicados.
alavancando padrões de projeto - tais como o design pattern template- que incentivam a reutilização do código.
extraindo funções utilitárias genéricas em pacotes-gems, módulos npm, pacotes NuGet, etc. - que podem ser reutilizados por toda a organização.

##### 5.Remover Código Obsoleto

**O que fazer?**
Há muitas razões pelas quais é uma boa idéia remover o código obsoleto, ou seja, o código morto de sua aplicação. Para nosso contexto, basta dizer que essa é uma forma “gratuita” de aumentar a cobertura de código e diminuir a complexidade ciclomática.

**Como fazer isso?**
Basta usar uma ferramenta que lhe permita identificar o código morto - até mesmo sua IDE pode ser capaz de fazê-lo - e depois apagá-lo impiedosamente.

##### 6.Não Reinvente a Roda

**O que fazer?**
Deixe o desenvolvedor que nunca escreveu uma função - ou mesmo alguns deles - realizar a formatação da data lançar a primeira pedra! É quase como um rito de passagem.

Escrever código que simplesmente duplica a funcionalidade que a biblioteca padrão de seu idioma ou sua estrutura já oferece é uma maneira segura de aumentar desnecessariamente a complexidade. Se código é uma responsabilidade, você quer escrever apenas a quantidade estritamente necessária.

**Como fazer isso?**
Implemente uma sólida estratégia de revisão de código que seja capaz de identificar e se livrar de tais reinvenções de roda.

### Reduzir a Complexidade Ciclomática, Aumentar a Clareza do Código

A complexidade ciclomática é uma das métricas mais valiosas na engenharia de software. Ela tem implicações importantes para a qualidade do código e a capacidade de manutenção, sem mencionar os testes. A alta complexidade ciclomática pode ser tanto um sinal de problemas existentes como um preditor de problemas futuros. Portanto, manter o valor desta métrica sob controle é certamente algo que você quer fazer se quiser alcançar uma base de código saudável. Mantê-lo sob controle é exatamente o que você aprendeu com nosso posto.

Antes de separar os caminhos, uma última advertência. Tenha em mente que nenhuma métrica é uma panacéia quando usada isoladamente. Muitas vezes, o que você realmente gostaria de fazer é rastrear e melhorar um grupo de métricas que, juntos, podem lhe dar uma visão geral da saúde de sua equipe e de seu projeto. 

