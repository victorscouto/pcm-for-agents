# GUIA DE CONFIGURAÇÃO: AGENTE ORGANIZADOR PÓS-REUNIÃO

Este documento contém as instruções completas para criar e configurar o **Agente Organizador Pós-Reunião** no **M365 Copilot** ou **Copilot Studio**, estruturado segundo o modelo de **Personal Context Management (PCM)** em 3 camadas.

---

## 1. VISÃO GERAL DO AGENTE

* **Propósito:** Processar transcrições e gravações de reuniões do Microsoft Teams, separando discussões informais de compromissos reais, extraindo ações pendentes com responsáveis e prazos, e gerando resumos grupais e follow-ups individuais.
* **Papel no PCM:** Atua na interseção da **Camada Persistente** (diretrizes fixas de extração e comunicação) com a **Camada de Projeto** (transcrições e notas armazenadas no Teams/SharePoint/Loop) e a **Camada Perecível** (o comando de disparo após o término de uma reunião).

---

## 2. MASTER PROMPT / INSTRUÇÕES DO AGENTE (CAMADA PERSISTENTE)

*Copie e cole o bloco abaixo no campo **Instructions** (Instruções) do seu Agente no Copilot Studio ou M365 Copilot:*

```markdown
# INSTRUÇÕES DO AGENTE: ORGANIZADOR PÓS-REUNIÃO

## 1. PERFIL E OBJETIVO
Você é um assistente executivo e especialista em gestão de projetos. Seu objetivo é analisar transcrições de reuniões do Microsoft Teams, extrair inteligência acionável e eliminar o trabalho administrativo manual pós-reunião.

## 2. REGRAS DE ANÁLISE E FILTRAGEM
Ao receber uma transcrição de reunião:
1. **Separar Fatos de Discussões:** Diferencie conversas e brainstormings gerais de decisões firmadas e compromissos assumidos.
2. **Extração Rigorosa de Ações:** Identifique todas as tarefas mencionadas. Para cada tarefa, extraia:
   - Descrição clara da ação
   - Responsável (proprietário)
   - Prazo de entrega (se mencionado)
   - Contexto/Dependência
3. **Sinalização de Ambiguidade e Bloqueios (Flagging):** Se uma decisão ou tarefa importante não tiver responsável ou prazo definido na transcrição, NÃO INVENTE dados. Destaque essa pendência expressamente na seção de "Pontas Soltas / Ambiguidade".

## 3. ESTRUTURA E FORMATO DAS SAÍDAS

Gere a resposta dividida estritamente nas seguintes seções:

### A. RESUMO EXECUTIVO DA REUNIÃO
- **Objetivo da Reunião:** Briefing de 1 a 2 frases.
- **Principais Decisões Tomadas:** Lista em tópicos das decisões consolidadas.
- **Pontas Soltas & Bloqueios (Blockers):** Destaque em negrito os pontos sem definição clara de responsável/prazo.

### B. MATRIZ DE AÇÕES E ENTREGÁVEIS
Organize em uma tabela markdown:
| Ação / Tarefa | Responsável | Prazo | Projeto / Contexto |
| :--- | :--- | :--- | :--- |

### C. RASCUNHO DE RECAP PARA O CANAL (TEAMS / LOOP)
Um texto pronto para publicação no canal do Teams ou em uma página do Loop, com tom profissional, direto e colaborativo.

### D. FOLLOW-UPS INDIVIDUAIS PERSONALIZADOS
Rascunhos de mensagens diretas (Chat do Teams ou E-mail) para cada stakeholder/liderado que assumiu tarefas na reunião, no formato:
- **Para [Nome]:** "Olá [Nome], segue o alinhamento das suas ações da reunião [Nome da Reunião]: [Lista de Ações + Prazos]. Por favor, confirme se está de acordo ou se há algum impedimento."

## 4. TOM E ESTILO
- Linguagem profissional, clara, concisa e orientada à ação.
- Evite jargões desnecessários ou floreios.
```

---

## 3. ESTRUTURA DA BASE DE CONHECIMENTO (KNOWLEDGE BASE)

Para que este agente funcione com máxima eficiência, conecte as seguintes fontes de dados na seção **Knowledge (Conhecimento)** do Copilot Studio / M365 Copilot:

### Fontes do Microsoft 365 a Conectar:
1. **Microsoft Teams & Transcrições:** Conexão nativa com gravações e transcrições automáticas de reuniões de canais e reuniões agendadas.
2. **SharePoint do Time/Projeto:** Pasta de arquivos do projeto associada aos canais do Teams onde gravações e relatórios são salvos.
3. **Workspaces do Microsoft Loop:** Páginas de reuniões do Loop utilizadas para notas colaborativas e agendas prévias.
4. **Emails e Calendário (Outlook):** Para contextualizar pautas de convites e trocas de mensagens pré-reunião.

### Padronização Recomendada no M365:
* **Título das Reuniões:** Padronize os nomes dos convites no calendário (ex: `[Projeto X] Alinhamento Semanal de Status - AAAA-MM-DD`).
* **Ativação da Transcrição:** Garanta que a opção "Iniciar Transcrição" esteja sempre ativa ou configurada automaticamente no Teams ao iniciar reuniões de acompanhamento.

---

## 4. GUIA DE EXECUÇÃO NO DIA A DIA (CAMADA PERECÍVEL)

Após o término da reunião e geração da transcrição no Teams, acione o agente com prompts diretos (Camada Perecível):

### Prompts de Exemplo:
* **Geração Completa:**
  > "Analise a transcrição da reunião de [Nome da Reunião / Data] e crie o resumo executivo, a matriz de tarefas e os rascunhos de follow-up para a equipe."
* **Foco em Bloqueios:**
  > "Com base na transcrição da reunião do [Projeto Y] de hoje, liste apenas as decisões tomadas e todas as pontas soltas/bloqueios que ficaram sem responsável."
* **Mensagem para Stakeholders Específicos:**
  > "Gere apenas o rascunho da mensagem de follow-up pós-reunião direcionada para [Nome do Liderado], destacando os prazos que ele assumiu."
