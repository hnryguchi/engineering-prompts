# 01 · Auditoria somente leitura antes de agir

## Quando usar

Antes de pedir qualquer alteração em um sistema que você não conhece por inteiro, ou que o agente já modificou bastante ao longo de várias sessões. Serve para obter um retrato fiel do estado atual (o que existe, o que funciona, o que é dado fictício, o que falta) e transformar isso em um plano que você aprova antes de qualquer escrita.

## Template

```text
Pare as alterações por enquanto. Nesta etapa você NÃO deve modificar nenhum
arquivo, banco de dados, configuração ou recurso externo.

Objetivo: auditar o estado atual de {{SISTEMA_OU_MÓDULO}} e me entregar um
diagnóstico que eu possa aprovar antes de qualquer implementação.

Permitido nesta fase:
- ler código, schema, configurações, histórico do git e documentação;
- executar comandos de leitura (git status/log/diff, typecheck, lint, testes);
- abrir a aplicação local apenas para observar.

Proibido nesta fase:
- editar, criar ou apagar arquivos;
- rodar migrations, seeds ou scripts que escrevam dados;
- commit, push, deploy ou alteração em infraestrutura;
- iniciar varreduras longas que não respondam às perguntas abaixo.

Responda especificamente:
1. Quais {{PÁGINAS_ROTAS_MÓDULOS}} estão implementados hoje.
2. O que você já verificou de fato (no navegador, nos testes) e o que ainda não verificou.
3. Problemas funcionais, visuais e de segurança encontrados, com arquivo/linha.
4. Todo dado que ainda é mockado, fixo no código ou fictício.
5. Erros atuais de build, tipos, lint, console ou runtime.
6. {{PERGUNTAS_ESPECÍFICAS_DO_DOMÍNIO}}
7. O que falta, objetivamente, para considerar a versão {{MARCO: demonstração | produção}}.

Formato da entrega:
- RESUMO EXECUTIVO curto, legível por alguém não especialista;
- análise técnica detalhada;
- plano proposto em fases, com riscos e o que exige minha autorização.

Ao terminar, pare e aguarde minha aprovação. Não comece a implementar.
```

## Por que funciona

- Separar leitura de escrita elimina a classe mais cara de erro de agentes: "consertar" algo que ainda não foi entendido.
- A distinção entre "verificado" e "não verificado" obriga o agente a declarar o nível de evidência em vez de afirmar que tudo está ok.
- Pedir a lista de dados fictícios expõe mocks esquecidos antes que cheguem a produção.
- A parada explícita no fim cria um portão de aprovação: o plano vira contrato para a fase seguinte.

## Exemplo anonimizado

> Contexto: portal Next.js de uma consultoria ambiental, depois de várias sessões de desenvolvimento assistido.
>
> "Pare as alterações. Faça apenas uma auditoria do portal. Confirme se o mapa do projeto usa as geometrias reais de referência ou apenas um mapa-base genérico, se as camadas de áreas e registros estão de fato renderizadas, e liste qualquer dado ainda fictício. Não inicie outra varredura longa; analise o estado atual e responda."

## Armadilhas

- Esquecer de proibir scripts "inofensivos" que escrevem dados (seed, cache warmers, geradores).
- Aceitar "está tudo funcionando" sem pedir a evidência (comando executado, captura de tela, teste).
- Deixar o escopo aberto demais: o agente gasta horas varrendo o repositório inteiro sem responder às perguntas que importam.
- Não pedir o resumo executivo: a análise técnica fica correta, mas ninguém além de você consegue decidir com ela.
