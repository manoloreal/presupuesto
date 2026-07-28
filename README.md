# presupuesto. — pipeline de deploy (GitHub + Netlify)

Depois disto configurado, o fluxo passa a ser: **edita o ficheiro → `git push` → o site atualiza sozinho em produção**, sem precisar de voltar a arrastar pastas manualmente.

## 0. Ficheiros deste projeto

```
index.html          → homepage de marketing
gerador.html         → a ferramenta (app)
como-usar.html        → página de ajuda
robots.txt
sitemap.xml
llms.txt
netlify.toml         → configuração do Netlify (redirects e headers)
.gitignore
```

Todos devem estar juntos, na raiz do repositório.

---

## 1. Criar o repositório no GitHub

1. Entre em [github.com](https://github.com) e faça login (ou crie conta, é grátis).
2. Clique em **"New repository"** (botão verde, ou `+` no canto superior direito → "New repository").
3. Nome sugerido: `presupuesto`.
4. Deixe como **Public** ou **Private** — para o Netlify funciona com qualquer um dos dois.
5. **Não** marque "Add a README" (já temos ficheiros para enviar) — crie o repositório vazio.
6. Clique em **"Create repository"**.

## 2. Enviar os ficheiros para o GitHub

Se ainda não tem o Git instalado no seu computador, descarregue em [git-scm.com](https://git-scm.com/downloads).

Abra o terminal (ou "Prompt de Comando" no Windows) na pasta onde estão todos os ficheiros do site, e execute:

```bash
git init
git add .
git commit -m "Primeira versão do site"
git branch -M main
git remote add origin https://github.com/SEU-UTILIZADOR/presupuesto.git
git push -u origin main
```

> Troque `SEU-UTILIZADOR` pelo seu nome de utilizador do GitHub. O endereço exato aparece na própria página do repositório recém-criado, no botão verde **"Code"**.

Na primeira vez, o GitHub vai pedir para confirmar login (pelo browser ou por token de acesso).

## 3. Ligar o repositório ao Netlify

1. Entre em [app.netlify.com](https://app.netlify.com).
2. Clique em **"Add new site"** → **"Import an existing project"**.
3. Escolha **"Deploy with GitHub"** e autorize o acesso quando pedido.
4. Selecione o repositório `presupuesto` que acabou de criar.
5. Nas definições de build:
   - **Build command**: deixe em branco (não há build — é HTML puro).
   - **Publish directory**: `.` (a própria raiz do repositório).
   - Isto já está definido no `netlify.toml`, por isso o Netlify normalmente preenche sozinho.
6. Clique em **"Deploy site"**.

Em menos de um minuto o site fica no ar, num endereço tipo `nome-aleatorio.netlify.app`.

## 4. O pipeline automático

A partir de agora, **sempre que fizer `git push` para o branch `main`, o Netlify deteta e publica a nova versão automaticamente** — não precisa mais de arrastar pastas.

Fluxo do dia a dia:

```bash
# depois de editar algum ficheiro...
git add .
git commit -m "Descrição da alteração"
git push
```

Em ~30 segundos a alteração já está em produção. Pode acompanhar o progresso em **app.netlify.com → o seu site → separador "Deploys"**.

### Bónus: pré-visualizações automáticas

Se criar um *branch* separado (ex: `git checkout -b testes`) e enviar alterações nele antes de fazer merge no `main`, o Netlify gera automaticamente um **link de pré-visualização único** para essa versão — ótimo para testar mudanças antes de as tornar públicas, sem mexer no site "oficial".

## 5. Ligar o domínio próprio

Já fizemos isto antes para o Netlify — os passos são os mesmos independentemente de como o site foi publicado (arrastar pasta ou via GitHub): **Site settings → Domain management → Add a domain**, e depois configurar os registos `A` e `CNAME` na GoDaddy. Consulte o guia anterior (`guia-deploy-netlify.md`) para o passo a passo completo.

## 6. Checklist

- [ ] Repositório criado no GitHub com todos os ficheiros
- [ ] `git push` feito com sucesso
- [ ] Site importado no Netlify a partir do GitHub
- [ ] Primeiro deploy automático concluído (site `.netlify.app` a funcionar)
- [ ] Domínio próprio ligado (registos DNS na GoDaddy)
- [ ] Testado: alterar um ficheiro localmente → `git push` → confirmar que o site atualiza sozinho
