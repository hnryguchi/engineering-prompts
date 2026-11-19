# 11 · AGENTS.md / CLAUDE.md do repositório (regras duradouras)

## Quando usar

Em todo repositório onde agentes de código trabalham com frequência. O arquivo guarda o que não muda de uma tarefa para outra (arquitetura, comandos, deploy, segurança, o que nunca fazer sem autorização), para que você não precise repetir isso em cada prompt.

## Template

```markdown
# {{NOME_DO_PROJETO}} — regras duradouras

> Aviso de versão: {{ex. esta versão do framework tem mudanças incompatíveis com
> o seu treinamento. Leia a documentação local em {{CAMINHO_DOCS}} antes de usar APIs.}}

## Arquitetura
{{FRAMEWORK}} + {{BANCO/ORM}}. Camadas: {{ex. rotas → validação → serviços
(regra de negócio, autorização) → repositórios (acesso a dados) → auditoria}}.
Detalhes em {{DOC_DE_ARQUITETURA}}.

Públicos/perfis: {{QUEM_ACESSA_O_QUÊ}}.
Mecanismos que devem ser reutilizados (não criar paralelos): {{ex. contexto de
projeto ativo, checagem de permissão por recurso}}.

## Integrações
{{APP_OU_SERVIÇO}}: {{CONTRATO, DIREÇÃO DO FLUXO, CACHE}}.
Endpoints públicos por desenho: {{LISTA}}, com allowlist de campos em
{{ARQUIVO}}. Qualquer mudança precisa preservar a allowlist.

## Comandos
    {{INSTALAR}}        # ex. npm ci (nunca install em CI/deploy)
    {{DEV}}             # dicas de cache quebrado, portas
    {{TYPECHECK}}
    {{LINT}}
    {{TESTES}}          # quais falhas são esperadas sem serviço local e por quê
    {{BUILD}}
    {{BANCO_LOCAL}} / {{MIGRATIONS}} / {{SEED}}

## Mecanismo de deploy atual
{{PASSO_A_PASSO_REAL, confirmado no histórico}}. Não fazer deploy automático:
sempre parar e pedir aprovação antes de publicar.

## Segurança e autorização
- Nunca vazar dados entre {{CLIENTES/PROJETOS/TENANTS}}; toda consulta sensível
  reverifica autorização no serviço, não só na interface.
- Segredos apenas em {{COFRE/CONFIGURAÇÃO_DO_AMBIENTE}}; nunca em arquivo versionado,
  log ou commit.
- Senhas: hash com {{ALGORITMO_E_CUSTO}}.
- {{REGRAS_ESPECÍFICAS: ex. assistente de IA interno nunca responde sobre um
  projeto usando dados públicos agregados; sempre rechecar permissão}}.

## Nunca alterar sem autorização explícita
- Deploy, push, infraestrutura (hospedagem, configurações, DNS, domínio, certificado, plano).
- Schema/migrations do banco de produção.
- Credenciais (não regenerar nem rotacionar sem pedido).
- {{REGRAS_DE_NEGÓCIO_SENSÍVEIS}}.

## Dados DEMO / não inventar dados
- Nunca inventar registros para "parecer preenchido"; estado vazio honesto sempre.
- Fixtures isoladas: {{ROTA/FLAG/BANNER}}, nunca servidas em produção (com teste que garanta).
- Em produção, falha de fonte de dados mostra estado "indisponível", nunca fallback silencioso para fixture.
- Dados reais de um cliente nunca são copiados para outro projeto.

## Convenções
- i18n: {{MECANISMO}}; manter paridade de chaves entre idiomas (com teste).
- Reutilizar componentes visuais existentes ({{EX}}) antes de criar novos.
- {{TERMINOLOGIA_DO_PRODUTO}}.
```

## Por que funciona

- Regras duradouras no repositório viajam com o código: qualquer agente, em qualquer sessão, começa com o mesmo contexto.
- A seção "nunca alterar sem autorização" faz dos portões de aprovação uma propriedade do projeto, não da memória de quem escreve o prompt.
- Documentar o deploy real (e não o idealizado) evita que o agente invente um pipeline diferente do que existe.
- Registrar falhas de teste esperadas impede que o agente "corrija" um teste que só falha por falta de serviço local.

## Exemplo anonimizado

> Contexto: portal de cliente de uma consultoria e um app público separado que consome um endpoint publicado pelo portal.
>
> O AGENTS.md do portal declara o endpoint como público por desenho, com duplo portão de publicação e allowlist de campos; o AGENTS.md do app público declara que ele nunca chama o endpoint pelo navegador, só no servidor, e que, se a API falhar em produção, mostra "indisponível" em vez de dados de exemplo.

## Armadilhas

- Transformar o arquivo em diário de bordo: decisões datadas e estado efêmero vão para um documento de handoff, não para as regras duradouras.
- Colocar nomes de recursos de nuvem, e-mails ou segredos em um AGENTS.md de repositório público.
- Escrever regras sem o motivo: o agente não sabe quando uma exceção é aceitável.
- Deixar o arquivo divergir da realidade (comandos antigos, deploy que mudou); revise a cada mudança de infraestrutura.
