Curso da DIO com conceitos básicos sobre Git e GitHub.

# 1. Introdução ao Git e Github
5h

Quando for aprender uma tecnologia, se perguntar: Por quê essa tecnologia é importante ?
Quando estamos escrevendo um arquivo e submetemos ele a uma revisão, são sugeridas mudanças. Daí nós pegamos esse em que foram feitas mudanças e transformamos no atual, deixando o antigo guardado para consulta talvez. Isso pode ocorrer várias vezes.
Git é um software de versionamento de código. Ele permite que várias pessoas editem um código. Nasceu a partir de Linus enquanto criava o Linux.
Github é a versão Microsoft do Git, porém são tecnologias diferentes. O primeiro é pago e o segundo é opensource.
Benefícios das tecnologias: controle de versão, armazenamento em nuvem (o repositório em si), trabalho em equipe, melhorar seu código, reconhecimento.
 
 
# 2. Navegação via command line interface e instalação
### Comandos Básicos
A forma de interagir com ele é CLI (comand line interface) e não GUI (guide user interface). Ou seja, é feita por códigos e não por recursos visuais.
O Windows e Unix diferenciam em algumas coisas.
Windows/Linux
  - Listar: comando DIR/LS
Lista os diretórios (pastas), usado para nos situar.
 	- CD: Usado para navegar em um lugar específico. CD / ...
Se usarmos o DIR dentro dessa pasta ele mostra o que tem nela.
 	- CD . . : retrocede um nível de pasta.
 	- CD (primeira letra da pasta) + TAB : completa o nome da pasta identificando o que tem no local em que estamos que começa com essa letra. Evita erros de digitação.
  - CLS/CLEAR ou Ctrl+L: Limpa o terminal (que vai acumulando informação conforme vamos usando)
  - MKDIR: Cria uma pasta
Ex: mkdir workspace
 	- >ECHO: Repete no terminal o que escrevemos
  ">" : dá o output do que escrevermos pra outra coisa
Ex: echo hello (printa hello)
echo hello > hello.txt (se já tiver um arquivo com esse nome o terminal vai avisar). Isso será criado na pasta em que estivermos.
del ... : deleta arquivos
Seta pra cima navega nos comandos dados anteriormente.
rmdir ... /S /Q \ rm -rf : remove o repositório e os arquivos dentro dele.
 
### Realizando a instalação do GIT
Site oficial do GIT
 
 
# 3. Entendendo como o GIT funciona por baixo dos panos
### Tópicos fundamentais para entender o funcionamento do GIT
  - SHA1
A sigla significs Secure Hash Algorithm, é um conjunto de funções hash criptografadas projetadas pela NSA (Agência de Segurança Nacional dos EUA).
Ele pega o arquivo e embaralha ele de uma forma específica. A encriptação gera um co junto de caractéres identificador  de 40 dígitos. Se pegarmos um arquivo e rodarmos esse algorítmo ele gera um identf de 40 dig específico, se fomos nesse arquivo e adicionarmos uma virgula e rodarmos denovo, ele gera um ident diferente. E se fomos lá denovo e retiear essa virgula e rodar o alg ele volta ao identf que era antes.
Dessa forma pode-se identificar arquivos de uma forma rápida e segura.
### Objetos internos do GIT
BLOBS, TREES, COMMITS
São objetos que dão versionamento prp código.
  - BLOBS
echo 'conteúdo' | git hash-object  - -stdin
Passa o output (que é conteúdo) para uma função do git que é hash-object. O stdin é apenas porque essa função espera receber um arquivo e estamos falando que estamos enviando texto.
Isso gerará um ident sha1
Fazendo agora de outra forma:
echo -e 'conteudo' | openssl sha1
Ele gerará um sha1 diferente. Isso porque o jeito que o Git lida com isso é através de objetos. Os arquivos ficam guardados dentro desse objeto Blob que contém metadados dentro dele.
Ele vai ter o tipo do objeto, o tamanho desse arquivo, um \0, e o conteúdo de fato, seja texto, binário, etc.
Blob   	tamanho: 42
\0
Olá mundo
Mas, se passarmos o metadado para a string, ele irá gerar o sha1 correto:
echo -e 'blob 9\0conteudo' | openssl sha1
  - Tree
Armazenam Blobs e apontam os tipos diferentes.
É como uma ascenção, onde os blobs são o mais básico, as Trees os armazenam e por fim os commits.
A Tree também contém metadados e aponta para um blob:
Tree
\0
blob	sa4d8s	texto.txt
A Tree guarda o nome do arquivo também (o blob só o sha1). Ela são responsáveis por montar toda a estrutura de onde estão esses arquivos, podendo aportar também para outras Trees. Isso faz sentido num programa, um diretório pode conter outros diretórios, é um tipo de objeto recursivo.
As Trees também tem um sha1 de seu metadado. Ou seja, o blob dentro dela tem seu sha1, e ela tem um para seu metadado. Se mudarmos algo em um arquivo, isso altera o sha1 desse blob e, consequentemente, altera o sha1 da árvore.
 
  - Commit
Commit  	<tamanho>
tree             	s4a5sq1
parente      	a98acq1
autor           	perkles
mensagem 	"inicia..."
timestamp
É o objeto mais importante. Ele junta tudo, dá sentido a alteração que estamos fazendo.
Ele aponta para:
Uma tree
Para um parente (último commit realizado amtes dele)
Para um autor (quem fez a alteração)
Para uma mensagem (explica a alteração, os montes de arquivos dentro das várias pastas)
Carimbo de tempo (timestamp) (data e hora de quando foi criado
Também possui um sha1 que pode mudar se qualquer arquivo for alterado. Por isso ele é tão confiável.
 
 
### Chaves SSH e Tokens
  - Chave SSH
Forma de estabelecer uma conexão segura e encriptada entre duas máquinas. Vamos nos conectar com o servidor do Github e vamos configurar nossa máquina local como uma máquina confiável pelo Github, estabelecendo a conexão com 2 chaves, uma pública e uma privada.
Pegamos a chave pública e colocamos no Github, e através disso ele vai reconhecer os repositórios e poderemos mandar tudo o que quisermos.
A SSH é feita nas configurações do site do Github, e através do CLI nós ligamos essa chave na nossa máquina.
Através do Gitbash no nosso computador, criamos nossa chave. Comandos:
ssh-keygen -t ed25519  -C  email@gmail.com
Ele vai salvar a chave em um arquivo do computador (começado com . - ex: .ssh/nome da chave - nos Users quer dizer que é uma pasta segura).
Ele pede para digitar 2 vezes a senha e a cria, colocando .pub no arquivo nome da chave pública, que diferencia da privada. Informa o tipo de criptografia.
Nós pegamos essa chave pública e levamos ao Github. Na aba de SSH keys, colocamos no título onde ela foi criada, e no conteúdo o que fopiamos  depois colocamos para gerar e ele pedirá esse password, ao colocarmos ele salvará essa chave no Github.
Após isso, voltamos ao CLI e acionamos o SSH agent, que é uma entidade que é responsável por lidar com essas chaves.
eval $(ssh-agent -s)
Ele gerará um agente pid número. Que significa que foi iniciado.
Dentro da pasta do ssh podemos passar a chave direto
ssh-add nome da chave privada
Ele pedirá a senha previamente criada para essa chave.
A partir desse momento, quando temos uma chave ssh na máquina não podemos simplesmente copiar um repositório do Github, precisamos usar o caminho da ssh:
< > Code -> local -> ssh -> copiar a url ->
No terminal:
git clone colar
Pode aparecer uma mensagem de "erro", digitamos yes.
 
  - Token de acesso pessoal
Segunda forma de identificação de acesso. Se assemelha mais ao sistema nickname e password.
Geramos um token no github e o guardamos. Sempre que fizermos um commit, o Git vai pedir o usuário e senha, colocamos o do github normal, e na senha o token.
Nas configurações do Github vamos em gerar um novo token, podendo colocar data de espiração.
Marcamos a opção repo em caso de desconhecinento. O Git irá gerá-lo e devemos copiar e guardar porque ele não será mostrado novamente.
Para copiar um repositório:
<> Code -> Local -> https -> copiar
No terminal
git clone colar
Pedirá o token como senha
 
 
 
 
# 4. Iniciando o Git e criando um Commit
  - git init
Inicia o respositório no git. Quando abrimos o terminal, nós chamamos o programa que vamos trabalhar (todo comando começa com o nome do programa, git ...).
  - git add
Mover arquivos, e começar a dar início no versionamento e conhecer os primeiros comandos.
  - git commit
 
Vamos até a pasta em que queremos começar a versionar o código.
git init
Isso criará uma pasta .git que até então estará vazia. Para visualizarmos ela digitamos -a. Conforme vamos trabalhando serão criados arquivos nela.
Se for a primeira vez que estamos começando, ele irá pedir algumas configurações (um nickname - autor -, e email).
git config --global user.email "email"
git config --global user.name nome
Colocamos global para servir para outros trabalhos, mas podemos definir configurações específicas para esse repositório.
- Adicionado um arquivo
git add *
git commit -m ""
| É a mensagem com explicação do commit e alterações.
Ele gera alguns caracteres (7) (sha1) que identifica aquele commit.
 
 
# Ciclo de vida dos arquivos no git
### Passo a passo no ciclo de vida
git init = cria-se um repositório
  - Tracked and untracked
Tracked (rastreáveis pelo git, ele tem ciência deles):
unmodified (ainda não foi modificado); modified (que sofreu alteração); staged (ex: backstage seria o que acontece atrás do palco, já o staged é onde ficam os arquivos se preparando pra fazer parte de outro tipo de agrupamento)
Untracked: o git ainda não sabe da existência dele
Quando usamos o git add, ele passa o arquivo para o staged. Quando editamos algo dentro do arquivo, ele passa de unmodified para modified (o git vê pelo sha1). Se rodarmos o git add dnv nesse modified ele vai para o staged, esperando para executar alguma ação. Se removemos o arquivo, ele volta para untracked.
O arquivo no staged está se preparando para um commit. E quando damos esse commit, eles deixam de ser staged e viram commit, que retorna todos os arquivos para unmodified, porque é como se tirássemos uma foto do arquivo e agora eles aguardam novas modificações, onde usamos o git add para mexer nele novamente. Tudo isso é um cíclo.
Temos 2 ambientes:
  - Ambiente de desenvolvimento
É o que está na nossa máquina, contém o work directory, staging area, local repository.
O que alteramos no código não muda o do nosso repositório no servidor. Ele fica rodando entre o working direc e a staging area até executarmos um commit e mandarmos ele para um local repository. Ele ficará lá até o "empurrarmos" para o repositório remoto. Para isso temos que executar uma série de comandos.
Quando fazemos um commit, mandamos o arquivo para o local repository onde eles vão atuar. Logo, ele é populado por commits.
  - Servidor
Repositório remoto.
 
*git status : ajuda a monitorar o status do arquivo (se é untracked, modified, unstaged, etc).
*mv nome do adquivo./nome do repositório : muda o arquivo de local.
O git só toma ciência do arquivo após o commit.
*git add */. : adiciona tudo que está no working directory no stage.
# Trabalhando Com Github
git config --global ---unset user.nickname/email
Para refazer o name e email. Quando for fazer o commit ele vai pedir novamente para refazer o código de cadastrar o email e name.
Geralmente coloca-se um readme (o mais usado no github é markdown) quando criamos um repositório. Ele diz detalhes do projetos. No githib também ele fornecerá uma url para que façamos o link entre o repositório local e o remoto, assim os commits vão para lá.
Para linkarmos, copiamos a url do github. Vamos no bash, no nosso repositório do projeto e usamos esse codigo:
git remote add origin colar link
git remote -v
Mostra os repositórios cadastrados.
Para empurrar os commits ao repositório remoto:
git push origin master
É possível criar readmes na plataforma e o github faz um commit para eles, porém eles não estarão no repositório de trabalho.
# Resolvendo Conflitos
### Como os conflitos acontecem no github
Conflito de merge: quando 2 ou mais pessoas pegam o código do github e fazem alterações na mesma coisa. Quando uma pessoa pusha o cód dela o github aceita, porém quando outra tentar, o github vê como uma versão mais antiga e ao tentar juntar os 2 vê conflitos no código, daí devemos fazer o pull (onde mostrará a mwmsagem conflict merge) e resolver qual a versão correta e então fazer o push.
Quando tentamos abrir o arquivo em conflito, talvez ele mostre algo bagunçado por causa da mescla das alterações ou da forma que é interpretado.
Após fazer as alterações, fazemos o git add e git commit com uma mensagem explicativa.
git clone url
Clonamos o código que encontramos no github. A diferença entre baixar o zip e clonar é que clonando ele vem como um repositório.

