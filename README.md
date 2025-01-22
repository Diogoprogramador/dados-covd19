# Análise de Dados COVID-19 com Visualizações Interativas

Este projeto visa realizar uma análise interativa dos dados de **COVID-19** com o uso de bibliotecas como **Pandas**, **Plotly** e **Bar Chart Race** para criar gráficos dinâmicos e interativos. O objetivo é visualizar o impacto da pandemia, observando as métricas de mortes por país ao longo do tempo e outras análises importantes.

## Índice
- [Objetivo do Projeto](#objetivo-do-projeto)
- [Tecnologias Utilizadas](#tecnologias-utilizadas)
- [Configuração do Ambiente](#configuração-do-ambiente)
- [Como Rodar o Projeto](#como-rodar-o-projeto)
- [Erros Comuns e Soluções](#erros-comuns-e-soluções)
- [Estrutura de Arquivos](#estrutura-de-arquivos)

## Objetivo do Projeto

O objetivo deste projeto é analisar os dados relacionados à pandemia de **COVID-19** de forma visual e interativa. O projeto utiliza os dados fornecidos por fontes públicas (como o **Johns Hopkins University**), que incluem informações de casos confirmados, mortes e recuperações por país, ao longo do tempo.

### Principais análises:
- **Animação de mortes por país**: Gráfico de barras animado mostrando o número de mortes por COVID-19 ao longo do tempo para os 6 países mais afetados.
- **Faturamento por categoria de produto** (se os dados tiverem essa variável).
- **Distribuição por forma de pagamento** (caso aplicável).

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

1. **Carregar e preparar os dados**:

    O código começa carregando um arquivo de dados sobre COVID-19. Para esse exemplo, você pode usar a base de dados `covid19_tutorial` disponível com a biblioteca **Bar Chart Race**, mas você pode substituir pelo seu próprio conjunto de dados.

    ```python
    import bar_chart_race as bcr

    # Carregar o dataset
    df = bcr.load_dataset('covid19_tutorial')

    # Pré-processamento (se necessário)
    df = df.fillna(0)
    df = df.T  # Transpor para ter os países nas colunas e as datas nas linhas
    ```

2. **Gerar gráficos animados**:

    Para gerar a animação do gráfico de barras sobre as mortes por COVID-19, você pode usar o seguinte código:

    ```python
    bcr.bar_chart_race(
        df=df,
        filename=None,  # Não salva o arquivo, apenas exibe
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
                                          's': f'Total deaths: {v.nlargest(6).sum():,.0f}',
                                          'ha': 'right', 'size': 8, 'family': 'Courier New'},
        perpendicular_bar_func='median',
        period_length=500,
        figsize=(5, 3),
        dpi=144,
        cmap='dark12',
        title='COVID-19 Deaths by Country',
        bar_label_size=7,
        tick_label_size=7,
        scale='linear',
        bar_kwargs={'alpha': .7}
    )
    ```

3. **Exibir gráficos**:

    Se estiver utilizando **Streamlit**, você pode exibir gráficos interativos com o **Plotly**. Aqui está um exemplo de como gerar gráficos para a visualização de casos ou mortes por país ao longo do tempo:

    ```python
    import streamlit as st
    import plotly.express as px

    st.set_page_config(layout="wide")

    # Carregar os dados
    df = pd.read_csv("covid19_data.csv")  # Substitua pelo seu arquivo de dados
    df["Date"] = pd.to_datetime(df["Date"])
    df = df.sort_values("Date")

    # Gerar gráficos
    fig_date = px.bar(df, x="Date", y="TotalDeaths", color="Country", title="Total de Mortes por País")
    st.plotly_chart(fig_date)
    ```

## Erros Comuns e Soluções

1. **Erro `FileNotFoundError`**:
    Esse erro ocorre quando o arquivo `covid19_data.csv` ou `covid19_tutorial` não está no diretório correto. Certifique-se de fornecer o caminho correto para o arquivo.

2. **Erro `ffmpeg not found`**:
    Se o erro indicar que o **ffmpeg** não está instalado, você precisará seguir as instruções para instalá-lo e configurá-lo corretamente no seu sistema. O **ffmpeg** é necessário para gerar as animações do gráfico de barras.

3. **Erro `ModuleNotFoundError: No module named 'bar_chart_race'`**:
    Se esse erro aparecer, significa que a biblioteca **bar-chart-race** não foi instalada corretamente. Execute:

    ```bash
    pip install bar-chart-race
    ```

## Estrutura de Arquivos


## Conclusão

Este projeto fornece uma análise visual interativa do impacto da pandemia de **COVID-19**, utilizando gráficos dinâmicos e animações para representar as mudanças ao longo do tempo. Através de bibliotecas como **Pandas**, **Plotly**, e **Bar Chart Race**, conseguimos transformar dados de uma crise global em informações visuais acessíveis e dinâmicas.

Para mais detalhes sobre a biblioteca **Bar Chart Race**, consulte a documentação oficial em [pypi.org/project/bar-chart-race](https://pypi.org/project/bar-chart-race/).

---

Se tiver alguma dúvida ou sugestão, sinta-se à vontade para abrir uma _issue_ ou enviar um _pull request_.

