# Executar tarefas administrativas em um contêiner

##
```sh
mkdir -p ~/fod/simple-task && cd ~/fod/simple-task

vim sample.txt

1234567890
  This is some text
   another line of text
 more text
    final line
```

##
```sh
docker container run --rm -it \
-v $(pwd):/usr/src/app \
-w /usr/src/app \
perl:slim sh -c "cat sample.txt | perl -lpe 's/^\s*//'"
```


##
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


##
```sh
docker container run --rm -it \
-v $(pwd):/usr/src/app \
-w /usr/src/app \
python:3.7.4-alpine python stats.py sample.txt
```
