## 📖 **Descrição do Projeto:**


## Principais Funcionalidades:


## 🛠️ Ferramentas Utilizadas:


## 📋 **Descrição do Processo:**
🔹 Renomear as colunas corretamente
🔹 Filtrar os anos de 2009 a 2023
🔹 Selecionar os 5 países que mais exportaram
🔹 Criar um gráfico de linha para mostrar a evolução dos valores (US$) ano a ano



## 💻 **Comandos:** 

🧠 PARTE 1 – Leitura e Renomeação
df = pd.read_csv('/content/ExpVinho (1).csv', sep='\t')
➡️ Lê o arquivo CSV usando tabulação (\t) como separador, pois seu arquivo não usa vírgula.


colunas_originais = df.columns.tolist()
➡️ Cria uma lista com todos os nomes de colunas do arquivo original.


novos_nomes = colunas_originais[:2]  # ['Id', 'País']
anos = colunas_originais[2:]
➡️ Guarda os dois primeiros nomes ("Id", "País") que não precisam mudar
➡️ A variável anos contém todas as colunas numéricas (ex: '1970', '1970.1', '1971', etc.)

for i in range(0, len(anos), 2):
    ano = anos[i].split('.')[0]
    novos_nomes.append(f"{ano}_kg")
    novos_nomes.append(f"{ano}_valor")
➡️ Esse for percorre a lista de anos, de 2 em 2 colunas, pois cada ano aparece 2 vezes (uma para kg e outra para valor).

split('.')[0] remove sufixos como .1, .2, que o pandas usa para diferenciar colunas com nomes duplicados

Ele cria nomes como 2009_kg, 2009_valor, 2010_kg, 2010_valor, etc.


df.columns = novos_nomes
➡️ Aplica os novos nomes de colunas ao DataFrame original.




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
Desde 2018, o Paraguai assumiu o posto de maior comprador de vinhos do Brasil. Segundo a Ibravin e o projeto Wines of Brasil, o país foi responsável por quase 80% das exportações de vinho tranquilo em 2021.

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



📈 Crescimento das Exportações de Vinho do Brasil para o Paraguai
A partir de 2018, o Paraguai se consolidou como o principal destino das exportações de vinhos tranquilos do Brasil. Em 2021, o país representou 79% das exportações de vinhos tranquilos brasileiros, com um crescimento de 190% em volume e 209% em valor em comparação com o ano anterior .

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


https://revistaadega.uol.com.br/artigo/analise-exportacoes-de-vinhos-brasileiros-crescem-52-em-valor-de-janeiro-setembro-de-2021-o-que-isto-quer-dizer_13472

https://blogs.oglobo.globo.com/lauro-jardim/post/paraguai-e-o-maior-importador-de-vinho-brasileiro.html


https://www.vinhospelomundo.com/vinhos-e-espumantes-brasileiros-cre?utm_source

https://engarrafadormoderno.com.br/mercado/exportacoes-de-vinhos-e-espumantes-crescem-mais-de-60?utm_source

https://agroemdia.com.br/2022/01/20/exportacoes-brasileiras-de-vinho-aumentam-8325-em-2021/?utm_source


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