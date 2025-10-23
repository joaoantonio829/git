🧩 **Aula Prática — Git Local (sem GitHub)**

---

### 🎯 Objetivo

Aprender a usar o **Git** localmente para versionar projetos: criar commits claros, trabalhar com branches, recuperar mudanças e manter um histórico limpo e seguro — tudo explicado de forma prática e didática.

---

## 🧱 1. Configuração inicial (essencial)

Configure seu nome, e-mail e editor:

```bash
# informações do usuário (obrigatórias para commits)
git config --global user.name "João Antônio"
git config --global user.email "joaoantoniocarvalhobrandao@gmail.com"

# definir editor padrão (ex.: VS Code)
git config --global core.editor "code --wait"

# recomendação: forçar branch inicial como 'main' ao criar repositórios
# (Git 2.28+ também aceita: git init -b main)
git config --global init.defaultBranch main

# ver todas as configurações relevantes
git config --list
```

> Dica: use `git init -b main` se seu Git for versão 2.28 ou superior — cria o repo já com a branch `main`.

---

## 📂 2. Criar e iniciar um repositório (rápido)

```bash
mkdir meu_projeto
cd meu_projeto
# cria repositório com branch inicial main
git init -b main
```

> Resultado: uma pasta oculta `.git/` contendo todo o histórico.

---

## 📄 3. Criar arquivos e verificar o status

```bash
# adicionar conteúdo inicial
printf "# Meu Projeto
Este é um repositório de exemplo.
" > README.md

# ver estado resumido
git status -s
```

> `git status -s` mostra um resumo enxuto (untracked = ??, staged = A, modified = M).

---

## 🧺 4. Adicionar mudanças (staging) — controlando o que vai para o commit

```bash
# adicionar um arquivo específico
git add README.md

# adicionar interativamente (ótimo para revisar mudanças)
git add -p

# adicionar tudo (cuidado com arquivos não desejados)
git add .
```

> `git add -p` permite escolher pedaços (hunks) de mudança — excelente para commits pequenos e coesos.

---

## 💾 5. Criar commits claros e atômicos

```bash
git commit -m "feat: adiciona README inicial com descrição do projeto"

# para abrir o editor e escrever uma mensagem extensa:
# git commit
```

Boas práticas de mensagem:

* Linha de assunto curta (50 caracteres) + verbo no imperativo.
* Linha em branco + corpo explicando *por que* a mudança.

---

## ✏️ 6. Editar, revisar diff e commitar atualização

```bash
# editar o arquivo
printf "
## Uso
Rodar: ./start.sh
" >> README.md

# ver diferenças entre working tree e staging
git diff

# ver diferenças staged vs HEAD
git diff --staged

# preparar e commitar
git add README.md
git commit -m "docs: atualiza README com seção de uso"
```

---

## ♻️ 7. Desfazer mudanças (com segurança)

```bash
# descarta mudanças locais não adicionadas ao staging
git restore README.md

# remove da área de staging (mantendo alterações no working tree)
git restore --staged README.md

# voltar o arquivo ao estado do último commit (cuidado: perde mudanças locais)
git checkout -- README.md    # (compatibilidade) ou git restore README.md
```

> Atenção: `git reset --hard` apaga mudanças locais. Use apenas se tiver certeza.

---

## 🌿 8. Branches: criar, listar e alternar (fluxo seguro)

```bash
# listar branches
git branch

# criar e alternar (mais moderno)
git switch -c nova_feature

# só alternar para branch existente
git switch main
```

> `git switch` e `git restore` são comandos mais intuitivos do que `git checkout`.

---

## 🧬 9. Exemplo: desenvolver em uma branch e trazer para main

```bash
# crie a branch e trabalhe nela
git switch -c feature/login
# ... faça alterações, adicione, commit
git add .
git commit -m "feat(login): adiciona fluxo de autenticação"

# volte para main e faça merge
git switch main
git merge --no-ff feature/login -m "merge: integra feature/login"
```

> `--no-ff` cria um commit de merge explícito, preservando a visibilidade da branch.

---

## 🔄 10. Alternativas de integração: rebase (limpar histórico)

```bash
# atualizar sua branch com mudanças da main (rebase local)
git switch feature/login
git fetch origin    # útil quando há um remoto
git rebase main

# resolver conflitos, depois continuar:
# git add <arquivo>
# git rebase --continue
```

> Rebase reescreve histórico — use em branches locais antes de compartilhar; evite reescrever branches públicas.

---

## 🧰 11. Recuperar mudanças: reflog e reset

```bash
# ver movimento do HEAD (útil para recuperar commits perdidos)
git reflog

# voltar o branch para um ponto anterior (local), mantendo arquivos
git reset --soft <commit>

# voltar e descartar alterações (cuidado)
git reset --hard <commit>
```

> Use `git reflog` quando "sumiu" um commit — ele mostra todos os HEADs recentes.

---

## 📦 12. Stash: salvar trabalho temporariamente

```bash
# salvar mudanças não commitadas
git stash save "WIP: ajustando login"

# listar stashes
git stash list

# aplicar o stash mais recente
git stash apply

# aplicar e remover do stash
git stash pop
```

> Útil para trocar de branch sem perder trabalho em andamento.

---

## 🗑️ 13. Excluir branches locais com segurança

```bash
# exclui branch que já foi mesclada
git branch -d feature/login

# força exclusão se ainda não foi mesclada
git branch -D feature/velha
```

---

## 🧹 14. Ignorar arquivos: `.gitignore` e `.gitattributes`

**.gitignore** (exemplo):

```
# logs e temporários
*.log
*.tmp

# node
node_modules/

# ambiente local
.env
.DS_Store

# build
dist/
```

**.gitattributes** (exemplo para lidar com finais de linha):

```
* text=auto
*.sh eol=lf
```

---

## 🔎 15. Dicas para inspeção rápida

```bash
# histórico compacto e visual
git log --oneline --graph --decorate --all

# status resumido
git status -s

# limpar arquivos não rastreados (apenas mostrar com -n)
git clean -n
# deletar arquivos não rastreados
git clean -f
```

---

## 💡 16. Truques úteis (amend, cherry-pick, short alias)

```bash
# alterar mensagem do último commit
git commit --amend -m "corrige: mensagem do commit"

# aplicar um commit de outra branch
git cherry-pick <commit>

# criar alias rápido localmente
git config --global alias.lg "log --oneline --graph --decorate"
# então use: git lg
```

---

## ✅ 17. Exemplo de fluxo completo — com comandos modernos

```bash
# iniciar repo
mkdir aula-git && cd aula-git
git init -b main

# criar arquivo inicial
printf "Aula prática Git local
" > README.md

# commit inicial
git add README.md
git commit -m "chore: commit inicial"

# criar branch de desenvolvimento
git switch -c dev
printf "Versão em desenvolvimento
" >> README.md
git add README.md
git commit -m "feat: atualiza README na dev"

# voltar e mesclar
git switch main
git merge --no-ff dev -m "merge: integra dev"

git log --oneline --graph --decorate
```

---

## 🧪 Exercícios práticos (3 minutos cada)

1. Crie uma branch `feature/x` e faça 2 commits; volte para `main` e mescle mantendo o commit de merge.
2. Modifique um arquivo, use `git add -p` para fazer staging parcial e comite só parte das mudanças.
3. Faça um commit, emende a mensagem com `--amend`, e recupere a versão anterior com `git reflog`.

---

## 📘 Créditos

Material adaptado e enriquecido para fins didáticos por *Anderson RM Gomes* 🧑‍🏫 — atualizado com comandos modernos e boas práticas.

---

🚀 **Próximo passo:** quer que eu gere uma versão em PDF para imprimir, slides (PowerPoint) ou uma folha de cola (cheatsheet) com os comandos mais usados? Respondendo aqui eu já gero para você.

# 🚀 **Próximos passos — Conectando ao GitHub**

---

## 🌍 1. Adicionar repositório remoto

Após criar seu repositório no **GitHub** (sem README, sem .gitignore, sem license):

```bash
# adiciona o repositório remoto chamado 'origin'
git remote add origin https://github.com/seuusuario/meu_projeto.git

# confirma o remoto configurado
git remote -v
```

> Dica: se preferir SSH (mais seguro), use
> `git remote add origin git@github.com:seuusuario/meu_projeto.git`

---

## 🚀 2. Enviar código local para o remoto

```bash
# envia a branch principal para o GitHub
git push -u origin main
```

> O parâmetro `-u` cria o vínculo entre a branch local e a remota,
> permitindo usar apenas `git push` ou `git pull` nas próximas vezes.

---

## 🔁 3. Atualizar e sincronizar com o remoto

```bash
# baixar e integrar alterações do GitHub
git pull origin main

# ou simplesmente, após o primeiro push:
git pull
```

> ⚠️ Sempre dê `git pull` antes de começar a trabalhar,
> para garantir que sua branch local está atualizada com a remota.

---

## 🦯 4. Verificar status e branches remotas

```bash
git branch -a     # lista todas as branches (locais e remotas)
git remote show origin
```

> Isso ajuda a confirmar se seu repositório local e o remoto estão sincronizados corretamente.

---

## ✅ Conclusão

Agora seu repositório local está **conectado ao GitHub**, permitindo que você **compartilhe, colabore e versiona remotamente** seu projeto.

**Na próxima aula:** veremos como **trabalhar em equipe** com *branches remotas*, *pull requests*, *rebase* e *fluxos colaborativos (Git Flow)*.

