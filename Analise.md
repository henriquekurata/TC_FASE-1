## 📖 **Descrição do Projeto:**
Panorama das Exportações de Vinhos Brasileiros dos últimos 15 anos (2009 - 2023): Evolução Histórica, Desafios e Caminhos Futuros.
Abaixo estão os comandos em python que foram utilizados no decorrer do relatório e anexo `Readme.md`

## 🛠️ Ferramentas Utilizadas:
Google Colab


## 📋 **Descrição do Processo:**

1. PARTE 1 – Leitura e Renomeação
2. PARTE 2 – Filtrar apenas os anos desejados
3. PARTE 3 – Soma total de valores por país
4. PARTE 4 – Criar o gráfico de linha com os 5 maiores que mais importaram vinhos do Brasil 
5. PARTE 5 – Criar gráfico para análise de correlação (Valor venda X Volume exportado)
6. PARTE 6 – Criar gráficos para análise de outliers (Valores que estão acima da média histórica)



## 💻 **Comandos em python que foram utilizados para o tratamentos e criação de gráficos de dados a partir do arquivo `ExpVinho.csv`, utilizando o Google Colab**: 

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

```
anos_desejados = [str(ano) for ano in range(2009, 2024)]
#Cria uma lista com os anos de 2009 a 2023 (em formato texto): ['2009', '2010', ..., '2023']


colunas_mantidas = ['Id', 'País']
for ano in anos_desejados:
    colunas_mantidas.append(f"{ano}_kg")
    colunas_mantidas.append(f"{ano}_valor")

#Constrói uma lista com as colunas que queremos manter no DataFrame, como:
['Id', 'País', '2009_kg', '2009_valor', ..., '2023_kg', '2023_valor']


df_filtrado = df[colunas_mantidas]
#Filtra o DataFrame para manter só as colunas listadas acima.
```

💰 PARTE 3 – Soma total de valores por país

```
colunas_valor = [f"{ano}_valor" for ano in anos_desejados]

#Cria a lista com todas as colunas de valor (US$) que vamos somar:
['2009_valor', '2010_valor', ..., '2023_valor']


df_filtrado["total_USD"] = df_filtrado[colunas_valor].sum(axis=1)
#Cria uma nova coluna total_USD com a soma total por país (linha), considerando os valores de todos os anos.


top_paises = df_filtrado.sort_values(by="total_USD", ascending=False).head(5)
#Ordena os países do maior para o menor total exportado e pega os 5 primeiros.
```


📈 PARTE 4 – Criar o gráfico de linha com os 5 maiores que mais importaram vinhos do Brasil 

```
import matplotlib.pyplot as plt
plt.figure(figsize=(14, 7))
#Prepara o gráfico com tamanho 14x7 polegadas.

for i, row in top_paises.iterrows():
    valores = [row[f"{ano}_valor"] for ano in anos_desejados]
    plt.plot(anos_desejados, valores, marker='o', label=row['País'])

#Para cada país no top 5:
#Pega os valores de exportação em US$
#Plota uma linha ao longo dos anos

plt.title('Evolução do Valor Exportado por País (2009–2023)')
plt.xlabel('Ano')
plt.ylabel('Valor em US$')
plt.legend(title='País')
plt.grid(True)
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
#Adiciona rótulos, legenda, grade, rotação do eixo X e exibe o gráfico.
```


📈 PARTE 5 – Criação de gráfico para análise de correlação (Valor venda X Volume exportado)

```
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

```


📈 PARTE 6 – Criação de gráficos para análise de outliers (Valores que estão acima da média histórica)

Script abaixo para a Rússia:

```
import matplotlib.pyplot as plt
import pandas as pd

#Anos e colunas
anos = list(range(2009, 2024))
colunas_valor = [f"{ano}_US$" for ano in anos]

#Seleciona a Rússia
russia = df_filtrado[df_filtrado['País'] == 'Rússia'].iloc[0]
valores = [russia[col] for col in colunas_valor]

#DataFrame auxiliar
df_plot = pd.DataFrame({
    'Ano': anos,
    'Valor Exportado (US$)': valores
})

#Média dos valores
media_valores = sum(valores) / len(valores)

#Plot
plt.figure(figsize=(10, 5))
plt.scatter(df_plot['Ano'], df_plot['Valor Exportado (US$)'], s=80, color='blue', label='Dados')

#Linha horizontal da média
plt.axhline(y=media_valores, color='gray', linestyle='--', linewidth=1, label=f'Média: {media_valores:,.0f} US$')

#Respiro no topo
max_y = max(valores)
plt.ylim(top=max_y * 1.10)

#Estilo
plt.title('Exportações de Vinho do Brasil para a Rússia (2009–2023)', fontsize=14)
plt.xlabel('Ano')
plt.ylabel('Valor Exportado (US$)')
plt.xticks(anos, rotation=45)
plt.grid(True)
plt.legend()
plt.tight_layout()
plt.show()

```


Script abaixo para o Paraguai:

```
import matplotlib.pyplot as plt
import pandas as pd

#Anos e colunas
anos = list(range(2009, 2024))
colunas_valor = [f"{ano}_US$" for ano in anos]

#Seleciona o Paraguai
paraguai = df_filtrado[df_filtrado['País'] == 'Paraguai'].iloc[0]
valores = [paraguai[col] for col in colunas_valor]

#DataFrame auxiliar
df_plot = pd.DataFrame({
    'Ano': anos,
    'Valor Exportado (US$)': valores
})

#Média dos valores para linha central
media_valores = sum(valores) / len(valores)

#Plot
plt.figure(figsize=(10, 5))
plt.scatter(df_plot['Ano'], df_plot['Valor Exportado (US$)'], s=80, color='green', label='Dados')

#Linha horizontal da média
plt.axhline(y=media_valores, color='gray', linestyle='--', linewidth=1, label=f'Média: {media_valores:,.0f} US$')

#Respiro superior
max_y = max(valores)
plt.ylim(top=max_y * 1.10)

#Estilo
plt.title('Exportações de Vinho do Brasil para o Paraguai (2009–2023)', fontsize=14)
plt.xlabel('Ano')
plt.ylabel('Valor Exportado (US$)')
plt.xticks(anos, rotation=45)
plt.grid(True)
plt.legend()
plt.tight_layout()
plt.show()

``` 