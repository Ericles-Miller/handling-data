# Beguin

As configurações que eu falei, em algumas situações, em Data Science, por exemplo, quando estamos trabalhando com dataset e abrimos esse dataset, a primeira coisa que queremos é ter um contato com ele e esse primeiro contato é justamente visual, eu quero ver esse dataset, as informações que nesse dataset, principalmente na questão das colunas, as variáveis que eu tenho disponíveis no meu dataset para executar o meu projeto.

Nessa visualização eu já posso ter alguma ideia dos tratamentos que eu posso ter que virar a usar no meu dataset, que tipo de informação eu tenho ali e que tipo de tratamento eu posso precisar, por exemplo, eu posso ter uma coluna com strings, outra coluna com números, ou uma mistura dessas duas coisas, eu já vou arrumando a minha cabeça o que eu tenho que fazer para organizar esse meus dados.

Para fazer essas configurações e eu poder visualizar todas essas informações aqui no notebook eu tenho que fazer algumas configurações no Pandas porque por default ele "trunca" o meu dataframe. Quando ele é muito grande, por exemplo, ele vai mostrar as primeiras linhas do meu dataframe, vai colocar reticências e mostrar as últimas, a mesma coisa para as colunas, vai mostrar as primeiras colunas, reticências e as últimas colunas.

**Como eu quero visualizar, eu tenho que mudar essa configuração. A primeira coisa é visualizar como está o default dessas configurações, para fazer isso chamo o Pandas, pd.get_option(),** que mostrará qual a configuração default das opções que quero trabalhar. **A primeira opção é o "display.max_rows"**, ou seja, vai mostrar o número máximo de linhas no meu dataframe, no máximo 60 linhas.

Se for um número maior que isso ele vai "truncar", isto é, vai mostrar as 30 primeiras, trunca, e mostra as últimas 30.

```python
pd.get_option("display.max_rows")
```

**A mesma coisa para "colunas",** só que em "colunas" ele vai dar uma resposta um pouco diferente. Então, vamos copiar a linha 1, colar e mudar de rows para columns, isto é, **pd.get_option("display.max_colums").** Assim, ele mostrará 0, que é o default para que ele configure considerando o tamanho do display, o tamanho da tela

```python
pd.get_option("display.max_rows")
```

Mas não são só essas, temos algumas opções disponíveis na ***documentação neste [link](https://pandas.pydata.org/pandas-docs/stable/user_guide/options.html#available-options)***. Ou seja, você vai ter um conjunto de opções para poder configurar no Pandas. Não vamos falar de todas elas, só das principais.

**Outra forma de visualizar essas opções é através do método pd.describe_option()**. **Se eu não passar nenhum argumento para o describe_option()**, ele vai mostrar todas as **informações existentes, ou seja, ele está praticamente mostrando a documentação**.

**Para eu visualizar uma em específico é só passar a opção para o describe_option()** a função, como, por exemplo, a do número máximo de colunas que visualizaremos: "display.max_columns".

```python
pd.get_option("display.max_colums")
```

```sql
display.max_columns : int If max_cols is exceeded, switch to truncate view. 
Depending on large_repr, objects are either centrally truncated or printed as a summary view. 'None' value means unlimited.

In case python/IPython is running in a terminal and large_repr equals 'truncate' this can be set to 0 and pandas will auto-detect the width of the terminal and print a truncated object which fits the screen width. The IPython notebook, IPython qtconsole, or IDLE do not run in a terminal and hence it is not possible to do correct auto-detection. [default: 0] [currently: 100]
```

**Para fazer essa modificação eu não vou utilizar o get_option()**, eu vou utilizar outro método que é o set_option(). Vou passar exatamente do mesmo jeito só que, no final, eu vou passar a configuração que eu quero que ele tenha agora.

```sql
pd.set_option("display.max_rows", 100)
pd.set_option("display.max_columns", 100)
```

S**e eu quiser voltar para configuração default**, eu posso usar o método reset_option(). Por enquanto, podemos fechar o notebook para não ocupar tanto espaço na tela.

```sql
pd.reset_option("display.max_rows")
```

## Carregando Dados

Se fizermos a leitura colocando dados embaixo, já teremos uma resposta.

```sql
dados = pd.read_json(
path_or_buf=data_json
)

#saida 
A	B	C
0	1	5	9
1	2	6	10
2	3	7	11
3	4	8	12

```

Eu vou deixar mais claro ainda **colocando outro parâmetro que é o orient** só para que você entenda como podemos controlar a forma como isso vai para dentro do data frame.

**O orient vai ser como 'index', isto é, orient='index'.** Ele vai assumir que essas chaves "A", "B" e "C", serão os índices das linhas A, B e C e ele colocará as informações das listas nas linhas 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, e 12.

```sql
dados = pd.read_json(
path_or_buf=data_json,
orient='index')
```

dados
        0	1	2	3
A	1	2	3	4
B	5	6	7	8
C	9	10	11	12

Esse não é o default como acabamos de aprender. **O default seria usá-lo como 'columns', então orient= 'columns'**. Neste caso, ele usaria novamente aos índices, às chaves "A", "B" e "C" como os índices das colunas, olhe o que ele fez.

## Carregando dados XLSX

Esse arquivo tem as informações de valor médio do metro quadrado de um imóvel dentro de um determinado bairro, e para cada tipo de imóvel. É essa tabela que estamos vendo na nossa tela, onde temos as colunas: Bairros, Tipos e Valor do m².

Bairros	Tipos	Valor do m²
São Cristóvão	Casa	13793
São Cristóvão	Apartamento	6306
São Cristóvão	Cobertura	11695
São Cristóvão	Consultório	6991
São Cristóvão	Imóvel Comercial	19633
São Cristóvão	Loja	4995
São Cristóvão	Sala Comercial	8529
Benfica	Casa	17699
Benfica	Apartamento	9074
Benfica	Cobertura	16333
Benfica	Consultório	3838
Benfica	Imóvel Comercial	14527
Benfica	Loja	4989
Benfica	Sala Comercial	6432
Caju	Casa	21226
Caju	Apartamento	9598
Caju	Cobertura	17947
...	...	...

Agora repare nesse arquivo Excel, ele não é aquele arquivo bem organizado CSV

Como ele tem características diferentes, outro ponto importante é em que planilha ele está, ele está na planilha "Preço médio por tipo". Eu copio essa informação, volto no notebook e venho com o parâmetro sheet_name= "Preço médio por tipo", ou seja, o nome da planilha e passo a informação. Mais uma configuração.

```python
bairros = pd.read_excel(
    io = "bairros.xlsx",
        sheet_name="Preço médio por tipo",

)
```

Agora eu tenho que dizer para a minha função em que posição esse dataset, essa informação, está. Na planilha eu vejo que ele está posicionado nas colunas C, D e E, então, eu já posso passar isso para o notebook usando o parâmetro usecols="C:E", ou seja, da coluna C até a coluna E.

```python
bairros = pd.read_excel(
    io = "bairros.xlsx",
        sheet_name="Preço médio por tipo",
        usecols="C:E",

)
```

Uma última configuração, que é justamente os nomes das colunas. Eu tenho três colunas, logo, tenho que colocar três nomes: names=['bairros', 'tipo', 'valor_m2_bairro']. Isso já fecha a minha configuração e eu já consigo fazer a leitura de bairros.

 Vou mostrar para você, vamos ver como isso fica. Rodou. Repare que ele tem 1127 linhas e uma coluna.

```python
bairros = pd.read_excel(
io = "bairros.xlsx",
sheet_name="Preço médio por tipo",
usecols="C:E",
header = 2,
index_col=[0, 1],
names=['bairros','tipo','valor_m2_bairro']
)
bairros
```

![1](https://user-images.githubusercontent.com/59926475/163844698-a95da968-8e3d-41f5-a10e-7fd795453678.png)


## Transformando e Tratando Dados

Vamos continuar o procedimento, agora vamos tratar um pouco o nosso dado. Nós abrimos o arquivo JSON e o colocamos dentro de uma variável dados. Ele ficou nesse formato esquisito.

normal	highlights
output	{'listings':[{'imovel':{'tipos':{'proprieda...	{'listings':[{'imovel':{'tipos':{proprieda...

 Precisamos tratar isso para transformá-lo em uma tabela e trabalhar esse dado sem ter que fazer o processo manualmente.

 O Pandas oferece algumas funcionalidades para nos ajudar, uma delas é essa chamada json_normalize(). Ela identificará que dentro da célula do dataframe tem um arquivo JSON e vai expandir isso, pegar o 'listings', que está como chave, e transformar em uma coluna.

 Se o próximo valor for um JSON, ele vai continuar essa expansão, pegar a chave e transformar em uma coluna, ou seja, ele vai juntar o 'listings' com o 'imovel' e transformar em uma coluna só. Veremos como isso funciona.

 Mas se o json_normalize() encontra, por exemplo, uma variável que acha que é uma variável final, vamos chamar assim, uma lista, ou um número, ou uma string, ele para de navegar no JSON e transforma aquilo em uma coluna. Vamos ver como isso funciona no passo a passo.

 Primeira coisa, vamos pegar os dados, e vamos fazer a leitura primeiro dessa coluna normal. Eu vou chamar de dados_normal = pd.json_normalize(data=dados.normal), significa que chamamos o Pandas ponto JSON normalize e passamos para o parâmetro data a informação do dataset. O dataframe é dados e queremos ler apenas a coluna normal.

 Em seguida, vou chamar dados_normal.head() novamente, para mostrar só os primeiros registros, os cinco primeiros registros. Na verdade, ele não vai ter mais de um registro, ele vai ter só uma informação, por isso, podemos deixar como dados_normal.

```python
dados_normal = pd.json_normalize(data=dados.normal)
dados_normal
#saida
listings
0	[{'imovel':{'tipos:'{'propriedade':'Casa'}...

```

Reparem que ele mostrou uma lista que parece ser uma lista de arquivos JSON. É justamente isso, se eu abrir "Online JSON Viewer" e acessar a coluna normal, perceberei que ele tem o output. No começo ele colocou como a linha, a informação do output está na linha. Isto é, dentro do output temos a lista de arquivos JSON que é justamente o que eu quero ler aqui dentro.

O primeiro passo seria esse: se eu quiser ver melhor essa informação, eu vou chamar o dados_normal['listings'].iloc[0] - "listings" significa listagens em inglês - e eu quero ver essa informação específica, vou selecionar ela com iloc e índice 0, porque ela está no índice zero 0 [{'imovel':{'tipos:'{'propriedade':'Casa'}....

 Eu consigo visualizar uma lista de arquivos JSON, de dicionários, vamos chamar assim, e tem vários, bastante informação.

```python
dados_normal['listings'].iloc[0]
#saida
[{'anuncio': {'descricao': 'Amplo imóvel para venda com 3 quartos, sendo 1 suítes, e 2 banheiros no total.', ... //restante da saída omitida
```

 Estou subindo aqui, não acaba, começa com um colchete, é uma lista de arquivos JSON ou de dicionário. Eu vou fechar para ficar mais claro.

 Se eu quiser saber o tipo de informação, é só usar o 

```python
type(dados_normal['listings'].iloc(0]) 
#e ele vai mostrar que é uma lista, um list.
```

```python
type(dados_normal['listings'].iloc[0])
list
```

 Se eu quiser saber o tamanho, eu posso chamar o len(dados_normal['listings'].iloc[0]). Ou seja, aqui dentro eu tenho 50 arquivos em formato de dicionário, formato JSON. Se eu abrir o “Online JSON Viewer”, eu consigo ver que dentro dessa listings eu tenho de 0 a 49, ou seja, 50. Significa que ele já leu tudo direito, ele está compreendendo como esse arquivo funciona.

```python
len(dados_normal['listings'].iloc[0])
50
```

 O que eu preciso fazer agora é transformar isso em uma tabela. Pegar todas essas informações e jogar como se fossem linhas na minha tabela.

[ Para fazer isso, eu tenho, novamente, o json_normalize(), só que, como eu estou expandindo, eu vou chamar de dados_normal_listings =.

 Chamo novamente o pd.json_normalize() e agora eu passo como data, eu vou passar justamente quem eu acabei de selecionar que é a minha lista de arquivos JSON, então (data=dados_normal['listings'].iloc[0]} e vou rodar isso.

 Novamente eu esqueci de mostrar o resultado dados_normal_listings. 

Olha que legal, ele já criou a minha tabela, com as informações todas no formato tabular.

```python
dados_normal_listings = pd.json_normalize(data=dados_normal['listings'].iloc[0])
dados_normal_listings
```

saida

imovel.tipos.propriedade	imovel.endereco.bairro	imovel.endereco.type	...
0	Casa	Barra da Tijuca	Point	...
1	Apartamento	Campo Grande	Point	...
2	Cobertura	Barra da Tijuca	Point	...
3	Cobertura	Barra da Tijuca	Point	...
4	Sala Comercial	Glória	Point	...
...	...	...	...	...
//restante da tabela omitida

Isso é bem legal. Reparem nos títulos que ele deu para as colunas, imovel.tipos.propriedade. Por que ele está fazendo isso?

 Porque ele está chegando no "imovel > tipos > propriedade:'Casa'", isto é, a informação está dentro de "tipos > propriedade:'Casa'", portanto, ele fez essa junção. Eu posso, ao invés de ponto em imovel.tipos.propriedade, usar a informação sep='_'), para usar um separador diferente. Vamos testar com um underscore e testar rodando de novo. Agora temos imovel_tipos_propriedade.

```python
dados_normal_listings = pd.json_normalize(data=dados_normal['listings'].iloc[0], sep='_')
dados_normal_listings
```

imovel_tipos_propriedade	imovel_endereco_bairro	imovel_endereco_type	...
0	Casa	Barra da Tijuca	Point	...
1	Apartamento	Campo Grande	Point	...
2	Cobertura	Barra da Tijuca	Point	...
3	Cobertura	Barra da Tijuca	Point	...
4	Sala Comercial	Glória	Point	...
...	...	...	...	...
//restante da tabela omitida

Outra coisa legal, que eu vou deixar como informação, é o número máximo de níveis que eu consigo descer. Reparou que ele foi carregando nos níveis do JSON, primeiro imóvel, depois tipos, depois propriedade, endereço, ele foi navegando.

 Eu posso limitar isso, indicar que ele navegue até, no máximo, o nível 1, max_level=1).

```python
dados_normal_listings = pd.json_normalize(data=dados_normal['listings'].iloc[0], sep='_', max_level=1)
dados_normal_listingsC
```

imovel_tipos	imovel_endereco	imovel_vagasGaragem	...
0	{'propriedade': 'Casa'}	{bairro': 'Barra da Tijuca', 'localizacao':{...	4	...
1	{'propriedade': 'Apartamento'}	{bairro': 'Campo Grande', 'localizacao':{'ty...	1	...
2	{'propriedade': 'Cobertura'}	{bairro': 'Barra da Tijuca', 'localizacao':{...	2	...
3	{'propriedade': 'Cobertura'}	{bairro': 'Barra da Tijuca', 'localizacao':{...	1	...
4	{'propriedade': 'Sala Comercial'}	{bairro': 'Glória', 'localizacao':{'type': '...	1	...
...	...	...	...	...
//restante da tabela omitida

 Agora ele só navegou até certo nível, ele trouxe para mim a informação de "imóvel > tipos > endereço" e parou.

 Reparem que as informações estão dentro de arquivos JSON, de dicionários, podemos ir variando isso conforme a necessidade, max_level=2).

```python
dados_normal_listings = pd.json_normalize(data=dados_normal['listings'].iloc[0], sep='_', max_level=2)
dados_normal_listings
```

imovel_tipos_propriedade	imovel_endereco_bairro	imovel_endereco_localizacao	imovel_vagasGaragem	...
0	Casa	Barra da Tijuca	{'type': 'Point', 'coordinates': [-43.3039086, -23.0139692]}	4	...
1	Apartamento	Campo Grande	{'type': 'Point', 'coordinates': [0, 0]}	1	...
2	Cobertura	Barra da Tijuca	{'type': 'Point', 'coordinates': [-43.3037186, -22.9951304]}	2	...
3	Cobertura	Barra da Tijuca	{'type': 'Point', 'coordinates': [-43.3548121, -23.0097423]}	1	...
4	Sala Comercial	Glória	{'type': 'Point', 'coordinates': [-43.1779703, -22.9174894]}	1	...
...	...	...	...	...	...
//restante da tabela omitida

Vou deixar como informação. Naveguei dois níveis, agora ele já abriu casa, propriedade, mas a localização continua com a mesma informação, se eu não colocar isso, ele vai ler tudo.

 Agora lembra que eu falei que tem como fazermos isso de uma vez só. Nós fizemos o passo 2, mas antes, tivemos que fazer o passo 1. Fizemos o json_normalize() para depois chegar no passo 2. Eu criei o dados_normal para depois criar o dados_normal_listings.

 Eu posso criar o dados_normal_listings de uma vez só usando um artifício bem simples, porque estamos trabalhando com este formato que eu vou copiar para ficar mais fácil:

 A única coisa diferente vai ser, reparem que eu coloquei o separador , é uma coisa, mas eu usei o dados.normal. Fui lá no meu arquivo "dados" e peguei a coluna normal, que é justamente a coluna inicial, com o arquivo cru dados que vimos no início desta aula.

Eu disse para ele que esse parâmetro record_path seria o meu 'listings', é justamente o meu grupo de listagens, o array, a lista onde estão todas as informações que eu quero colocar como linhas na minha tabela.

 Se eu passar esse parâmetro 'listings', ele vai fazer todo o processo que fizemos de uma vez. Fará a leitura do arquivo sem eu ter que fazer essas várias etapas.

```python
dados_normal_listings = pd.json_normalize(data=dados.normal, sep='_', record_path=['listings'])
dados_normal_listings
```

imovel_tipos_propriedade	imovel_endereco_bairro	imovel_endereco_localizacao_type	...
0	Casa	Barra da Tijuca	Point	...
1	Apartamento	Campo Grande	Point	...
2	Cobertura	Barra da Tijuca	Point	...
3	Cobertura	Barra da Tijuca	Point	...
4	Sala Comercial	Glória	Point	...
...	...	...	...	...
//restante da tabela omitida

 Eu posso fazer isso também para a coluna highlights, porque ela tem o mesmo formato, só tem menos informações, mas ela tem a mesma formatação. Então, ao invés de normal eu vou chamar de highlights.

 Os highlights são os destaques, os anúncios que provavelmente alguém pagou um pouco a mais para ter um destaque no site. Então, no lugar de dados.normal eu vou usar dados.highlights, que é a segunda coluna.

 Mesma coisa record_path=[‘listings’] usando separador, vou rodar e olha lá, o meu arquivo. Reparem que agora eu tenho menos linhas, tenho 20 linhas só. É tudo igual, casa, Engenho Novo é o nome do bairro.

```python
dados_highlights_listings = pd.json_normalize(data=dados.highlights, sep='_', record_path=['listings'])
dados_highlights_listings
```

imovel_tipos_propriedade	imovel_endereco_bairro	imovel_endereco_localizacao_type	...
0	Casa	Engenho Novo	Point	...
1	Cobertura	Vargem Grande	Point	...
2	Imóvel Comercial	Ribeira	Point	...
3	Apartamento	Praça Seca	Point	...
4	Apartamento	Honório Gurgel	Point	...
...	...	...	...	...

Temos algumas informações que estão no formato de lista mesmo, eu quero que fique nesse formato, vamos aprender a trabalhar com cada uma delas, vamos aprender a juntá-las de várias formas.

 Era isso nesse vídeo. No próximo vídeo vamos começar a entender como trabalhamos com dados no formato de texto dentro de objetos do Pandas. Até lá.

## Trabalhando com Dados Textuais

Algo interessante que temos aqui é justamente esse conjunto de informações, imovel_caracteristica_propriedade. São características da propriedade, do condomínio e do entorno, todas elas em listas, ou seja, são várias características que cada imóvel possui. Isso é bastante interessante de trabalharmos.

 Repare, ele está, pelo menos visualmente, no formato de lista. Vamos ver se isso realmente está no formato de lista.

 Eu vou criar uma variável chamada lista_str =, lista string, chamar novamente o dados_normal_listings, fazer uma seleção .loc[] e selecionar a primeira informação do imóvel_caracteristicas_propriedade. Eu vou selecionar a primeira linha, vou chamar o índice 0 e como eu chamei o loc[], passarei também um label. Depois, exibiremos com lista_str.

```python
lista_str = dados_normal_listings.loc[0,'imovel_caracteristicas_propriedade']
lista_str
```

![2](https://user-images.githubusercontent.com/59926475/163844699-1fea8b67-af72-4996-a2ed-a7cfada7b608.png)


Se quisermos fazer a leitura do primeiro item da lista, por exemplo, eu faria uma lista, passaria o índice e leria: lista_str[0].

```python
lista_str[0]
# saida 
'['
```

Como se trata de uma string, ele vai ler cada caractere da string. Não é isso que eu quero, eu quero conseguir ler cada item dentro dessa lista, portanto, preciso transformar essa informação em uma informação válida, uma informação de lista, de string para lista.

Farei isso usando alguns métodos de string do próprio Python e depois veremos como fazer isso no Pandas. Só para iniciarmos, farei no Python, como você viu, conseguimos fazer a seleção de cada item, para eu transformar o arquivo em uma lista, a primeira coisa que eu quero é eliminar os colchetes, porque estão atrapalhando.

Para eliminar os colchetes, eu posso copiar o lista_str[] e fazer uma seleção dentro das chaves, eu quero começar a minha seleção no item 1 e quero terminar no -1,que vai eliminar o último item da minha lista, lista_str[1, -1]. Ele eliminou os colchetes do começo e do final.

```python
lista_str[1:-1]
```

![3](https://user-images.githubusercontent.com/59926475/163844701-294c6ae0-5465-4e43-9e48-3bbb5a5b9191.png)


Outra forma de fazer isso, seria fazer com o método .strip(), eu passo para os parênteses as informações que eu quero que ele remova do começo e do final da lista, ou seja, os colchetes, lista_str.strip("[]"). Todos os colchetes que ele encontrar no começo e no final serão eliminados, gerando a mesma resposta anterior, você escolhe o que quiser utilizar.

```python
lista_str.strip("[]")
```

![4](https://user-images.githubusercontent.com/59926475/163844704-d8213b17-af4f-40b4-9280-70ea44d56934.png)


Próximo passo: precisamos eliminar as aspas, porque isso é uma string dentro de uma string, eu quero eliminar todas essas aspas simples.

Eu vou usar o lista_str[1: -1] de cima. Outro método para retirar coisas e substituir por outras é o .replace(), método de string com o qual conseguimos fazer as substituições.

Vou passar aspas duplas ("'", ""), eu quero que as aspas simples (') sejam substituídas por nada, ou seja, vou passar aspas duplas com nada dentro, lista_str[1: -1].replace("",""). Ele vai rodar, agora eu tenho as informações separadas por vírgula e um espaço, cada item está separado por vírgula e um espaço.

```python
lista_str[1:-1].replace("'", "")
```

![5](https://user-images.githubusercontent.com/59926475/163844706-468462c4-9c30-4902-b5ff-87f5612e6b31.png)


Para eu criar uma lista com isso agora eu tenho outro método de string que me ajuda, o .split(), ou seja, eu pego todo o código, reparem que eu estou encadeando, estou fazendo o encadeamento, e no final passo o .split() e caractere que eu quero que ele faça os cortes, lista_str[1:-1].replace("'", "").split("")

```python
lista_str[1:-1].replace("'", "").split(", ")
```

## Atributo STR

O replace não pode ser aplicado em uma series, se eu quiser encadear isso, fazer essa concatenação, eu tenho que, novamente, chamar o atributo STR e então chamar o replace.

Vou copiar o código do Texto, vou deixar esse como exemplo, e vou fazer o correto aqui em baixo. 

```python
Texto.str[1:-1]., aqui no final eu chamo str.replace("'", "").
Texto.str[1:-1].str.replace("'", "")
```

 Sempre que eu tiver no retorno uma series, eu preciso chamar novamente o STR para aplicar o outro método de string. Agora ele já funcionou.

 O próximo passo é fazer o split que vai fazer a separação de cada item e colocar dentro de uma lista, e teremos os dados em formato de lista, eu chamo novamente o .str.split e passo para ele o separador, (", "), o que fizemos no vídeo anterior" “Enter".

```python
Texto.str[1:-1].str.replace("'", "").str.split(", ")
```

 Tenho o meu dado no formato de texto. Eu vou salvar o código em uma series com o mesmo nome, na linha abaixo, Texto "Enter".

```python
Texto.str[1:-1].str.replace("'", "").str.split(", ")
Texto

```

 Já temos essa informação. Se eu digitar type(Texto[0]), eu vou ver que esse dado está no formato de lista, o primeiro item está no formato de lista.

 O que falta agora é justamente aplicar todas essas funcionalidades nas três colunas que temos com o mesmo problema no nosso data frame. Isso aprenderemos no próximo vídeo e usaremos um novo método do Pandas para nos ajudar. Até lá.

## Convertendo strings em Listas

### Filter

Reparem que todas são iguais e todas têm o mesmo problema, eu já testei, e todas têm o nome parecido, todas elas começam com "imovel_caracteristicas" e depois um underscore e a parte mais específica, "propriedade", "condomínio" e "entorno".

 Eu posso pegar a informação "imovel_caracteristicas". O objetivo é fazer isso de maneira mais rápida, já que poderíamos ter muito mais variáveis do que só três.

 A partir disso, vou mostrar uma forma de filtrarmos e criarmos subconjuntos de um dataframe, um método para fazermos subconjuntos de linhas ou de colunas. O default são as colunas, agora queremos as colunas.

É o método .filter(). Tem a ajuda também na [documentação neste link.](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.filter.html) Ele cria justamente esse subconjunto passando algumas informações do nome, do label das variáveis, o parâmetro que eu vou configurar é o like= e eu passo justamente a parte inicial do label da coluna.

```python
dados_normal_listings.filter(like='imovel_caracteristicas')
```

Se eu rodar isso, ele faz um subconjunto justamente dessas variáveis com que estamos trabalhando.

 Ele me trouxe somente as três colunas que eu quero fazer o tratamento. E se eu, ao final, acrescentar o .columns, eu terei simplesmente o nome dessas colunas.

```python
columns = dados_normal_listings.filter(like='imovel_caracteristicas')
```

E eu vou criar um for column, preste atenção que é no singular, eu posso usar qualquer nome válido do Phyton, in columns, eu estou varrendo ele. Vamos fazer um exemplo mais simples, vou fazer o print(column).

```python
columns = dados_normal_listings.filter(like='imovel_caracteristicas')

for column in columns: 
    print(column)
```

Eu vou printar o nome das variáveis com que eu quero trabalhar. A partir daqui, eu posso aplicar todos os métodos que vimos nos vídeos anteriores, eu posso inclusive copiar eles de cima, copiar todos eles .str[1:-1].str.replace("'", "").str.split(", "). Mas eu vou aplicar em cima de uma coisa diferente. Eu vou chamar o meu dados_normal aqui em baixo.

 Vou abrir o indexador e vou passar para ele justamente o nome da coluna [column] =, vai ser igual a series dados_normal_listings[column]' aplicado a todos os procedimentos que trabalhamos até agora.

```python
columns = dados_normal_listings.filter(like='imovel_caracteristicas')

for column in columns: 
    dados_normal_listings[column] = dados_normal_listings[column].str[1:-1].str.replace("'","").str.split(",")
```

Podemos fazer isso também, não só para o `normal`, mas para o `highlights`, já resolvemos dois problemas de uma vez só, eu vou substituir o `normal` por `highlights`.

 Deixa eu copiar o nome aqui em cima pode ser que eu esteja digitando errado, então vou vir aqui em cima, onde estão os meus documentos, `normal`, vai ser justamente esse nome que eu estou usando, `normal`, está aqui `highlights`, é isso mesmo.

```python
columns = dados_normal_listings.filter(like='imovel_caracteristicas')

for column in columns: 
    dados_normal_listings[column] = dados_normal_listings[column].str[1:-1].str.replace("'","").str.split(",")
        dados_highlights_listings[column] = dados_highlights_listings[column].str[1:-1].str.replace("'","").str.split(",")
```

Eu estou aplicando isso para as colunas dos dois *data frames*, se eu rodá-lo, ele vai executar, não vai mostrar nada para mim.

 E eu posso simplesmente fazer agora uma seleção simples, a que eu estava tentando fazer e ele estava selecionando só um dos colchetes.

 Do dados_normal, eu estou selecionando do "imovel_caracteristicas_propriedade" só a primeira linha, o primeiro item, que vai ser a `'Área de Serviço'`.

code

```python
columns = dados_normal_listings.filter(like='imovel_caracteristicas')

for column in columns:
    dados_normal_listings[column] = dados_normal_listings[column].str[1:-1].str.replace("'","").str.split(",")
        dados_highlights_listings[column] = dados_highlights_listings[column].str[1:-1].str.replace("'","").str.split(",")
```

```python
dados_normal_listings.loc[0, 'imovel_caracteristicas_propriedade'][0]

# saida 
#'Area de Servico'
```

Agora eu consigo fazer seleções de informações dentro, eu estou selecionando o item dentro da lista.

 Agora eu já consigo fazer isso e o meu dado já está tratado para os dois arquivos, o `normal` e o `highlights`. O tratamento igual.


