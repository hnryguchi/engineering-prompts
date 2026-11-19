# 05 · Checklist pré-deploy e confirmação de mudança em produção (incluindo migration)

## Quando usar

Quando você já validou a versão localmente e decidiu publicar, especialmente se houver migration de banco pendente. O prompt concede uma autorização estreita e verificável, com limites claros sobre o que o agente pode executar contra produção.

## Template

```text
Confirmado: pode aplicar a mudança em produção, SOMENTE nos termos abaixo.

PARTE A — Banco de dados (se houver migration pendente)
- Execute apenas: {{COMANDO_DE_MIGRATION_DE_PRODUÇÃO: ex. "migrate deploy"}}.
- PROIBIDO: comandos de desenvolvimento ({{ex. migrate dev, db push}}), reset,
  alteração manual de tabelas, edição de migrations já aplicadas.
- Antes de executar, confirme que a conexão aponta para o banco de produção
  correto (host/nome, sem exibir a senha).
- Depois, confirme que {{NOME_DA_MIGRATION}} foi aplicada e que nenhuma outra
  mudança ocorreu.
- Não copie dados de teste/QA para produção.

PARTE B — Checklist pré-deploy (reporte PASS/FAIL de cada item)
[ ] Árvore de trabalho limpa ou alterações conhecidas e intencionais
[ ] Nenhum segredo, .env, credencial ou dado confidencial no pacote
[ ] Typecheck, lint, testes e build de produção passando
[ ] Pacote contém só o necessário (sem banco local, backups, documentos internos)
[ ] Configuração do ambiente de destino conferida (runtime, comando de start, variáveis)
[ ] Plano de rollback: versão anterior identificada e como restaurá-la
[ ] Destino é {{APP_DE_DESTINO}} e nenhum outro

Se qualquer item falhar, PARE e me informe antes de publicar.

PARTE C — Publicação e verificação
- Publique somente {{APP_DE_DESTINO}}.
- HTTP 200 não é sucesso. Verifique em navegador real na URL de produção:
  {{FLUXOS_CRÍTICOS: ex. login, página principal, mapa renderizado, PT/EN, mobile}}.
- Verifique console, rede e políticas de segurança (CSP) no ambiente publicado.

Fora do escopo desta autorização: DNS, domínio, certificado, tier/plano,
outros apps, outras migrations. Peça antes.

Entrega: resultado da migration, checklist PASS/FAIL, identificador do
deploy, resultado do smoke test por fluxo e qualquer anomalia observada.
```

## Por que funciona

- A autorização tem forma de lista fechada: o agente sabe exatamente qual comando pode rodar contra produção e quais estão vetados.
- Confirmar o alvo da conexão antes de migrar previne o erro mais destrutivo possível: rodar contra o banco errado.
- O checklist PASS/FAIL com parada em falha converte boas práticas em portões executáveis.
- Exigir verificação visual no ambiente publicado pega defeitos que só existem depois do deploy (CSP, workers, assets estáticos ausentes).

## Exemplo anonimizado

> Contexto: portal com uma migration aditiva pendente (nova tabela de planejamento).
>
> "Pode aplicar a migration pendente em produção usando apenas o comando de deploy de migrations. Antes, confirme que a conexão é mesmo o banco de produção. Depois, empacote o build, publique só o portal e faça smoke test na URL real. Não toque em migrations já aplicadas."

## Armadilhas

- Autorizar "faça o deploy" sem nomear o destino quando há mais de um app no mesmo grupo de recursos.
- Esquecer que builds standalone podem arrastar pastas locais (banco de dev, manuais, backups) para dentro do pacote.
- Considerar a tarefa concluída com o status de deploy "sucesso" sem abrir a aplicação.
- Misturar na mesma autorização mudanças de DNS ou plano: são decisões separadas, com riscos separados.
