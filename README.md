# skills_gratis

Coleção de [skills do Claude Code](https://docs.claude.com/en/docs/claude-code/overview)
gratuitas e de uso livre — extraídas de projetos reais, generalizadas para
funcionar em qualquer repositório, não só no projeto de origem.

## O que é uma skill aqui

Cada pasta é uma skill independente no formato padrão do Claude Code: um
`SKILL.md` com frontmatter (`name`, `description`) e o corpo em Markdown com
as instruções. `description` é o que o Claude usa para decidir quando
carregar a skill sozinho — escreva a sua tarefa normalmente e ele carrega a
skill certa quando o contexto bate.

## Como usar

**Numa sessão do Claude Code, num projeto qualquer:**

```bash
# dentro do repositório onde você quer usar a skill
mkdir -p .claude/skills
cp -r /caminho/para/skills_gratis/<nome-da-skill> .claude/skills/
```

Ou, se preferir manter só um clone deste repositório e linkar:

```bash
ln -s /caminho/para/skills_gratis/<nome-da-skill> .claude/skills/<nome-da-skill>
```

A skill fica disponível na próxima vez que o Claude Code listar as skills do
projeto — não precisa reiniciar nada além da sessão.

## Skills disponíveis

| Skill | Para quê |
|---|---|
| [`redacao-tecnica`](./redacao-tecnica/SKILL.md) | Escrever ou revisar prosa e equacionamento num documento LaTeX longo e numerado (livro técnico, relatório, manual, tese) — numeração/citação de equações, figuras e tabelas, aspas, estrutura de capítulo, veracidade de referências, critério de elaboração de exercícios. |
| [`backlog-tecnico`](./backlog-tecnico/SKILL.md) | Levantar, priorizar e fechar itens de backlog em qualquer projeto técnico (código ou redação) sem acumular status congelado nem aceitar achado de revisor externo sem verificar a causa. |

## Origem

Ambas as skills nasceram como convenções documentadas ao longo de várias
sessões de trabalho num projeto real de redação técnica (um livro/coleção
técnica em LaTeX com implementações de referência em código) e foram
generalizadas aqui — sem os nomes e detalhes específicos daquele projeto —
para serem reaproveitadas em qualquer outro.

## Licença

MIT — use, copie, modifique e redistribua livremente. Ver [`LICENSE`](./LICENSE).
