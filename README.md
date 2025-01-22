

Este projeto visa realizar uma análise das vendas de uma base de dados fornecida, utilizando bibliotecas como **Pandas**, **Plotly**, e **Bar Chart Race** para criar visualizações interativas e animadas das informações. O objetivo é gerar gráficos de barras dinâmicos para visualização do desempenho das vendas por categoria, por dia, por tipo de pagamento, entre outras métricas.

## Índice
- [Objetivo do Projeto](#objetivo-do-projeto)
- [Tecnologias Utilizadas](#tecnologias-utilizadas)
- [Configuração do Ambiente](#configuração-do-ambiente)
- [Como Rodar o Projeto](#como-rodar-o-projeto)
- [Erros Comuns e Soluções](#erros-comuns-e-soluções)
- [Estrutura de Arquivos](#estrutura-de-arquivos)

## Objetivo do Projeto

O objetivo deste projeto é analisar os dados de vendas de um supermercado, considerando aspectos como:
- Faturamento por dia
- Faturamento por categoria de produto
- Faturamento por filial
- Faturamento por forma de pagamento
- Avaliações das filiais

Através de gráficos interativos e animações, o projeto busca fornecer uma visualização dinâmica para facilitar a análise e interpretação dos dados.

## Tecnologias Utilizadas

- **Pandas**: Para manipulação e limpeza dos dados.
- **Plotly**: Para criação de gráficos interativos.
- **Bar Chart Race**: Para criar animações de gráficos de barras.
- **Streamlit** (opcional): Para gerar uma interface de usuário interativa para a visualização.
- **ffmpeg**: Para renderizar animações.

## Configuração do Ambiente

1. **Instalar o Python 3.x**: Certifique-se de que você tenha o Python instalado em sua máquina. Para verificar, execute o comando:

    ```bash
    python --version
    ```

2. **Criar e Ativar um Ambiente Virtual** (opcional, mas recomendado):

    No terminal, execute:

    ```bash
    python -m venv venv
    ```

    Ative o ambiente virtual:
    - No Windows:

      ```bash
      .\venv\Scripts\activate
      ```

    - No Mac/Linux:

      ```bash
      source venv/bin/activate
      ```

3. **Instalar as dependências**:

    Com o ambiente ativado, instale as bibliotecas necessárias:

    ```bash
    pip install pandas plotly bar-chart-race openpyxl ffmpeg-python
    ```

4. **Instalar o ffmpeg**: Para rodar as animações de gráficos de barras, você precisa do **ffmpeg**. Siga as instruções em [FFmpeg Official](https://www.ffmpeg.org/download.html) para instalar o ffmpeg no seu sistema operacional e adicionar o ffmpeg nas variáveis de ambiente.

## Como Rodar o Projeto

1. **Carregar a base de dados**:
    O código começa carregando um arquivo Excel chamado `BarChartRace.xlsx`, que contém dados de vendas. Certifique-se de ter o arquivo em seu diretório ou especifique o caminho correto para ele.

    ```python
    base = pd.read_excel("BarChartRace.xlsx")
    ```

2. **Preparar os Dados**:
    O próximo passo é preparar os dados para a análise, ajustando-os de acordo com as colunas desejadas.

    ```python
    base = base.pivot_table('Venda', 'Data', 'Categoria')
    ```

3. **Gerar os Gráficos Animados**:
    Para gerar a animação do gráfico de barras, você pode usar a função `bar_chart_race` da biblioteca **Bar Chart Race**.

    ```python
    import bar_chart_race as bcr

    bcr.bar_chart_race(
        df=base,
        filename=None,
        orientation='h',
        sort='desc',
        n_bars=6,
        fixed_order=False,
        fixed_max=True,
        steps_per_period=10,
        interpolate_period=False,
        label_bars=True,
        bar_size=.95,
        period_label={'x': .99, 'y': .25, 'ha': 'right', 'va': 'center'},
        period_fmt='%B %d, %Y',
        period_summary_func=lambda v, r: {'x': .99, 'y': .18,
                                          's': f'Total de venda: {v.nlargest(6).sum():,.0f}',
                                          'ha': 'right', 'size': 8, 'family': 'Courier New'},
        perpendicular_bar_func='median',
        period_length=500,
        figsize=(5, 3),
        dpi=144,
        cmap='dark12',
        title='Venda por categoria',
        bar_label_size=7,
        tick_label_size=7,
        scale='linear',
        bar_kwargs={'alpha': .7}
    )
    ```

4. **Exibir Gráficos**:
    Se estiver utilizando **Streamlit**, você pode exibir os gráficos de forma interativa. Aqui está um exemplo de como gerar gráficos de vendas por categoria, avaliação, entre outros:

    ```python
    import streamlit as st
    import plotly.express as px

    st.set_page_config(layout="wide")

    # Carregar os dados
    df = pd.read_csv("supermarket_sales.csv", sep=";", decimal=",")
    df["Date"] = pd.to_datetime(df["Date"])
    df = df.sort_values("Date")

    # Gerar gráficos
    fig_date = px.bar(df_filtered, x="Date", y="Total", color="City", title="Faturamento por dia")
    st.plotly_chart(fig_date)
    ```

## Erros Comuns e Soluções

1. **Erro `FileNotFoundError`**:
    Esse erro ocorre quando o arquivo `BarChartRace.xlsx` não está no diretório correto. Certifique-se de fornecer o caminho correto para o arquivo. Você pode verificar o diretório atual com:

    ```python
    import os
    print(os.getcwd())  # Exibe o diretório atual
    ```

2. **Erro `ffmpeg not found`**:
    Se o erro indicar que o **ffmpeg** não está instalado, você precisará seguir as instruções para instalá-lo e configurá-lo corretamente no seu sistema. O ffmpeg é necessário para gerar as animações do gráfico de barras.

3. **Erro `ModuleNotFoundError: No module named 'bar_chart_race'`**:
    Se esse erro aparecer, significa que a biblioteca **bar-chart-race** não foi instalada corretamente. Execute:

    ```bash
    pip install bar-chart-race
    ```

## Estrutura de Arquivos


## Conclusão

Este projeto fornece uma análise detalhada de vendas com visualizações interativas e animações. Usando as ferramentas certas como **Pandas**, **Plotly** e **Bar Chart Race**, conseguimos transformar dados em insights visuais interessantes e dinâmicos.

Para mais detalhes sobre a biblioteca **Bar Chart Race**, consulte a documentação oficial em [pypi.org/project/bar-chart-race](https://pypi.org/project/bar-chart-race/).

---

Se tiver alguma dúvida ou sugestão, sinta-se à vontade para abrir uma _issue_ ou enviar um _pull request_.



