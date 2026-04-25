# CÁLCULO DE PROBABILIDADES

## Introdução

Probabilidade é a área da matemática que lida com fenômenos incertos, chamados de modelos não-determinísticos ou probabilísticos. Não se sabe o resultado exato de um experimento, mas é possível prever, com certo grau de segurança, quais resultados podem ocorrer e com que frequência.

## Conceitos Fundamentais

Um experimento aleatório (E) é qualquer processo cujo resultado varia mesmo sob condições idênticas. O espaço amostral (S) é o conjunto de todos os resultados possíveis. Um evento é qualquer subconjunto do espaço amostral. Existem três eventos especiais: o evento certo (igual ao próprio S), o evento impossível (conjunto vazio) e o evento simples (com um único resultado).

## Relações entre Eventos

O complemento de A reúne tudo que não pertence a A, sendo P(Ā) = 1 − P(A). Eventos mutuamente excludentes não podem ocorrer ao mesmo tempo: sua interseção é vazia. Eventos coletivamente exaustivos são aqueles cuja união cobre todo o espaço amostral, garantindo que ao menos um deles ocorre.

## Definição e Axiomas da Probabilidade

P(A) é um número entre 0 e 1. A probabilidade do espaço amostral inteiro é 1. Se A e B são mutuamente excludentes, P(A∪B) = P(A) + P(B). Para eventos quaisquer, vale: P(A∪B) = P(A) + P(B) − P(A∩B).

## Espaço Amostral Equiprovável

Quando todos os resultados têm a mesma chance de ocorrer, a probabilidade de um evento A é simplesmente: P(A) = número de casos favoráveis ÷ número total de casos. É o tipo mais cobrado em prova com dados, baralhos e urnas.

## Probabilidade Condicional e Teorema do Produto

A probabilidade condicional P(A|B) representa a chance de A ocorrer sabendo que B já ocorreu: P(A|B) = P(A∩B) / P(B). O Teorema do Produto decorre disso: P(A∩B) = P(A) × P(B|A).

## Independência Estatística

Dois eventos são independentes quando a ocorrência de um não altera a probabilidade do outro: P(A|B) = P(A). Nesse caso, P(A∩B) = P(A) × P(B). Isso ocorre, por exemplo, em retiradas com reposição.

## Teorema de Bayes

Usado quando se conhece a probabilidade de causas (A₁, A₂, ..., Aₙ) e se quer saber, dado que um efeito B ocorreu, qual causa é mais provável. A fórmula é: P(Aᵢ|B) = [P(Aᵢ) × P(B|Aᵢ)] ÷ [soma de P(Aⱼ) × P(B|Aⱼ) para todo j]. Muito aplicado em diagnósticos, controle de qualidade e seleção de populações.

## O que mais cai em prova

Cálculo de P(A) em espaços equiprováveis com combinações; uso do complemento para "pelo menos um"; probabilidade condicional P(A|B); distinção entre eventos independentes e dependentes (com e sem reposição); e aplicação do Teorema de Bayes em problemas com urnas, máquinas ou diagnósticos.
