# Introdução
## Análise do risco de inadimplência de mutuários

---

O projeto visa preparar um relatório para a divisão de empréstimos de um banco. Em que será preciso descobrir se o estado civil de um cliente e o número de filhos têm impacto sobre o inadimplemento do empréstimo.
O banco já tem alguns dados sobre a capacidade de crédito dos clientes.

O relatório considerará criar uma **pontuação de crédito** para clientes em potencial. A **contagem de crédito** será usada para avaliar a capacidade de um devedor em potencial de pagar seu empréstimo.

---

### Descrição de dados:


- children : o número de crianças na família
- days_employed : quanto tempo o cliente trabalhou
- dob_years : a idade do cliente
- education : o nível de educação do cliente
- education_id : identificador da educação do cliente
- family_status : estado civil do cliente
- family_status_id : identificador do estado civil do cliente
- gender : o sexo do cliente
- income_type : o tipo de renda do cliente
- debt : se o cliente já deixou de pagar um empréstimo
- total_income : renda mensal
- purpose : motivo para fazer um empréstimo



---


O projeto tem por fim tecer uma análise de risco, com o intuito de previnir inadimplência por parte dos pretensos clientes, já que em um empréstimo as características dos clientes e o risco do mercado importam. 

Caracteristicas como: 

* quantidade de filhos;
* idade;
* rendimento;
* tipo de trabalho;
* fonte de renda pagadora;
* onde mora;
* quais riscos se expõe no trabalho (se há risco de morte) ou acidente.

Interefere na decisão de tomada de risco por parte da credora pois diz respeito a capacidade do cliente cumprir ou não com a sua dívida.

---


## Pré-processamento os dados:

* Identificar e preencher os valores ausentes.
* Substituir o tipo de dados de número real pelo tipo inteiro.
* Excluir dados duplicados.
* Categorizar os dados.

### Explicar:

* quais valores ausentes você foram identificados;
* possíveis razões que esses valores ausentes estavam presentes;
* qual método foi usado para preencher os valores ausentes;
* qual método foi usado para localizar e excluir dados duplicados;
* possíveis razões para a presença de dados duplicados;
* qual método foi usado para alterar o tipo de dados;
* quais dicionários biblotecas foram selecionadas para este conjunto de dados.

Os dados podem conter artefatos ou valores que não correspondem à realidade – por exemplo, um número negativo de dias empregados.

## Perguntas sobre os dados:

* Existe alguma relação entre ter filhos e pagar um empréstimo em dia?
* Existe alguma relação entre o estado civil e o pagamento de um empréstimo no prazo estipulado?
* Existe uma relação entre o nível de renda e o pagamento de um empréstimo no prazo?
* Como as diferentes finalidades do empréstimo afetam o pagamento pontual do empréstimo?


# Metodologia

Esse projeto procura avaliar e descobrir os fatores de risco para o inadimplemento na base de dados fornecida pela credora. Para esse fim, sera abordada três estratégias:

- Análise exploratória;

- Realização de análise estatística, utilizando técnicas de análise descritiva e visualização gráfica, como, por exemplo:
> - Gráficos de Dispersão
> - Boxplots
> - Histogramas

a fim de extrair conhecimento dos dados, 🛠**(Em Andamento)** 🛠

- Aplicação de técnicas de Machine Learning (Classificação). 🛠**(Em Andamento)** 🛠

# Conclusão


A presente análise trouxe inicialmente a exploração dos dados, onde foi possível observar  dados ausentes, valores discrepantes, e erros em algumas colunas. 

Após a fase exploratória deu-se início ao tratamento superficial desses dados para que a leitura dos mesmos fosse mais suave, foram tratadas valores extremos, duplicatas explícitas e implícitas, algumas decisões a respeito dos dados foram tomadas, como deletar dados cujo a idade dos clientes era igual a 0, pois sem tal informação que não poderiamos auferir corretamente a idade do cliente, não podendo aferir corretamente o risco intrinseco a essa característica
Foi feito uma categorização nos propósitos apresentados pelos clientes, unindo eles em um propósito único correlacionado o que eles indicaram, como por exemplo, "comprar um carro de segunda mão" apenas para "Carro", pois assim temos dados mais enxutos.

Ainda no tratamento, houve a análise e o tratamento dos valores nulos. Pois fica evidente a relação entre dias trabalhados e valores recebidos, os dados ausentes são claramente simétricos correspondendo a aproximadademente a 10% da quantidade total nos dados, com tal expressividade não poderia ser cogitada a exclusão dos dados, e tais dados são possíveis de preenchimento usando ou a média, ou a mediana, sendo esta usada para tal. 

Ao analisar o conjunto de dias trabalhados percebe-se que há valores negativos e valores exorbitantes, como por exemplo pessoas que trabalharam mais de 100 anos, então a medida tomada foi tratar o valores negativos como valores absolutos pois os dados negativos correspondiam a cerca de 73,89% dos dados totais. 

Seguindo o roteiro proposto no projeto, pate-se então para a verificação e correção da coluna, "dob_years"  que possuiam clientes com idade 0, que como exposto acima foram retirados do conjunto total, pelos motivos já mencionados. 

Foram corrigidos os valores em "education", "family_status" e "income_type", retirando os espaços entre as palavras e colocando "_" para se adequar ao "snake_case" que é o ideal ao python. Houve também a correção do dando de gênero, pois possuia um valor não específicado, "XNA", que foi alterado para "not_informed".

Após isso, toma-se por alvo os erros em "total_income" e "days_employed", de início foi criada uma função com o intuito de criar grupos de idades, de 10 em 10 anos, que preencheu a coluna "age_group". Após isso foi analisado a concentração de valores das rendas e crianda um novo DataFrame sem os valores ausentes e foram calculadas as médias e medianas para serem analisadas e tendo a mediana como escolhida para prencher os valores ausentes pois a média tem como formula o somatório de todos os elementos de um conjunto e dividido pela quantidade desses elementos, logo valores extremos podem influenciar a média e "puxar" ela para um dos extremos.

Então, foi criada uma tabela dinâmica explorando características que poderiam influenciar o quanto o cliente poderia ganhar e os escolhidos foram "age_group","education", "gender", "income_type", pois esses valores correspondem a idade, nível de escolaridade, gênero e tipo de trabalho. Infelizmente sabemos que ainda há essa variação de salários entre homens e mulheres, por isso esse ponto foi abordado no projeto.

Após "total_income" ser tratado, foi o momento de "days_employed". Primeiramente os valores máximos foram exibidos e dívididos por 365,25 (pois temos os anos bissextos em quatro em quatro anos), após analisar o dado vi que os valores mais extremos foram de clientes aposentados e desempregados, logo essas pessoas não poderiam mais ter dias trabalhados, pois eles não estão empregados e a quantidade de hora trabalhada poderia ter sido inserida com erros. então esses valores foram convertidos em "NaN" e começou o tratamento, o foi "plotado" um gráfico do tipo "box" e pode-se observar valores do tipo "outliers", mais uma vez valores que fogem da concentração daquele dado. Então mais uma vez, a mediana foi escolhida para preencher esses dados. 

Uma vez que a tabela foi tratada, deu-se início a categorização dos dados. Primeiramente, se deu a criação dos dados de classes de renda, a saber:

- E 
- D 
- C
- B 
- A

Com isso descobre-se que as classes com maior quantidade de cliente são as classes B e A, isso claramente pode influenciar nos valores finais da análise.
Também começa a criação de uma nova coluna com valores qualitativos de "compliant" e "defaulter", para cliente com débitos ao pagar.

Então, com os dados preparados começa-se a verificar hipóteses.

As principais conclusões foram:

- De acordo com os dados a quantidade de filhos não parece influenciar de forma significativa a inadimplência. Mas não ter filhos aumentam minimamente a chances de adimplência.

- Pessoas solteiras possuem ligeiramente uma propensão a ser inadimplente. Viúvos(as) são os que possuem menores índices de inadimplência.

Conforme a divisão de classes em "total_income" que produziu a variável classes, vemos que o nível de inadiplência não varia muito entre as classes. Apesar dos classe B e da classe C estarem com os maiores índices de inadimplência.

Os empréstimo com finalidade imobiliária, possuem o menor índice de inadimplência, devido a sua possibilidade de tomada do bem e a revenda. Enquanto veículos que foi o maior índice de inadimplência.







