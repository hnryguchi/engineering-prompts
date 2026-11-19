# 07 · Auditoria final somente de verificação (sem implementação)

## Quando usar

No fim de um ciclo de desenvolvimento, antes de decidir por commit, push ou deploy. O agente não deve criar funcionalidades: apenas reexecutar todas as verificações, corrigir exclusivamente o que falhar nelas e entregar um quadro objetivo de prontidão.

## Template

```text
Faça agora uma auditoria final SOMENTE de verificação. Não implemente
funcionalidades novas e não refatore.

1. Reexecute, do zero: build de produção, typecheck, lint e todos os testes.
2. Smoke test em {{URL_LOCAL}} dos fluxos principais:
   {{ex. login real, logout, proteção de rotas, perfis/permissões, navegação,
   CRUDs principais, uploads, tratamento de erros, troca de idioma}}.
3. Segurança de conteúdo:
   - confirme que nenhum documento confidencial está acessível publicamente
     ({{PASTAS_PÚBLICAS_A_CONFERIR}});
   - confirme que endpoints públicos expõem apenas os campos permitidos.
4. Higiene do repositório: git status; confirme que nenhum segredo, .env,
   credencial ou dado confidencial está preparado para commit.
5. {{VERIFICAÇÕES_ESPECÍFICAS: ex. nenhuma string da interface no idioma errado,
   nenhum dado fictício em produção}}.

Regras:
- Corrija somente falhas encontradas nestas verificações; registre cada correção.
- Não faça commit, push nem deploy.
- "Build passou" não é critério suficiente: cada fluxo precisa ser exercitado.

Entrega:
- tabela PASS/FAIL, uma linha por item, com a evidência (comando, captura, rota);
- correções feitas durante a auditoria;
- tudo o que ainda impede chamar o sistema de pronto para {{AMBIENTE_ALVO}};
- número de arquivos alterados e se considera seguro commitar/publicar.
{{OPCIONAL: Se tudo passar, deixe o servidor local rodando para minha revisão.}}
```

## Por que funciona

- Proibir implementação mantém a auditoria neutra: quem audita não aproveita para "melhorar" e introduzir risco novo.
- A tabela PASS/FAIL com evidência é um artefato de decisão; dá para mostrar a um revisor sem reexplicar o contexto.
- Checar segredos no git status e arquivos confidenciais em pastas públicas cobre os dois vazamentos mais comuns em apps recém-construídos.
- Perguntar o que ainda impede a prontidão força o agente a listar bloqueios em vez de declarar sucesso.

## Exemplo anonimizado

> Contexto: portal bilíngue de uma consultoria, antes do primeiro commit da versão internacionalizada.
>
> "Antes de commit/push/deploy: reexecute testes, lint, typecheck e build; faça a varredura visual de todas as telas em desktop, tablet e mobile, nos dois idiomas; procure textos hardcoded no idioma errado e liste exceções intencionais. Ao final diga se ainda existe string de interface no idioma errado, se todas as telas passaram e se está seguro publicar."

## Armadilhas

- Permitir "corrigir o que achar" sem restringir às falhas das verificações: a auditoria vira nova rodada de desenvolvimento.
- Aceitar a tabela sem evidência por linha.
- Não incluir verificação de exposição pública (pastas estáticas, endpoints sem autenticação).
- Rodar a auditoria com cache antigo de build; peça execução limpa.
