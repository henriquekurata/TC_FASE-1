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

