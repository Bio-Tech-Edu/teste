# Sprint 1: Comentários e comunidade

Entrega do Sprint 1 do site Bio+Tech EduDesign (repositório Bio-Tech-Edu/bio-tech-edu.github.io), a partir do commit 42edd47. Data: 06/11/2026.

## 1. Diagnóstico

A mensagem « Os comentários serão ativados em breve » aparecia por dois motivos:

- Correção sobre a investigação inicial: o commit 42edd47 é automático do bot e altera apenas o HTML de uma publicação. A configuração do Disqus está em assets/js/comments.js, nos commits 6795b62 e cd456fa.
- O script lia d.biotechedu, uma chave que não existe. A configuração usa disqus.shortname. Por isso o ramo do Disqus nunca era ativado.
- O blog/config.json estava com provedor giscus, IDs vazios e shortname vazio. Sem provedor válido, a página mostrava a mensagem de « em breve ».

Corrigir só o JSON não resolveria, porque o primeiro erro continuaria. Isso foi reproduzido em navegador simulado: com o JSON corrigido e o script antigo, o botão não aparece.

## 2. O que foi feito

- Comentários com Disqus (shortname biotechedu): carregamento somente após o clique, aviso de consentimento com link para a política de privacidade e nova tentativa em caso de falha.
- scripts/check-comments.mjs: valida provedor, shortname, formato do config e as chaves lidas pelo script. Incluído no npm test e no workflow. Detecta o erro original.
- Política de privacidade e termos de uso (LGPD), escritos em paginas/\*.md e gerados como privacidade.html e termos.html. São minutas, sujeitas a revisão jurídica.
- Wiki no menu principal e no rodapé de todas as páginas.
- Modelos de issue (problema, recurso, tema de blog, denúncia de comentário), modelo de pull request e CONTRIBUTING.md.
- Limpeza: removidos o patch e o zip versionados por engano na raiz, e a pasta assets/blog com PNGs antigos. O .gitignore passa a ignorar \*.patch e \*.zip de fato.

## 3. Arquivos modificados (38 no total)

| Grupo | Arquivos | Situação |
| --- | --- | --- |
| Comentários | blog/config.json, assets/js/comments.js, package.json, scripts/check-comments.mjs | M, M, M, A |
| Privacidade e navegação | paginas/privacidade.md, paginas/termos.md, privacidade.html, termos.html, scripts/build-blog.mjs, scripts/check-links.mjs, index.html, assets/css/styles.css, sitemap.xml, .github/workflows/blog.yml | A, A, A, A, M, M, M, M, M, M |
| Regenerados pelo build (rodapé novo) | 404.html, blog.html, blog/\*.html (5 publicações) | M |
| Documentação e governança | CONTRIBUTING.md, .github/ISSUE\_TEMPLATE/ (5 arquivos), .github/PULL\_REQUEST\_TEMPLATE.md, README.md, docs/PLANO-DE-SPRINTS.md | A, A, A, M, M |
| Limpeza | bio-tech-edu-v6.patch, bio-tech-edu-v6.zip, assets/blog/\*.png (5), .gitignore | D, D, D, M |

O detalhamento de cada arquivo está no RELATORIO-SPRINT-1.md.

## 4. Configuração manual

Estes passos não podem ser feitos por commit. Os menus do Disqus podem mudar com atualizações do painel.

1. Confirmar o site biotechedu no painel do Disqus, com nome e URL https://bio-tech-edu.github.io. Se o shortname for outro, alterar comentarios.disqus.shortname em blog/config.json.
2. Incluir bio-tech-edu.github.io nos domínios permitidos do Disqus. Sem isso, o carregamento pode ser recusado.
3. Ativar a pré-moderação de comentários, pois o público inclui estudantes.
4. Publicar e moderar um comentário de teste, e apagá-lo depois. Esse é o critério de aceite do sprint.
5. Confirmar o GitHub Pages em main / root e que as Issues estão ativadas.
6. Revisar a política de privacidade e os termos com a assessoria jurídica da instituição antes da divulgação oficial. Verificar se há encarregado de dados (DPO) institucional a indicar na política.

Decisão a considerar: o plano gratuito do Disqus pode exibir anúncios e usar dados de navegação para isso. A política informa esse ponto. Se for um problema, o Giscus também está suportado: basta trocar o provedor para giscus no config e preencher repoId e categoryId.

## 5. Como aplicar

Opção A, com patch (4 commits, preserva o histórico):

```
git checkout main && git pull
git checkout -b sprint1-comentarios
git am bio-tech-edu-sprint1.patch
npm ci && npm test
git push -u origin sprint1-comentarios
```

Depois, abrir o pull request e aguardar o check ficar verde.

Opção B, com zip (sem Git local): extrair bio-tech-edu-sprint1.zip sobre o repositório, apagar assets/blog/, bio-tech-edu-v6.patch e bio-tech-edu-v6.zip, e enviar pela interface web do GitHub.

## 6. Verificação e limites

Verificado: o patch foi aplicado sem conflito num clone novo da main; o npm test passa; o build reproduz exatamente os arquivos versionados; 31 testes anteriores e 41 novos passam em navegador simulado, incluindo os casos negativos dos verificadores.

Não verificado: a existência do shortname biotechedu no Disqus, a renderização real do Disqus, a aparência nos dois temas e o comentário publicado e moderado. Esses pontos dependem dos passos da seção 4.

## 7. Arquivos da entrega

- RELATORIO-SPRINT-1.md: descrição detalhada de cada arquivo e a configuração manual.
- bio-tech-edu-sprint1.patch: 4 commits, com 15 MB por causa da remoção dos PNGs antigos.
- bio-tech-edu-sprint1.zip: árvore completa do projeto, com 2,5 MB.
