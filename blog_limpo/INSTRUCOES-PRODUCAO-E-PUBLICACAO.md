# Instruções de produção e publicação: blog sem Hero e sem bloco de apresentação

Repositório: Bio-Tech-Edu/bio-tech-edu.github.io · Base: branch `main` no commit `d0a422d` · Site: https://bio-tech-edu.github.io

## 1. O que esta entrega faz

1. Remove o **Hero da página inicial** das páginas geradas (blog, publicações, privacidade, termos e 404). Ele estava sendo copiado para essas páginas pelo cabeçalho compartilhado. O `index.html` não é alterado: a home continua com o Hero.
2. Remove o **bloco de apresentação do blog** (rótulo "Blog", título "Ideias para ensinar…" e texto de apoio), que ficava acima do carrossel. O carrossel passa a ser o primeiro bloco da página.
3. Mantém um **título oculto** (`h1` com a classe `visually-hidden`) em cada página do blog, para que leitores de tela continuem anunciando o título da página.
4. Adiciona verificações automáticas: falha o build se o Hero voltar a aparecer nas páginas geradas e se alguma página tiver número de `h1` diferente de um.

Não foram alterados: CSS, JavaScript (carrossel, busca, filtros, comentários, tema), `index.html` e os textos das publicações.

## 2. Arquivos desta entrega

| Arquivo | Uso |
|---|---|
| `bio-tech-edu-blog-limpo.patch` | Série de 2 commits sobre a `main` em `d0a422d`. **Use este para publicar.** |
| `bio-tech-edu-blog-limpo.zip` | Árvore completa do projeto já com as alterações. Use se não quiser usar Git local. |
| `blog.html` | Versão gerada da página do blog, para conferência visual antes da publicação. |
| `INSTRUCOES-PRODUCAO-E-PUBLICACAO.md` | Este documento. |

Os commits da série:

- `bb96717` fix(blog): remove o hero da página inicial das páginas geradas
- `05ef177` feat(blog): remove o bloco de apresentação do topo e mantém um h1 oculto

## 3. Pré-requisitos

- Git 2.30 ou superior.
- Node.js 22 (o projeto aceita a partir da versão 20).
- Permissão para criar branch e abrir pull request no repositório.
- Acesso ao painel do GitHub, em *Actions* e *Settings → Pages*, para acompanhar o deploy.

## 4. Aplicar e testar localmente

```
git clone https://github.com/Bio-Tech-Edu/bio-tech-edu.github.io.git
cd bio-tech-edu.github.io
git checkout main && git pull
git checkout -b blog-sem-hero-e-apresentacao
git am bio-tech-edu-blog-limpo.patch
npm ci
npm test
```

Resultado esperado no `npm test`:

```
✔ 5 de 5 publicação(ões) gerada(s); blog, páginas institucionais e sitemap atualizados.
✔ 10 páginas verificadas: links, imagens e IDs ok.
✔ comentários: provedor "disqus" configurado e chaves do script conferidas.
```

Depois, confira que o build não altera nada versionado:

```
git status --short
```

A saída deve estar vazia. Se houver arquivos listados, pare e avise antes de publicar.

Para visualizar localmente:

```
python3 -m http.server 8000
```

Abra http://localhost:8000/blog.html. Não abra o arquivo direto pelo explorador de arquivos: o blog carrega dados em JSON e não funciona sem servidor.

## 5. Publicar

```
git push -u origin blog-sem-hero-e-apresentacao
```

Abra o pull request para `main` no GitHub e confirme:

- O check **Gerar blog e validar site** está verde.
- Os arquivos alterados são estes: `scripts/blog.template.html`, `scripts/build-blog.mjs`, `scripts/check-links.mjs`, `blog.html`, `404.html`, `termos.html`, `privacidade.html` e os 5 arquivos em `blog/*.html`.
- Nenhum arquivo de `index.html`, `assets/css/` ou `assets/js/` aparece na lista.

Faça o *merge* quando a revisão estiver aprovada. A sugestão é usar *Squash and merge* para manter um único commit na `main`, ou *Rebase and merge* para manter os dois commits, conforme a política da organização.

## 6. Acompanhar o deploy

1. Em **Actions**, acompanhe o workflow **Gerar blog e validar site** disparado pelo *push* na `main`. Ele leva cerca de um minuto.
2. Se o build gerar alguma diferença, o bot faz um commit `chore(blog): atualiza publicações geradas`. Com esta entrega, isso não deve acontecer. Se acontecer, abra o commit e avise antes de continuar.
3. O GitHub Pages publica a versão nova logo depois. Em *Settings → Pages*, confirme que a origem é `main` / `(root)`.

## 7. Checklist de verificação em produção

Abra as páginas com cache limpo (janela anônima ou Ctrl+F5, porque o navegador e o GitHub Pages podem manter a versão antiga por alguns minutos).

| Página | O que conferir |
|---|---|
| https://bio-tech-edu.github.io/blog.html | Não aparece o Hero "Ciência e tecnologia para aprender fazendo.". Não aparece o bloco "Ideias para ensinar…". O carrossel começa logo abaixo do menu, com 5 publicações e botões funcionando. |
| Busca e filtros | Digitar um termo filtra a lista. Áreas, atuações, formatos e tags filtram e limpam corretamente. |
| https://bio-tech-edu.github.io/ | A home **continua** com o Hero e com a contagem de 8 recursos. |
| Uma publicação, por exemplo `blog/desafios-do-eca-digital-e-bncc-computacao.html` | Sem Hero. Botão "Carregar comentários (Disqus)" visível. |
| `privacidade.html`, `termos.html` e uma página inexistente (ex.: `/xyz`) | Sem Hero. A página 404 continua mostrando a mensagem de erro. |
| Teclado | Pressionar Tab a partir do início: o primeiro item é o link "Pular para o conteúdo", seguido do menu. O foco fica visível em todos os itens. |
| Leitor de tela (NVDA, VoiceOver ou TalkBack) | Ao entrar em `blog.html`, o título "Blog Bio+Tech EduDesign" é anunciado. Na página há exatamente um título principal. |
| Temas | Alternar entre claro e escuro: texto legível e contraste adequado nos dois temas. |
| Celular (largura de cerca de 390 px) | Carrossel, busca e menu lateral aparecem em coluna única, sem rolagem horizontal. |

## 8. Ponto de atenção visual

Com a remoção do bloco de apresentação, o carrossel fica mais próximo do cabeçalho do que antes, porque o bloco tinha 56 px de espaço no topo. Isso não quebra nada, mas pode parecer apertado. Esta entrega não altera CSS, por pedido. Se o espaçamento não agradar, a correção é uma regra de uma linha em `assets/css/blog.css`, que pode ser feita em outra entrega. Exemplo, sem aplicar agora:

```css
#conteudo > .container:first-of-type { margin-top: 40px; }
```

## 9. Como reverter, se necessário

Se algo der errado depois do *merge*, reverta os dois commits em um novo pull request:

```
git checkout main && git pull
git checkout -b reverter-blog-sem-hero
git revert --no-edit 05ef177 bb96717
git push -u origin reverter-blog-sem-hero
```

Abra o pull request, aguarde o check e faça o *merge*. O workflow regenera as páginas no estado anterior.

## 10. Pendências conhecidas, fora deste escopo

- **Imagens antigas ainda presentes:** os 5 PNGs em `assets/blog/` (cerca de 8,8 MB) continuam no repositório e geram um aviso de peso no build. Eles não são mais usados pelas publicações. Podem ser removidos em uma entrega própria.
- **Estilos órfãos:** as regras `.blog-hero` em `assets/css/blog.css` deixaram de ser usadas. Podem ser removidas depois, com aprovação, para manter o CSS enxuto.
- **Pendências do Sprint 1:** configuração manual do Disqus (domínio permitido, pré-moderação e comentário de teste) e revisão jurídica da política de privacidade e dos termos. Os detalhes estão no `RELATORIO-SPRINT-1.md` e em `docs/PLANO-DE-SPRINTS.md`.
