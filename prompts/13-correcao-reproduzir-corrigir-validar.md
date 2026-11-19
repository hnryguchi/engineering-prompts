# 13 · Correção de defeito: reproduzir, achar a causa, corrigir e validar no navegador

## Quando usar

Quando um usuário relata um defeito (geralmente com captura de tela) e relatórios anteriores do agente já o deram como resolvido. O prompt obriga a reproduzir o comportamento atual, separar causas, corrigir na origem e provar a correção, sem efeitos colaterais.

## Template

```text
Investigue e corrija o problema abaixo no código existente. Não considere o
item concluído sem reproduzir o comportamento atual, corrigir a causa e
validar no navegador. Um relatório anterior dizendo que foi resolvido não conta.

1. SINTOMA
{{DESCRIÇÃO_DO_QUE_O_USUÁRIO_VÊ, com referência à captura}}
Há sintomas possivelmente relacionados: {{SINTOMA_2}}. Verifique se têm a mesma
causa ou causas diferentes.

2. INVESTIGAÇÃO
Localize o componente/consulta responsável e verifique:
- {{HIPÓTESE_1: ex. dimensões, margens, escala automática do eixo}}
- {{HIPÓTESE_2: ex. container com overflow, largura mínima, coluna fixa}}
- {{HIPÓTESE_3: ex. filtro, ordenação, paginação, limite de resultados, vínculo entre entidades}}
Registre a causa raiz encontrada.

3. CORREÇÃO
- Corrija a origem; não mascare o sintoma (não esconda dados, não reduza pontos,
  não troque só o rótulo, não acrescente itens fictícios).
- Preserve: {{O_QUE_NÃO_PODE_MUDAR: cálculos, filtros, layout aprovado pelo usuário}}.
- Reutilize o componente atual; não crie um duplicado.
- Sem mudanças sem relação com o problema.

4. VALIDAÇÃO
- Abra a tela real com {{DADOS_QUE_REPRODUZEM: ex. período com pico próximo do máximo}}.
- Teste {{VARIAÇÕES: filtros, troca de projeto, desktop e larguras menores}}.
- Confirme que {{ÁREAS_VIZINHAS}} não regrediram.
- Capturas de tela antes e depois.
- Testes automatizados pertinentes, typecheck, lint e build. Build passando não
  é critério de conclusão.

5. RESTRIÇÕES
Não publique em produção sem minha autorização explícita. Não altere dados
oficiais, planilhas de origem ou migrations.

ENTREGA
Causa (ou causas) do defeito; arquivos modificados; como a correção funciona;
testes executados e resultado; capturas antes/depois; pendências abertas.
```

## Por que funciona

- Exigir reprodução antes da correção impede o padrão "corrigi algo parecido e declarei sucesso".
- Listar hipóteses concretas acelera a investigação e mostra ao agente onde defeitos desse tipo costumam morar.
- Proibir correções cosméticas (esconder, truncar, rotular) mantém a integridade dos dados exibidos.
- Capturas antes/depois e validação de áreas vizinhas dão evidência objetiva e pegam regressões.

## Exemplo anonimizado

> Contexto: painel com gráfico de evolução diária cuja linha encostava no limite superior e parecia cortada, além de conteúdo lateral saindo da tela.
>
> O agente encontrou duas causas distintas (escala vertical sem folga acima do máximo e uma coluna lateral com largura mínima fixa), corrigiu ambas no componente existente, testou um período com pico, verificou os outros gráficos da página e entregou capturas antes/depois.

## Armadilhas

- Aceitar "corrigido" com base no build ou em teste unitário que não exercita a tela.
- Não dizer o que deve ser preservado: o agente redesenha justamente o componente que o usuário aprovou.
- Tratar dois sintomas como uma causa só (ou o contrário) sem verificar.
- Corrigir listagens vazias inserindo conteúdo de exemplo em vez de consertar a consulta.
