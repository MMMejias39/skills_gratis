---
name: backlog-tecnico
description: Metodologia para levantar, priorizar e fechar itens de backlog em qualquer projeto técnico (código ou redação) — evita os dois defeitos mais comuns em projetos de longa duração com múltiplas sessões/colaboradores (humanos ou IA): status congelado (item marcado "pendente" muito depois de já resolvido) e aceitar achado de revisor externo sem verificar a causa. Carregar antes de gerar, atualizar ou fechar um backlog, ou antes de confiar num relatório de auditoria externo.
---

# Backlog técnico — como gerar, priorizar e fechar sem acumular lixo

Método para manter um backlog confiável num projeto que atravessa muitas
sessões de trabalho (com ou sem IA) sem que ele vire uma lista que ninguém
confia mais. Os dois defeitos de processo mais caros observados na prática:

1. **Status congelado**: um item descreve corretamente o dia em que foi
   escrito, mas a linha "pendente"/"nada implementado" nunca é revisitada —
   meses depois, uma leitura apressada do backlog produz uma lista com boa
   parte dos itens já feitos, e ninguém percebe até alguém tentar reabrir um
   deles.
2. **Achado de revisor externo tratado como escopo fechado**: um relatório de
   auditoria (humano ou outra IA) aponta um problema, e a resposta vira
   implementar o checklist inteiro sem verificar se o diagnóstico está
   certo. Revisores externos acertam o sintoma e erram a causa com
   frequência suficiente para que isso seja caro.

Esta skill cobre **como estruturar e manter** um backlog. Convenções de
redação de prosa/equação (se o backlog for sobre um documento técnico) estão
em `redacao-tecnica`.

## 1. Princípios gerais

1. **Verificar antes de listar.** Nenhum item entra no backlog por lembrança
   de um registro anterior — toda lacuna vem de um comando/execução/leitura
   feita na data em que o item é escrito. Reler um relatório antigo com
   pressa e assumir que o estado nele descrito ainda vale é a causa nº 1 de
   status congelado.
2. **Criticidade ≠ ordem de descoberta.** Priorize pelo que bloqueia ou
   degrada o consumo do artefato (quem lê o documento seguinte, quem usa o
   código depois), não pela ordem em que o item apareceu numa sessão.
3. **Item sem verificação não é item.** Cada um traz o comando ou passo que
   confirma que ele ainda está aberto — para a próxima sessão reverificar em
   segundos, sem precisar reler todo o histórico.
4. **Fechar o item no arquivo de origem.** Ao implementar, atualize o status
   *no backlog em que o item nasceu* — nunca só num changelog geral que
   ninguém vai reabrir para conferir o backlog específico.
5. **Separar item executável de decisão em aberto.** "Implementar se
   possível"/"avaliar viabilidade" não é item de backlog — ou se decide com
   quem pode decidir, ou vai para uma lista de escopo/observação separada.
6. **Não reabrir o que está encerrado por decisão explícita.** Se algo foi
   deliberadamente recusado (com justificativa registrada), reabrir sem que
   a pessoa responsável levante o assunto de novo é desperdício.
7. **Revisor externo é ponto de partida, não escopo fechado.** Verifique a
   **causa**, não só o sintoma, e verifique **antes** de implementar. Manter
   um registro informal de acerto/erro de cada fonte de revisão (humana ou
   de IA) ajuda a calibrar quanto ceticismo aplicar da próxima vez.

## 2. Template de item

```markdown
### <ID> — <título curto>
- **Onde é exigido/consumido:** <arquivo:linha / módulo / capítulo>, o ponto
  exato onde o problema aparece para quem usa o artefato — não onde ele
  nasceu.
- **Gap (evidência de hoje):** comando + resultado. Ex.:
  `grep -c "X" arquivo` → 0.
- **Ação:** o que entra, em que ponto, respeitando as convenções do projeto.
- **Verificação:** o comando/passo que confirma que o item foi resolvido.
- **Prioridade:** P1–P4 (ou sua escala), com a justificativa numa linha.
- **Status:** pendente | em andamento | implementado (data, referência).
```

A diferença entre um item bom e um mau é o par **"Gap (evidência)" ×
"Verificação"**: o primeiro prova que o item existe hoje, o segundo prova que
ele deixou de existir amanhã. Um item sem os dois é uma intenção, não um
item de backlog.

## 3. Critério de prioridade (adapte a escala, mantenha o critério)

- **P1 — bloqueia** o uso/leitura/build de algo que depende disso.
- **P2 — degrada** de forma visível, mas não impede o uso.
- **P3 — acabamento/didática** — melhora real, não urgente.
- **P4 — cosmético ou decisão de quem é dono do projeto.**

**Conformidade precede conteúdo, quando ambos estão no backlog**: defeitos de
convenção/estilo (formatação, nomenclatura, estrutura) entram antes de
conteúdo novo — corrigir o mesmo defeito duas vezes (uma vez manualmente,
outra ao adicionar conteúdo em cima dele) é desperdício evitável.

## 4. Filtro de observabilidade (antes de prometer um item de código/dado)

Antes de promover "implementar X" a item executável, pergunte:

> **Existe canal de medição/dado real para o que este item introduz?**

Se não existe, o item não é executável agora — é **bloqueado por
instrumentação/dado**, e vai para uma lista de escopo com a justificativa,
não para o backlog ativo. Regra prática: nunca implementar algo "para
constar" que não pode ser verificado contra nada real.

## 5. Classificação do item e critério de pronto

| Tipo | Critério de pronto |
|---|---|
| **Lógica/física nova** | função isolada e testável, parâmetro não fechado marcado como assumido/estimado, teste de sanidade direcional (o resultado se move na direção esperada quando a entrada muda) |
| **Algoritmo com aprendizado** | resíduo/métrica conferida contra um caso conhecido **antes** de treinar/otimizar de verdade; tolerância de teste calibrada por execução real, não por teoria; validado em mais de uma semente/condição inicial |
| **Identificação de parâmetro** | sensibilidade do resultado ao parâmetro verificada em mais de um ponto de operação antes de escrever o teste — um parâmetro pode ser observável num regime e não noutro |
| **Interface/UI** | testado de ponta a ponta (não só a lógica isolada) + verificação visual quando há mudança visual |
| **Paridade entre implementações** (ex. duas linguagens do mesmo sistema) | suíte de testes de ambas passando + concordância numérica verificada |
| **Infraestrutura** | ambiente reconstruído do zero funciona; dependência nova testada com a suíte completa depois |

## 6. Ordem de execução de um item

1. Rodar a suíte/baseline **antes** de mexer, para não confundir regressão
   pré-existente com regressão introduzida agora.
2. Implementar com o diff mínimo e focado no item — limpeza oportunista não
   pedida vira ruído de revisão e risco de regressão silenciosa.
3. Suíte/verificação completa, não só o teste novo.
4. Atualizar o status no arquivo de origem do item (regra 4 da seção 1).
5. Registrar a decisão em qualquer changelog/histórico do projeto, se a
   convenção do projeto pedir isso — nunca reescrever entradas antigas desse
   histórico, só acrescentar.
6. Respeitar a regra de autorização do projeto para commit/push — não
   presumir consentimento implícito.

## 7. Anti-padrões (o que já custou tempo em projetos reais)

1. Item sem comando de verificação — impossível de reconferir depois sem
   reler tudo.
2. Item vindo de revisor externo sem checagem na fonte primária.
3. Implementar primeiro e verificar o achado depois — a ordem inversa é
   sempre mais barata.
4. Reabrir item encerrado por decisão deliberada de quem é dono do projeto.
5. Duplicar item que já está listado como pendência conhecida sem conferir o
   estado atual primeiro.
6. "Implementar se possível"/"avaliar viabilidade" sem decisão — vira status
   congelado na sessão seguinte.
7. Inserir conteúdo no meio de uma sequência numerada sem considerar o custo
   de renumeração em cascata.
8. Reescrever entradas antigas de um changelog/histórico — se o projeto trata
   esse arquivo como registro cronológico, sempre acrescentar.
9. Declarar um item pronto sem rodar a verificação que o próprio item define
   — "verificar antes de declarar pronto" nunca o contrário.

## 8. Rotina barata de fim de sessão

- Rodar as varreduras/checagens aplicáveis ao que foi editado nesta sessão.
- Comparar contagens-chave (linhas, casos de teste, páginas, o que fizer
  sentido no projeto) contra o baseline anterior — qualquer piora é
  regressão até provado o contrário.
- Atualizar o status nos arquivos de origem dos itens tocados (não só num
  resumo de sessão).
- Deixar o estado do repositório limpo ou explicitamente explicado — nada
  commitado/enviado sem autorização, se essa for a regra do projeto.
