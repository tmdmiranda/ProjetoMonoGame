# Star Shooter Readme

### Autores do jogo: Alex Jeter and Zachary Mendoza
### Link repositório: https://github.com/AlexJeter17/MonoGameStarShooter.git
### Trabalho realizado por: Tiago Miranda-27937 e Gonçalo Araújo-27928

O grande objetivo dos autores do desenvolvimento deste jogo era mostrar de forma detalhada e mais simples de como uma pessoa que estivesse a fazer um jogo pela primeira vez em monogame pudesse seguir os passos deles e conseguir criar o seu próprio jogo, onde vai ter de lidar com as Sprites, as classes, os objetos, as colisões, o formato do jogo e até uma forma de ao fim conseguir enviar o seu jogo para os amigos.

## Como jogar
* Comandos:
  - Tecla ENTER para iniciar o jogo;
  - Tecla A ou tecla seta ESQUERDA para mover para a esquerda;
  - Tecla D ou tecla seta DIREITA para mover para direita;
  - Tecla SPACE para disparar.

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
  - Foram adicionadas também variáveis que vão controlar a vida que os inimigos têm e a velocidade que se vão mover.

## Código
```csharp
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
```
## SpriteArt.cs
* A classe SpriteArt vai ser responsável pela criação das referências desejadas para as sprites, texturas do jogo, música e os efeitos sonoros do jogo.
  - Nesta classe existe uma única função, a função "Load" que vai ser pública e estática e que vai ser através dela que serão carregadas as sprites, texturas e os efeitos sonoros desejados, e os vai atribuir aos objetos criados na classe que como são do tipo "private set" após a sua inicialização os seus valores não podem ser alterados.

## Código
  ```csharp
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
  ```
## Entity.cs
* A classe Entity é uma classe abstrata que serve como base para todos os objetos do jogo, como jogador, inimigos e projéteis. Ela lida com as texturas, posições e hitboxes das entidades.
* Os métodos principais incluem:
 - Update(): Atualiza o estado das entidades a cada frame do jogo.
 - Draw(): Desenha as entidades no ecrã com suas texturas, posições e efeitos.
 - createHitbox(): Cria a hitbox da entidade com base na sua posição, tamanho da textura e escala do jogo.
 - updateHitbox(): Atualiza continuamente a posição da hitbox de acordo com o movimento da entidade para permitir detecções precisas das colisões.

## Código
  ```csharp
     protected Texture2D texture;
     public Vector2 pos;
     public bool isActive = true;
     public Rectangle hitbox;
     //Abstract Methods
     public abstract void Update();

     //Common Draw Method
     public virtual void Draw(SpriteBatch spriteBatch)
     {
         spriteBatch.Draw(
             texture,                        //Texture
             pos,                            //Position
             null,                           //What portion of the sprite to draw (Default Draws whole sprite)
             Color.White,                           //Tint Color
             0f,                             //Rotation
             Vector2.Zero,                   //Origin
             GameManager.SCALE,       //Scale
             SpriteEffects.None,             //Effects
             0f);                            //Z-Layer
     }

     public virtual void createHitbox()
     {
         hitbox = new Rectangle((int)pos.X, (int)pos.Y, (int)(texture.Width * GameManager.SCALE), (int)(texture.Height * GameManager.SCALE));
     }

     public virtual void updateHitbox()
     {
         hitbox.X = (int)pos.X;
         hitbox.Y = (int)pos.Y;
     }
  ```
## EntityCollections.cs
* A classe EntityCollections é responsável pelo gerenciamento de todas as entidades durante o decorrer do jogo. Ela vai criar listas estáticas de entidades, projéteis e inimigos.
* Os seus principais métodos são:
 - Initialize(): Inicializa as entidades do jogo, cria uma instância do jogador e adiciona-o à lista das entidades, para evitar múltiplas inicializações.
 - Instantiate(Entity entity): Adiciona uma nova entidade à lista geral das entidades e, conforme o tipo de entidade, adiciona também à lista de projéteis ou inimigos.
 - Draw(SpriteBatch spriteBatch): Desenha todas as entidades no ecrã.
 - Update(): Atualiza o estado das entidades, remove as entidades desativadas da lista e verifica as colisões entre inimigos e projéteis.
 - ClearEntities(): Limpa todas as entidades do jogo, define o estado isActive como falso para os inimigos e projéteis e remove-os da lista.

## Código
```csharp
 static class EntityCollections
 
     //From Part 4
     static List<Entity> entities = new List<Entity>();
     static List<Bullet> bullets = new List<Bullet>();
     //The enemy list to add
     public static List<Enemy> enemies = new List<Enemy>();

     //New Variables and Method
     public static Player player;

     public static int score = 0;
     public static bool hasInitialized = false;

     public static void Initialize()
     {
         if (hasInitialized == false)
         {
             player = new Player(SpriteArt.Player, new Vector2(GameManager.screenWidth / 2, GameManager.screenHeight - 200));
             Instantiate(player);
             hasInitialized = true;
         }
     }
     public static void Instantiate(Entity entity)
     {
         entities.Add(entity);
         if (entity is Bullet) bullets.Add(entity as Bullet);
         else if (entity is Enemy) enemies.Add(entity as Enemy);
     }
     public static void Draw(SpriteBatch spriteBatch)
     {
         for (int i = 0; i < entities.Count; i++)
         {
             entities[i].Draw(spriteBatch);
         }
     }

     public static void Update()
     {
         //From part 4
         for (int i = 0; i < entities.Count; i++)
         {
             entities[i].Update();
         }

         //If any entities have isActive set to false, remove them from the lists
         entities = entities.Where(obj => obj.isActive).ToList();
         bullets = bullets.Where(obj => obj.isActive).ToList();
         enemies = enemies.Where(obj => obj.isActive).ToList();

         for (int i = 0; i < enemies.Count; i++)
         {
             for (int j = 0; j < bullets.Count; j++)
             {
                 //Check if any of the enemies or bullets collide with each other
                 if (bullets[j].hitbox.Intersects(enemies[i].hitbox))
                 {
                     //Subtract enemy health by one, remove bullet
                     //enemies[i].health -= 1;
                     enemies[i].OnHit();
                     bullets[j].isActive = false;
                 }
             }
         }
     }

     public static void ClearEntities()
     {
         for (int i = 0; i < entities.Count; i++)
         {
             if (entities[i] is Enemy || entities[i] is Bullet)
             {
                 entities[i].isActive = false;
             }
         }
         enemies.Clear();
         bullets.Clear();
     }
```
## Colisões e Hitboxes
  -  A ideia de adicionar colisões é criar caixas em torno do sprite que determinam a área de colisão, sempre que essas duas caixas se sobrepuserem, uma função será chamada e qualquer lógica relacionada a colisões será aplicada.
  -  Para aplicar a lógica das hitboxes, a classe entity vai possuir no seu construtor a hitbox, e terá duas funções onde criará a hitbox e a vai atualizar de acordo com a posição da entindade que a possui.
  -  As dimensões da hitbox vão ser definidas automaticamente pelas dimensões da sprite.
  -  Após isso, dentro dos Construtores dos objetos que queremos que possuam uma hitbox, chama a função para criar a hitbox, e na respetiva função update será chamado a funcão para dar update à posição dela de acordo com a posição da entidade.
 ```csharp
    public virtual void createHitbox()
{
    hitbox = new Rectangle((int)pos.X, (int)pos.Y, (int)(texture.Width * GameManager.SCALE), (int)(texture.Height * GameManager.SCALE));
}
 ```
  - Com isso, a função que verifica se há colisões pode ser chamada no update, esta funciona iterando a verificar se alguma das hitboxes existentes colidiram. Assim, podemos subtraír a vida sempre que há uma colisão entre a bala e um inimigo, e com isso adicionar também os efeitos de colisão
  ```csharp
public virtual void updateHitbox()
{
    hitbox.X = (int)pos.X;
    hitbox.Y = (int)pos.Y;
}
  ```
  - Efeitos de Colisão vão ser aplicados primeiramente definindo na class "Entity.cs", a cooldown do hit "hitCooldown", as frames que o "hitFrames" "Hit" dura, a distância de knockback que o inimigo leva "hitKnockbackMultiplier" e o "ColorTint" que vai definir a cor predefinida do sprite, esta sendo branca.
      - Com o tint definido em todas as entidade, quando a colisão é observada, é chamada a função onHit e onHitEffect. O cooldown é definido, e durante a duração desse cooldown a função Effect aplica o knockback e o muda a cor do sprite para vermelho.
        ### OnHit()
        ```csharp
        public void OnHit() 
        {
          health -= 1;
          hitCooldown = hitFrames;
          pos.Y -= dropSpeed * knockbackMultiplier;
        }
        ```
        ```csharp
        private void OnHitEffect()
        {
          if (hitCooldown >= 0)
          {
            hitCooldown -= 1;
            tint = Color.Red;
          }
          else
          {
            tint = Color.White;
          }
        }
        

## Player.cs
* A classe Player herda da classe Entity, o que significa que ela recebe todos os comportamentos e propriedades definidos na classe Entity. Neste caso, o jogador terá acesso às texturas, posições e métodos de desenho e atualização definidos na classe Entity.
* Os campos da classe Player são:
  - sideSpeed: Determina a velocidade de movimento horizontal do jogador, para a esquerda ou para a direita.
  - cooldownRemaining: Controla o tempo restante entre cada disparo do jogador.
  - fireRate: Define a taxa de disparo do jogador, ou seja, o número de tiros necessários entre cada disparo.
  - hp: Representa a vida do jogador. Se a vida chegar a 0, o jogo acaba.
* O construtor Player(Texture2D image, Vector2 initialPosition) recebe a textura do jogador e sua posição inicial como parâmetros e inicializa os campos da classe com esses valores.
* No método Update(), primeiro verifica se a vida do jogador é menor ou igual a 0. Se for, o jogo termina, exibindo a sprite de game over e reproduz a música correspondente.
* Em seguida, o método verifica se as teclas pressionadas pelo jogador para movimentá-lo para a esquerda (teclas A ou LEFT) ou para a direita (teclas D ou RIGHT) estão a ser pressionadas e garante também que o jogador não sai da janela de jogo ao atualizar a sua posição.
* Finalmente, o método verifica se a tecla de disparo (espaço) foi pressionada e se o tempo de cooldown permite um novo disparo. Se sim, cria um novo projétil na posição do jogador e reinicia o tempo de cooldown, permitindo disparos regulares. Além disso, reproduz o som do disparo.

## Código
```csharp
     // HEALTH CHECK 
     if (hp <= 0)
     {
         GameManager.inGame = false;
         SpriteArt.gameOver.Play(0.5f, 0.0f, 0.0f);
         hp = GameManager.playerHealth;
         MediaPlayer.Play(SpriteArt.song);
     }
     //Keyboard Controls
     //Side to Side Movement
     //Moving Left
     if (Keyboard.GetState().IsKeyDown(Keys.Left) || Keyboard.GetState().IsKeyDown(Keys.A))
     {
         //Constraint so that it can't move outside the window
         if (pos.X > 0)
         {
             pos.X -= sideSpeed;
         }
     }
     //Moving Right
     if (Keyboard.GetState().IsKeyDown(Keys.Right) || Keyboard.GetState().IsKeyDown(Keys.D))
     {
         //Constraint
         if (pos.X < GameManager.screenWidth - (texture.Width * GameManager.SCALE))
         {
             pos.X += sideSpeed;
         }
     }
     if (Keyboard.GetState().IsKeyDown(Keys.Space) && cooldownRemaining <= 0)
     {
         Vector2 projectileSpawn = new Vector2(pos.X + ((texture.Width * GameManager.SCALE) / 2), pos.Y);
         cooldownRemaining = fireRate;
         EntityCollections.Instantiate(new Bullet(projectileSpawn, SpriteArt.Bullet));

         SpriteArt.shootSound.Play(0.25f, 0.0f, 0.0f);
     }
     if (cooldownRemaining > 0)
     {
         cooldownRemaining--;
     }
```
## Bullet.cs
* A classe "Bullet" vai herdar a classe "Entity" e com isso também vai estar de certa forma ligada com a classe "EntityCollections", pois na classe "Bullet" ao serem adicionados novos projéteis os mesmos têm de ser adicionados à sua respetiva lista durante o decorrer do jogo, com a herdade da classe "Entity" a classe "Bullet" vai assim herdar as características de todas as entidades do jogo tal como a sua textura, a sua posição e os seus métodos de atualização e desenho no ecrã.
  - Nesta classe é que vai ser definida a velocidade das balas quando são disparadas na vertical.
  - O primeiro construtor vai receber a posição inicial e a textura da bala, a posição inicial vai ser atribuida ao campo correspondente na classe que é a pos, e a textura vai ser atribuida ao campo texture, vai ajustar a escala da posição da bala para que a mesma esteja centrada na posição inicial e em seguida vai chamar o método "CreateHitbox" para criar a área de colisão da bala.
  - No método "Update" que vai ser responsável pela a atualização da bala a cada frame, vai mover o projétil na vertical atualizando a sua posição de acordo com a sua velocidade, através de uma verificação se o projétil atingir o limite superior dp ecrã o seu estado vai passar a ser falso para assim o mesmo ser removido do jogo e após isso vai chamar o método "updateHitbox" para atualizar a área de colisão da bala de acordo com a sua nova posição.

## Código
```csharp
     //Bullet speed
     public int BulletSpeed = 8;
     public Bullet(Vector2 inital, Texture2D image)
     {
         texture = image;
         pos = inital;
         pos.X -= (texture.Width * GameManager.SCALE) / 2;
         createHitbox();
     }
     public override void Update()
     {
         pos.Y -= BulletSpeed;
         if (pos.Y <= 0)
         {
             isActive = false;
         }
         //Update hitbox here
         updateHitbox();
     }
 ```
## Enemy.cs
* Herdando a Classe "Entity", podemos criar a classe "Enemy.cs", primeiramente o mais importante será o seu construtor. O construtor é uma peça importante da classe Inimigo, pois permite criar diferentes tipos de inimigos sem ter de reescrever código.
  - O construtor recebe quatro parâmetros e executa as seguintes ações:
    - Define a posição inicial do inimigo numa zona no topo da cena de jogo, de onde o inimigo se irá deslocar para baixo (pos).
    - Recebe uma imagem para usar como textura (texture).
    - Define uma nova velocidade de movimento para o inimigo (dropSpeed).
    - Define a quantidade máxima de pontos de vida do inimigo (health).
    ## Código
      ```csharp
      class Enemy : Entity
      {
        public int health = 2;
        protected float dropSpeed = (1 * GameManager.SCALE);


        //Variables that control OnHit effects
        protected Color tint = Color.White;
        private int hitCooldown = 0;
        private int hitFrames = 10;
        private int knockbackMultiplier = 3;
        
        public Enemy(int width, Texture2D image, float newSpeed, int healthPoints)
        {
            //From Part 4
            pos = new Vector2(width, 0);
            texture = image;
            dropSpeed = newSpeed;
            health = healthPoints;
            //Create the hitbox for the Enemy object
            createHitbox();
        }
      }
      ```
* Para além disso, o "Enemy" terá de se deslocar para baixo no espaço, para isso ele segue um simples Update:
    - Primeiro, ele verifica se a vida atual é igual a 0. Se for, o inimigo remove-se a si próprio.
    - De seguida, verificamos se o Inimigo está fora do campo de jogo enquanto se move para baixo. Se estiver, remove-se a si próprio.
    - Por fim, a posição do Inimigo é atualizada somando a velocidade à sua posição para continuar o seu movimento descendente.
    ## Código
    ```csharp
            public override void Update()
        {
            //When Health is 0, remove itself
            if (health <= 0)
            {
                EntityCollections.score += ((int)dropSpeed - 1);
                SpriteArt.explosion.Play(0.7f, 0.0f, 0.0f);
                isActive = false;
            }
            //When the Enemy reaches the edge of screen, remove itself
            if (pos.Y >= GameManager.screenHeight)
            {
                SpriteArt.hpDown.Play(0.25f, 0.0f, 0.0f);
                isActive = false;
                EntityCollections.player.hp -= 1;
            }
            //Apply movement to the Enemy by updating its position
            pos.Y += dropSpeed;
            //Update hitbox every frame
            updateHitbox();
            OnHitEffect();
        }

        public override void Draw(SpriteBatch spriteBatch)
        {
            spriteBatch.Draw(
                texture,                        //Texture
                pos,                            //Position
                null,                           //What portion of the sprite to draw (Default Draws whole sprite)
                tint,                           //Tint Color
                0f,                             //Rotation
                Vector2.Zero,                   //Origin
                GameManager.SCALE,       //Scale
                SpriteEffects.None,             //Effects
                0f);
        }
    ```
- De seguida a classe "Enemy" será instanciada da mesma forma que a classe "Bullet" no "EntityCollections.cs" para poder ter um maior controlo dos inimigos.
  
## WaveManager.cs
* Esta classe vai ser responsável por adicionar os inimigos de forma automática para que não seja necessário adicioná-los manualmente. Vai se certificar da lógica usada para o aparecimento das várias ondas de inimigos no decorrer do jogo ao aumentar o número de inimigos, a taxa de aparecimento e as ondas.
  - Agora já dentro da classe "WaveManager" são criadas e inicializadas 6 variáveis estáticas que são: wave que vai servir para manter a contagem das ondas de inimigos, a spawnRate que vai determinar a frequência com que os inimgos vão ser gerados, a spawnDelay vai controlar o atrasado entre os aparecimentos dos inimigos, a maxNumEnemies vai armazenar o número máximo de inimigos que podem ser gerados numa única onda e a remainingEnemiesToSpawn que vai manter o número de inimigos restantes para serem gerados na onda que o jogo tiver e a variável "Random" para gerar números aleatórios.
  - No método "InitializeWave" que tal como o nome indica vai servir para inicializar uma nova onde de inimigos, vai ser feita uma verificação usando a classe "EntityCollections" para saber se não há inimigos restantes no jogo e se todos os inimigos foram eliminados na onda que o jogo se encontrar, se assim for o número da onda vai ser incrementado e o número máximo de inimigos para a onda seguinte vai ser atualizado e aumentado em 2 unidades.
  - Através do método "Reset" que mais uma vez como o nome indica vai servir para resetar todos os valores que estejam relacionados com as ondas do jogo. Vai ser feita a definição do número da onda como zero, o número de inimigos restantes por gerar também vai ser zero e vai resetar o número máximo de inimigos outra vez para dois.
  - Com o método "Update" que vai ser chamado a cada frame do jogo para atualizar o gerenciamento das ondas, vai chamar o método criado "InitializeWave" para iniciar uma nova onda se necessário, em seguida verifica se o "spawnDelay" é zero e se não há mais inimigos por gerar, se isso se verificar, é usada uma outra verificação para gerar os inimigos em que todos os que forem par são inimigos nível 1 e os inimigos com número ímpar são nível 2, ao gerar o inimgo vai subtrair um nos inimigos por gerar e de seguida gera o inimigo através do "EntityCollections" de acordo com o seu nível e vai resetar o "spawnDelay" para o "spawnRate", e através de outra verificação se o "spawnDelay" não for zero vai ser subtraido um. Vai repetir esse processo a cada frame de forma a garantir que todos os inimigos sejam gerados de acordo com as configuarções já definidas.
    ## Codigo
    ```csharp
        static class WaveManager
    {
        public static int wave = 0;

        static int spawnRate = 50;
        static int spawnDelay = 0;
        static int maxNumEnemies = 0;
        static int remainingEnemiesToSpawn = 0;

        static Random random = new Random();

        public static void InitializeWave()
        {
            if (EntityCollections.enemies.Count == 0 && remainingEnemiesToSpawn == 0)
            {
                wave += 1;
                remainingEnemiesToSpawn += maxNumEnemies;
                maxNumEnemies += 2;
            }
        }
        public static void Reset()
        {
            wave = 0;
            remainingEnemiesToSpawn = 0;
            maxNumEnemies = 2;
        }

        public static void Update()
        {
            //The method that you created earlier
            InitializeWave();
            //Spawning logic
            //First check if the spawn delay is 0 and 
            //that there are no enemies left to spawn form current wave
            if (spawnDelay <= 0 && remainingEnemiesToSpawn != 0)
            {
                //Every even enemy is a level 1 enemy, every odd enemy is a level 2 enemy
                if (remainingEnemiesToSpawn % 2 == 0)
                {
                    //Subtract 1 from remaining enemies, then spawn the enemy object using EntityManager
                    remainingEnemiesToSpawn -= 1;

                    //Spawn enemy using the Instantiate method from EntityManager
                    EntityCollections.Instantiate(new Enemy(
                        random.Next(128, (GameManager.screenWidth - 128)),    //Position to Spawn Enemy
                        SpriteArt.EnemyTypeOne,                              //Texture for the Enemy
                        GameManager.level1Speed,                             //Speed of the Enemy
                        GameManager.level1Health                             //Health of the Enemy
                    ));
                }
                else
                {
                    remainingEnemiesToSpawn -= 1;
                    EntityCollections.Instantiate(new Enemy(
                        random.Next(128, (GameManager.screenWidth - 128)),
                        SpriteArt.EnemyTypeTwo,
                        GameManager.level2Speed,
                        GameManager.level2Health
                    ));
                }
                //Reset the spawn delay to the spawn rate.
                spawnDelay = spawnRate;

            }
            //If the spawn delay is not 0, subtract 1 from it.
            if (spawnDelay > 0) spawnDelay--;
        }
    }

## UserInterface.cs
* Vai ser responsável por fazer a interface do user no jogo, através de dois métodos:
  - HomeScreen e o gameScreen, o método HomeScreen vai exibir a sprite que aparece antes do jogo iniciar se o jogo ainda não tiver iniciado vai ser exibida uma mensagem para o jogador pressionar a tecla "ENTER" para o iniciar, o método gameScreen vai exibir o ecrã de jogo enquanto o mesmo está a decorrer mostrando assim a interface do jogo como a vida do player, o número da wave em que se encontra e o seu score, obtém as informações sobre o score do player através da classe "EntityCollections" e o número da onda através do "WaveManager".

## Código
```csharp
static class UserInterface
    {
        public static bool hasStarted = false;
        private static string titleString = "My First Game!";
        public static void HomeScreen(SpriteBatch _spriteBatch, SpriteFont font)
        {
            if (!hasStarted) titleString = "My First Game!";   
            else  titleString = "Game Over! | Score :" + EntityCollections.score; 
            _spriteBatch.DrawString(font, "Press ENTER to start a new game!", new Vector2(GameManager.screenWidth / 10, GameManager.screenHeight / 2), Color.WhiteSmoke);
            _spriteBatch.DrawString(font, titleString, new Vector2(GameManager.screenWidth / 10, (GameManager.screenHeight / 2) - 100), Color.Red);
        }

        public static void gameScreen(SpriteBatch _spriteBatch, SpriteFont font) 
        {
            EntityCollections.Draw(_spriteBatch);
            _spriteBatch.DrawString(font, "Score: " + EntityCollections.score, new Vector2(10, 10), Color.White);
            _spriteBatch.DrawString(font, "Wave: " + WaveManager.wave, new Vector2(GameManager.screenWidth - 200, 10), Color.White);
            _spriteBatch.DrawString(font, "HP: " + EntityCollections.player.hp, new Vector2(400, 10), Color.White);
        }
    }
```
## Game1.cs
* Esta é a classe principal do jogo que herda a classe "game" proveniente do monogame, vai ser nesta classe que vai conter os métodos para inicializações, carregar algum tipo de contéudo, atualização e para a arte do jogo.
  - Dentro da classe, existem dois campos privados: "private GraphicsDeviceManager _graphics" que vai fazer o gerenciamento da exibição gráfica do jogo e "private SpriteBatch _spriteBatch" que vai fazer o gerenciamento para carregar as sprites para o jogo.
  - No construtor Game1 vai ser usado para inicializar o jogo através da configuração dos dispositivos gráficos, pois ele vai definir o diretório do conteúdo para o carregamento dos recursos e vai tornar o mouse visível, e através do "GameManager.cs" vai controlar o tamanho da janela do jogo com medidas já inicializadas.
  - Como todos os projetos, existe o método "Initialize" para ser chamado uma vez no início do jogo para executar qualquer atualização que tenha sido feita para o jogo.
  - Tal como a o método "Initialize" vai existir sempre também o método "LoadContent" que vai ser usado para carregar todos os recursos necessários para o funcionamento do jogo, neste caso, vai dar load das texturas do jogo, dos sons que os criadores usaram e também   controla o volume da música de fundo do jogo.
  - O método "Update" vai ser usado para atualizar o estado do jogo e os criadores do jogo fazem uma verificação neste método para saber se o utilizador pressionou a tecla "ENTER" para assim dar inicio ao jogo, se assim for vai dar iniciar o jogo e atualizar as entidades criadas e as ondas de inimigos para quando o jogo está em execução.
  - Através do método "Draw" os autores do código inicializam as sprites para a imagem de fundo e determinam o tamanho da janela para quando o utilizador ainda está no menu antes de dar inicio ao jogo, depois disso fazem a verificação se o jogo está a ser executado e aí vão ser executadas também as funções para desenhar a interface do jogador in-game. 
