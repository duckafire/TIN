<div align="center">
    <img src="https://github.com/duckafire/LIM/blob/main/lim-icon.png" width="200"/>
</div>

<br>

<div align="center">
    <p>
   		<a href=""><img alt="Last release" src="https://img.shields.io/badge/Last%20release-v0.1.0-%2325a319"/></a>
    	<a href=""><img alt="License" src="https://img.shields.io/badge/License-MIT-%23a61f82"/></a>
    </p>
    <p>
    	<a href=""><img alt="LUA version" src="https://img.shields.io/badge/LUA%20version-5.4-%236d1993"/></a>
    	<a href=""><img alt="Tic80 API" src="https://img.shields.io/badge/Tic80%20API-1.0.2164-blue"/></a>
	</p>
</div>

## Introdução
###### [English version](https://github.com/duckafire/LIM/blob/main/info/README.md)
&emsp; LIM é um pequeno e peculiar compactador de código, criado para facilitar a compressão de *pequenas* bibliotecas escritas em LUA. Ele foi pensado para ser usado na compactação de bibliotecas do projeto [TinyLibrary](https://github.com/duckafire/TinyLibrary "Repositório") para o computador/console de fantasia [Tic80 Tiny Computer](https://tic80.com "Site oficial"), entretanto nada impede que o mesmo seja usado para compactar códigos LUA para uso em outros programas, como o [Pico8](https://lexaloffle.com/pico-8.php "Site oficial") e até mesmo o [LÖVE](https://love2d.org "Site oficial"). <br>

> [!WARNING]\
> Atualmente, o LIM só possui suporte à API do Tic80, à qual sempre será considerada, mesmo que o objetivo não seja gerar um código para tal programa. Felizmente isso mudará em versões futuras, onde a leitura dessa API tornará-se opcional. Haverá também a possibilidade de especificar palavras reservadas, assim ampliando o suporte à bibliotecas para o LIM e tornando mais fácil a compactação de códigos LUA para programas diversos.

<br>

## Tópicos
* [Estruturas de argumentos](#estruturas-de-argumentos)
	* [Bandeiras](#bandeiras)
		* [Nenhum](#nenhum)
		* [Versão](#versão)
		* [Ajuda](#ajuda)
		* [Substituir](#substituir)
	* [Ordem dos argumentos](#ordem-dos-argumentos)
		* [Informativa](#informativa)
		* [Executiva](#executiva)
* [Regras de escrita](#regras-de-escrita)
	* [Primeira](#primeira)
	* [Segunda](#segunda)
	* [Terceira](#terceira)
	* [Quarta](#quarta)
	* [Quinta](#quinta)
* [Como a compactação funciona](#como-a-compactação-funciona)
* [Boas práticas](#boas-práticas)

<br>

## Estruturas de argumentos
&emsp; Assim como qualquer outro programa de terminal, LIM possui uma série de bandeiras e estruturas de posicionamento para argumentos, às quais devem ser seguidas ao executar o programa. A quebra dessa organização resultará em mensagens de erro.

### Bandeiras
&emsp; Este é o nome dado à caracteres especificos que, quanto precedidos por `'-'`, solicitam a execução de uma determinada ação por parte do LIM. Abaixo encontram-se listadas todas as bandeiras do LIM, junto às suas funcionalidades:

> [!NOTE]\
> `user@computer:~$` refere-se às informações exibidas por terminais, como o CMD e o qTerminal.

##### Nenhuma
&emsp; Caso LIM seja chamada sem argumentos, essa mensagem será exibida:

```
user@computer:~$ lim
[ LIM - Lua lIbrary coMpactor - https://github.com/duckafire/LIM ]
Try: "lim -h"
```

##### Versão
&emsp; Algo muito importante em todo programa é a explicitação de sua versão em execução, e para obter tal informação no LIM basta usar a banadeira `-v` (**v**ersion).

```
user@computer:~$ lim -v
[ LIM - vX.X.X - MIT]
```

##### Ajuda
&emsp; Como todo bom programa de terminal, LIM conta com uma pequena mensagem de ajuda (útil para sanar dúvidas simples), que pode ser invocada com a bandeira `-h` (**h**elp).

```
user@computer:~$ lim -h
[ LIM - Infomations ]

[!] Flags [!]
* -v : Print the running version and the current license.
* -h : Print informations about LIM.
* -r : Force the replacement of an already existig library,
       if [libName] equals its name.

[!] Structure [!]
* "lim [-v|-h]"
* "lim [-r] <origin>.lua [libName]"

[!] Rules [!]
1. Functions declared with "local function", that start in
   the beginning of the line, they will be added to the
   library. E.g.: local function name() -> lib.name=function()
2. Variables and tables declared with "local" (or "_G."),
   that start in the beginning of the line, they will not be
   compacted.
3. Do not create strings with '[[  ]]'
4. Do not ise '@' in code (reserved words).
5. Variable and table names cannot finish with two numbers
   followed (e.g.: 'car12' -> 'car12_').

[!] Full documentation [!]
* https://github.com/duckafire/LIM/tree/main/info

[ LIM - Infomations ]"
```    

##### Substituir
&emsp; Como medida de segurança, LIM não pode substituir arquivos já existente, a menos que o usuário solicite isso através do uso da bandeira `-r` (**r**eplace).

```
user@computer:~$ lim -r origin.lua library
```

> [!IMPORTANT]\
> Essa substituição é referente ao produto final. O LIM não possui a capacidade de manipular o arquivo original, apenas ler.

### Ordem dos argumentos
&emsp; LIM possui uma estrutura própria em relação a ordem do posicionamente de seus argumentos, à qual deve ser seguida a risca, pois sua quebra resultará em mensagens de erro. Tais estruturas dividem-se em duas categorias:

##### Informativa
&emsp; Gera uma mensagem referente a alguma funcionalidade do LIM. Estas estruturas, em geral, são compostas pela utilização de uma única bandeira.

```
lim [-v|-h]
```

##### Executiva
&emsp; São ordens que desencadeiam a função principal do LIM.

```
lim [-r] <file>.lua [libName]
```

> [!IMPORTANT]\
> Caso `libName` seja omitido, `file` será usado como nome para a biblioteca.


> [!IMPORTANT]\
> Argumentos cercados por `[]` são opcionais, por `<>` são obrigatório e *livres* devem ser escritos da mesma maneira que aparecem.

<br>

## Regras de escritas
&emsp; Para evitar que o LIM quebre a lógica do código após a compactação, há uma série de regras que devem ser seguidas pelo usuário durante a escrita do código, como forma de orientar o compactador durante sua execução. Estas estão listadas abaixo:


###### Primeira
[1] **Funções declaradas com `local function`, que inicião no começo da linha, serão adicionadas a biblioteca.**

* Este é um pequeno artifício usado pelo LIM para permitir que as funções das bibliotecas compactadas possam ser chamadas fora do seu ambiente (`do ... end`), o qual visando manter certos valores *privados*.


``` lua
local functions example() -- [...]

------------\/------------

lib.example=function() -- [...]
```

> [!NOTE]\
> Para declarar *funções locais*, ou seja, que não devam ser implementadas na tabela da biblioteca, é necessário adicionar, no mínimo, uma tabulação em prefixo a sua declaração (mesma linha). Futuramente esse método será melhorado signifcativamente.

###### Segunda
[2] **Variáveis e tabelas declaradas com `local` (or `_G.`), que inicião no começo da linha, não serão compactadas.**

* Após a compactação seus nomes permanecerão inalterados. Isso é útil para que certos valores possam ser armazenados para uso posterior e automático por parte das funções da biblioteca.

``` lua
local turnOn = false
local classes = {}

_G.libErr = ""
```

> [!IMPORTANT]\
> Variáveis e tabelas declaradas com `local` só poderão ser acessadas por funções internas a bibliotecas, enquanto aquelas *declaradas* com `_G.` poderão ser acessadas em qualquer trecho do código, por qualquer *coisa*.

> [!NOTE]\
> `_G` é uma tabela LUA responsável por armazenar o ambiente global. LUA não usa essa tabela em si, logo alterar seu valor não afeta nenhum ambiente, e vice-versa. Ela é útil para tornar explícita a declaração de variáveis globais. O uso de `_G.` como prefixo para variáveis e tabelas declaradas com ela (`_G.example = true`) é opcional em chamadas futuras (`P11(example)`).

###### Terceira
[3] **Não crie cadeias de caracteres com `[[ ]]`.**

* Como o LIM agrupa a biblioteca em uma única linha, a funcionalidade desses declaradores de cadeia de caractere acaba sendo perdida. Use `"` ou `'` em aliança com `\n` para obter um resultado igual.

``` lua
local msg0 = [[
Hello
World!
]]

local msg1 = "Hello\nWorld!"
```

###### Quarta
[4] **Não use `@` no código.**

* LIM usa esse caractere para realizar certas marcações no código, que o orientarão em futuras ações. Ele foi escolhido por não possuir funcionalidade alguma em LUA (pelo menos não até a 5.4), resumindo-se apenas ao uso em cadeias de caracteres.

``` lua
local text = "@duckafire"

------------\/------------

local text = "
```

> [!WARNING]\
> A quebra dessa regra poderá acarretar em resultados imprevíveis.

> [!IMPORTANT]\
> Em versões futuras, o sistema de marcação do LIM será melhorado a ponto de permitir o uso deste caractere.

###### Quinta
[5] **Os nomes das variáveis e tabelas não devem terminar com dois ou mais números seguidos.**

* Ao longo do seu progresso na leitura do código fornecido, LIM deixa uma série de identificadores espalhados pelo código, que podem entrar em conflito com nomes de variáveis e tabelas que possuam tais características.

``` lua
example01 -> example1 | example_1 | example01n | example01th
```

> [!WARNING]\
> A quebra dessa regra poderá acarretar em resultados imprevíveis.

> [!NOTE]\
> Em futuras atualizações, o método de orientação do LIM será alterado, o que possibilitará o descarte dessa regra.

<br>

## Como a compactação funciona
&emsp; O processo de compactação do LIM ocorre em quatro estágios, com cada um deles tendo um papel crucial no funcionamento do estágio adjacente. Eles recebem as seguintes nomeclaturas:

1. **Definição**: Atribui as funções do código, que seguem a [primeira regra](#primeira), à tabela da biblioteca. Além de gerar os *códigos de proteção* para variáveis e tabelas.
2. **Limpeza**: Remove espaços desnecessários, comentários, tabulações e quebras de linha. Além de *proteger* as cadeias de caracteres.
3. **Referenciação**: Substitui as funções LUA encontradas no código por referências (variáveis que recebem, implicitamente, o *endereço* de uma função).
4. **Abreviação**: Reduz os nomes de variáveis, tabelas e funções à suas letras iniciais. Além de remover os *códigos de proteção*.

* Após esses processos, a geração do produto final se inicia, e, durante a mesma, são adicionadas:
	* Tabela da biblioteca
	* Bloco `do .. end` envolvendo a biblioteca
	* Referências as funções LUA
	* Referência para a biblioteca (uso opcional)

<br>

## Boas práticas
&emsp; Dadas algumas pequenas limitações do LIM, em relação ao quão mínimo ele consegue deixar o código, abaixo está disponibilizada uma lista de bons hábitos na escrita de códigos LUA para o LIM:

[1] **Declaração de variáveis e tabelas juntas** (*apenas com `local`*): Esse hábito, à longo prazo, possibilita um ganho de espaço considerável, já que não haverá a necessidade de reescrever o `=` e o `local`. 

``` lua
local name = "player"
local damage = 37
local alive = true
local currentGun

------------\/------------

local name, damage, alive, currentGun = "player", 37, true, nil
```

> [!WARNING]\
> Isso não é possível com variáveis e tabelas declaradas com `_G.`

[2] **Iniciar variáveis *compactáveis* com uma letra disnexa ao seu nome**: Como dito [aqui](#funcionamento-da-compactação), após a compactação, variáveis e tabelas são reduzidas à sua letra inicial, logo é necessária uma certa atenção com relação à isso, para assim evitar variáveis com nomes *pós-compilação* idênticos. Por isso, acaba sendo eficiente à aplicação desse hábito.

``` lua
enemy -> aenemy | aEnemy | a_enemy
block -> bblock | bBlock | b_block
```

> [!NOTE]\
> Recomenda-se seguir uma ordem alfabética, dando a primeira letra do alfabeto ao primeira nome.

<br>

> [**Voltar ao topo**](#introdução)