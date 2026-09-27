---
name: redacao-tecnica
description: Convenções de redação técnica para documentos LaTeX longos e numerados (livro técnico, relatório, manual, dissertação) — numeração e citação de equações, figuras e tabelas, aspas, estrutura de capítulo, veracidade de referências e critério de elaboração de exercícios. Carregar ANTES de escrever ou editar prosa, equação, figura, tabela ou exercício num documento .tex extenso com numeração por capítulo/seção, ou antes de revisar um capítulo existente contra essas convenções.
---

# Redação técnica — documentos LaTeX longos e numerados

Regras práticas para escrever e revisar prosa e equacionamento em qualquer
documento `.tex` extenso (livro, relatório técnico, manual, tese) que numera
equações/figuras/tabelas por capítulo ou seção e é pensado para ser
**consultado por partes**, não só lido do início ao fim. Cada regra abaixo
nasceu de um defeito real encontrado em projetos desse tipo — o objetivo é
detectar a classe de erro antes que ela se repita, não reconstruir a lógica
do zero a cada sessão.

Esta skill cobre prosa e equacionamento. Metodologia de backlog (como
levantar/priorizar/fechar item de melhoria) é uma skill separada,
`backlog-tecnico`.

## 1. Equações

- Toda equação numerada usa `\begin{equation}\label{eq:<prefixo>}...\end{equation}`
  — nunca solta em `$...$`/`$$...$$` no meio do texto corrido, salvo troca de
  símbolo por valor numérico específico ou definição de termo num "Onde
  $X=...$" que já segue uma equação numerada.
- Escolha um prefixo de label estável e documente-o (ex.: `eq:c<capítulo>-<n>`
  se a numeração for `\numberwithin{equation}{section}`, ou um prefixo por
  volume/parte se o projeto tiver mais de um documento). Preserve labels
  existentes ao editar.
- **Toda equação cita `\eqref{}` de si mesma no parágrafo que a antecede** —
  não basta explicar o conceito, o número tem que aparecer antes dela. Um
  parágrafo que só explica o conceito, sem `\eqref{}`, não satisfaz a regra.
  Verificação barata: para cada `\label{eq:...}`, o `\eqref{}` do mesmo label
  deve aparecer nos parágrafos imediatamente anteriores — script simples de
  regex sobre o `.tex` acha 100% das violações em segundos.
- **Equação nova entra sempre ao fim do capítulo/subseção**, nunca no meio —
  se a numeração é por posição física (`\numberwithin{equation}{section}`),
  inserir no meio força renumeração em cascata de todos os labels e
  `\eqref{}` seguintes daquele capítulo, em qualquer parte do documento que
  os cite.
- Referencie equações **sempre** por `\eqref{}`, nunca hardcode "Equação 9.6"
  como texto solto — nem em prosa que cita um capítulo diferente do que está
  sendo editado (é aí que o hardcode mais escapa, porque o hábito de usar
  `\eqref{}` parece mais automático dentro do próprio capítulo). Ao encadear
  várias referências em sequência (célula de tabela ou frase), use vírgulas
  ("Eqs. 9.7, 9.18"), nunca barra sem espaço ("9.7/9.18/9.19" já causa
  overfull hbox por falta de ponto de quebra de linha).
- Nunca deixe linha em branco dentro de `\begin{equation}...\end{equation}`
  — quebra a compilação (diferente de `$...$`, que tolera).
- Verifique colisão de símbolo na nomenclatura/lista de símbolos do documento
  antes de introduzir letra nova, especialmente se o alfabeto grego já estiver
  bem usado. Símbolo reaproveitado com significado distinto em contextos que
  nunca coexistem na mesma equação é aceitável, mas precisa de nota explícita
  nos dois pontos de uso ("não confundir com..."), nunca silenciosa.

## 2. Figuras e tabelas

- `\label{fig:<id>}` / `\label{tab:<id>}`, sempre referenciadas por
  `Figura~\ref{}` / `Tabela~\ref{}` **antes** de aparecerem no texto — nunca
  "a figura/tabela abaixo" (quebra em qualquer reflow de layout).
- `\caption[curta]{longa}` sempre que o documento tiver Lista de
  Figuras/Tabelas — sem a forma curta, a legenda inteira vaza para a lista e
  a torna ilegível como índice escaneável.
- Tabelas em `booktabs` (`\toprule`/`\midrule`/`\bottomrule`), nunca `\hline`
  ou grade com barras verticais. Coluna de texto corrido largo usa
  `>{\raggedright\arraybackslash}p{Xcm}`, não `p{Xcm}` justificado puro (que
  produz espaçamento irregular entre palavras em colunas estreitas).
- Padronize larguras de figura em 2-4 categorias por tipo de conteúdo
  (ex.: gráfico simples, diagrama com legenda, figura composta, foto
  vertical) em vez de escolher um valor ad-hoc por figura — o documento
  ganha ritmo visual e fica mais fácil de manter.
- Floats sempre com fallback de posição (`[htbp]`), nunca só uma letra
  (`[h]`) — o LaTeX promove sozinho e gera warning de ruído no log.
- Imagem de terceiros só com licença livre **verificada na página real** (não
  só "parece livre" nem nome de arquivo sugestivo) — creditar autor, fonte e
  licença no próprio `\caption{}`.
- **Sempre renderizar e inspecionar visualmente** (`pdftoppm` ou equivalente)
  qualquer figura nova, e a página inteira onde ela entra — o log de
  compilação não acusa geometria errada, texto ilegível por fonte pequena
  numa figura densa, ou cabeçalho colidindo com número de página.

## 3. Prosa

- Se o idioma usa aspas curvas (português, francês etc.) e o documento usa
  `babel`, confira se o caractere `"` reto está sendo tratado como shorthand
  ativo — em português (`babel-portuguese`) ele engole o espaço seguinte.
  Prefira sempre as aspas tipográficas do idioma (` `` `/`''` em português) em
  texto corrido; exceção: dentro de `verbatim`, onde o texto/código deve
  continuar literal.
- Termo técnico em outro idioma recorrente, ou acrônimo novo: acrescente ao
  `\hyphenation{}` do preâmbulo para não sofrer hifenização errada pelo
  pacote de idioma; acrônimo sem nenhum ponto de quebra. Primeira ocorrência
  no corpo do texto: "termo no idioma do documento (*termo original*)", com
  um glossário reunindo os já cobertos.
- Se o projeto tiver regra própria sobre marcas de autoria/ferramenta no
  texto (comum em obras autorais), documente-a de forma explícita e trate
  como regra sem exceção — é fácil de violar sem querer ao gerar conteúdo em
  lote.

## 4. Estrutura de capítulo

- Padronize se cada capítulo começa em página nova (`\clearpage` antes do
  `\section`/`\chapter`) — para um documento pensado como obra de consulta,
  isso é quase sempre desejável; documente a exceção, se houver, em vez de
  deixar inconsistente sem registro.
- Título de capítulo longo (dezenas de caracteres) provavelmente colide com
  o número de página no cabeçalho corrente (`fancyhdr`) sem gerar nenhum
  aviso de compilação — só aparece em inspeção visual. Use
  `\markboth{<forma curta>}{}` logo após o comando de seção para encurtar só
  o cabeçalho, sem tocar no título real.
- Toda seção não numerada de front/back matter (prefácio, glossário,
  agradecimentos, lista de símbolos) também precisa do `\markboth{}{}`
  correspondente — comandos de seção com `*` não atualizam o cabeçalho
  corrente sozinhos, e sem isso o cabeçalho mente sobre em que seção o leitor
  está.
- Se o documento tem uma seção de fechamento padronizada por capítulo (ex.:
  "Diretriz técnica final", "Resumo do capítulo", "Pontos-chave"), mantenha o
  mesmo formato em todo capítulo novo — é o que sustenta a leitura por
  partes em vez de cover-to-cover.

## 5. Veracidade de referências e fontes

- Nenhuma referência bibliográfica nova entra sem ser verificada contra fonte
  real (editora, periódico, DOI, número/edição de norma) — nunca por "parece
  familiar" ou de memória, e nunca aceita de um LLM anterior sem checar.
  Título fabricado ou norma citada sem a edição/emenda que o conteúdo
  pressupõe já aconteceu em projetos reais e é difícil de notar depois.
- Qualquer número de norma/padrão citado só como texto (sem entrada na
  bibliografia) merece o mesmo rigor de verificação antes de aparecer no
  texto — não copie por proximidade de contexto com outra citação já
  existente.
- Quando outra parte do mesmo projeto (outro volume, outro capítulo) já traz
  uma referência validada para o mesmo autor/obra, copie a chave *verbatim*
  em vez de redigir uma nova entrada.
- Toda informação factual (técnica, histórica, numérica) inserida no texto
  deve ter fonte rastreável — se não tiver, marcar explicitamente como
  estimativa/suposição, nunca apresentar como fato verificado.

## 6. Critério de elaboração de exercícios (e de qualquer prosa didática)

Aplica-se a exercício resolvido, exemplo numérico, estudo de caso — qualquer
trecho que "resolve" algo:

1. **Fechamento pergunta↔resposta**: toda formulação que a resolução usa está
   anunciada no enunciado (como dado, equação citada, ou o próprio produto
   pedido) ou ensinada no capítulo — nunca aparece pela primeira vez dentro
   da resolução. Se falta a ferramenta, ensine-a no capítulo; não a
   contrabandeie na resposta.
2. **Responda o que foi perguntado**: se o enunciado pede um número, a
   resolução entrega aquele número, não um substituto conveniente. Regra
   prática/aproximação entra como comentário *depois* da resposta exata.
3. **Enunciado sem redundância nem ambiguidade**: cada grandeza aparece uma
   vez, com o tipo certo (escalar × vetor); todo termo técnico é definido ali
   ou no capítulo.
4. **Toda conta refeita à mão** antes de aceitar como correta — inclusive as
   dos comentários interpretativos.
5. **A leitura/conclusão decorre do que foi calculado** — não compare
   grandezas incompatíveis nem conclua mais do que os números mostram.
6. **Coerência com a prosa do próprio capítulo** — a resolução não pode
   contradizer o que o capítulo já afirmou, nem atribuir efeito à causa
   errada.

Verificação barata: para cada símbolo/equação citado numa resolução, procure
no documento onde foi definido **antes** daquele ponto. Um símbolo que só
aparece dentro da resolução é o sintoma.

## 7. Antes de declarar pronto

1. Compile o documento **3 vezes** (ou use `latexmk`) — 0 erros, 0
   referências indefinidas, 0 labels duplicados. Se o `.log` não for UTF-8
   (comum com `pdflatex`+babel), use uma busca que trate encoding
   corretamente (ex.: `grep -a` no Linux), senão buscas por "Overfull"/
   "Warning" podem falhar em silêncio.
2. Compare a contagem de overfull hboxes contra um baseline documentado —
   overfull novo é regressão até provado o contrário.
3. Renderize as páginas alteradas em imagem e inspecione visualmente — o log
   limpo não garante que a página renderiza bem (colisão de cabeçalho,
   tabela cortada, figura com geometria errada nunca aparecem como erro).
4. Varreduras baratas de regressão quando a sessão escreveu prosa nova:
   - Aspas retas indevidas, se o idioma usa aspas curvas.
   - Auto-referência de equação: cada label tem `\eqref{}` de si mesma no
     parágrafo anterior?
   - Busca por padrão de "Equação N.N" hardcoded fora de `\eqref{}`.
5. Se o projeto gera um documento derivado (edição combinada, versão para
   impressão, apostila) a partir de fontes menores, regenere-o em vez de
   editá-lo à mão, e recompile-o também.
6. Trate qualquer regra de "não commitar sem autorização" do projeto como
   absoluta — não presuma consentimento implícito de uma sessão para a
   próxima.
