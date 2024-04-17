# Star Shooter Readme

### Autores do jogo: Alex Jeter and Zachary Mendoza
### Link repositório: https://github.com/AlexJeter17/MonoGameStarShooter.git
### Trabalho realizado por: Tiago Miranda-27937 e Gonçalo Araújo-27928

O grande objetivo dos autores do desenvolvimento deste jogo era mostrar de forma detalhada e mais simples de como uma pessoa que estivesse a fazer um jogo pela primeira vez em monogame pudesse seguir os passos deles e conseguir criar o seu próprio jogo, onde vai ter de lidar com as Sprites, as classes, os objetos, as colisões, o formato do jogo e até uma forma de ao fim conseguir enviar o seu jogo para os amigos.

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
  - As variáveis "screenWidth" e "screenHeight" permitem alterar o tamanho da janela da aplicação.
  - Todos estes valores podem ser modificados de acordo com as necessidades para o jogo.

## Código
    static class GameManager
    {
        public static float SCALE = 0.75f;
        //screenWidth and screenHeight will control the size of the window
        public static int screenWidth = 720;
        public static int screenHeight = 1080;

        public static int playerSpeed = 5;
        public static int playerHealth = 5;


        public static int level1Health = 2;
        public static float level1Speed = 3f;

        public static int level2Health = 3;
        public static float level2Speed = 5f;

        public static bool inGame = false;
    }

## SpriteArt.cs
* A classe SpriteArt vai ser responsável pela criação das referências desejadas para as sprites, texturas do jogo, música e os efeitos sonoros do jogo.
  - Nesta classe existe uma única função, a função "Load" que vai ser pública e estática e que vai ser através dela que serão carregadas as sprites, texturas e os efeitos sonoros desejados, e os vai atribuir aos objetos criados na classe que como são do tipo "private set" após a sua inicialização os seus valores não podem ser alterados.

## Código
    //The class name should generally be the same as the file name
    static class SpriteArt
    {
        //Fonts
        public static SpriteFont font { get; private set; }
        //Create the texture references here
        public static Texture2D Player { get; private set; }
        public static Texture2D Bullet { get; private set; }
        public static Texture2D EnemyTypeTwo { get; private set; }
        public static Texture2D EnemyTypeOne { get; private set; }
        public static Texture2D backGround { get; private set; }

        //Sound and Music References
        public static Song song { get; private set; }
        public static SoundEffect shootSound { get; private set; }
        public static SoundEffect explosion { get; private set; }
        public static SoundEffect gameOver { get; private set; }
        public static SoundEffect hpDown { get; private set; }


        //Function to load our content
        public static void Load(ContentManager content)
        {
            //Load Font and Sprites
            font = content.Load<SpriteFont>("File");
            Player = content.Load<Texture2D>("PlayerShip");
            EnemyTypeOne = content.Load<Texture2D>("EnemyTier1");
            EnemyTypeTwo = content.Load<Texture2D>("EnemyTier2");
            Bullet = content.Load<Texture2D>("Bullet");
            backGround = content.Load<Texture2D>("StarBackground");

            //Load Songs and SoundEffects
            song = content.Load<Song>("11_FreeSpaceMusic");
            explosion = content.Load<SoundEffect>("11_Explosion");
            shootSound = content.Load<SoundEffect>("11_ShootingNoise");
            gameOver = content.Load<SoundEffect>("11_GameOver");
            hpDown = content.Load<SoundEffect>("11_HealthDrop");
        }

    }
## Entity.cs
* Esta classe é uma classe abstrata que que vai representar todos os objetos do jogo, como: o jogador, os inimigos e as balas, e esta classe vai também tratar das texturas/sprites do player e inimigo como das suas posições durante o jogo e também uma área ao qual chamaram "hitbox" para representar as colisões das entidades.
  - Contém o método "update" que é responsável pela atualização do estado das entidades.
  - O método "Draw" que vai servir para desenhar as entidades nas suas respetivas posições, com a sua devida cor, escala, e os efeitos de cada.
  - E foram criados os métodos "createHitBox" e "updateHitBox" em que através do primeiro método vai garantir que cada entidade tenha uma hitbox configurada corretamente para permitir assim a deteção de colisões entre entidades no decorrer do jogo, ele vai criar u retângulo com base na posição e tamanho da textura da entidade, multiplicado pela escala da janela do jogo para assim garantir que a hitbox tenha um tamanho de acordo com o tamanho do ecrã de jogo, e o segundo tal como o nome indica vai ser essencial para garantir a deteção precisa das colisões entre as entidades no jogo, porque ao atualizar de forma contínua a posição da hitbox de acordo com o movimento da entidade vai ser possivel uma deteção mais eficaz das colisões e que as interações entre as mesmas ocorram de acordo com o esperado.
* Para haver um controlo de todos os recursos que as entidades vão necessitar dentro jogo quando está a ser executado, como os desenhos, as texturas, os autores criaram a classe "EntityCollections.cs", para assim conseguirem lidar e controlar as funcionalidades das entidades quando o jogo está a ser executado, para isso:
  - Esta classe está responsável por gerir e manipular todas a entidades utilizadas no jogo, incluindo o jogador, os projéteis e pos inimigos e estes vão estar relacionados através das listas estáticas criadas pelos autores do jogo.
  - Através do método "Initialize" que vai ser responsável por incializar as entidades do jogo, ele vai verificar se essas entidades já foram inicializadas anteriormente para assim evitar múltiplas incializações, se as mesmas ainda não tiverem sido incializadas, vai ser criada uma instância do jogador com a sua textura e sua posição inicial no centro do ecrã de jogo, e após isso é adicionado à lista de entidades através do método "Instantiate" e vai definir a variável global "hasInitialized" como "true", pois ela no inicio do código estava inicializada como "false".
  - O método "Instantiate" vai ser receber como parâmetro a entidade que vai ser adicionada na lista, por isso o seu papel fundamental é adicionar a nova entidade à lista, ela vai adicionar a entidade à lista "geral" das entidades e depois através de duas verificações, caso seja um projétil vai ser adicionado também à lista dos projéteis e caso seja um inimigo é adicionado na lista dos inimigos.
  - Com o método "Draw" que vai ser responsável por desenhar as entidades no ecrã do jogo em que vai utilizar um loop for para percorrer as entidades na lista das entidades, e vai chamar o método "Draw" e passar como parâmetro o "SpriteBatch" para desenhar cada entidade no ecrã.
  - Como já é costume mais uma vez é usado o método "update" que vai ser responsável por atualizar as entidades do jogo, através de um loop for para percorrer a lista das entidades e assim chamar o "update" para atualizar o estado de cada entidade, após essa atualização vai remover das listas todas a entidades que tiverem definidades como falso usando as funções "Where" e "ToList", e com uma verificação vai detetar se ocorreu colisões entre os inimigos e os projéteis para assim realizar a subtração da vida dos inimigos.
  - O método "ClearEntities" vai ser o método que tal como o nome indica vai limpar todas a entidades do jogo, e para isso vai ser utilizado um loop for para percorrer mais uma vez a lista das entidades e caso seja um inimigo ou projétil o seu estado "isActive" vai definido como falso e vão ser limpos das listas através da função "Clear".

## Colisões e Hitboxes
  -  A ideia de adicionar colisões é criar caixas em torno do sprite que determinam a área de colisão, sempre que essas duas caixas se sobrepuserem, uma função será chamada e qualquer lógica relacionada a colisões será aplicada.
  -  Para aplicar a lógica das hitboxes, a classe entity vai possuir no seu construtor a hitbox, e terá duas funções onde criará a hitbox e a vai atualizar de acordo com a posição da entindade que a possui.
  -  As dimensões da hitbox vão ser definidas automaticamente pelas dimensões da sprite.
  -  Após isso, dentro dos Construtores dos objetos que queremos que possuam uma hitbox, chama a função para criar a hitbox, e na respetiva função update será chamado a funcão para dar update à posição dela de acordo com a posição da entidade.
  - Com isso, a função que verifica se hã colisões pode ser chamada no update, esta funciona iterando a verificar se alguma das hitboxes existentes colidiram. Assim, podemos subtraír a vida sempre que há uma colisão entre a bala e um inimigo, e com isso adicionar também os efeitos de colisão
  - Efeitos de Colisão vão ser aplicados primeiramente definindo na class "Entity.cs", a cooldown do hit "hitCooldown", as frames que o "hitFrames" "Hit" dura, a distância de knockback que o inimigo leva "hitKnockbackMultiplier" e o "ColorTint" que vai definir a cor predefinida do sprite, esta sendo branca.
      - Com o tint definido em todas as entidade, quando a colisão é observada, é chamada a função onHit e onHitEffect. o cooldown é definido, e durante a duração desse cooldown a função Effect aplica o knockback e o muda a cor do sprite para vermelho.

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
 
## Enemy.cs
* Herdando a Classe "Entity", podemos criar a classe "Enemy.cs", primeiramente o mais importante será o seu construtor. O construtor é uma peça importante da classe Inimigo, pois permite criar diferentes tipos de inimigos sem ter de reescrever código.
  - O construtor recebe quatro parâmetros e executa as seguintes ações:
    - Define a posição inicial do inimigo numa zona no topo da cena de jogo, de onde o inimigo se irá deslocar para baixo (pos).
    - Recebe uma imagem para usar como textura (texture).
    - Define uma nova velocidade de movimento para o inimigo (dropSpeed).
    - Define a quantidade máxima de pontos de vida do inimigo (health).
* Para além disso, o "Enemy" terá de se deslocar para baixo no espaço, para isso ele segue um simples Update:
    - Primeiro, ele verifica se a vida atual é igual a 0. Se for, o inimigo remove-se a si próprio.
    - De seguida, verificamos se o Inimigo está fora do campo de jogo enquanto se move para baixo. Se estiver, remove-se a si próprio.
    - Por fim, a posição do Inimigo é atualizada somando a velocidade à sua posição para continuar o seu movimento descendente.
- De seguida a classe "Enemy" será instanciada da mesma forma que a classe "Bullet" no "EntityCollections.cs" para poder ter um maior controlo dos inimigos.
  
## WaveManager.cs
* Esta classe vai ser responsável por adicionar os inimigos de forma automática de forma a que não seja necessário adicioná-los manualmente. Vai se certificar da lógica usada para o aparecimento das várias ondas de inimigos no decorrer do jogo ao aumentar o número de inimigos, a taxa de aparecimento e as ondas.
  - Para começar foram adicionadas variáveis na classe "GameManager" que vai controlar a vida que os inimigos têm e a velocidade que se vão puder mover.
  - Agora já dentro da classe "WaveManager" são criadas e inicializadas 6 variáveis estáticas que são: wave que vai servir para manter a contagem das ondas de inimigos, a spawnRate que vai determinar a frequência com que os inimgos vão ser gerados, a spawnDelay vai controlar o atrasado entre os aparecimentos dos inimigos, a maxNumEnemies vai armazenar o número máximo de inimigos que podem ser gerados numa única onda e a remainingEnemiesToSpawn que vai manter o número de inimigos restantes para serem gerados na onda que o jogo tiver e a variável "Random" para gerar números aleatórios.
  - No método "InitializeWave" que tal como o nome indica vai servir para inicializar uma nova onde de inimigos, vai ser feita uma verificação usando a classe "EntityCollections" para saber se não há inimigos restantes no jogo e se todos os inimigos foram eliminados na onda que o jogo se encontrar, se assim for o número da onda vai ser incrementado e o número máximo de inimigos para a onda seguinte vai ser atualizado e aumentado em 2 unidades.
  - Através do método "Reset" que mais uma vez como o nome indica vai servir para resetar todos os valores que estejam relacionados com as ondas do jogo. Vai ser feita a definição do número da onda como zero, o número de inimigos restantes por gerar também vai ser zero e vai resetar o número máximo de inimigos outra vez para dois.
  - Com o método "Update" que vai ser chamado a cada frame do jogo para atualizar o gerenciamento das ondas, vai chamar o método criado "InitializeWave" para iniciar uma nova onda se necessário, em seguida verifica se o "spawnDelay" é zero e se não há mais inimigos por gerar, se isso se verificar, é usada uma outra verificação para gerar os inimigos em que todos os que forem par são inimigos nível 1 e os inimigos com número ímpar são nível 2, ao gerar o inimgo vai subtrair um nos inimigos por gerar e de seguida gera o inimigo através do "EntityCollections" de acordo com o seu nível e vai resetar o "spawnDelay" para o "spawnRate", e através de outra verificação se o "spawnDelay" não for zero vai ser subtraido um. Vai repetir esse processo a cada frame de forma a garantir que todos os inimigos sejam gerados de acordo com as configuarções já definidas.

## Game1.cs

* Esta é a classe principal do jogo que herda a classe "game" proveniente do monogame, vai ser nesta classe que vai conter os métodos para inicializações, carregar algum tipo de contéudo, atualização e para a arte do jogo.
  - Dentro da classe, existem dois campos privados: "private GraphicsDeviceManager _graphics" que vai fazer o gerenciamento da exibição gráfica do jogo e "private SpriteBatch _spriteBatch" que vai fazer o gerenciamento para carregar as sprites para o jogo.
  - No construtor Game1 vai ser usado para inicializar o jogo através da configuração dos dispositivos gráficos, pois ele vai definir o diretório do conteúdo para o carregamento dos recursos e vai tornar o mouse visível, e através do "GameManager.cs" vai controlar o tamanho da janela do jogo com medidas já inicializadas.
  - Como todos os projetos, existe o método "Initialize" para ser chamado uma vez no início do jogo para executar qualquer atualização que tenha sido feita para o jogo.
  - Tal como a o método "Initialize" vai existir sempre também o método "LoadContent" que vai ser usado para carregar todos os recursos necessários para o funcionamento do jogo, neste caso, vai dar load das texturas do jogo, dos sons que os criadores usaram e também   controla o volume da música de fundo do jogo.
  - O método "Update" vai ser usado para atualizar o estado do jogo e os criadores do jogo fazem uma verificação neste método para saber se o utilizador pressionou a tecla "ENTER" para assim dar inicio ao jogo, se assim for vai dar iniciar o jogo e atualizar as entidades criadas e as ondas de inimigos para quando o jogo está em execução.
  - Através do método "Draw" os autores do código inicializam as sprites para a imagem de fundo e determinam o tamanho da janela para quando o utilizador ainda está no menu antes de dar inicio ao jogo, depois disso fazem a verificação se o jogo está a ser executado e aí vão ser executadas também as funções para desenhar a interface do jogador in-game. 
