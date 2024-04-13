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

### Classe Game1.cs
* Esta é a classe principal do jogo que herda a classe "game" proveniente do monogame, vai ser nesta classe que vai conter os métodos para inicializações, carregar algum tipo de contéudo, atualização e para a arte do jogo.
  - Dentro da classe, existem dois campos privados: "private GraphicsDeviceManager _graphics" que vai fazer o gerenciamento da exibição gráfica do jogo e "private SpriteBatch _spriteBatch" que vai fazer o gerenciamento para carregar as sprites para o jogo.
  - No construtor Game1 vai ser usado para inicializar o jogo através da configuração dos dispositivos gráficos, pois ele vai definir o diretório do conteúdo para o carregamento dos recursos e vai tornar o mouse visível, e através do "GameManager.cs" vai controlar o tamanho da janela do jogo com medidas já inicializadas.
  - Como todos os projetos, existe o método "Initialize" para ser chamado uma vez no início do jogo para executar qualquer atualização que tenha sido feita para o jogo.
  - Tal como a o método "Initialize" vai existir sempre também o método "LoadContent" que vai ser usado para carregar todos os recursos necessários para o funcionamento do jogo, neste caso, vai dar load das texturas do jogo, dos sons que os criadores usaram e também   controla o volume da música de fundo do jogo.
  - O método "Update" vai ser usado para atualizar o estado do jogo e os criadores do jogo fazem uma verificação neste método para saber se o utilizador pressionou a tecla "ENTER" para assim dar inicio ao jogo, se assim for vai dar iniciar o jogo e atualizar as entidades criadas e as ondas de inimigos para quando o jogo está em execução.
  - Através do método "Draw" os autores do código inicializam as sprites para a imagem de fundo e determinam o tamanho da janela para quando o utilizador ainda está no menu antes de dar inicio ao jogo, depois disso fazem a verificação se o jogo está a ser executado e aí vão ser executadas também as funções para desenhar a interface do jogador in-game. 

## GameManager.cs
* A classe GameManager contem variáveis que controlam a escala dos sprites e o tamanho da janela da aplicação, tambem armazena as propriedades das Entidades dentro do jogo, tais como velocidade, saúde, etc..

* O GameManager é uma classe estática, o que significa que as suas variáveis podem ser usadas sem ter de criar uma inicializar a classe como um objeto.
  - A variável "scale" permite alterar uniformemente a escala dos sprites e dos movimentos. Por defeito, está definida para escalar para 75% do tamanho original do sprite.
  - As variáveis "screenWidth" e "screenHeight" permitem-te alterar o tamanho da janela da aplicação.
  - Todos estes valores podem ser modificados de acordo com as necessidades para o jogo

## SpriteArt.cs
* A classe SpriteArt vai ser responsável pela criação das referências desejadas para as sprites, texturas do jogo, música e os efeitos sonoros do jogo.
  - Nesta classe existe uma única função, a função "Load" que vai ser pública e estática e que vai ser através dela que serão carregadas as sprites, texturas e os efeitos sonoros desejados, e os vai atribuir aos objetos criados na classe que como são do tipo "private set" após a sua inicialização os seus valores não podem ser alterados.

## Entity.cs
* Esta classe é uma classe abstrata que que vai representar todos os objetos do jogo, como: o jogador, os inimigos e as balas, e esta classe vai também tratar das texturas/sprites do player e inimigo como das suas posições durante o jogo e também uma área ao qual chamaram "hitbox" para representar as colisões das entidades.
  - Contém o método "update" que é responsável pela atualização do estado das entidades.
  - O método "Draw" que vai servir para desenhar as entidades nas suas respetivas posições, com a sua devida cor, escala, e os efeitos de cada.
  - E foram criados os métodos "createHitBox" e "updateHitBox" em que um vai criar, tal como o nome indica, as colisões do player com os inimigos ateravés da escala e da posição e, em seguida sempre que ocorrer essa colisão o outro método vai atualizar essa área de colisão.
* Para haver um controlo de todos os recursos que as entidades vão necessitar dentro jogo quando está a ser executado, como os desenhos, as texturas, os autores criaram a classe "EntityCollections.cs", para assim conseguirem lidar e controlar as funcionalidades das entidades quando o jogo está a ser executado, para isso:
  - Esta classe está responsável por gerir e manipular todas a entidades utilizadas no jogo, incluindo o jogador, os projéteis e pos inimigos e estes vão estar relacionados através das listas estáticas criadas pelos autores do jogo.
  - Através do método "Initialize" que vai ser responsável por incializar as entidades do jogo, ele vai verificar se essas entidades já foram inicializadas anteriormente para assim evitar múltiplas incializações, se as mesmas ainda não tiverem sido incializadas, vai ser criada uma instância do jogador com a sua textura e sua posição inicial no centro do ecrã de jogo, e após isso é adicionado à lista de entidades através do método "Instantiate" e vai definir a variável global "hasInitialized" como "true", pois ela no inicio do código estava inicializada como "false".
  - O método "Instantiate" vai ser receber como parâmetro a entidade que vai ser adicionada na lista, por isso o seu papel fundamental é adicionar a nova entidade à lista, ela vai adicionar a entidade à lista "geral" das entidades e depois através de duas verificações, caso seja um projétil vai ser adicionado também à lista dos projéteis e caso seja um inimigo é adicionado na lista dos inimigos.
  - Com o método "Draw" que vai ser responsável por desenhar as entidades no ecrã do jogo em que vai utilizar um loop for para percorrer as entidades na lista das entidades, e vai chamar o método "Draw" e passar como parâmetro o "SpriteBatch" para desenhar cada entidade no ecrã.
  - Como já é costume mais uma vez é usado o método "update" que vai ser responsável por atualizar as entidades do jogo, através de um loop for para percorrer a lista das entidades e assim chamar o "update" para atualizar o estado de cada entidade, após essa atualização vai remover das listas todas a entidades que tiverem definidades como falso usando as funções "Where" e "ToList", e com uma verificação vai detetar se ocorreu colisões entre os inimigos e os projéteis para assim realizar a subtração da vida dos inimigos.
  - O método "ClearEntities" vai ser o método que tal como o nome indica vai limpar todas a entidades do jogo, e para isso vai ser utilizado um loop for para percorrer mais uma vez a lista das entidades e caso seja um inimigo ou projétil o seu estado "isActive" vai definido como falso e vão ser limpos das listas através da função "Clear".

## Player.cs
* Como a classe anterior foi criada como uma classe abstrata, agora nesta classe usaram alguns métodos e funções dessa classe, neste caso o player vai herdar as texturas e a posição entre outros.
* A classe player vai ter os campos "sidespeed" que vai ser usado para determinar a velocidade com que o player se move na horizontal neste caso para a esquerda ou para a direita e essa variavél está declarada no "GameManager", o campo "cooldownRemaining" que vai controlar o tempo restante entre cada disparo do player, o campo "fireRate" que vai definir a taxa de tiros que são necessários entre cada tiro, e o campo "hp" que como toda a gente que joga videojogos sabe que isso se refere à vida do player, que no caso de chegar a 0 o jogo vai encerrar e dar uma mensagem de gameover.
* O construtor player vai receber os parâmetros da textura do jogador através do "image" e a sua posição inicial através do "initialPosition".
* No método update, primeiro os criadores fizeram uma verificação da hp do player, que se a mesma for menor ou igual a 0, o jogo vai acabar, vai dar display da sprite do gameover e também vai dar a música que foi definida para esse momento.
* Após isso, fazem as verificações para o movimento do player dentro do jogo, definem as teclas para o movimento como a tecla "A" e "LEFT" para o player ir para a esquerda caso sejam pressionadas e dentro dessa verificação fazem uma outra para o player não sair fora da janela de jogo e ir atualizando a sua posição, e repetem o mesmo processo para o movimento para a direita, mas para esse atribuiram as teclas "D" e "RIGHT".
* E ainda fazem mais uma verificação para quando o player for disparar os projéteis e o tempo de cooldown o permitir, em que a tecla definida para o disparo foi o "SPACE", e sempre que o cooldown o permitir vai ser criado um novo projétil na posição do jogador e seja assim disparado e após isso o tempo de cooldown vai ser reiniciado permitindo assim ao player disparar em intervalos de tempo regulares, e sempre que houver um disparo o seu som também vai ser reproduzido.

## Bullet.cs
* A classe "Bullet" vai herdar a classe "Entity" e com isso também vai estar de certa forma ligada com a classe "EntityCollections", pois na classe "Bullet" ao serem adicionados novos projéteis os mesmos têm de ser adicionados à sua respetiva lista durante o decorrer do jogo, com a herdade da classe "Entity" a classe "Bullet" vai assim herdar as características de todas as entidades do jogo tal como a sua textura, a sua posição e os seus métodos de atualização e desenho no ecrã.
  - Nesta classe é que vai ser definida a velocidade das balas quando são disparadas na vertical.
  - O primeiro construtor vai receber a posição inicial e a textura da bala, a textura vai ser atribuida ao campo correspondente na classe que é a pos, e a textura vai ser atribuida ao campo texture, vai ajustar a escala da posição da bala para que a mesma esteja centrada na posição inicial e em seguida vai chamar o método "CreateHitbox" para criar a área de colisão da bala.
  - No método "Update" que vai ser responsável pela a atualização da bala a cada frame, vai mover o projétil na vertical atualizando a sua posição de acordo com a sua velocidade, através de uma verificação se o projétil atingir o limite superior dp ecrã o seu estado vai passar a ser falso para assim o mesmo ser removido do jogo e após isso vai chamar o método "updateHitbox" para atualizar a área de colisão da bala de acordo com a sua nova posição.
