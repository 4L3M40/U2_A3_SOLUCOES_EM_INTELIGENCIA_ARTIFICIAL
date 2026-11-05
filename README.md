# U2_A3_SOLUCOES_EM_INTELIGENCIA_ARTIFICIAL

# Avaliação de Qualidade de Atendimento com Lógica Nebulosa

![Python](https://img.shields.io/badge/Python-3.x-blue)
![NumPy](https://img.shields.io/badge/NumPy-%E2%9C%94-lightgrey)
![Matplotlib](https://img.shields.io/badge/Matplotlib-%E2%9C%94-lightgrey)
![Google Colab](https://img.shields.io/badge/Google%20Colab-notebook-orange)

Sistema de **lógica nebulosa (fuzzy)** escrito em Python puro para avaliar a qualidade de um atendimento a partir de duas variáveis: o **tempo de espera** e a **cordialidade** do atendente. O projeto implementa as funções de pertinência, a base de regras e a defuzzificação sem bibliotecas fuzzy, usando apenas `numpy` e `matplotlib`.

> Trabalho prático desenvolvido para a disciplina de **Fundamentos da Inteligência Artificial** (Unidade 2 – Métodos e Aplicações de Inteligência Artificial, Aula 3 – Soluções em Inteligência Artificial), do curso de Bacharelado em Engenharia de Software da Anhanguera.

---

## Sumário

- [Como funciona](#como-funciona)
- [Exemplos de execução](#exemplos-de-execução)
- [Visualizações](#visualizações)
- [Como executar](#como-executar)
- [Estrutura do repositório](#estrutura-do-repositório)
- [Limitações e melhorias](#limitações-e-melhorias)
- [Autor](#autor)

---

## Como funciona

O sistema recebe duas entradas e devolve a pertinência a quatro classes de qualidade (excelente, boa, regular e ruim), além de uma nota final de 0 a 10.

### 1. Variáveis de entrada

| Variável | Faixa | Termos linguísticos |
|---|---|---|
| Tempo de espera | 0 a 60 minutos | Rápido, Médio, Lento |
| Cordialidade | 0 a 10 pontos | Baixa, Média, Alta |

### 2. Funções de pertinência

Todas são funções lineares por partes (trapezoidais e triangulares), implementadas como funções Python.

**Tempo de espera**

| Termo | Pertinência |
|---|---|
| Rápido | 1 até 10 min; cai linearmente até 0 em 20 min |
| Médio | 0 até 10 min; sobe a 1 em 20 min; cai a 0 em 30 min |
| Lento | 0 até 25 min; sobe linearmente até 1 em 35 min; 1 depois disso |

**Cordialidade**

| Termo | Pertinência |
|---|---|
| Baixa | 1 até 3; cai linearmente até 0 em 5 |
| Média | 0 até 3; sobe a 1 em 5; cai a 0 em 7 |
| Alta | 0 até 6; sobe linearmente até 1 em 8; 1 depois disso |

### 3. Base de regras

O operador **E** é implementado com o mínimo (`min`) e o operador **OU** com o máximo (`max`).

| # | Regra | Saída |
|---|---|---|
| 1 | Tempo rápido **E** cordialidade alta | Excelente |
| 2 | Tempo rápido **E** cordialidade média | Boa |
| 3 | Tempo médio **E** cordialidade alta | Boa |
| 4 | Tempo médio **E** cordialidade média | Regular |
| 5 | Qualquer outra combinação (por exemplo, tempo lento ou cordialidade baixa) | Ruim |

As regras 2 e 3 compartilham a saída "Boa", combinadas com `max`. A classe "Ruim" reúne todas as combinações restantes.

### 4. Defuzzificação

A nota final é uma **média ponderada** dos graus de pertinência de cada classe, usando um valor representativo para cada uma:

```
              9·excelente + 7·boa + 5·regular + 2·ruim
Qualidade = ──────────────────────────────────────────────
                excelente + boa + regular + ruim
```

Os valores 9, 7, 5 e 2 representam, respectivamente, as classes excelente, boa, regular e ruim. O denominador tem um valor mínimo de `0.001` para evitar divisão por zero.

---

## Exemplos de execução

Saída do sistema para quatro casos de teste:

| Caso | Descrição | Tempo (min) | Cordialidade | Excelente | Boa | Regular | Ruim | **Qualidade final** |
|---|---|---|---|---|---|---|---|---|
| 1 | Atendimento rápido e muito cordial | 8 | 9 | 1,00 | 0,00 | 0,00 | 0,00 | **9,00** |
| 2 | Atendimento médio | 15 | 6 | 0,00 | 0,50 | 0,50 | 0,00 | **6,00** |
| 3 | Atendimento demorado e pouco cordial | 35 | 3 | 0,00 | 0,00 | 0,00 | 1,00 | **2,00** |
| 4 | Bom atendimento geral | 12 | 8 | 0,80 | 0,20 | 0,00 | 0,00 | **8,60** |

Exemplo do caso 2, passo a passo:

- Tempo = 15 min: pertinência a *rápido* = 0,5 e a *médio* = 0,5.
- Cordialidade = 6: pertinência a *média* = 0,5 e a *alta* = 0.
- Regra 2: `min(0,5; 0,5)` = 0,5, e regra 3: `min(0,5; 0)` = 0, então **boa** = 0,5.
- Regra 4: `min(0,5; 0,5)` = 0,5, então **regular** = 0,5.
- Qualidade = (7 × 0,5 + 5 × 0,5) / (0,5 + 0,5) = **6,00**.

---

## Visualizações

O código gera três gráficos:

1. **Funções de pertinência do tempo de espera** (rápido, médio e lento).
2. **Funções de pertinência da cordialidade** (baixa, média e alta).
3. **Superfície de qualidade**: mapa de contorno que mostra a nota final para cada combinação de tempo (0 a 60 min) e cordialidade (0 a 10).

<!-- Salve a imagem na pasta images/ e ajuste o nome do arquivo, se necessário -->
![Funções de pertinência e superfície de qualidade](images/funcoes_pertinencia.png)

---

## Como executar

### Google Colab (recomendado)

1. Abra o notebook no Google Colab.
2. Execute as células na ordem (`Ambiente de execução > Executar tudo`).

<!-- Opcional: substitua pelo link do seu notebook
[![Abrir no Colab](https://colab.research.google.com/assets/colab-badge.svg)](LINK_DO_NOTEBOOK_NO_GITHUB)
-->

### Localmente

Pré-requisitos: Python 3.8 ou superior.

```bash
# 1. Clone o repositório
git clone https://github.com/SEU_USUARIO/NOME_DO_REPOSITORIO.git
cd NOME_DO_REPOSITORIO

# 2. (Opcional) Crie um ambiente virtual
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate

# 3. Instale as dependências
pip install numpy matplotlib

# 4. Execute
python qualidade_atendimento.py
```

Para testar um caso próprio, chame a função de avaliação:

```python
resultado = avaliar_atendimento(tempo=10, cordialidade=7)
print(resultado)
# {'excelente': ..., 'boa': ..., 'regular': ..., 'ruim': ..., 'qualidade_final': ...}
```

---

## Estrutura do repositório

```
.
├── qualidade_atendimento.py        # Sistema fuzzy (ou o notebook .ipynb)
├── images/
│   └── funcoes_pertinencia.png     # Gráficos gerados pelo código
└── README.md
```

---

## Limitações e melhorias

- A defuzzificação por média ponderada é uma simplificação. Métodos como o **centroide** (centro de gravidade) sobre conjuntos de saída contínuos dão resultados mais suaves.
- Os parâmetros das funções de pertinência e os pesos das classes de saída foram definidos manualmente e podem ser ajustados com dados reais de atendimento.
- A classe "Ruim" agrupa todos os casos restantes. Uma base de regras mais completa, cobrindo cada combinação de termos, daria mais controle sobre a saída.
- A biblioteca [`scikit-fuzzy`](https://github.com/scikit-fuzzy/scikit-fuzzy) pode ser usada para comparar os resultados com uma implementação pronta.

---

## Autor

**Evandro Wagencknecht**
Bacharelado em Engenharia de Software – Anhanguera
Jaraguá do Sul – SC
