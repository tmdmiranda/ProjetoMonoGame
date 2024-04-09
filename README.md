# Star Shooter Readme

### Autores do jogo: Alex Jeter and Zachary Mendoza
### Trabalho realizado por: Tiago Miranda-27937;Gonçalo Araújo-27928

O grande objetivo dos autores do desenvolvimento deste jogo era mostrar de forma detalhada e mais simples de como uma pessoa que estivesse a fazer um jogo pela primeira vez em monogame pudesse seguir os passos deles e conseguir criar o seu próprio jogo, onde vai ter de ligar com as Sprites, as classes, os objetos, as colisões, o formato do jogo e até uma forma de ao fim conseguir enviar o seu jogo para os amigos.
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
* Esta é a classe principal do jogo que herda a classe "game" proveniento do monogame, vai ser nesta classe que vai conter os métodos para inicializações, carregar algum tipo de contéudo, atualização e para a arte do jogo.
* Dentro da classe, existem dois campos privados: "private GraphicsDeviceManager _graphics" que vai fazer o gerenciamento da exibição gráfica do jogo e "private SpriteBatch _spriteBatch" que vai fazer o gerenciamento para carregar as sprites para o jogo.
* No construtor Game1 vai ser usado para inicializar o jogo através da configuração dos dispositivos gráficos, pois ele vai definir o diretório do conteúdo para o carregamento dos recursos e vai tornar o mouse visível, e através do "GameManager.cs" vai controlar o tamanho da janela do jogo com medidas já inicializadas.
* Como todos os projetos, existe o método "Initialize" para ser chamado uma vez no início do jogo para executar qualquer atualização que tenha sido feita ao jogo.
