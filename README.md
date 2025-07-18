# Modelagem Dinâmica de Desempenho de Redes e Análise de Erros

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Análise comparativa entre modelos de tempo contínuo (EDOs) e tempo discreto (ARX) para a caracterização do desempenho de conexões de rede a partir de dados experimentais esparsos.

---

### Tabela de Conteúdos
* [Sobre o Projeto](#sobre-o-projeto)
* [Metodologia Aplicada](#metodologia-aplicada)
* [Resultados e Conclusão](#resultados-e-conclusão)
* [Tecnologias Utilizadas](#tecnologias-utilizadas)
* [Como Rodar](#como-rodar)
* [Estrutura do Repositório](#estrutura-do-repositório)
* [Autores](#autores)
* [Licença](#licença)

---

## Sobre o Projeto

Este repositório contém o código e a análise de um trabalho acadêmico para a disciplina de Matemática Computacional. O projeto explora e compara modelos matemáticos de tempo contínuo (baseados em Equações Diferenciais Ordinárias - EDOs) e tempo discreto (baseados em Equações de Diferenças - ARX) para descrever a performance de conexões de rede a partir de dados experimentais com medições esparsas.

O objetivo foi determinar a abordagem de modelagem mais adequada para este tipo de dataset, que é comum em cenários de monitoramento de rede.

## Metodologia Aplicada

A análise foi dividida em duas fases:

#### 1. Abordagem por Tempo Contínuo (EDOs)
* **Técnica:** Os dados discretos foram transformados em funções contínuas via **splines cúbicas** para permitir a calibração de um sistema de EDOs através de **regressão linear**.
* **Resultado:** A abordagem se mostrou **inadequada**. A baixa frequência de amostragem dos dados quebrou a premissa de continuidade, resultando em modelos instáveis e simulações que divergiam da realidade.

#### 2. Abordagem por Tempo Discreto (ARX + RLS)
* **Técnica:** Em resposta às limitações encontradas, a análise foi pivotada para um modelo **Autorregressivo com Entradas Exógenas (ARX)**, que trata os dados como uma sequência de eventos. Os coeficientes foram estimados de forma online com o algoritmo **Recursive Least Squares (RLS)**.
* **Resultado:** Esta abordagem se mostrou **robusta e eficaz**. O modelo foi capaz de aprender a dinâmica sequencial dos dados e suas previsões acompanharam as tendências das medições reais.

## Resultados e Conclusão

A análise demonstrou que modelos de tempo discreto (ARX) são mais adequados para caracterizar conexões de rede com dados esparsos. O modelo final aprendeu com sucesso o comportamento da rede, e sua convergência foi visualizada através de animações do processo de aprendizado do RLS.

> **Conclusão principal:** A escolha da classe de modelo (contínuo vs. discreto) é crucial e deve ser guiada pela natureza do processo de coleta de dados.

## Tecnologias Utilizadas
* **Linguagem:** Python 3.12
* **Bibliotecas Principais:** Pandas, NumPy, Matplotlib, SciPy, Sk-Learn
* **Ambiente:** Jupyter Notebook

## Como Rodar

1.  Clone o repositório:
    ```sh
    git clone https://github.com/guscuri/Desempenho-de-Rede
    ```
2.  Crie e ative um ambiente virtual.
3.  Instale as dependências:
    ```sh
    pip install -r requirements.txt
    ```
4.  Execute o Jupyter Notebook localizado na pasta `notebooks/`. Para gerar as animações, é necessário ter o `ffmpeg` instalado no sistema.


## Autores
* Gustavo Portovedo Curi
* Samyla Nascimento Silva

## Licença
Este projeto está licenciado sob a Licença MIT.
