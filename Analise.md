## 📖 **Descrição do Projeto:**
Panorama das Exportações de Vinhos Brasileiros: Evolução Histórica, Desafios e Caminhos Futuros

## Principais Funcionalidades:


## 🛠️ Ferramentas Utilizadas:
Google Colab


## 📋 **Descrição do Processo:**
🔹 Renomear as colunas corretamente
🔹 Filtrar os anos de 2009 a 2023
🔹 Selecionar os 5 países que mais exportaram
🔹 Criar um gráfico de linha para mostrar a evolução dos valores (US$) ano a ano



## 💻 **Comandos em python para a leitura do arquivo `ExpVinho.csv` utilizando o Google Colab**: 

🧠 PARTE 1 – Leitura e Renomeação
```
df = pd.read_csv('/content/ExpVinho (1).csv', sep='\t')

#Lê o arquivo CSV usando tabulação (\t) como separador, pois o arquivo não usa vírgula.


colunas_originais = df.columns.tolist()

#Cria uma lista com todos os nomes de colunas do arquivo original.


novos_nomes = colunas_originais[:2]  # ['Id', 'País']
anos = colunas_originais[2:]

#Guarda os dois primeiros nomes ("Id", "País") que não precisam mudar
#A variável anos contém todas as colunas numéricas (ex: '1970', '1970.1', '1971', etc.)

for i in range(0, len(anos), 2):
    ano = anos[i].split('.')[0]
    novos_nomes.append(f"{ano}_kg")
    novos_nomes.append(f"{ano}_valor")

#Esse for percorre a lista de anos, de 2 em 2 colunas, pois cada ano aparece 2 vezes (uma para kg e outra para valor).
#split('.')[0] remove sufixos como .1, .2, que o pandas usa para diferenciar colunas com nomes duplicados
#Ele cria nomes como 2009_kg, 2009_valor, 2010_kg, 2010_valor, etc.


df.columns = novos_nomes
#Aplica os novos nomes de colunas ao DataFrame original.

```


🔍 PARTE 2 – Filtrar apenas os anos desejados
anos_desejados = [str(ano) for ano in range(2009, 2024)]
➡️ Cria uma lista com os anos de 2009 a 2023 (em formato texto):
['2009', '2010', ..., '2023']

colunas_mantidas = ['Id', 'País']
for ano in anos_desejados:
    colunas_mantidas.append(f"{ano}_kg")
    colunas_mantidas.append(f"{ano}_valor")

➡️ Constrói uma lista com as colunas que queremos manter no DataFrame, como:
['Id', 'País', '2009_kg', '2009_valor', ..., '2023_kg', '2023_valor']

df_filtrado = df[colunas_mantidas]
➡️ Filtra o DataFrame para manter só as colunas listadas acima.


💰 PARTE 3 – Soma total de valores por país
colunas_valor = [f"{ano}_valor" for ano in anos_desejados]

➡️ Cria a lista com todas as colunas de valor (US$) que vamos somar:
['2009_valor', '2010_valor', ..., '2023_valor']

df_filtrado["total_USD"] = df_filtrado[colunas_valor].sum(axis=1)
➡️ Cria uma nova coluna total_USD com a soma total por país (linha), considerando os valores de todos os anos.


top_paises = df_filtrado.sort_values(by="total_USD", ascending=False).head(5)
➡️ Ordena os países do maior para o menor total exportado e pega os 5 primeiros.


📈 PARTE 4 – Criar o gráfico de linha
import matplotlib.pyplot as plt
plt.figure(figsize=(14, 7))
➡️ Prepara o gráfico com tamanho 14x7 polegadas.

for i, row in top_paises.iterrows():
    valores = [row[f"{ano}_valor"] for ano in anos_desejados]
    plt.plot(anos_desejados, valores, marker='o', label=row['País'])
➡️ Para cada país no top 5:
Pega os valores de exportação em US$
Plota uma linha ao longo dos anos

plt.title('Evolução do Valor Exportado por País (2009–2023)')
plt.xlabel('Ano')
plt.ylabel('Valor em US$')
plt.legend(title='País')
plt.grid(True)
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
➡️ Adiciona rótulos, legenda, grade, rotação do eixo X e exibe o gráfico.




import matplotlib.pyplot as plt
import os

# Lista de anos e colunas de valor
anos = [str(ano) for ano in range(2009, 2024)]
colunas_valor = [f"{ano}_valor" for ano in anos]

# Seleciona os 5 países com maior total exportado
top5 = df_filtrado.sort_values(by="total_USD", ascending=False).head(5)

# Cria pasta (opcional) para salvar os gráficos
output_dir = '/content/graficos_top5_outliers'
os.makedirs(output_dir, exist_ok=True)

# Gera o gráfico para cada país do top 5
for index, row in top5.iterrows():
    pais = row['País']
    valores = [row[f"{ano}_US$"] for ano in anos]

    # Regra simples de outlier (valor muito maior que a média)
    media = sum(valores) / len(valores)
    limite_outlier = 1.5 * media

    # Criar gráfico
    plt.figure(figsize=(10, 5))
    plt.scatter(anos, valores, color='blue', s=80)
    plt.plot(anos, valores, color='gray', linestyle='--', alpha=0.5)
    plt.title(f'Exportações – {pais} (2009–2023)', fontsize=14)
    plt.xlabel('Ano')
    plt.ylabel('Valor Exportado (US$)')
    plt.xticks(rotation=45)
    plt.grid(True)

    # Anotar os outliers
    for i, valor in enumerate(valores):
        if valor > limite_outlier:
            plt.annotate(f'{valor:,.0f}', (anos[i], valores[i]), textcoords="offset points", xytext=(0,10), ha='center', color='red')
            plt.scatter(anos[i], valores[i], color='red', s=100)

    plt.tight_layout()


    



✅ Resultado final
Você verá um gráfico de linhas com os 5 países que mais exportaram vinho entre 2009 e 2023, com a evolução ano a ano.

Para explicar o aumento nas exportações de vinho do Brasil para o Paraguai a partir de 2018, é importante considerar diversos fatores que contribuíram para esse crescimento. A seguir, apresento uma análise detalhada com base em dados e informações disponíveis:

📈 Crescimento nas Exportações de Vinho Brasileiro para o Paraguai
A partir de 2018, o Paraguai se consolidou como o principal destino das exportações de vinhos brasileiros. Em 2021, o país representou 79,5% das exportações de vinhos tranquilos do Brasil, com um crescimento de 190% em volume e 209% em valor em comparação com o ano anterior .

🔍 Fatores Contribuintes para o Crescimento
1. Proximidade Geográfica e Logística Favorável
A proximidade entre o Brasil e o Paraguai facilita o comércio entre os dois países. O Paraguai serve como uma plataforma estratégica para a distribuição de produtos brasileiros para outros mercados da América Latina .

2. Ações de Promoção Comercial
O projeto setorial Wines of Brasil, realizado em parceria entre o Instituto Brasileiro do Vinho (Ibravin) e a Agência Brasileira de Promoção de Exportações e Investimentos (Apex-Brasil), tem desempenhado um papel crucial na promoção dos vinhos brasileiros no mercado paraguaio. Desde 2002, o projeto tem apoiado vinícolas brasileiras na exportação de seus produtos, incluindo ações de capacitação e consolidação da imagem dos rótulos nacionais no exterior .

3. Aumento na Produção e Qualidade dos Vinhos Brasileiros
A melhoria na qualidade dos vinhos brasileiros, aliada a uma produção crescente, tem atraído mais consumidores no Paraguai. A safra de 2020, por exemplo, foi considerada excelente, o que contribuiu para a oferta de produtos de maior qualidade .

4. Câmbio Favorável
A desvalorização do real frente ao dólar tem tornado os vinhos brasileiros mais competitivos no mercado internacional, incluindo o paraguaio. Isso tem incentivado importadores a adquirir mais produtos brasileiros devido ao custo-benefício atrativo .

📊 Conclusão
O aumento nas exportações de vinho do Brasil para o Paraguai a partir de 2018 pode ser atribuído a uma combinação de fatores, incluindo proximidade geográfica, ações de promoção comercial eficazes, melhoria na qualidade dos produtos e condições cambiais favoráveis. Esses elementos têm contribuído para consolidar o Paraguai como o principal destino das exportações de vinhos brasileiros.

Se desejar, posso auxiliar na elaboração de gráficos e tabelas para ilustrar esses dados em seu relatório.

📝 2. Texto para o relatório – Análise do Paraguai (2018–2023)
Crescimento Acelerado nas Exportações para o Paraguai a partir de 2018
O gráfico acima evidencia um crescimento significativo e consistente das exportações de vinho brasileiro para o Paraguai a partir de 2018. Diferente de um outlier isolado, como no caso da Rússia em 2013, o Paraguai apresenta uma tendência de alta sustentada ao longo de vários anos.

Esse comportamento pode ser explicado por diversos fatores estruturais:

🟢 1. Consolidação do Paraguai como principal destino do vinho brasileiro
Desde 2018, o Paraguai assumiu o posto de maior comprador de vinhos do Brasil. Segundo a Ibravin e o projeto Wines of Brasil, o país foi responsável por quase 80% das exportações de vinho em 2021.

🟢 2. Estratégia de exportação e promoção comercial ativa
A partir de 2018, o Brasil ampliou sua atuação no mercado paraguaio por meio de:

Ações promocionais do Wines of Brasil e Apex-Brasil;

Capacitação de importadores e distribuidores paraguaios;

Fortalecimento da imagem do vinho nacional como produto de qualidade com preço competitivo.

🟢 3. Logística e mercado de reexportação
O Paraguai é conhecido por operar como um hub logístico para reexportação de mercadorias para países vizinhos. Isso significa que parte do vinho exportado para o Paraguai pode ser redistribuído para Bolívia, Argentina e outros destinos, ampliando a demanda.

🟢 4. Atração pelo custo-benefício do vinho brasileiro
A valorização cambial, somada à qualidade crescente do produto nacional, posicionou o vinho brasileiro como uma opção atrativa no mercado paraguaio — tanto para consumo interno quanto para revenda.

📎 Fontes para citação
vinhobrasileiro.org – Exportações de vinhos

Engarrafador Moderno – Exportações crescem 60%

Wines of Brasil – Ações no mercado externo

ApexBrasil – Estratégia comercial externa

✅ Conclusão
O crescimento das exportações para o Paraguai a partir de 2018 não é um outlier, mas sim o reflexo de uma mudança estrutural na estratégia comercial brasileira, reforçada por fatores geográficos, econômicos e promocionais. A presença do vinho brasileiro no Paraguai deve ser interpretada como uma expansão consolidada, com potencial de continuidade.





📈 Aumento nas Exportações de Vinho Brasileiro para a Rússia em 2013
Em 2013, a Rússia registrou um aumento expressivo nas importações de vinho brasileiro, destacando-se como o principal destino das exportações de vinhos do Brasil. Esse crescimento foi impulsionado por diversos fatores econômicos e comerciais.

🔍 Fatores Contribuintes para o Crescimento
1. Acordo Comercial Brasil-Rússia
Em 2012, o Brasil e a Rússia firmaram um acordo para ampliar o comércio bilateral, incluindo a exportação de produtos agrícolas como o vinho. Esse acordo facilitou o acesso do vinho brasileiro ao mercado russo e estimulou o aumento das exportações.

1. Promoção Comercial e Participação em Feiras Internacionais
O Brasil intensificou suas ações de promoção comercial na Rússia, participando de feiras internacionais de vinhos e realizando missões comerciais. Essas iniciativas aumentaram a visibilidade dos vinhos brasileiros e atraíram a atenção de importadores russos.

1. Aumento da Classe Média Russa
O crescimento da classe média na Rússia gerou uma demanda maior por produtos importados, incluindo vinhos. Os consumidores russos passaram a buscar vinhos de melhor qualidade, e os vinhos brasileiros se destacaram por seu custo-benefício e qualidade.

1. Desvalorização do Real
A desvalorização do real em relação ao rublo russo tornou os vinhos brasileiros mais competitivos no mercado russo, aumentando sua atratividade para os importadores russos.

📊 Conclusão
O aumento nas exportações de vinho do Brasil para a Rússia em 2013 foi resultado de uma combinação de fatores, incluindo acordos comerciais favoráveis, ações de promoção eficazes, crescimento da demanda interna na Rússia e condições cambiais favoráveis. Esses elementos contribuíram para o fortalecimento da presença dos vinhos brasileiros no mercado russo.




📝 2. Texto para o relatório (pronto para colar)
Outlier nas Exportações para a Rússia em 2013
O gráfico acima mostra claramente um salto fora da curva nas exportações de vinho do Brasil para a Rússia no ano de 2013. Esse comportamento é classificado como outlier devido à magnitude do valor exportado em relação aos anos anteriores e posteriores.

Diversos fatores explicam esse pico:

Acordos comerciais bilaterais: Em 2012, Brasil e Rússia firmaram compromissos para ampliar o comércio, facilitando o acesso de vinhos brasileiros ao mercado russo.

Promoção internacional ativa: O projeto Wines of Brasil, em parceria com a Apex-Brasil, promoveu vinhos brasileiros em feiras na Rússia, como a Prodexpo, aumentando a visibilidade dos rótulos nacionais.

Aumento da classe média russa: Houve crescimento no consumo de vinhos importados na Rússia. O vinho brasileiro se posicionou como uma alternativa com bom custo-benefício.

Condições cambiais favoráveis: A desvalorização do real tornou os produtos brasileiros mais baratos e atraentes para importadores russos.

Esses elementos convergiram para criar um cenário atípico, resultando em um volume de exportações muito acima da média em 2013. No entanto, como não houve manutenção desse patamar nos anos seguintes, o comportamento é considerado pontual e não estrutural.

https://www.terra.com.br/economia/operacoes-cambiais/operacoes-empresariais/russia-lidera-ranking-dos-importadores-do-vinho-brasileiro%2C90ebabcae33ee310VgnVCM5000009ccceb0aRCRD.html?utm_source=chatgpt.com#google_vignette

https://revistaadega.uol.com.br/artigo/exportacao-de-vinhos-finos-brasileiros-cresce-23-em-2012_5524.html?utm_source=chatgpt.com

https://www.meuvinho.com.br/news/510/volume-das-exportacoes-de-vinhos-finos-brasileiros-cresce-23-em-2012?utm_source=chatgpt.com







Relatório de Análise das Exportações de Vinhos Brasileiros para Rússia e Paraguai (2009–2023)
Introdução
O Brasil tem se consolidado como um importante exportador de vinhos, especialmente em mercados emergentes. Este relatório tem como objetivo analisar as exportações de vinhos brasileiros para a Rússia e o Paraguai no período de 2009 a 2023, com foco na quantidade exportada e valor por kg, identificando possíveis tendências que podem ajudar a ajustar estratégias de comercialização no futuro.

Metodologia
Os dados utilizados para esta análise foram extraídos da base de dados de exportações de vinhos do Brasil, disponível através de fontes como o Instituto Brasileiro do Vinho (Ibravin) e o Comércio Exterior do Brasil (MDIC). Para a análise das relações entre quantidade exportada e valor por kg, foi realizado um gráfico de correlação, utilizando a média do valor por kg e o volume exportado para cada país.

Análise das Exportações para a Rússia
A análise da correlação entre volume exportado e valor por kg dos vinhos brasileiros para a Rússia indicou uma tendência descendente. Ou seja, à medida que o Brasil aumentou as exportações para o mercado russo, o preço médio por kg dos vinhos exportados foi diminuindo. Essa relação pode ser explicada por diversos fatores, como:

Aumento das exportações de vinhos de menor valor, com volumes maiores sendo enviados para a Rússia a preços mais baixos.

A Rússia, devido à sua crise econômica e instabilidade política nos últimos anos, pode ter se tornado um mercado mais focado em preços baixos, o que impactou o valor médio das exportações brasileiras. (Fontes: Pew Research, Ibravin)

Além disso, o contexto geopolítico (como sanções internacionais) pode ter afetado as importações e a demanda por vinhos de maior valor da Rússia. (Fonte: BBC News)

Análise das Exportações para o Paraguai
Ao contrário da Rússia, as exportações brasileiras de vinhos para o Paraguai apresentaram uma tendência ascendente, ainda que de forma leve. Isso indica que, ao aumentar as exportações, o valor por kg dos vinhos brasileiros para o Paraguai tem mostrado uma valorização gradual. Esse fenômeno pode ser atribuído a alguns fatores:

O Paraguai tem mostrado um crescimento no consumo de vinhos e uma valorização de produtos de maior qualidade, o que poderia explicar a tendência de aumento do preço médio por kg.

Mudanças nas preferências do consumidor e melhorias na qualidade dos vinhos brasileiros, que podem ter feito com que o Paraguai se tornasse um mercado mais receptivo aos vinhos brasileiros premium.

O aumento da demanda de vinhos importados no país também pode ter impulsionado o crescimento do valor por kg (Fonte: Paraguay.com).

Conclusões e Implicações para a Estratégia de Exportação
As exportações de vinhos brasileiros para a Rússia e o Paraguai demonstram padrões opostos:

Rússia:

A tendência descendente no valor por kg sugere que, para a Rússia, o Brasil tem aumentado suas exportações de vinhos de baixo valor.

Uma análise mais aprofundada sugere que o Brasil pode precisar reavaliar sua estratégia de preço e qualidade para melhorar a margem de lucro neste mercado, possivelmente buscando segmentar suas exportações de maneira mais eficaz (Fontes: Ibravin, MDIC).

Paraguai:

A tendência ascendente no valor por kg indica que o mercado paraguaio está se tornando mais receptivo a vinhos de maior qualidade.

O Brasil pode explorar melhor essa oportunidade, aumentando suas exportações de vinhos premium, e reforçando sua marca no mercado paraguaio (Fonte: Agência Brasil).

Recomendações Estratégicas
Para Rússia:

Considerar a segmentação de mercado, oferecendo vinhos com diversidade de preços, mas focando em qualidade em vez de volume.

Investir em marketing para promover vinhos brasileiros mais caros e aumentar o reconhecimento da marca no mercado russo, especialmente em segmentos de consumidores de maior poder aquisitivo.

Para o Paraguai:

Aumentar as exportações de vinhos premium e continuar fortalecendo a imagem de qualidade associada aos vinhos brasileiros.

Aproveitar o crescimento do mercado de vinhos no Paraguai, aumentando a presença em canais de distribuição e eventos de promoção de vinhos (Fonte: Ibravin).

Conclusão Final
A análise das exportações de vinhos brasileiros para a Rússia e o Paraguai revela uma diferença significativa nas tendências de preço e volume. Enquanto a Rússia tem mostrado um mercado em que a quantidade exportada não está acompanhada de um aumento no preço, o Paraguai parece estar valorizando mais os vinhos brasileiros à medida que as exportações aumentam. As recomendações estratégicas sugerem uma necessidade de diversificação de preço e qualidade para o mercado russo, enquanto para o Paraguai, o foco deve ser o crescimento em exportações de vinhos premium.

Fontes Citadas:
Instituto Brasileiro do Vinho (Ibravin). "Vinhos brasileiros no mercado internacional." Ibravin

Ministério da Indústria, Comércio Exterior e Serviços (MDIC). "Comércio Exterior do Brasil." MDIC

Pew Research. "Rússia e sua economia: Desafios atuais." Pew Research

BBC News. "Como as sanções econômicas impactaram o mercado russo." BBC News

Agência Brasil. "Mercado de vinhos no Paraguai: crescimento e tendências." Agência Brasil

Paraguay.com. "Demanda crescente por vinhos importados." Paraguay.com


import pandas as pd
import matplotlib.pyplot as plt
import numpy as np

# Carrega a base de dados (já com as colunas de valor por kg)
df = pd.read_csv('vinhos_com_valor_por_kg.csv')
df.columns = df.columns.str.strip()

anos = list(range(2009, 2024))

# Preparar dados
dados_plot = []

df_filtrado = df[df['País'].isin(['Rússia', 'Paraguai'])]

for _, row in df_filtrado.iterrows():
    pais = row['País']
    for ano in anos:
        kg_col = f'{ano}_kg'
        usd_per_kg_col = f'{ano}_USD_per_kg'
        if kg_col in row and usd_per_kg_col in row:
            quantidade = row[kg_col]
            valor_por_kg = row[usd_per_kg_col]
            if quantidade > 0:
                dados_plot.append({
                    'País': pais,
                    'Ano': ano,
                    'Quantidade_kg': quantidade,
                    'Valor_USD_por_kg': valor_por_kg
                })

# DataFrame com dados
df_corr = pd.DataFrame(dados_plot)

# Plotar
plt.figure(figsize=(10, 6))

for pais in df_corr['País'].unique():
    df_p = df_corr[df_corr['País'] == pais]
    
    # Scatter
    plt.scatter(df_p['Quantidade_kg'], df_p['Valor_USD_por_kg'], label=pais, alpha=0.7)
    
    # Regressão linear
    x = df_p['Quantidade_kg']
    y = df_p['Valor_USD_por_kg']
    if len(x) > 1:
        coef = np.polyfit(x, y, 1)
        poly1d_fn = np.poly1d(coef)
        plt.plot(sorted(x), poly1d_fn(sorted(x)), linestyle='--', label=f'{pais} - tendência')

# Eixos e título
plt.xlabel('Quantidade exportada (kg)')
plt.ylabel('Valor por kg (US$)')
plt.title('Correlação entre quantidade exportada e valor por kg (com tendência)')
plt.legend()
plt.grid(True)
plt.tight_layout()
plt.show()


Introdução
O Brasil tem se consolidado como um dos maiores produtores e exportadores de vinho no cenário mundial. O país, com sua diversidade de vinhedos e a crescente qualidade de seus produtos, tem ganhado destaque, especialmente na América Latina, com mercados como a Rússia e o Paraguai sendo importantes destinos das exportações brasileiras.

Neste relatório, será realizada uma análise das exportações de vinhos brasileiros para a Rússia e o Paraguai no período de 2009 a 2023. Serão exploradas as tendências temporais dessas exportações, com foco na evolução do valor exportado e na correlação entre o volume exportado e o valor por kg.

No decorrer do relatório, apresentaremos:

Gráfico temporal: A evolução do valor exportado de vinhos para cada país ao longo dos anos, destacando o crescimento desses mercados em relação a outros destinos.

Correlação entre quantidade exportada e valor por kg: Analisaremos a relação entre o volume de vinhos exportados e o valor médio por kg, destacando as diferenças nas trajetórias da Rússia e do Paraguai.

Análise de outliers: Utilizaremos um gráfico de dispersão para verificar anomalias nos dados de exportação, como o pico de 2013 na Rússia e o crescimento gradual do Paraguai a partir de 2018.

As conclusões que surgirem a partir dessas análises ajudarão a entender as dinâmicas de exportação e as oportunidades de aprimoramento para o mercado de vinhos brasileiros.

Espaço para Gráfico 1: Evolução Temporal do Valor Exportado para Rússia e Paraguai
Espaço para Gráfico 2: Correlação entre Quantidade Exportada e Valor por Kg
Espaço para Gráfico 3: Análise de Outliers - Gráfico de Dispersão
Conclusão


📊 Análise do Gráfico de Dispersão – Rússia
Ao analisar o gráfico de dispersão das exportações brasileiras de vinho para a Rússia entre 2009 e 2023, observa-se que três anos apresentaram valores acima da média histórica do período. No entanto, destaca-se o ano de 2013, que registrou um valor exportado significativamente mais alto em comparação aos demais anos, caracterizando-se como um possível outlier positivo. Esse pico abrupto em 2013 não se repetiu nos anos seguintes, indicando que se tratou de um evento pontual e não de uma tendência sustentada. A visualização reforça a importância de considerar variações atípicas ao analisar séries históricas de exportação, especialmente em mercados voláteis como o russo.



📊 Análise do Gráfico de Dispersão – Paraguai
A análise do gráfico de dispersão das exportações de vinho do Brasil para o Paraguai entre 2009 e 2023 revela uma mudança de patamar a partir do ano de 2017. A partir desse ponto, os valores exportados passaram a se manter consistentemente acima da média histórica, indicando um crescimento sustentado na demanda paraguaia por vinhos brasileiros. Diferentemente da Rússia, onde se observou um pico isolado, no caso do Paraguai, os dados mostram uma tendência de estabilização em um novo patamar mais elevado, que se manteve até 2023. Essa evolução sugere uma consolidação do mercado paraguaio como destino importante e estável para os vinhos brasileiros.


import matplotlib.pyplot as plt
import pandas as pd

# Anos e colunas
anos = list(range(2009, 2024))
colunas_valor = [f"{ano}_US$" for ano in anos]

# Seleciona a Rússia
russia = df_filtrado[df_filtrado['País'] == 'Rússia'].iloc[0]
valores = [russia[col] for col in colunas_valor]

# DataFrame auxiliar
df_plot = pd.DataFrame({
    'Ano': anos,
    'Valor Exportado (US$)': valores
})

# Média dos valores
media_valores = sum(valores) / len(valores)

# Plot
plt.figure(figsize=(10, 5))
plt.scatter(df_plot['Ano'], df_plot['Valor Exportado (US$)'], s=80, color='blue', label='Dados')

# Linha horizontal da média
plt.axhline(y=media_valores, color='gray', linestyle='--', linewidth=1, label=f'Média: {media_valores:,.0f} US$')

# Respiro no topo
max_y = max(valores)
plt.ylim(top=max_y * 1.10)

# Estilo
plt.title('Exportações de Vinho do Brasil para a Rússia (2009–2023)', fontsize=14)
plt.xlabel('Ano')
plt.ylabel('Valor Exportado (US$)')
plt.xticks(anos, rotation=45)
plt.grid(True)
plt.legend()
plt.tight_layout()
plt.show()


import matplotlib.pyplot as plt
import pandas as pd

# Anos e colunas
anos = list(range(2009, 2024))
colunas_valor = [f"{ano}_US$" for ano in anos]

# Seleciona o Paraguai
paraguai = df_filtrado[df_filtrado['País'] == 'Paraguai'].iloc[0]
valores = [paraguai[col] for col in colunas_valor]

# DataFrame auxiliar
df_plot = pd.DataFrame({
    'Ano': anos,
    'Valor Exportado (US$)': valores
})

# Média dos valores para linha central
media_valores = sum(valores) / len(valores)

# Plot
plt.figure(figsize=(10, 5))
plt.scatter(df_plot['Ano'], df_plot['Valor Exportado (US$)'], s=80, color='green', label='Dados')

# Linha horizontal da média
plt.axhline(y=media_valores, color='gray', linestyle='--', linewidth=1, label=f'Média: {media_valores:,.0f} US$')

# Respiro superior
max_y = max(valores)
plt.ylim(top=max_y * 1.10)

# Estilo
plt.title('Exportações de Vinho do Brasil para o Paraguai (2009–2023)', fontsize=14)
plt.xlabel('Ano')
plt.ylabel('Valor Exportado (US$)')
plt.xticks(anos, rotation=45)
plt.grid(True)
plt.legend()
plt.tight_layout()
plt.show()


✅ Conclusão
A análise das exportações de vinhos brasileiros para a Rússia e o Paraguai no período de 2009 a 2023 revela importantes tendências sobre o comportamento desses mercados e as dinâmicas de comércio exterior do Brasil. O país, como um dos principais produtores e exportadores de vinho no mundo, tem mostrado um crescimento contínuo nas suas exportações, com o Paraguai e a Rússia se destacando como destinos significativos para os vinhos brasileiros.

A partir da evolução temporal do valor exportado, ficou evidente que ambos os mercados apresentaram trajetórias distintas. Para a Rússia, observou-se uma flutuação acentuada, com um pico considerável em 2013, caracterizando um outlier positivo. A partir desse ano, os valores exportados para esse país apresentaram volatilidade e não houve uma tendência de crescimento constante, sugerindo que o mercado russo pode ser afetado por variáveis externas, como crises econômicas e políticas.

Em contraste, o Paraguai apresentou uma tendência crescente e estável a partir de 2017, quando os valores de exportação passaram a se manter acima da média histórica. Essa mudança indica que o Paraguai se consolidou como um mercado mais estável e sustentável para os vinhos brasileiros, com um crescimento gradual e sem grandes oscilações. A correlação entre quantidade exportada e valor por kg reforçou essa análise, mostrando que o aumento nas exportações para o Paraguai não está apenas relacionado ao volume, mas também à valorização do vinho brasileiro nesse mercado.

A análise de outliers revelou, de forma mais clara, a anomalidade de 2013 na Rússia, quando as exportações dispararam, mas não se repetiram nos anos seguintes. Já o Paraguai, por sua vez, manteve-se dentro de uma tendência estável, com um crescimento contínuo após 2017.

Em resumo, a partir dos gráficos e das análises realizadas, conclui-se que o Paraguai se apresenta como um mercado mais promissor e constante, enquanto a Rússia, devido à sua volatilidade, exige estratégias específicas para lidar com flutuações de demanda. O entendimento dessas dinâmicas é crucial para os produtores e exportadores brasileiros, que devem considerar essas características ao planejar suas estratégias de exportação e explorar novos mercados internacionais.