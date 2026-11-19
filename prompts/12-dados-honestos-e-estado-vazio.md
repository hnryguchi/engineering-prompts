# 12 · Regra de dados honestos e política de estado vazio

## Quando usar

Como trecho para colar em qualquer prompt ou AGENTS.md que envolva painéis, indicadores, mapas, relatórios ou integrações com dados reais. Agentes tendem a preencher lacunas com valores plausíveis; esta regra torna isso proibido e define o que fazer no lugar.

## Template

```text
REGRA DE DADOS HONESTOS

1. Não invente dados. Nenhum registro, valor, data, posição, nome ou métrica
   pode ser criado para "a tela não parecer vazia". Estado vazio honesto é
   sempre preferível a dado fabricado.

2. Não invente regras. Quando uma regra de negócio, fórmula ou data oficial não
   estiver definida em {{FONTES_AUTORIZADAS}}, não escolha por conta própria:
   registre como pendência, com a pergunta exata e quem pode respondê-la.

3. Distinga os estados, na interface e no código:
   - zero (medido e igual a zero);
   - ausente / não informado (a fonte não traz o dado);
   - não aplicável (ex.: meta zero → indicador N/A);
   - indisponível (a fonte falhou);
   - previsto vs. realizado.
   Cada estado tem rótulo próprio; nunca mostre "0" para ausente.

4. Valores derivados respeitam o significado: {{ex. percentuais limitados a 100%
   com indicação de excedente; períodos rotulados com as datas reais de cada
   indicador; atividades que começaram em datas diferentes não são forçadas a
   um início comum}}.

5. Fallbacks são explícitos: se for preciso usar uma regra substituta por falta
   de dado, use a regra documentada em {{ONDE}} e mostre a limitação ao usuário.
   Nunca subtraia, some ou estime "por suposição".

6. Fixtures e DEMO:
   - isolados por {{ROTA/FLAG/PAPEL}}, rotulados visivelmente;
   - jamais servidos em build de produção (cubra com teste);
   - em produção, falha de fonte → estado "indisponível", não fixture.

7. Não esconda o problema: não resolva defeitos removendo dados, cortando
   pontos de um gráfico ou ocultando o componente afetado.

8. Associações exigem evidência: não trate correspondência aproximada (nome
   parecido, cargo igual) como confirmação. Classifique o grau de certeza.

No relatório final, liste: cada lacuna de dados encontrada, como foi exibida,
e as pendências que dependem de decisão humana.
```

## Por que funciona

- Dados fabricados passam por revisão visual com facilidade justamente porque parecem plausíveis; a proibição explícita é a única defesa barata.
- Separar zero, ausente, N/A e indisponível evita a classe de erro mais comum em painéis: decisões tomadas sobre um "0" que na verdade é "não sabemos".
- Transformar lacunas em pendências com pergunta e responsável mantém o trabalho avançando sem decisões silenciosas.
- O teste que impede fixture em produção converte a política em garantia verificável.

## Exemplo anonimizado

> Contexto: painel de indicadores de um programa de monitoramento ambiental com várias fontes (planilhas da equipe, registros de campo).
>
> Um cartão de "transferências realizadas" contava qualquer registro de resíduo pesado como transferência. Com a regra, o agente passou a diferenciar realizada, prevista, não informada e ausência de registros, e listou como pendência a dúvida sobre qual evento conta como transferência, para decisão da equipe.

## Armadilhas

- Permitir "valores de exemplo temporários" que nunca são removidos.
- Copiar números de uma captura de tela como constantes no código.
- Fallback silencioso para dados de exemplo quando a API falha: em produção isso é desinformação.
- Agrupar casos de certeza diferente (confirmado, provável, sem correspondência) em um único total.
