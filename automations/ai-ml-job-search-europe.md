---

name: ai-career-search
status: active

schedule: "todos os dias pela manhã"
timezone: Europe/Lisbon
updated: 2026-09-01

config:
base_location: "Lisbon, Portugal"

email:
enabled: true
recipients:
- "[your-email@example.com](mailto:your-email@example.com)"
subject: "AI Jobs Report — {{DATE}} — new matches"
send_only_if_changes: true

tracking:
enabled: false
jobs_database: "{{JOBS_DATABASE}}"
networking_database: "{{NETWORKING_DATABASE}}"
----------------------------------------------

# AI Career Search

## Objetivo

Executar uma busca recorrente por oportunidades de AI/ML Engineering, avaliando não apenas a existência de vagas, mas também:

* fit técnico;
* senioridade;
* qualidade da empresa;
* elegibilidade geográfica;
* possibilidade real de trabalho remoto;
* oportunidades estratégicas de networking;
* evidências públicas para personalização de outreach;
* projetos de portfólio que possam fortalecer a candidatura.

A automação também evita repetir vagas já encontradas e só envia um relatório quando existem oportunidades novas ou atualizações relevantes.

## Agenda

* Frequência: diária
* Horário: pela manhã
* Fuso horário: Europe/Lisbon
* Condição de término: nenhuma
* Condição de envio: somente quando houver oportunidades `NEW` ou `UPDATED` relevantes

## Configuração

Antes de usar esta automação, revise os valores definidos no front matter.

### Localização

`config.base_location`

Localização usada para avaliar:

* praticidade geográfica;
* regras de trabalho remoto;
* necessidade de relocation;
* compatibilidade com políticas EU / Europe / EMEA remote.

### Email

`config.email.enabled`

Define se o relatório deve ser enviado por email.

`config.email.recipients`

Lista de destinatários que receberão o relatório.

Exemplo:

```yaml
recipients:
  - "your-email@example.com"
  - "another-email@example.com"
```

Nunca publique seus endereços pessoais na versão pública do repositório.

`config.email.subject`

Template utilizado como assunto do email.

`config.email.send_only_if_changes`

Quando `true`, nenhum email deve ser enviado se não existirem oportunidades `NEW` ou `UPDATED` relevantes.

### Tracking

`config.tracking.enabled`

Ativa ou desativa a persistência dos resultados em uma ferramenta externa.

`config.tracking.jobs_database`

Destino utilizado para armazenar vagas.

`config.tracking.networking_database`

Destino utilizado para armazenar contatos de networking.

Não inclua IDs privados, tokens ou credenciais neste arquivo.

## Prompt

```text
Search for new mid-level through senior AI/ML engineering roles across Portugal, Spain, France, the United Kingdom, and selected high-value European remote opportunities.

The goal is not simply to collect job listings.

The goal is to identify high-quality AI engineering opportunities that are realistically actionable from {{config.base_location}}, understand the companies and teams behind them, identify the most strategically relevant people associated with those opportunities, and generate genuinely personalized outreach based on real public signals such as technical work, talks, research, open-source contributions, events, articles, product launches, engineering challenges, or shared professional interests.

Prioritize quality over quantity.

[...rest of the job-search prompt...]

STAGE 4 — EMAIL DELIVERY

Check the automation configuration before attempting email delivery.

If:

config.email.enabled = true

AND the run discovers meaningful NEW or UPDATED opportunities,

send the completed report through Gmail to every recipient listed in:

config.email.recipients

Use:

config.email.subject

as the email subject.

Include:

- Ranked opportunities
- Application links
- Recommended priorities
- Networking contacts
- Personalization evidence
- Suggested outreach messages
- Tailored side-project concepts

If:

config.email.send_only_if_changes = true

AND there are no meaningful NEW or UPDATED opportunities:

- Do not email
- Do not notify

If email delivery is disabled or no email integration is available, generate the report normally without attempting delivery.


STAGE 5 — OPTIONAL TRACKING

If:

config.tracking.enabled = true

save discovered jobs to:

config.tracking.jobs_database

and relevant networking contacts to:

config.tracking.networking_database

If tracking is disabled or the required integration is unavailable, skip this step.

Never expose credentials, API keys, access tokens, private database IDs, or other secrets.
```

## Entradas e integrações

### Web

Necessária para:

* encontrar vagas atuais;
* verificar freshness;
* consultar páginas oficiais das empresas;
* verificar políticas remote;
* pesquisar informações profissionais públicas para networking.

### Histórico de execuções

Necessário para:

* identificar vagas `NEW`;
* identificar vagas `UPDATED`;
* reconhecer vagas `STILL OPEN`;
* evitar duplicações.

### Gmail

Opcional.

Usado quando:

```yaml
config:
  email:
    enabled: true
```

Os destinatários são definidos em:

```yaml
config.email.recipients
```

### Tracking / database

Opcional.

Pode ser utilizado para armazenar:

* vagas encontradas;
* contatos de networking;
* status de candidatura;
* histórico das execuções.

A implementação pode utilizar Notion, Airtable, uma database própria ou outra integração compatível.

## Saída esperada

A execução deve produzir um relatório contendo:

1. Ranking das vagas encontradas.
2. Status `NEW`, `UPDATED`, `STILL OPEN` ou `UNCERTAIN`.
3. Link oficial para candidatura.
4. Avaliação de fit técnico.
5. Avaliação de senioridade.
6. Verificação de elegibilidade geográfica.
7. Working language quando relevante.
8. Top oportunidades para candidatura.
9. Estratégias de networking.
10. Evidências públicas usadas na personalização.
11. Mensagens de outreach quando houver sinais fortes.
12. Projetos de portfólio alinhados às melhores vagas.
13. Ordem recomendada de ação.

Quando o email estiver habilitado, o mesmo relatório deve ser enviado aos destinatários configurados.

Quando o tracking estiver habilitado, os dados relevantes também devem ser persistidos.

## Validação

A execução é considerada correta quando:

* respeita os filtros de senioridade;
* remove vagas junior e irrelevantes;
* valida remote eligibility;
* não assume que "remote" significa automaticamente compatível com Portugal;
* não apresenta vagas repetidas como novas;
* prioriza fontes oficiais;
* utiliza apenas informações profissionais públicas para networking;
* não inventa sinais de personalização;
* fornece fontes para mensagens personalizadas;
* gera projetos diretamente relacionados aos requisitos da vaga;
* respeita a configuração de envio de email;
* não envia email sem mudanças relevantes quando `send_only_if_changes` estiver ativo;
* não expõe credenciais ou dados privados.

## Segurança e privacidade

Nunca armazenar neste arquivo público:

* emails pessoais reais;
* API keys;
* OAuth tokens;
* access tokens;
* secrets;
* IDs privados de databases;
* credenciais de serviços;
* informações confidenciais de processos seletivos.

A versão pública deve usar valores de exemplo ou placeholders.
