# Retencao-por-tipo-de-documento
Para compreender a retenção de acordo com o tipo de documento, inicialmente utilizamos a função Kaplan-Meier Fitter (KMF), um método estatístico para estimar a curva de sobrevivência de um conjunto de indivíduos ao longo do tempo.
Metodologia - Kaplan-Meier Fitter (KMF) 
Para compreender a retenção de acordo com o tipo de documento, inicialmente utilizamos a função Kaplan-Meier Fitter (KMF), um método estatístico para estimar a curva de sobrevivência de um conjunto de indivíduos ao longo do tempo.

A ideia principal é calcular a probabilidade de um evento não ocorrer até determinado tempo — ou seja, a taxa de sobrevivência (ou retenção).

O método calcula a probabilidade acumulada de sobrevivência em diferentes momentos, considerando:

Os tempos até o evento ocorrer (exemplo: tempo até um cliente dar churn).

Se o evento realmente aconteceu ou se os dados foram censurados (censura significa que não sabemos o que aconteceu com aquele indivíduo após certo tempo).

A fórmula do Kaplan-Meier para a probabilidade acumulada de sobrevivência até um tempo t é:

image-20250403-180651.png
Onde:

S(t) = Probabilidade de sobrevivência no tempo t.

di​ = Número de eventos (exemplo: número de clientes que deram churn naquele tempo).

ni​ = Número de indivíduos ainda em análise naquele tempo.

Clientes que ainda não deram churn são considerados censurados e não afetam diretamente o declínio da curva.

A curva ajuda a entender quanto tempo, em média, os clientes permanecem antes de dar churn.
<img width="846" height="548" alt="image" src="https://github.com/user-attachments/assets/474b54f9-8d18-4c94-9f6f-878bb6c0730a" />

image-20250403-191929.png
Imagem 1: Curva de retenção - Kaplan-Meier Fitter (Todos os Clientes)
Como interpretar o gráfico?
Eixo X (Meses desde o início): Representa o tempo de acompanhamento dos clientes.

Eixo Y (Probabilidade de Retenção): Percentual acumulado de clientes ainda ativos ao longo do tempo.

Curvas de cores diferentes: Cada cor representa um tipo de documento.

Área sombreada (Unidentified): Intervalo de confiança — mostra a incerteza na estimativa.

Análise dos Resultados
Clientes com CNPJ (Laranja): Retenção ligeiramente maior. Indica que empresas (CNPJ) possuem ciclo de vida mais longo.

Clientes com CPF (Verde): Retenção mais baixa, indicando churn mais rápido.

Clientes ‘Unidentified’ (Azul): Retenção intermediária, mas com alta incerteza, possivelmente por número pequeno de dados.

e incerteza (região sombreada larga), possivelmente devido a um número menor de dados nesse grupo.

Metodologia Teste Log-Rank (Comparação de Curvas de Sobrevivência)
O teste log-rank é um método estatístico que compara duas ou mais curvas de sobrevivência para verificar se há diferenças significativas entre os grupos (CPF, CNPJ e Indefinido).

Após visualizarmos a curva de retenção realizada função Kaplan-Meier Fitter conseguimos comparar as diferenças entre CPF, CNPJ e Unidentified e compreendermos se são significamente estátisticos ou não. 

Se o p-valor for baixo (<0.05), há uma diferença estatisticamente significativa entre os grupos.

Se for alto (>0.05), não há evidência forte para afirmar que um tipo de documento tem maior retenção que outro.

Resultados:
CPF vs CNPJ

p-valor: 0.0000000

📌 Conclusão: Diferença estatística clara. CNPJ tem maior retenção.

Unidentified vs CNPJ

p-valor: 0.0000000

📌 Conclusão: Retenção de Unidentified é significativamente menor que CNPJ.

CPF vs Unidentified

p-valor: 0.0040864

📌 Conclusão: Também há diferença, embora menos intensa. Unidentified tem menor retenção que CPF.

Metodologia - Cox Proportional Hazards
O modelo de riscos proporcionais de Cox analisa o impacto de variáveis explicativas no tempo até a ocorrência de um evento.

Diferente do Kaplan-Meier, ele permite avaliar o efeito de variáveis no tempo de sobrevivência.

O modelo calcula a taxa de risco (hazard rate), que mede a probabilidade instantânea de um evento acontecer em um determinado tempo, dado que ele ainda não ocorreu.

A equação do modelo é:

image-20250403-184121.png
Onde:

h(t) = taxa de risco no tempo ttt.

h0(t) = taxa de risco base (hazard baseline) sem considerar variáveis.

X1,X2,...,Xn​ = variáveis explicativas (exemplo: idade, renda, tipo de produto).

β1,β2,...,βn= coeficientes que mostram o impacto de cada variável.

 

💡 Interpretação dos coeficientes β:

Se β>0 → A variável aumenta o risco do evento ocorrer mais rápido.

Se β<0 → A variável reduz o risco, ou seja, aumenta a retenção.

Se β=0 → A variável não tem efeito significativo no tempo de sobrevivência.

 

Utilizando a variável explicativa como o tipo de documentos, temos:

image-20250403-183109.png
CPF: Empresas com CPF (pessoa física) têm HR de 1.53 que significa 53% mais chance de churn do que empresas CNPJ, porém o IC 95% do HR é 1.48 a 1.60 mostra que estamos 95% confiantes de que o verdadeiro aumento do risco está entre 48% e 60%.

Unidentified: Empresas com document_type = unidentified têm HR de 1.90 que significa  90% mais chance de churn do que CNPJ, porém o IC 95% do HR (1.71 a 2.11) indica que o verdadeiro efeito está entre 71% e 111% de aumento no risco.

Análise do cliente ICP
Após realizamos a compreensão atráves do métodos KMF, teste log-rank e Cox Proportional Hazards para toda base de clientes foi utilizado as mesmas metodologias para compreender a retenção de acordo com o tipo de documento quando o cliente é considerado ICP ou seja possui um faturamento médio maior que R$ 20mil e menor que R$ 250 mil.

Método KMF
Apresentou resultados similares a análise de toda base, onde podemos visualizar: 

Clientes com CNPJ (Laranja): Apresentam uma retenção ligeiramente maior ao longo do tempo. Isso pode indicar que empresas (CNPJ) possuem um ciclo de vida mais longo.

Clientes com CPF (Verde): Possuem uma retenção mais baixa em relação ao CNPJ, indicando um churn mais acelerado.

Clientes ‘Unidentified’ (Azul): Parece ter uma retenção intermediária, mas com grande incerteza (região sombreada larga), possivelmente devido a um número menor de dados nesse grupo.

image-20250404-142039.png
Imagem 2: Curva de retenção - Kaplan-Meier Fitter (Clientes ICP)
 Teste log-rank 
CPF vs CNPJ

p-valor: 0.0000000

📌 Conclusão: CNPJ retém mais, diferença estatisticamente significativa.

Unidentified vs CNPJ

p-valor: 0.0000301

📌 Conclusão: Unidentified tem menor retenção que CNPJ.

CPF vs Unidentified

p-valor: 0.0735485

📌 Conclusão: Diferença não significativa ao nível de 5%. Pode haver similaridade, ou amostra insuficiente.

Cox Proportional Hazards
Este método apresentou os seguintes resultados:

🔹 CPF
coef = 0.40 → O coeficiente positivo indica que ser CPF aumenta o risco de churn.

exp(coef) = 1.49 → O risco de churn para CPF é 1.49x maior do que o CNPJ

p < 0.005 → O efeito é estatisticamente significativo.

IC 95% do HR: (1.38 – 1.61) → A estimativa é confiável, pois não inclui 1.

🔹 Unidentified
coef = 0.97 → Também tem coeficiente positivo, indicando maior risco de churn.

exp(coef) = 2.64 → O risco de churn para Unidentified é 2.64x maior que o grupo base.

p < 0.005 → O efeito também é significativo.

IC 95% do HR: (1.64 – 4.25) → A incerteza é maior do que para CPF, mas ainda confiável.

image-20250404-142803.png
Comparação todos os clientes e clientes ICP
Observamos uma tendência consistente entre os dois grupos analisados:

Clientes com CNPJ apresentam a maior retenção ao longo do tempo.

Clientes com CPF vêm em seguida, com retenção mais baixa.

Clientes Unidentified mostram a menor retenção.

Além disso, em ambos os casos, o grupo Unidentified apresenta uma maior incerteza nas estimativas, evidenciada pelas largas faixas de intervalo de confiança. Isso possivelmente está relacionado à menor quantidade de dados disponíveis para esse grupo, o que reduz a confiabilidade estatística das análises.

 
