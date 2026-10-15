# Perfis de operação e risco de falha em uma máquina industrial

Projeto final de clusterização hierárquica · **Tema:** Indústria e Manufatura · **Base:** AI4I 2020 Predictive Maintenance (UCI)

**[Código e análise completa](projeto_final_clusterizacao.py)** · **[Apresentação para o CEO (PDF)](Recomenda%C3%A7%C3%B5es%20para%20o%20CEO.pdf)**

> **Pergunta do projeto:** quais perfis de operação a máquina apresenta, olhando só para os sensores, e qual deles concentra mais falhas?

## Resultado

As 10.000 operações foram agrupadas só pelos cinco sensores. A informação de falha não entrou no agrupamento e serviu depois para validar os grupos. A clusterização hierárquica (Ward) encontrou **dois perfis**:

| Perfil | Operações | Falhas | Taxa de falha | Parcela das falhas |
|---|---|---|---|---|
| 🟧 Operação pesada | 4.868 (49%) | 283 | 5,8% | 83% |
| 🟦 Operação leve | 5.132 (51%) | 56 | 1,1% | 17% |
| Base inteira | 10.000 | 339 | 3,4% | 100% |

- A **operação pesada** tem torque maior, rotação menor, temperaturas mais altas e ferramenta mais desgastada. Sua taxa de falha é **5,3 vezes** a da operação leve, e ela reúne **83% das falhas** com 49% das operações.
- Todas as 115 falhas por calor e 97 das 98 por sobrecarga estão na operação pesada.
- **Recomendação:** priorizar a manutenção preventiva nas operações pesadas, com foco em calor e sobrecarga.

> **Como ler este resultado:** os perfis se sobrepõem (silhouette 0,19; correlação cofenética 0,40), então servem para **priorizar** a manutenção, e não para classificar cada operação. Com uma estrutura tão fraca, a divisão em dois perfis é frágil: outro algoritmo, outra amostra ou outro corte pode mudar os números. Além disso, a base é sintética e as falhas seguem regras sobre os mesmos sensores; as relações são associações, e não prova de causa.

Os gráficos principais estão na [apresentação](Recomenda%C3%A7%C3%B5es%20para%20o%20CEO.pdf). O código da análise gera todos os gráficos ao ser executado (veja [Como executar](#como-executar)).

## Como foi feito

Cada etapa está explicada, junto com o código que gera as tabelas e os gráficos, na [análise completa](projeto_final_clusterizacao.py) (os números das seções abaixo são os do arquivo).

1. **Análise exploratória** (seção 3): cada sensor, a relação entre eles e a taxa de falha da base (3,4%).
2. **Padronização** (seção 4.1): cada sensor vira z-score, para que nenhum domine a distância por causa da escala.
3. **Algoritmo** (seção 4.2): clusterização hierárquica aglomerativa com distância euclidiana. Foram comparados os critérios de ligação *single*, *complete*, *average* e *Ward*; o Ward foi o escolhido por ser o único que forma grupos de tamanho comparável (51% e 49%).
4. **Número de grupos** (seção 4.3): o maior intervalo de altura do dendrograma e o silhouette médio apontam **k = 2**.
5. **Avaliação** (seção 5): silhouette e correlação cofenética, que indicam estrutura fraca.
6. **Descrição dos perfis** (seção 6): comparação dos sensores, da taxa de falha e dos modos de falha de cada perfil.
7. **Conclusões e limites** (seção 7).

## Conteúdo do repositório

```text
.
├── README.md
├── projeto_final_clusterizacao.py   código e textos da análise (exportado do Google Colab)
└── Recomendações para o CEO.pdf     apresentação com as recomendações (9 slides)
```

## Como executar

O arquivo `.py` foi exportado do Google Colab, onde o projeto foi feito: os textos entre aspas triplas (`"""`) são as explicações do notebook, e o resto é o código. Para ver as tabelas e os gráficos, execute-o dentro de um notebook.

**No Google Colab (sem instalar nada):** abra um notebook novo em [colab.research.google.com](https://colab.research.google.com), cole as duas linhas abaixo em uma célula e execute.

```python
!wget -q https://raw.githubusercontent.com/FWerner04/projeto-final-clusterizacao/main/projeto_final_clusterizacao.py
%run projeto_final_clusterizacao.py
```

**No Jupyter, no computador:**

```bash
git clone https://github.com/FWerner04/projeto-final-clusterizacao.git
cd projeto-final-clusterizacao
pip install numpy pandas matplotlib scipy scikit-learn jinja2 notebook
jupyter notebook
```

Crie um notebook novo (botão *New*) e execute `%run projeto_final_clusterizacao.py` em uma célula.

## Dados

**AI4I 2020 Predictive Maintenance Dataset**, de S. Matzka, publicado no UCI Machine Learning Repository sob a licença [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). São 10.000 operações de uma máquina industrial, com 5 sensores e rótulos de falha. A base é **sintética** (simulada para imitar dados reais de manutenção), o que limita as conclusões. O dicionário das variáveis está na seção 2 do código.

O código baixa o CSV de um link público (uma cópia no GitHub). Se o link falhar, baixe a base na [página da UCI](https://archive.ics.uci.edu/dataset/601/ai4i) e salve o CSV como `ai4i2020.csv` na mesma pasta do código (no Colab, use o painel de arquivos, à esquerda).

## Referências

- *AI4I 2020 Predictive Maintenance Dataset* [Dataset]. (2020). UCI Machine Learning Repository. https://doi.org/10.24432/C5HS5C
- Matzka, S. (2020). *Explainable Artificial Intelligence for Predictive Maintenance Applications*. International Conference on Artificial Intelligence for Industries (AI4I).
- Kaufman, L.; Rousseeuw, P. J. (1990). *Finding Groups in Data: An Introduction to Cluster Analysis*. Wiley (regra prática para interpretar o silhouette).

## Autor

Fernando Werner · [@FWerner04](https://github.com/FWerner04)
