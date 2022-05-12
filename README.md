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


Agora eu consigo fazer seleções de informações dentro, eu estou selecionando o item dentro da lista.

 Agora eu já consigo fazer isso e o meu dado já está tratado para os dois arquivos, o `normal` e o `highlights`. O tratamento igual.

 

Outra situação que pode acontecer é pegarmos dados de outra fonte e juntar também no nosso *dataset*. Seria uma junção mais lateral, veremos isso nos próximos vídeos.

 Voltando no nosso assunto, aprenderemos como fazer essa junção, esse empilhamento primeiro. Eu tenho os dois *dataframes*, o *normal* e o *highlights*, visualmente eu percebo que eles têm as mesmas características, as mesmas variáveis.

 Podemos testar isso fazendo um teste simples, eu posso pegar o nome do meu primeiro *data set*, as colunas e igualar às colunas do segundo.

Veremos se eles têm o mesmo tamanho e as mesmas colunas. Agora basta aprertar "Shift + Enter".

```python
dados_normal_listings.colums == dados_highlights_listings.columns
```

> array([ True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True])
> 

Ele me retorna um *array* cheio de *trues*, o que indica que eles têm a mesma dimensão e todas as colunas são iguais. Então posso proceder com o meu *append* sem o menor problema.

Como eu disse, o *append*, que é o empilhamento, um dos métodos do Pandas tem justamente esse nome, temos a [documentação do `append()` aqui](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.append.html), e a [documentação do `concat()` neste link](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.concat.html) que está logo embaixo.

 O `append()` funciona da seguinte forma: eu pego um dos meus *dataframes*, posso escolher qualquer um dos dois, `dados_normal_listings`, ele seria o *dataframe* base, eu chamo o método `append()` desse *dataframe* e passo o segundo como argumento, quem eu quero empilhar com esse *dataframe*. No nosso caso, o `dados.highlights_listings`.

```python
dados_normal_listings.append(dados_highlights_listings)
```

![Captura de tela de 2022-05-03 10-31-35](https://user-images.githubusercontent.com/59926475/168146470-34b48413-2d8c-42c1-98b1-9a32d9701dc6.png)


É só rodar e ele vai fazer tudo acontecer. Repare, ele tem agora 70 linhas, ele juntou os dois, um de 50 e um de 20 linhas, mas tem uma coisa estranha, o índice começa com 0, e segue com 1, 2, 3, 4, ele tem aquela truncada que já conhecemos, e termina no 19. Na minha ideia, ele teria que terminar no 69.

Temos que fazer uma configuração extra para resolver esse problema. Eu vou pegar esse mesmo código que eu fiz, `dados_normal_listings.append(dados_highlights_listings)`
 e vou colocar uma configuração extra nele.

A configuração é o `, ignore_index=True`
, significa que vou configurar isso como *true* . Agora ele vai ignorar os índices dos dois *data frames*
 e vai criar um índice novo.

```python
dados_normal_listings.append(dados_highlights_listings, ignore_index=True)
```

![2](https://user-images.githubusercontent.com/59926475/168146422-158850d5-7fec-4511-a4eb-f0c6a9d4bc4c.png)


Reparem que agora ele vai do 0 e termina no 69. Para terminar, eu simplesmente preciso criar um novo *dataframe* para jogar essa informação dentro dele, porque aqui é só uma visualização.

 Eu vou chamar esse novo *dataframe* de `dados_listings =` e vou igualar a isso que eu acabei de fazer.

```python
dados_listings = dados_normal_listings.append(dados_highlights_listings, ignore_index=True)
dados_listings
```

![3](https://user-images.githubusercontent.com/59926475/168146428-3877f625-6a12-4db6-847b-f777e063e1c0.png)

Para visualizar é só digitar `dados_listings`. Já está resolvido o meu problema de junção, já tenho todos os dados que eu preciso.

 Outra forma de fazer isso é usando o método `concat()`, ele funciona basicamente do mesmo jeito, só que diferente do `append()`, o `concat()` consegue fazer junções do tipo empilhar e também lateralmente, nós aprenderemos outro método para fazer isso também. Mas, agora, vamos empilhar o dado.

Eu vou chamar de `listings` também. Vou fechar o `append()` para não ficar tanto dado na nossa tela. Vou chamar com o mesmo nome `dados_listings =`, igualar e chamar o `concat()`.

Nós o chamamos direto do Pandas `pd.concat()`, eu abro e fecho o parênteses e jogo uma lista. Essa lista vai ter justamente os dois *dataframes* que eu quero concatenar.

 Eu vou chamar o `normal` e o `highlights`, `([dados_normal_listings,dados_highlights_listings])`. No final, vou visualizar novamente com `dados_listings`.

```python
dados_listings = pd.concat([dados_normal_listings,dados_highlights_listings])
dados_listings
```

![4](https://user-images.githubusercontent.com/59926475/168146430-332efba3-83a8-4c81-974c-99f106ed4b53.png)

Temos 70 linhas novamente, mas aparece aquele mesmo problema do index, o dado não está concatenado até o final. Usaremos a mesma solução, vou até copiar para ficar mais claro. Colocaremos `ignore_index=True`  no final.

```python
dados_listings = pd.concat([dados_normal_listings,dados_highlights_listings], ignore_index=True)
dados_listings
```

![5](https://user-images.githubusercontent.com/59926475/168146436-d943c4d5-5aed-4cf5-8548-ce22df248362.png)

Agora temos 70 linhas, o dado concatenado de 0 até 69, todos os dados concatenados. Era isso que eu queria mostrar para vocês. Pessoal, no próximo vídeo faremos as junções de outra maneira, usando o `merge()`, mas antes vamos fazer um **tratamento na variável de ligação** .

Precisamos juntar esses dois *dataframes* para poder utilizar essa informação de preço por metro quadrado para fazer algumas análises comparativas, ou usar isso como uma *feature* no nosso projeto de *machine learning*.

Para fazer isso, precisamos de uma **variável de ligação** que seja **comum** nos dois *dataframes*, porque faremos a **junção lateral**, juntaremos linha por linha. Precisamos de uma variável que faça essa **compatibilização**, o problema é que no nosso projeto essas variáveis não são totalmente compatíveis, precisamos fazer algumas elaborações para resolver esse problema.

Eu já tenho o `dados_listings.head(2)` que criamos no vídeo anterior, eu vou dar uma olhada nele e, dele, eu vou tirar as informações de bairro, "imóvel_endereco_bairro". E temos o `bairros` que é justamente o que tem o valor do m², e ele também tem as informações de bairro.

```python
dados_listings.head(2)
```

![6](https://user-images.githubusercontent.com/59926475/168146438-91ef3899-0872-44a9-81b3-3235efb86afe.png)

```python
bairros
```

![7](https://user-images.githubusercontent.com/59926475/168146441-10bf3bb9-1458-45ba-91df-9038868737bf.png)

Para fazer isso, precisamos usar alguns métodos que nos ajudarão nessa elaboração. Primeira coisa que eu vou fazer é separar essas duas informações, a primeira eu vou chamar de `bairros_amostra`, porque vão ser os bairros que vão vir do arquivo `dados_listings.head(2)`. Vou até copiar o nome dele, trazer aqui para baixo, isto é, `bairros_amostra = dados_listings`.

 E eu vou selecionar a coluna "imóvel_endereco_bairro", vou copiar e   colar, `['imovel_endereco_bairro']`. Para visualizarmos, vou colocar `bairros_amostra` na linha abaixo. Temos os bairros desse nosso arquivo de anúncios `dados_listings`.

```python
bairros_amostra = dados_listings['imovel_endereco_bairro']
bairros_amostra
```

> 0 Barra da Tijuca 1 Campo Grande 2 Barra da Tijuca 3 Barra da Tijuca 4 Glória ...65 Copacabana 66 Leblon 67 Copacabana 68 Cachambi 69 Tijuca Name: imovel_endereco_bairro, Length: 70, dtype: object
> 

A próxima parte é pegar a informação que está dentro do "bairros_amostra", mas reparem que não é uma coluna separada do *dataframe*, ele faz parte do índice do *dataframe*, repare que é um índice múltiplo, ele tem duas variáveis como índice.

 Para retirar isso, eu posso usar método `get_level_value()`, que vai retirar para mim a informação do índice, eu vou chamar isso de `bairros_todos`, porque supostamente eu tenho todas as informações de todos os bairros da cidade do Rio de Janeiro.

 Eu vou chamar `bairro_todos = bairros.index` e eu pego o método `get_level_values()`. Tem a [documentação aqui](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.MultiIndex.get_level_values.html) dele se você estiver curioso para saber mais informações sobre ele.

 Ele me permite selecionar uma das informações do índice que, no caso, tem o nome `bairros`, é só passar esse nome, `bairro_todos = bairros.index.get_level_values('bairros')`. E conseguimos isolar esse índice e criar como se fosse um *array*, uma lista com todas essas informações. Transformaremos isso em uma *series*.

```python
bairros_todos = bairros.index.get_level_values('bairros')
bairros_todos
```

> Index(['São Cristóvão', 'São Cristóvão', 'São Cristóvão', 'São Cristóvão', 'São Cristóvão', 'São Cristóvão', 'São Cristóvão', 'Benfica', 'Benfica', 'Benfica', ... 'Vila Kosmos', 'Vila Kosmos', 'Vila Kosmos', 'Vista Alegre', 'Vista Alegre', 'Vista Alegre', 'Vista Alegre', 'Vista Alegre', 'Vista Alegre', 'Vista Alegre'], dtype='object', name='bairros', length=1127)
> 

Feito isso, eu preciso separá-los e identificar os registros únicos, ou seja, o total de categorias que temos dentro desses dois arquivos. Para fazer isso, eu posso visualizar primeiro o `shape`
 da amostra.

```python
bairros_amostra.shape
```

> (70,)
> 

 O `shape` dele tem `70` registros, é o tamanho do *dataframe* que juntamos na aula anterior. Eu posso também identificar quais são as categorias que eu tenho, quantos bairros eu tenho dentro desses 70 que não se repetem, usando o `.nunique()`.

```python
bairros_amostra.nunique()
```

> 30
> 

Usando o `.nunique()`, eu consigo identificar que dentro de 70 eu tenho apenas 30 bairros, alguns valores se repetem. Para pegar esses valores, posso fazer do mesmo jeito só que usando outro método, eu tiro o “n” do código e uso `unique ()`.

```
bairros_amostra.unique()
```

> array(['Barra da Tijuca', 'Campo Grande', 'Glória', 'Vila Isabel', 'Andaraí', 'Copacabana', 'Recreio dos Bandeirantes', 'Tijuca','Méier', 'Ipanema', 'Paciência', 'Freguesia (Jacarepaguá)','Vargem Pequena', 'Pechincha', 'Freguesia', 'Botafogo','Vila da Penha', 'Jacarepaguá', 'Laranjeiras', 'Ribeira', 'Lapa', 'Rocha', 'Flamengo', 'Engenho Novo', 'Vargem Grande', 'Praça Seca','Honório Gurgel', 'Lagoa', 'Leblon', 'Cachambi'], dtype=object)"
> 

Ele vai me retornar um *array* do `numpy`. Se eu visualizar o tipo de informação que eu tenho eu vou ver que é um *array* do `numpy`. O `type()` dele é um *array* do `numpy`, `type(bairros_amostra.unique())`. Olha lá, `numpy.ndarray`.

```
type(bairros_amostra.unique())
```

> numpy.ndarray
> 

 E como isso se trata de um *array* do `numpy`, eu posso usar para criar uma *series*. Portanto, `bairros_amostra =` substituindo aquela informação, chamo `pd.Series())`, é uma maneira de construir *series* no Pandas e passo para ele justamente esse *array*. Então chamaremos `bairros_amostra` para exibirmos.

```
bairros_amostra = pd.Series(bairros_amostra.unique())
bairros_amostra
```

> 0 Barra da Tijuca, 1 Campo Grande, 2 Glória, 3 Vila Isabel, 4 Andaraí, 5 Copacabana, 6 Recreio dos Bandeirantes, 7 Tijuca, 8 Méier, 9 Ipanema, 10 Paciência, 11 Freguesia (Jacarepaguá), 12 Vargem Pequena, 13 Pechincha, 14 Freguesia, 15 Botafogo, 16 Vila da Penha, 17 Jacarepaguá, 18 Laranjeiras, 19 Ribeira, 20 Lapa, 21 Rocha, 22 Flamengo, 23 Engenho Novo, 24 Vargem Grande, 25 Praça Seca, 26 Honório Gurgel, 27 Lagoa, 28 Leblon, 29 Cachambi, dtype: object"
> 

 Ele já vai criar uma *series* com as informações únicas, ou seja, todos os bairros que eu tenho dentro da minha amostra. Vamos fazer esse mesmo procedimento para o `bairro_todos`, mesma coisa, eu posso ver o `shape` dele, `bairros_todos_shape`.

```
bairros_todos.shape
```

> (1127,)
> 

 Tem 1127, bastante coisa. Se eu vier com `.nunique()` aqui, a mesma coisa que fizemos acima, só estou mudando bairro de amostra para todos `bairros_todos_nunique()`.

Com o `nunique()` eu consigo ver que eu só tenho 161, ou seja, eu tenho 161 bairros no Rio de Janeiro ao todo.

```
bairros_todos.nunique()
```

> 161
> 

 Eu chamo `pd.Series()` e passo a elaboração que fizemos no lugar de `bairros_amostra`, vai ficar `bairros_todos_nunique())`, e crio um novo `bairros_todos`. Novamente uma *series* com todos os bairros, tamanho 161. Perfeito.

```
bairros_todos = pd.Series(bairros_todos.unique())
bairros_todos
```

> 0 São Cristóvão, 1 Benfica, 2 Caju, 3 Catumbi, 4 Centro, ... , 56 Vicente de Carvalho, 157 Vigário Geral, 158 Vila da Penha, 159 Vila Kosmos, 160 Vista Alegre, Name: bairros, Length: 161, dtype: object"
> 

 Feito isso, precisamos fazer uma **verificação** na variável de ligação. Eu confio que esse todos está correto, ele vem de uma fonte segura, mas a minha amostra é uma amostra *web*, talvez venha algo bagunçado, eu quero dar uma olhada nesse meu `bairros_amostra`.

```
bairros_amostra
```

> 0 Barra da Tijuca, 1 Campo Grande, 2 Glória, 3 Vila Isabel, 4 Andaraí, 5 Copacabana, 6 Recreio dos Bandeirantes, 7 Tijuca, 8 Méier, 9 Ipanema, 10 Paciência, 11 Freguesia (Jacarepaguá), 12 Vargem Pequena, 13 Pechincha, 14 Freguesia, 15 Botafogo, 16 Vila da Penha, 17 Jacarepaguá, 18 Laranjeiras, 19 Ribeira, 20 Lapa, 21 Rocha, 22 Flamengo, 23 Engenho Novo, 24 Vargem Grande, 25 Praça Seca, 26 Honório Gurgel, 27 Lagoa, 28 Leblon, 29 Cachambi, dtype: object"
> 

 Para fazer essa verificação eu posso passar esse `bairros_amostra` e perguntar, usando a função `.isin(bairros_todos)`. Dentro desse `bairros_todos` existe meu `bairros_amostra`? Eles estão ali dentro?

```
bairros_amostra.isin(bairros_todos)
```

> 0 True, 1 True, 2 True, 3 True, 4 True, 5 True, 6 True, 7 True, 8 True, 9 True, 10 True, 11 False, 12 True, 13 True, 14 True, 15 True, 16 True, 17 True, 18 True, 19 True, 20 True, 21 True, 22 True, 23 True, 24 True, 25 True, 26 True, 27 True, 28 True, 29 True, dtype: bool
> 

 Se eu rodar isso, eu vou ter uma *series booleana* e, perceba, do tamanho, lógico, do `bairros_amostra`, que são 30, e perceba, todo mundo está linkado direito, mas no 11 eu percebo que tem um falso, ou seja, esse 11 não está batendo, o 11 é `Freguesia (Jacarepaguá)`.

 Se eu vier no meu arquivo de bairros e verificar com o `loc[]` se o bairro Freguesia existe, `bairros.loc["Freguesia"]`.

 Lógico, eu estou usando o *label*, não uso o `iloc[]` que é para dado posicional, o `loc[]` é para *label*, portanto, `bairros.loc["Freguesia"]`.

```
bairros.loc["Freguesia"]
```

![8](https://user-images.githubusercontent.com/59926475/168146444-bf304455-b4f8-4710-ada1-6c53a3d5a46a.png)

Eu tenho informações para o bairro Freguesia, mas e para o bairro Jacarepaguá? Vamos testar com `bairros.loc["Jacarepaguá"]`. O que eu quero fazer é verificar se essas coisas são iguais ou diferentes.

```
bairros.loc["Jacarepaguá"]
```

![9](https://user-images.githubusercontent.com/59926475/168146447-e750d5f2-36a4-4ccd-b0f7-4fc0f3fe0922.png)

Eu também tenho informações para Jacarepaguá, Freguesia e Jacarepaguá são bairros diferentes. Eu sei que é um bairro próximo, o que isso está indicando é que esse Freguesia é um bairro que está próximo de Jacarepaguá, existe outro bairro chamado Freguesia que é próximo de outro bairro aqui no Rio.

[08:54] Eu posso modificar essa informação para considerar somente Freguesia, que é o bairro que ele está querendo dizer.

Para isso, usamos simplesmente o `.replace()`, no nosso *listings* eu passo o `dados_listings` e passo o `[imovel_endereco_bairro]`. E eu vou fazer a substituição, `.replace()`, *replace* é substituir, eu vou substituir a informação que está desse jeito `('Freguesia (Jacarepaguá)'` simplesmente por `'Freguesia')`.

 Assim, eu elimino esse problema que estava atrapalhando. Para sacramentar eu uso `inplace=True`, ou seja, ele vai fazer a modificação aqui mesmo. E agora eu posso fazer `bairros_amostra = pd.Series(dados_listings['imovel_endereco_bairro']` e usamos a função que acabamos de conhecer `.unique())`, que retorna os valores únicos.

 Para visualizarmos, `bairros_amostra`. Ele Está demorando um pouquinho. Eu já posso ir adiantando a comparação que fizemos, lembra da comparação `bairros_amostra.isin(bairros_todos`, eu posso pegar a comparação e refazê-la.

```python
dados_listings['imovel_endereco_bairro'].replace('Freguesia (Jacarepaguá)', 'Freguesia', inplace=True)
```

```python
bairros_amostra = pd.Series(dados_listings['imovel_endereco_bairro'].unique())
bairros_amostra
```

> 0 Barra da Tijuca 1 Campo Grande 2 Glória 3 Vila Isabel 4 Andaraí 5 Copacabana 6 Recreio dos Bandeirantes 7 Tijuca 8 Méier 9 Ipanema 10 Paciência 11 Freguesia 12 Vargem Pequena 13 Pechincha 14 Botafogo 15 Vila da Penha 16 Jacarepaguá 17 Laranjeiras 18 Ribeira 19 Lapa 20 Rocha 21 Flamengo 22 Engenho Novo 23 Vargem Grande 24 Praça Seca 25 Honório Gurgel 26 Lagoa 27 Leblon 28 Cachambi dtype: object
> 

 Ele refez o meu bairro e agora, reparem, eu estou com tudo *True* e agora podemos usar essas duas variáveis para fazer as nossas junções.

```python
bairros_amostra.isin(bairros_todos)
```

> 0 True 1 True 2 True 3 True 4 True 5 True 6 True 7 True 8 True 9 True 10 True 11 True 12 True 13 True 14 True 15 True 16 True 17 True 18 True 19 True 20 True 21 True 22 True 23 True 24 True 25 True 26 True 27 True 28 True dtype: bool
> 

Agora que fizemos a compatibilização da variável de ligação, podemos fazer a **combinação**
 dos *dataframes*  usando o método `merge()`  do Pandas. Eu deixei um [link para a documentação](https://pandas.pydata.org/docs/reference/api/pandas.merge.html?highlight=merge#pandas.merge)
 ao selecioná-lo, você vai abrir a janela “pandas.merge”.

Eu também deixei na primeira célula da seção “Combinando os DataFrames”, uma tradução dos parâmetros mais importantes desse método.

 Temos inicialmente os parâmetros `left` e `right`, é onde definiremos quais são os *dataframes* que juntaremos. Lembrando que os juntaremos lateralmente, o `left` vai ser o *data frame* a **esquerda** e o `right`, a **direita**.

 O parâmetro `how` é como essa junção vai ser feita, temos quatro opções, `left`, `right`, `outer` e `inner`. O *default* é o `inner`, que é o que usaremos no nosso exemplo. O **`left`** quer dizer que ele vai usar **apenas** a chave ou as chaves do *dataframe* que está configurado no parâmetro `left`, ou seja, todas as informações desse *dataframe* vão fazer parte da nova junção.

 E os do *data frame* `right` vão entrar **se** tiver compatibilização, a forma **`right`** é justamente o contrário da que eu acabei de explicar.

 O **`outer`** vai fazer a **união** de todas as informações, as informações dos dois *dataframes* vão aparecer, quando tiver união, vão ficar juntas e quando não tiver, vão ficar soltas.

 O **`inner`**, que é o que usaremos, vai fazer justamente a **interseção** dessas duas variáveis de ligação, só quando elas fizerem conexão é que vai existir o registro no novo *dataframe*.

 O **`on`** é para quando temos as variáveis de ligação **nos dois** arquivos, `left` e `right`, iguais, colocamos o `on`.

 Quando temos diferença de nome das variáveis, usamos o **`left_on`** para identificar quais são as variáveis de ligação, as colunas de ligação do arquivo que está do lado esquerdo. E o **`right_on`** para identificar quais são as colunas de ligação no arquivo do lado direito.

 O **`left_index`** e o **`right_index`** são booleanos, *true* ou *false*, indicando se usaremos o **índice** como variável de ligação do arquivo da esquerda ou do arquivo da direita.

 Vamos fazer isso no nosso exemplo. Eu deixei o `dados_listings` e o `bairros`, que são os dois que usaremos para fazer a junção.

```python
dados_listings[['imovel_endereco_bairro', 'imovel_tipos_propriedade']]
```

![10](https://user-images.githubusercontent.com/59926475/168146449-5c4785f4-ce80-4799-9ac6-eff86856c7d0.png)

O `dados_listings`, só para vermos as variáveis que usaremos como junção, "imovel_endereco_bairro" e "imovel_tipos_propriedade".

 Nós fizemos a compatibilização dos bairros, você deve estar lembrado disso do vídeo anterior, e no `bairros` faremos uma junção um pouco diferente porque usaremos, reparem, o "bairros" e o "tipo", que são índices.

 Faremos uma mistura de todos esses parâmetros. Eu quero colocar o resultado no `dados_listings =` mesmo, eu vou chamar o `pd` que é o nosso Pandas e o método `merge.()`.

 Eu vou pular uma linha para a configuração de cada um desses ficar mais clara. No parâmetro `left =` eu vou colocar o `dados_listings,`. No `right =` eu vou usar o `bairros`.

```python
dados_listings = pd.merge(
   left = dados_listings,
     right = bairros
)
```

 Como as variáveis de ligação não são as mesmas nos dois arquivos, eu não posso usar o *on*, eu vou usar o `left_on`, porque as informações estão em colunas no arquivo da esquerda, no arquivo da direita temos índices.

 Eu vou pegar o `left_on` e vou passar para ele os nomes das duas colunas que eu quero usar como coluna de ligação, `dados_listinfs['imovel_endereco_bairro', 'imovel_tipos_propriedade']]`. Eu copio a informação que tem os nomes dentro de uma lista.

 O `imovel_endereco_bairro` é o primeiro item dessa lista e `imovel_tipos_propriedade` é o segundo item dessa lista.

 Faltou uma vírgula no final.

```python
dados_listings = pd.merge(
   left = dados_listings,
     right = bairros
     left_on = ['imovel_endereco_bairro', 'imovel_tipos_propriedade'],

)
```

E vamos agora indicar que do arquivo *right* eu quero usar o *index* configurando o parâmetro `right_index =` como `True` . Somente isso.

[04:43] Embaixo eu vou copiar a variável `dados_listings` e colar para que, depois que ele fizer essa junção, eu veja o resultado. Aperto "Shift + Enter". Ele rodou.

```python
dados_listings = pd.merge(
    left = dados_listings,
    right = bairros,
    left_on = ['imovel_endereco_bairro', 'imovel_tipos_propriedade'],
    right_index = True
)
dados_listings
```

Para selecionar isso eu passei o `dados_listings` e selecionei apenas a coluna `['anuncio_descricao']`. Eu posso fazer um “Ctrl + Enter” e visualizar isso. Temos todas as descrições. Para visualizarmos melhor eu passo o `.values` e faço uma seleção, abro o operador de indexação `[:10]` "Shift + Enter". Isso vai permitir que eu veja os 10 primeiros mais amplamente.

```
dados_listings['anuncio_descricao'].values[:10]COPIAR CÓDIGO
```

> array(['Amplo imóvel para venda com 3 quartos, sendo 1 suítes, e 2 banheiros no total.','Amplo imóvel para venda com 4 quartos, sendo 1 suítes, e 2 banheiros no total.','Amplo imóvel para venda com 2 quartos, sendo 0 suítes, e 1 banheiros no total.', 'Amplo imóvel para venda com 2 quartos, sendo 0 suítes, e 1 banheiros no total.', 'Amplo imóvel para venda com 5 quartos, sendo 4 suítes, e 5 banheiros no total.', 'Amplo imóvel para venda com 2 quartos, sendo 1 suítes, e 2 banheiros no total.', 'Amplo imóvel para venda com 0 quartos, sendo 0 suítes, e 1 banheiros no total.', 'Amplo imóvel para venda com 1 quartos, sendo 0 suítes, e 0 banheiros no total.', 'Amplo imóvel para venda com 2 quartos, sendo 0 suítes, e 1 banheiros no total.', 'Amplo imóvel para venda com 2 quartos, sendo 0 suítes, e 1 banheiros no total.'], dtype=object)
> 

![11](https://user-images.githubusercontent.com/59926475/168146452-c44ace3f-61e6-42dd-aedf-f538936ffaf6.png)

 Cada descrição é idêntica, só muda a informação numérica, aqui está dizendo amplo imóvel para venda com 3 quartos, sendo uma suíte, dois banheiros. Isso é o primeiro registro, todos os outros seguem esse mesmo padrão.

 Isso é uma simplificação da realidade, lógico que as descrições dos imóveis não seguem um padrão idêntico, isso é só para aprendermos a usar as ferramentas para fazer extrações dentro dessa *string*.

 Para fazer isso, você deve estar lembrado que usamos o atributo STR nas aulas anteriores, é um atributo do objeto *series*, que nos permite fazer vetorizações. É como se estivéssemos aplicando métodos de *string* diretamente em uma *series*, conseguimos fazer isso de uma vez só em todos os itens da *string*.

O método que usaremos agora é o `str.extractall` que dá suporte a **Regex**, que é um padrão de texto, uma *string* que descreve um padrão que podemos usar para buscar informações dentro de uma *string*, fazer substituições.

 Vamos começar a estudar isso. [Neste link do tópico "Desafio: Regex"](https://cursos.alura.com.br/course/python-pandas-tecnicas-avancadas/task/91771), deixei um problema bem interessante para você trabalhar com Regex também. Siga o exercício que vai ser bem legal.

 Eu vou repetir o `dados_listings`, fazer essa seleção somente da coluna `['anuncio_descricao']`, passar o `.str`, chamar esse atributo e chamar a função `.extractall('')`, dentro dos parênteses eu posso passar o padrão Regex que eu quero que me ajude a buscar as informações numéricas dentro dessas *strings* que estamos vendo.

 Para fazer isso, e abro e fecho parênteses dentro `()`, depois eu passo um caractere de barra invertida `\` que seria um caractere de escape. Logo depois o `d` que vai indicar que eu estou procurando um digito numérico dentro da minha *string* e no final eu coloco o símbolo de soma `+`.

```python
dados_listings['anuncio_descricao'].str.extractall('(\d+)')COPIAR CÓDIGO
```

 Esse símbolo funciona da seguinte forma: se ele encontrar dois dígitos numéricos, por exemplo, o número 32, e se eu passar esse caractere `+`, ele vai considerar o número 32 como um número. Se eu não passar, ele vai considerar os dígitos, separando o 3 do 2, isto é, considera, mas separadamente.

 Eu não quero isso, eu quero que o 32 seja um número inteiro, por isso, eu coloquei o `+` aqui.

Rodando isso apertando "Shift + Enter", temos como resposta esse *data frame*, um pouco estranho.

```python
dados_listings['anuncio_descricao'].str.extractall('(\d+)')
```

![12](https://user-images.githubusercontent.com/59926475/168146454-4ebd5802-b809-4b25-9314-ee22250dcc75.png)

Mas reparem, o primeiro índice é o índice do meu *dataframe* normal, seriam os índices que criamos anteriormente, o próprio índice do *dataframe* e esse match é justamente o que ele encontrou.

 Em primeiro lugar ele encontrou o número 3, em segundo lugar, o número 1 e em terceiro lugar, o número 2. 3, 1 e 2. Como está na descrição dos imóveis. Depois ele encontrou o 4, 1 e 2 e assim sucessivamente.

 Ele colocou isso em uma só coluna, precisamos organizar isso um pouco melhor para que possamos jogar essas informações em colunas que façam sentido, por exemplo, um coluna de número de quartos, número de suítes, número de banheiros dentro do nosso *dataframe*.

 Eu vou salvar isso como configurações, vou chamar de configurações, seria configurações, `configuracao =` é melhor. Eu vou continuar com uma visualização embaixo, só que uma visualização de apenas alguns itens, `configuracao.head(9)`, só dos nove primeiros para ficar mais claro. Os 9 primeiros, temos as três informações de cada um desses registros.

```python
configuracao = dados_listings['anuncio_descricao'].str.extractall('(\d+)')
configuracao.head(9)
```

![13](https://user-images.githubusercontent.com/59926475/168146456-bcd87aba-2c97-467a-a01d-cdcd2b5d0504.png)

Para transformar isso no formato de coluna, por exemplo, eu quero que o 3, o 1 e o 2 estejam na mesma linha, o 4, 1 e 2, na mesma linha, o 2, 0 e 1 na mesma linha, temos o índice múltiplo. Para transformar esse índice em colunas eu uso outro método, o `.unstack()`, para fazer isso eu passo `configuração` e passo `.unstack()`.

 Só isso. Já vemos a modificação, ele transformou as linhas em colunas, o 3, 1 e 2 agora estão na mesma linha.

```python
configuracao.unstack()
```

![14](https://user-images.githubusercontent.com/59926475/168146457-ee23c6b7-58ee-4e6f-a6d1-b51e63d861e5.png)

Ele organizou, o *match* seria o índice que tínhamos no *dataframe* anterior, ele fez uma organização, por isso não estamos vendo o 47 agora, mas isso não é problema. Eu quero ainda fazer uma modificação, está vendo que tem um 0 no topo das colunas?

 Eu quero tirar esse 0 e quero também modificar esse 0, 1, 2 para o nome que representa essa coluna, são os quartos, as suítes e os banheiros. Para fazer isso, eu posso fazer diretamente chamando o .`rename()`, passo o parâmetro `columns={}` e dentro eu passo um dicionário com as informações que eu preciso para renomear essas colunas `{0: 'quartos', 1: 'suites, 2: 'banheiros'})`.

```
configuracao.unstack().rename(columns={0: 'quartos', 1: 'suites', 2: 'banheiros'}
```

 Eu quero que o 0 seja chamado de quartos, o 1, de suíte, o 2, de banheiros. Eu vou substituir isso tudo dentro de configurações, vou fazer `configuracao = configuracao`. E finalize aqui `configuracao`. Está tudo conforme esperamos.

```
configuracao = configuracao.unstack().rename(columns={0: 'quartos', 1: 'suites', 2: 'banheiros'})
configuracao
```

![15](https://user-images.githubusercontent.com/59926475/168146459-6c78d031-17a5-4ddc-adc5-654de2d4a8ad.png)

Se eu visualizar o `columns` de configuração, `configuracao.columns`, eu vou ver que é um índice múltiplo. Repare em cima, ele tem um índice múltiplo (*match*, quartos, suítes, banheiros).

```python
configuracao.columns
```

> MultiIndex([('quartos', 'quartos'), ('quartos', 'suites'), ('quartos', 'banheiros')], names=[None, 'match'])
> 

 Para eu juntar no meu *dataframe*, eu preciso eliminar o primeiro índice onde está escrito quartos. Para isso eu uso o outro método, o `droplevel()`.

Eu vou retirar um dos níveis do índice usando esse método. É bem simples, eu posso passar `configuracao.droplevel()`, dizer que nível eu quero que ele retire, como ele está pegando o primeiro nível, o índice começa com 0, eu vou dizer que eu quero que ele retire o nível 0 `level=0`.

 E vou dizer que ele está trabalhando no eixo 1 que é o eixo das colunas, `axis=1`. Eu já vou fazer a modificação direto colocando `configuracao =`na frente da linha de código. Na próxima linha, faço `configuracao`. Olha lá.

```python
configuracao = configuracao.droplevel(level=0, axis=1)
configuracao
```

![16](https://user-images.githubusercontent.com/59926475/168146461-47e04f31-ec92-4723-9a82-5d7275998cf3.png)

Ele retirou aquele índice, agora já está pronto para que eu possa juntar, fazer o `.merge()` do nosso *dataframe*. Eu passo `dados_listings = pd.merge(dados_listings, configuracao,`.

 O `dados_listings` vai ser o meu esquerdo, o outro vai ser o direito, eu não estou passando explicitamente porque eles estão na ordem dos parâmetros, não tem problema. Eu vou usar os índices para fazer a junção, eu vou falar que o `left_index=True, right_index=True)`. Eu quero visualizar o resultado que eu estou obtendo `dados_listings`.dados_listings

```python
dados_listings = pd.merge(dados_listings, configuracao, left_index=True, right_index=True)
dados_listings
```
![17](https://user-images.githubusercontent.com/59926475/168146465-5b59e133-2475-41ea-8c1b-75b548bd1e0e.png)
