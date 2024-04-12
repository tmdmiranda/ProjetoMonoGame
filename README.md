# Star Shooter Readme

### Autores do jogo: Alex Jeter and Zachary Mendoza
### Trabalho realizado por: Tiago Miranda-27937 e Gonçalo Araújo-27928

O grande objetivo dos autores do desenvolvimento deste jogo era mostrar de forma detalhada e mais simples de como uma pessoa que estivesse a fazer um jogo pela primeira vez em monogame pudesse seguir os passos deles e conseguir criar o seu próprio jogo, onde vai ter de lidar com as Sprites, as classes, os objetos, as colisões, o formato do jogo e até uma forma de ao fim conseguir enviar o seu jogo para os amigos.
#### Em mais detalhe, o que este tutorial nos consegue mostrar é o seguinte:

* Criar o teu primeiro projeto de Monogame no Visual Studio 2022.
* Desenvolver classes de organização como uma boa prática programação.
* Aprender a utilizar sprites e a manipulá-los no jogo.
* Criar novos métodos e desenvolver bons hábitos de programação.
* Aprender a usar variáveis não globais para uma melhor gestão.
* Implementar colisões e remover entidades que forem destruídas.
* Melhorar o teu jogo usando ondas de inimigos através de algoritmos.
* Desenvolver uma interface de utilizador para garantir que o teu jogo pode ser executado infinitamente.
* Comprimir o teu jogo num ficheiro ZIP para enviar às pessoas para o jogarem.

### Classe Game1.cs
* Esta é a classe principal do jogo que herda a classe "game" proveniente do monogame, vai ser nesta classe que vai conter os métodos para inicializações, carregar algum tipo de contéudo, atualização e para a arte do jogo.
  - Dentro da classe, existem dois campos privados: "private GraphicsDeviceManager _graphics" que vai fazer o gerenciamento da exibição gráfica do jogo e "private SpriteBatch _spriteBatch" que vai fazer o gerenciamento para carregar as sprites para o jogo.
  - No construtor Game1 vai ser usado para inicializar o jogo através da configuração dos dispositivos gráficos, pois ele vai definir o diretório do conteúdo para o carregamento dos recursos e vai tornar o mouse visível, e através do "GameManager.cs" vai controlar o tamanho da janela do jogo com medidas já inicializadas.
  - Como todos os projetos, existe o método "Initialize" para ser chamado uma vez no início do jogo para executar qualquer atualização que tenha sido feita para o jogo.
  - Tal como a o método "Initialize" vai existir sempre também o método "LoadContent" que vai ser usado para carregar todos os recursos necessários para o funcionamento do jogo, neste caso, vai dar load das texturas do jogo, dos sons que os criadores usaram e também   controla o volume da música de fundo do jogo.
  - O método "Update" vai ser usado para atualizar o estado do jogo e os criadores do jogo fazem uma verificação neste método para saber se o utilizador pressionou a tecla "ENTER" para assim dar inicio ao jogo, se assim for vai dar iniciar o jogo e atualizar as entidades criadas e as ondas de inimigos para quando o jogo está em execução.
  - Através do método "Draw" os autores do código inicializam as sprites para a imagem de fundo e determinam o tamanho da janela para quando o utilizador ainda está no menu antes de dar inicio ao jogo, depois disso fazem a verificação se o jogo está a ser executado e aí vão ser executadas também as funções para desenhar a interface do jogador in-game. 

### Estruturamento do projeto

* O projeto está estruturado de maneira onde a Principal classe encontra-se na root do projeto, e dentro da pasta "Components" encontram-se todas as classes necessarias para o jogo

* Dentro da pasta tem:
  - Bullet.cs
  - Enemy.cs
  - Entity.cs
  - EntityCollection.cs
  - GameManager.cs
  - Player.cs
  - SpriteArt.cs
  - UserInterface.cs
  - WaveManager.cs

## GameManager.cs
* A classe GameManager contem variáveis que controlam a escala dos sprites e o tamanho da janela da aplicação, tambem armazena as propriedades das Entidades dentro do jogo, tais como velocidade, saúde, etc..

* O GameManager é uma classe estática, o que significa que as suas variáveis podem ser usadas sem ter de criar uma inicializar a classe como um objeto.
  
  - A variável "scale" permite alterar uniformemente a escala dos sprites e dos movimentos. Por defeito, está definida para escalar para 75% do tamanho original do sprite.

  - As variáveis "screenWidth" e "screenHeight" permitem-te alterar o tamanho da janela da aplicação.

  - Todos estes valores podem ser modificados de acordo com as necessidades para o jogo

## SpriteArt.cs
* A classe SpriteArt vai ser responsável pela criação das referências desejadas para as sprites, texturas do jogo, música e os efeitos sonoros do jogo.
  - Nesta classe existe uma única função, a função "Load" que vai ser pública e estática e que vai ser através dela que serão carregadas as sprites, texturas e os efeitos sonoros desejados dos ficheiros e os vai atribuir aos objetos criados na classe que como são do tipo "private set" após a sua inicialização os seus valores não podem ser alterados.

## Entity.cs
* Esta classe é uma classe abstrata que que vai representar todos os objetos do jogo, como: o jogador, os inimigos e as balas, e esta classe vai também tratar das texturas/sprites do player e inimigo como das suas posições durante o jogo e também uma área ao qual chamaram "hitbox" para representar as colisões das entidades.
  - Contém o método "update" que é responsável pela atualização do estado das entidades.
  - O método "Draw" que vai servir basicamente para desenhar as entidades nas suas respetivas posições, com a sua devida cor, escala, e os efeitos.
  - E criaram os métodos "creatHitBox" e "updateHitBox" em que um vai criar, tal como o nome indica, as colisões do player com os inimigos ateravés da escala e da posição e, em seguida sempre que ocorrer essa colisão o outro método vai atualizar essa área de colisão.
* Para haver um controlo de todos os recursos que as entidades vão necessitar dentro jogo quando está a ser executado, como os desenhos, as texturas e tudo mais, os autores criaram a classe "EntityManager", para assim conseguirem lidar e controlar as funcionalidades das entidades quando o jogo está a ser executado.
  - Criaram uma lista para guardar e criar as entidades no ecrã, e de seguida adicionaram na lista as entidades que vão ser usadas para o jogo através de uma função add.
  - Através do método draw criado na classe "Entity.cs", vai ser possivel desenhar as entidades para a lista.
  - Após isso, é criado o método "update" com um loop for para percorrer todas as entidades que estiverem na lista e depois usar o update para cada entidade.
  - E assim como os métodos já estão criados, os mesmos vão ser usados na classe principal "Game1" para assim serem adicionadas as funcionalidades para o jogo. 
