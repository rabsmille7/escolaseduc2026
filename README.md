# Análise do Fluxo Escolar da SEDUC-SP (2024)

Este projeto realiza uma Análise Exploratória de Dados (EDA) sobre o fluxo escolar (taxas de aprovação, reprovação e abandono) nas Diretorias de Ensino do Estado de São Paulo.

## 📌 Objetivos
* Compreender a distribuição das taxas de aprovação no Ensino Médio.
* Avaliar se o abandono escolar no Ensino Fundamental está correlacionado com o Ensino Médio.
* Identificar as 10 Diretorias de Ensino com melhor desempenho em aprovação.

## 🛠️ Tecnologias Utilizadas
* **Python 3.12**
* **Pandas**: Limpeza e manipulação dos dados.
* **Seaborn & Matplotlib**: Visualização de dados e gráficos.

## 📂 Como Executar o Projeto

1. Clone o repositório:
   ```bash
   git clone [https://github.com/rabsmille7/escolaseduc2026.git](https://github.com/rabsmille7/escolaseduc2026.git)
   cd escolaseduc2026




Crie e ative o ambiente virtual:
    Bash

    python3 -m venv venv
    source venv/bin/activate

    Instale as dependências:
    Bash

    pip install pandas seaborn matplotlib numpy

    Execute o script:
    Bash

    python escola.py

📊 Insights Obtidos

    Adicione aqui as suas conclusões sobre os gráficos gerados!

    Exemplo: "Notou-se que as Diretorias de Ensino X e Y possuem os melhores índices..."


---

## Próximo Passo: Atualizar o GitHub

Agora que o seu código `escola.py` está funcionando e não dá mais erro, salve-o no seu computador e envie as atualizações para o GitHub usando o terminal:

```bash
# 1. Veja os arquivos modificados
git status

# 2. Adicione as mudanças
git add escola.py README.md

# 3. Crie a mensagem da alteração
git commit -m "Fix: Correção na conversão de tipos e colunas do projeto"

# 4. Envie para o GitHub
git push origin main






# escolaseduc2026
Dados das escolas taxa de aprovação de ensino medio e fundamnental
Formado em Ciências de Dados, criei um acesso Banco de dados da Seduc Sp, obtemos a linguaguem a partir do CSV.



# =====================================================================
# 1. IMPORTAÇÃO DE BIBLIOTECAS E CONFIGURAÇÃO
# =====================================================================
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Configuração estética dos gráficos
sns.set_theme(style="whitegrid")

# =====================================================================
# 2. CARREGAMENTO DOS DADOS (Arquivo Local)
# =====================================================================
try:
    df = pd.read_csv('fluxo_escolar.csv', sep=';', encoding='utf-8')
except UnicodeDecodeError:
    df = pd.read_csv('fluxo_escolar.csv', sep=';', encoding='latin1')

print("--- Dados originais carregados com sucesso! ---")
print(f"Linhas: {df.shape[0]} | Colunas: {df.shape[1]}")

# =====================================================================
# 3. LIMPEZA E CONVERSÃO DE DADOS (Data Wrangling)
# =====================================================================
colunas_taxas = ['APR_1', 'REP_1', 'ABA_1', 'APR_2', 'REP_2', 'ABA_2', 'APR_3', 'REP_3', 'ABA_3']

for col in colunas_taxas:
    if col in df.columns:
        # 1. Garante que tudo é string, remove espaços e troca a vírgula por ponto
        df[col] = df[col].astype(str).str.replace(',', '.').str.strip()
        
        # 2. Converte para número. Se houver algum erro, vira NaN (Not a Number)
        df[col] = pd.to_numeric(df[col], errors='coerce')

# Preenche possíveis valores nulos gerados na conversão com a média da coluna
for col in colunas_taxas:
    if col in df.columns:
        df[col] = df[col].fillna(df[col].mean())

# Remover duplicatas
df = df.drop_duplicates()

print("\n--- Estrutura dos dados após a conversão ---")
print(df.info())

# =====================================================================
# 4. ANÁLISE EXPLORATÓRIA E VISUALIZAÇÃO
# =====================================================================

# ---------------------------------------------------------------------
# Gráfico 1: Distribuição da Taxa de Aprovação no Ensino Médio (APR_3)
# ---------------------------------------------------------------------
plt.figure(figsize=(10, 5))
sns.histplot(data=df, x='APR_3', kde=True, color='royalblue', bins=12)
plt.title('Distribuição da Taxa de Aprovação no Ensino Médio por DE', fontsize=14, pad=15)
plt.xlabel('Taxa de Aprovação - Ensino Médio (%)')
plt.ylabel('Frequência (Quantidade de DEs)')
plt.tight_layout()
plt.show()

# ---------------------------------------------------------------------
# Gráfico 2: Comparação de Abandono - Fundamental II (ABA_2) vs Ensino Médio (ABA_3)
# ---------------------------------------------------------------------
plt.figure(figsize=(8, 6))
sns.scatterplot(data=df, x='ABA_2', y='ABA_3', color='crimson', s=60, alpha=0.8)
sns.regplot(data=df, x='ABA_2', y='ABA_3', scatter=False, color='black', line_kws={"linestyle": "--"})

plt.title('Comparação da Taxa de Abandono (Fundamental II vs Ensino Médio)', fontsize=13, pad=15)
plt.xlabel('Taxa de Abandono - Fundamental Anos Finais (%)')
plt.ylabel('Taxa de Abandono - Ensino Médio (%)')
plt.tight_layout()
plt.show()

# ---------------------------------------------------------------------
# Gráfico 3: Top 10 Diretorias de Ensino com Maior Aprovação no Ensino Médio
# ---------------------------------------------------------------------
top_10_des = df.sort_values(by='APR_3', ascending=False).head(10)

plt.figure(figsize=(11, 6))
sns.barplot(data=top_10_des, x='APR_3', y='NM_DIRETORIA', palette='viridis')
plt.title('Top 10 Diretorias de Ensino em Aprovação (Ensino Médio)', fontsize=14, pad=15)
plt.xlabel('Taxa de Aprovação (%)')
plt.ylabel('Diretoria de Ensino (DE)')

# Adicionando os valores exatos nas barras
for index, value in enumerate(top_10_des['APR_3']):
    plt.text(value - 5, index, f'{value:.1f}%', color='white', va='center', fontweight='bold')

plt.tight_layout()
plt.show()




o link do acesso
https://dados.educacao.sp.gov.br/dataset/fluxo-escolar-por-diretoria-de-ensino/resource/a726cfb3-f1b8-4442-9f42-4247635cf2a2

