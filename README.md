# Modelagem Dinâmica de Desempenho de Redes e Análise de Erros

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Análise comparativa entre modelos de tempo contínuo (Equações Diferenciais Ordinárias - EDOs) e tempo discreto (Modelo Autorregressivo com Entradas Exógenas - ARX) para a caracterização e previsão do desempenho de conexões de rede a partir de dados experimentais esparsos.

---

### Tabela de Conteúdos
* [Sobre o Projeto](#sobre-o-projeto)
* [Metodologia Aplicada](#metodologia-aplicada)
  * [Fase 1: Abordagem por Tempo Contínuo (EDOs)](#1-a-abordagem-por-tempo-contínuo-edos)
  * [Fase 2: Abordagem por Tempo Discreto (ARX + RLS)](#2-a-abordagem-por-tempo-discreto-arx--rls)
* [Resultados e Conclusão](#resultados-e-conclusão)
* [Tecnologias e Conceitos-Chave](#tecnologias-e-conceitos-chave)
* [Estrutura do Repositório](#estrutura-do-repositório)
* [Autores](#autores)
* [Licença](#licença)

---

## Sobre o Projeto

Este repositório contém o código e a análise do trabalho acadêmico desenvolvido para a disciplina de Matemática Computacional. O projeto investiga e compara duas metodologias fundamentalmente distintas — tempo contínuo e tempo discreto — para modelar a performance de conexões de rede (vazão e latência) utilizando um conjunto de dados do mundo real.

O desafio central era lidar com dados coletados em baixa frequência (intervalos de 30 minutos a 2 horas), o que viola premissas clássicas de modelos dinâmicos. A jornada documentada neste trabalho mostra a investigação sistemática, a falha de abordagens baseadas em Equações Diferenciais Ordinárias (EDOs) e a subsequente validação de um modelo de séries temporais (ARX) como a solução mais robusta e adequada. O objetivo final é demonstrar como a escolha do modelo matemático é criticamente dependente das características do processo de amostragem dos dados.

## Metodologia Aplicada

A análise foi dividida em duas fases sequenciais, onde o fracasso da primeira informou e justificou a adoção da segunda.

### 1. A Abordagem por Tempo Contínuo (EDOs)

A hipótese inicial foi tratar a performance da rede como um sistema dinâmico contínuo.

* **Pré-processamento com Splines Cúbicas**: Para aplicar EDOs a dados discretos, foi necessário primeiro interpolar as medições. Utilizamos **splines cúbicas**, implementadas manualmente e validadas, para gerar funções contínuas e deriváveis. No entanto, esta etapa revelou a alta sensibilidade do método a ruído e outliers, frequentemente gerando curvas fisicamente implausíveis (overshoot, valores negativos).

* **Modelagem com EDOs**: Foram testados múltiplos modelos, desde um sistema fenomenológico simples até abordagens mais complexas baseadas na teoria de fluidos do TCP e em osciladores harmônicos. A calibração dos parâmetros foi tentada via Regressão Linear, utilizando a **Equação Normal** implementada manualmente.

* **Resultado e Diagnóstico**: Esta abordagem se mostrou **inadequada**. As simulações dos modelos calibrados falharam sistematicamente em reproduzir o comportamento real dos dados. A causa raiz foi identificada como o **espaçamento temporal esparso** entre as medições, que quebra a premissa de continuidade causal que fundamenta os modelos de EDOs.

### 2. A Abordagem por Tempo Discreto (ARX + RLS)

Em resposta às limitações do modelo contínuo, a estratégia foi pivotada para um modelo que respeita a natureza sequencial e discreta dos dados.

* **Modelo**: Foi adotado um modelo **Autorregressivo com Entradas Exógenas (ARX)**, que descreve o estado da rede no próximo teste ($n+1$) como uma função linear do estado no teste atual ($n$). A equação principal para a vazão de download ($D$) é:
    $D[n+1] = c_0 + c_1 D[n] + c_2 L_D[n] + c_3 U[n] + c_4 L_U[n]$

* **Calibração**: O modelo foi calibrado em tempo real utilizando o algoritmo **Recursive Least Squares (RLS)**. Diferente de uma regressão em lote, o RLS é um método de aprendizado online que ajusta os coeficientes (`c_i`) a cada nova medição, permitindo que o modelo se adapte e que seu processo de aprendizado seja visualizado.

* **Resultado**: A abordagem discreta demonstrou ser **significativamente mais robusta e precisa**. Ela validou a hipótese de que uma caracterização sequencial é mais apropriada para descrever o comportamento de redes em equilíbrio estável, especialmente com dados esparsos.

## Resultados e Conclusão

A validação final do modelo ARX, através de animações do processo de aprendizado do RLS, constitui o principal resultado positivo do trabalho. As animações demonstram que:
1.  Os coeficientes do modelo convergem para valores estáveis à medida que o sistema é exposto a mais dados.
2.  A previsão do modelo acompanha de perto as tendências de subida e descida dos dados reais.
3.  O erro de previsão, após uma fase inicial de calibração, estabiliza-se em uma faixa de ruído em torno de zero.

> **A conclusão central do trabalho é que a escolha da classe de modelo (contínuo vs. discreto) é mais crucial do que os detalhes do próprio modelo. Para sistemas monitorados em baixa frequência, onde a causalidade contínua é quebrada, modelos de tempo discreto como o ARX oferecem maior robustez, precisão e validade física.**

## Tecnologias e Conceitos-Chave

* **Modelagem de Sistemas**: Equações Diferenciais Ordinárias (EDO), Séries Temporais, Equações de Diferenças (ARX).
* **Técnicas Numéricas**: Interpolação por Spline Cúbica, Regressão por Mínimos Quadrados (Equação Normal).
* **Algoritmos de Estimação**: Recursive Least Squares (RLS).
* **Domínio**: Desempenho de Redes, Análise de Sistemas Dinâmicos, Controle Adaptativo.
* **Linguagens e Bibliotecas**: Python, Jupyter Notebook, NumPy, Pandas, Matplotlib, SciPy.

## Autores

* **Gustavo Portovedo Curi**
* **Samyla Nascimento Silva**

## Licença

Este projeto está licenciado sob a Licença MIT. Veja o arquivo `LICENSE` para mais detalhes.
