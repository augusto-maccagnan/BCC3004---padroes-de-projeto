# BCC3004 - padroes-de-projeto

Este projeto tem como objetivo documentação de 3 padrões de projeto sendo, um padrão criacional, um padrão estrutural, um padrão comportamental.

A consulta dos padrões será feita utilizando como base os exemplos disponíveis no site: https://refactoring.guru/design-patterns/

### Foram escolhidos para o projeto a documentação dos seguintes padrões:
- <a href="https://refactoring.guru/design-patterns/factory-method">Método da fábrica</a> 
- <a href="https://refactoring.guru/design-patterns/adapter">Adaptador</a>
- <a href="https://refactoring.guru/design-patterns/state">Estado</a>

#

### Método fábrica
#### Descrição
Também conhecido como construtor virtual é um padrão criacional que fornece uma interface para criação de objetos em uma superclasse, mas permite que subclasses alterem o tipo dos objetos que serão criados.
#### Problema:
Imagine que você está criando uma aplicação para gerenciamento de logística. A primeira versão do aplicativo gerencia apenas transporte por meio de caminhões, então a maioria do seu código está dentro da classe "Caminhao".

Depois de certo tempo, seu aplicativo se torna popula. Cada dia você recebe vários pedidos de empresas de transporte marítmo para incorporar logística marítma no aplicativo.

Como seu código todo está acoplado a classe "Caminha". Para fazer esta adição você teria que mudar toda a base de código existente para adicionar a classe "Barco".</br>
E ainda se fosse necessário futuramente adicionar outro tipo de transporte para o aplicativo, você precisaria alterar todo o código novamente
#### Solução
O Método Fábrica sugere que você substitua a construção direta do objeto (usando o operador ```new```) por chamadas de um método de <i>fábrica</i> especial. Os objetos ainda serão criados via o operador ```new``` mas sendo chamado dentro do método fábrica. Os objetos criados pelo método fábrica são referidos como <i>produtos</i>.

Esta mudança pode parecer inútil, mas movemos a chamada do construtor para outra parte do programa. Sendo assim podemos sobrescrever o método fábrica em uma subclasse que muda a classe dos produtos sendo criados pelo método.

<b>Porém existe uma limitação</b>, as subclasses podem retornar diferentes tipos de produtos, apenas quando estes produtos tem uma classe e interface base comum. Além disso, o método fábrica da classe base deve ter seu tipo de retorno declarado como esta interface.</br>
Por exemplo a classe ```Caminhão``` e a classe ```Barco``` devem implentar a classe ```Transporte```, que por sua vez declara o método chamado ```entregar```.</br>
Cada classe implementa o método de diferentes modos: O caminhão entrega pela terra e o barco entrega pelo mar.</br>
O método fábrica na classe ```LogisticaRodovia``` devolve um objeto caminhão, e o método fábrica na classe ```LogisticaMar``` devolve um objeto tipo Barco.

#

### Adaptador
#### Descrição
Adaptador é um padrão estrutural que permite que objetos com interfaces incompatíveis colaborem.
#### Problema
Imagine que você está criando um aplicativo de monitoramento de estoque de mercado. O aplicativo baixa os dados de estoque de múltiplas fontes em formato XML e então disponibiliza diagramas e tabelas com melhor visual para o usuário.

Em algum momento você decide melhorar o aplicativo integrando uma biblioteca inteligente de análise de um terceiro. Porém a biblioteca de análise trabalha com dados no formato JSON.</br>
Você poderia mudar a biblioteca para funcionar com XML, mas isso poderia quebrar o código existente da biblioteca, e no pior caso você nem teria acesso ao código fonte da biblioteca, tornando essa solução impossível.
#### Solução
Você pode criar um adaptador. Este objeto especial converte a interface de um objeto para outro objeto conseguir entender.

O adaptador envolve um objeto para esconder a complexidade da conversão que está acontecendo entre as interfaces debaixo dos panos. O objeto que foi envolvido nem sabe da existência do adaptador.

Ele funciona da seguinte forma
1. O adaptador recebe uma interface compatível com um dos objetos existentes.
2. Usando esta interface o objeto existente chama os métodos do adaptador.
3. Quando ele recebe uma chamada o adaptador passa a requisição para o segundo objeto, mas em um formato e ordem que o segundo objeto espera.

Alguma vezes é possível criar um adaptador ida-e-volta, que converte a chamada tanto do objeto 1 para o objeto 2, quanto do objeto 2 para o objeto 1, ou seja, em ambas as direções.
#### Analogia ao mundo real
Uma analogia ao mundo real seriam os plugs de tomadas, viajando para outros estados é possível encontrar tomadas com formatos de plug diferentes, este problema pode ser solucionado com um adaptador para tomada de um formato para outro.
#### Estrutura
O adaptador pode ser implementado tanto como um objeto ou como uma classe.
##### Objeto
Sendo um objeto ele deve envolver um dos objetos para fazer a interface entre as chamadas.
##### Classe
Sendo uma classe o adaptador pode herdar os métodos de uma classe e fazer a adaptação na sobeescrita do método, podendo assim ser utilizada a classe adapatador no lugar do objeto original.
#

### Estado
#### Descrição
Estado é um padrão comportamental que permite que objetos alterem seu comportamento quando seu estado interno muda. Aparentando que o objeto mudou a própria classe.
#### Problema
Este padrão está relacionada ao conceito da Máquina de Estados Finita.</br>
A ideia principal é que em todos os momentos existe um número finito de estados no qual o programa pode estar. Em cada estado único o programa se comporta de forma diferente, e o programa pode trocar de um estado para outro instantâneamente, porém dependendo do estado atual o programa pode ou não trocar para outros estados, sendo as regras de mudança de estado chamadas <i>Transições</i>, elas também são finitas e predeterminadas.

Você também pode aplicar este comportamento em objetos. Imagine que temos uma classe ```Documento```. Um documento pode estar em um destes estados: ```Rascunho```, ```Avaliação```, ```Publicado```. O método ```publicar``` do documento funciona diferente para cada estado:
- Em ```Rascunho```, ele move o documento para avaliação
- Em ```Avaliação```, ele faz o documento ficar público, mas apenas se o usuário atual é um administrador.
- Em ```Publicado```, ele não faz nada.

As máquinas de estado normalmente são implementadas utilizando várias condicionais(```if``` ou  ```switch```) que selecionam o comportamento apropriado para o estado atual do objeto. Normalmente este estado é alguns valores específicos dos campos do objeto.

O maior problema de uma máquina de estados é que quanto mais estados existem em um objeto, maior ficam as condições que buscam o comportamento apropriado para o método de acordo com o estado atual, código assim é de dificil manutenção, pois qualquer mudança na lógica de transição dos estados pode precisar de mudanças nas condições de estado de cada método.
#### Solução
O padrão comportamental de estados sugere que você crie novas classes para cada estado possível de um objeto e extraia todo o comportamento relacionado aos estados para estas classes.

Ao invés de implementar todos os comportamentos sozinho, o objeto original, chamado de contexto, guarda as referências para um dos objetos de estado que representa seu estado atual, e delega todo trabalho referente ao estado para este objeto.

Para transicionar o contexto para oturo estado, substituir o objeto de estado ativo com outro objeto que representa este novo estado. Isso é possível apenas se todas as classes de estados seguirem a mesma interface e o contexto sozinho utiliza dos objetos pela interface.

A diferença desta estrutura e do padrão <a href="https://refactoring.guru/design-patterns/strategy">Estratégia</a> se da por que dentro do padrão de Estados, cada estado sabe da existência de outro e pode iniciar uma transição de um estado para outro.
#### Analogia ao mundo real
Uma analogia do padrão de estados pode ser encontrada nos botões do seu <i>smartphone</i>, que se comportam de maneira diferente dependendo do estado atual do dispositivo
- Quando ele está desbloqueado, pressionar os botões executa várias funções.
- Quando ele está bloqueado, pressionar qualquer botão leva a tela de desbloqueio.
- Quando ele está com pouca bateria, pressionar qualquer botão leva a tela de carregamento.
 
#### Estrutura
Deve existir uma classe de contexto, que guarda a referência a uma dos objetos de estado concreto e delega para ele tarefas relacionadas aos estados, a comunicação deve acontecer por meio de uma interface de estado, o contexto deve expor um ```setter``` para receber um novo objeto de estado.

A interface de estado declara todos os métodos específicos de estado. Estes métodos devem fazer sentido para todos os estados concretos, para não disperdiçar métodos que nunca serão chamados nos estados concretos.

Os estados concretos devem providenciar sua própria implementação para os métodos específicos para cada estado. Para evitar duplicação de código parecido, você pode fornecer uma classe intermediária abstrata que encapsula alguns comportamentos comuns.

Objetos de estado podem guardar referência para o objeto de contexto. Por meio desta referência o estado pode buscar informações necessárias do contexto e iniciar uma transição de estados.

Ambos contexto e estado concreto podem alterar o próximo estado do contexto e fazer a transição de estado, alterando o objeto de estado referênciado no contexto.

#
