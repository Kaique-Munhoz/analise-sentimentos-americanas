# Análise de Sentimentos e Tópicos em Avaliações de E-commerce

**Projeto acadêmico de Ciência de Dados e Processamento de Linguagem Natural (NLP) — PUC-SP**

Este projeto analisa avaliações reais do conjunto **B2W-Reviews01 (Americanas)** para classificar sentimentos, investigar limitações dos modelos e identificar temas recorrentes que possam apoiar decisões de negócio.

## Objetivos

- Auditar a qualidade dos dados e a consistência dos rótulos.
- Classificar avaliações em **negativas, neutras e positivas**.
- Comparar modelos de Machine Learning com um baseline simples.
- Investigar erros e métricas por classe, em vez de confiar apenas na acurácia.
- Identificar temas recorrentes com **NMF** e relacioná-los aos sentimentos.

## Base de dados

- **132.373 avaliações** e 14 colunas no arquivo original.
- **129.098 avaliações com texto**, utilizadas na modelagem textual.
- **3.275 avaliações sem texto**.
- **126.724 textos distintos não nulos** e **2.374 repetições** entre os textos não nulos.

### Definição dos sentimentos

O rótulo foi criado a partir de `overall_rating`:

| Nota | Classe |
|---|---|
| 1–2 estrelas | Negativo |
| 3 estrelas | Neutro |
| 4–5 estrelas | Positivo |

A coluna `recommend_to_a_friend` foi utilizada para auditar a consistência dessa regra. **88,47% das avaliações de três estrelas indicavam recomendação do produto**, sugerindo que a definição operacional da classe neutra é ambígua.

![Distribuição das notas](imagens/distribuicao_notas.png)

## Metodologia

1. **Auditoria:** valores ausentes, duplicatas, distribuição das notas e consistência dos rótulos.
2. **Preparação textual:** inspeção de URLs, HTML, repetições e comprimento dos textos; limpeza conservadora para preservar informações de sentimento.
3. **Vetorização:** TF-IDF com unigramas e bigramas, ajustado somente sobre o conjunto de treino.
4. **Classificação:** Regressão Logística e Linear SVM, com separação estratificada de 80% para treino e 20% para teste.
5. **Avaliação:** baseline, acurácia, Macro-F1, recall por classe, matriz de confusão e análise de erros.
6. **Tópicos:** NMF, comparação de soluções com 6, 8 e 10 tópicos e interpretação dos oito temas escolhidos.

## Resultados dos classificadores

Resultados salvos no notebook executado:

| Métrica | Resultado |
|---|---:|
| Baseline — acurácia no teste | 61,44% |
| Regressão Logística — acurácia | **84,59%** |
| Regressão Logística — Macro-F1 | **70,09%** |
| Regressão Logística — recall negativo | 90,01% |
| Regressão Logística — recall neutro | **23,24%** |
| Regressão Logística — recall positivo | 94,66% |
| Linear SVM — acurácia | 84,04% |
| Linear SVM — Macro-F1 | 70,21% |
| Linear SVM — recall neutro | 25,58% |

**Principal aprendizado:** embora a Regressão Logística tenha alcançado 84,59% de acurácia, reconheceu corretamente apenas cerca de 23 em cada 100 avaliações rotuladas como neutras. Isso mostra por que uma métrica global não é suficiente para avaliar a qualidade de um classificador.

## Temas identificados com NMF

Os oito tópicos foram nomeados após a inspeção dos termos mais representativos:

| Tópico | Interpretação |
|---|---|
| 0 | Entrega rápida |
| 1 | Não recebimento do produto |
| 2 | Cumprimento do prazo |
| 3 | Avaliação geral do produto |
| 4 | Qualidade do produto |
| 5 | Custo-benefício |
| 6 | Atendimento às expectativas |
| 7 | Satisfação e recomendação |

No cruzamento entre tema e sentimento, o tópico **Não recebimento do produto** apresentou **58,35% de avaliações negativas**, enquanto **Satisfação e recomendação** apresentou **91,44% de avaliações positivas**. Essas relações são **associações observadas**, não evidências de causalidade.

## Tecnologias

**Python · Pandas · NumPy · scikit-learn · Matplotlib · TF-IDF · Regressão Logística · Linear SVM · NMF · NLP**

## Como executar

1. Obtenha o arquivo original `B2W-Reviews01.csv` na fonte pública do conjunto B2W-Reviews01.
2. Coloque o CSV na mesma pasta de `projeto_nlp_americanas.ipynb`.
3. Instale as dependências:

   ```bash
   pip install -r requirements.txt
   ```

4. Abra o notebook no **Jupyter** ou **Google Colab** e execute as células na ordem.

> O CSV não é incluído neste repositório. O notebook contém as etapas de análise e os resultados de sua execução; métricas podem variar se os dados ou configurações forem alterados.

## Conclusão

O projeto demonstra um fluxo de NLP que vai além do treinamento de classificadores: inclui auditoria, validação, análise de erros e interpretação de tópicos. O principal resultado metodológico foi identificar que uma boa acurácia geral pode esconder desempenho insuficiente em uma classe específica, especialmente quando a própria definição do rótulo apresenta ambiguidades.
