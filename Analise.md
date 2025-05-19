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



https://revistaadega.uol.com.br/artigo/analise-exportacoes-de-vinhos-brasileiros-crescem-52-em-valor-de-janeiro-setembro-de-2021-o-que-isto-quer-dizer_13472

https://blogs.oglobo.globo.com/lauro-jardim/post/paraguai-e-o-maior-importador-de-vinho-brasileiro.html


https://www.vinhospelomundo.com/vinhos-e-espumantes-brasileiros-cre?utm_source

https://engarrafadormoderno.com.br/mercado/exportacoes-de-vinhos-e-espumantes-crescem-mais-de-60?utm_source


