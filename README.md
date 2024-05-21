1- Criei variáveis simples com ref() e objetos com reactive({}) pra dispensar o armazenamento no arquivo index.js e criar um vinculo reativo com os componentes que o usam

2- Os elementos em repetição (< li > e < option >) foram substituidos por v-for iterando arrays com os valores antigos, pra melhorar a legibilidade

3- O uso de diretivas elimina grande parte do uso do arquivo index.js, porque elimina a necessidade de "listeners", por exemplo:
-identificar o clique em elementos com "v-on:click"
-identificar mudanças no elemento com "v-on:change"
-utilizar "v-for" pra renderizar elementos repetidamente e deixar o código mais legível, pode substituir elementos usado muitas vezes no código como as listas < ul > e < li > e caixas de seleção com < select > e < option > no código.

4- A propriedade "watch" adiciona reatividade nos valores alterados nos inputs, por providenciar a possibilidade de gatilhos nas mudanças de valores, como usado na alteração das imagens

5- Usar a propriedade "computed" nos dados usados nos elementos, como cores e tamanhos, providencia dados calculados dinamicamente com base em outras propriedades reativas, que poderia ser usado pra  alterar as dimensões da imagem sem deixa-la esticada (se X aumenta, Y aumenta de acordo), além de dar maior desempenho por somente usar os cálculos quando houver uma alteração nos elementos relacionados
