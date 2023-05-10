# Executar tarefas administrativas em um contêiner

## O comando mkdir -p ~/fod/simple-task && cd ~/fod/simple-task é uma linha de comando usada no terminal do sistema operacional Linux ou macOS.
Esse comando faz duas coisas:
Cria um novo diretório chamado simple-task dentro do diretório fod na pasta home do usuário (~/). A opção -p cria diretórios pais ausentes também, se necessário.
Navega para o novo diretório simple-task usando cd (change directory).
Em seguida, o comando vim sample.txt é executado para criar um novo arquivo de texto chamado sample.txt dentro do diretório simple-task usando o editor de texto Vim. As linhas de texto são adicionadas ao arquivo sample.txt depois que o arquivo é aberto no editor. As linhas de texto são apenas um exemplo de texto inserido no arquivo.
```sh
mkdir -p ~/fod/simple-task && cd ~/fod/simple-task

vim sample.txt

1234567890
  This is some text
   another line of text
 more text
    final line
```

## --rm: Remove o contêiner quando ele é encerrado.
-it: Habilita a interação do usuário com o contêiner.
-v $(pwd):/usr/src/app: Monta o diretório atual (o diretório atual é obtido pelo comando $(pwd)) como um volume dentro do contêiner no diretório /usr/src/app.
-w /usr/src/app: Define o diretório de trabalho do contêiner para /usr/src/app.
perl:slim sh -c "cat sample.txt | perl -lpe 's/^\s*//'": Inicia o contêiner e executa o comando sh -c com o argumento "cat sample.txt | perl -lpe 's/^\s*//'", que remove espaços em branco no início de cada linha do arquivo sample.txt.
Em resumo, o comando executa um contêiner Docker que remove espaços em branco no início de cada linha do arquivo sample.txt usando Perl. O arquivo sample.txt é lido do diretório atual do host, que é montado como um volume dentro do contêiner. O resultado é impresso na saída padrão do contêiner.
```sh
docker container run --rm -it \
-v $(pwd):/usr/src/app \
-w /usr/src/app \
perl:slim sh -c "cat sample.txt | perl -lpe 's/^\s*//'"
```


## Este conjunto de comandos cria um novo arquivo Python chamado stats.py no diretório simple-task. O arquivo contém o código Python que calcula o número de linhas, palavras e letras em um arquivo de texto especificado pelo usuário.
Aqui está uma explicação do que cada linha do código faz:
python
Copy code
import sys
Importa o módulo sys, que fornece acesso a algumas variáveis usadas ou mantidas pelo interpretador Python.
python
Copy code
fname = sys.argv[1]
Atribui o primeiro argumento passado para o script como o nome do arquivo de texto para analisar.
python
Copy code
lines = 0
words = 0
letters = 0
Inicializa as variáveis lines, words e letters com zero.
python
Copy code
for line in open(fname):
Abre o arquivo especificado pelo usuário e itera sobre cada linha no arquivo.
python
Copy code
lines += 1
letters += len(line)
Incrementa a contagem de linhas e soma o número de caracteres em cada linha.
python
Copy code
pos = 'out'
for letter in line:
    if letter != ' ' and pos == 'out':
        words += 1
        pos = 'in'
    elif letter == ' ':
        pos = 'out'
Itera por cada letra em cada linha e verifica se é um espaço em branco ou não. Quando encontra uma letra não em branco, adiciona 1 à contagem de palavras. A variável pos é usada para verificar se a posição atual está dentro ou fora de uma palavra. Se a letra atual não for um espaço em branco e a posição atual for "fora" de uma palavra, a posição é alterada para "dentro" da palavra e a contagem de palavras é incrementada. Se a letra atual for um espaço em branco, a posição é alterada para "fora" de uma palavra.
python
Copy code
print("Linhas:", lines)
print("Palavras:", words)
print("Letras:", letters)
Imprime o número de linhas, palavras e letras no arquivo de texto analisado.
```sh
cd ~/fod/simple-task

vim stats.py

#stats.py
import sys
fname = sys.argv[1]
lines = 0
words = 0
letters = 0

for line in open(fname):
	lines += 1
	letters += len(line)
	pos = 'out'
	for letter in line:
		if letter != ' ' and pos == 'out':
			words += 1
			pos = 'in'
		elif letter == ' ':
			pos = 'out'

print("Linhas:", lines)
print("Palavras:", words)
print("Letras:", letters)
```


## --rm: Remove o contêiner quando ele é encerrado.
-it: Habilita a interação do usuário com o contêiner.
-v $(pwd):/usr/src/app: Monta o diretório atual (o diretório atual é obtido pelo comando $(pwd)) como um volume dentro do contêiner no diretório /usr/src/app.
-w /usr/src/app: Define o diretório de trabalho do contêiner para /usr/src/app.
python stats.py sample.txt: Inicia o contêiner e executa o comando python stats.py sample.txt, que executa o script Python stats.py no arquivo sample.txt. O resultado é impresso na saída padrão do contêiner.
Em resumo, o comando executa um contêiner Docker que executa o script Python stats.py no arquivo sample.txt, que está localizado no diretório atual do host, que é montado como um volume dentro do contêiner. O resultado é impresso na saída padrão do contêiner. O script Python conta o número de linhas, palavras e letras no arquivo sample.txt e imprime o resultado na saída padrão do contêiner.
```sh
docker container run --rm -it \
-v $(pwd):/usr/src/app \
-w /usr/src/app \
python:3.7.4-alpine python stats.py sample.txt
```
