---
name: git-guidelines
description: Strict guidelines for Git operations. ALWAYS consult before performing ANY git operantio, like (pull, commit, rebase, merge, fetch, branch, worktree, add).
---

# Git Guidelines - Reference for AI Agent

## ⛔ Regras Absolutas (Nunca violar)

### 1. 🚫 Nunca commitar arquivos que estejam dentro de `.gitignore`

**Sob hipótese alguma** você deve commitar arquivos que estão listados no `.gitignore`.

- Verifique sempre se o arquivo está no `.gitignore` antes de adicionar ao staging
- Se um arquivo estiver no `.gitignore`, ele **NÃO** pertence ao repositório
- Exceção: O próprio arquivo `.gitignore` e templates de configuração

### 2. 🚫 Nunca alterar o remote history

**De maneira nenhuma** você deve:

- Forçar push para o remote (`git push --force` ou `git push -f`)
- Reescrever history no remote (`git push --force-with-lease` sem aprovação explícita)
- Usar `git rebase --force` no remote
- Amassar commits no remote (`git push --force` em branches compartilhados)

### 3. 🚫 Nunca violar os hooks de git

**De maneira nenhuma** você deve:

- Forçar commit com `--no-verify`
- Usar qualquer flag ou artimanha para ignorar hooks (`--no-verify`, `--skip-hook`, etc.)
- Desativar hooks temporariamente ou permanentemente
- Usar `git commit --no-verify` ou `git push --no-verify`
- Bypass de pre-commit, commit-msg, pre-push ou qualquer outro hook

**Se erros de hooks aparecerem (formatting, linting, validation, etc.):**

- **SEMPRE corrija o código** que está causando o erro do hook
- Isso se aplica mesmo se o erro **não for decorrente das suas últimas alterações**
- Nunca ignore o erro — ajuste o código até que passe nos hooks
- Exemplos de correção: formatar código, corrigir lint, ajustar mensagens de commit, resolver conflitos de validação
- O erro do hook é um sintoma de que algo no código precisa ser ajustado, não um obstáculo a ser contornado

---

## ⚠️ Operações que Requerem Consentimento Explícito

### Regra Geral de Permissão

**A menos que o prompt do usuário diga explicitamente para executar commits ou pushes, você deve SEMPRE pedir permissão antes de executá-los.** Isso se aplica a `git commit`, `git push`, e qualquer operação que modifique o repositório de forma permanente.

Se o usuário disser "commita tudo", "sube isso", "faz o push", etc., você **deve confirmar** os detalhes (mensagem, arquivos, branch) antes de executar.

---

### Antes de executar, SEMPRE pergunte ao usuário:

#### `git commit`

Antes de fazer commit, pergunte:
- Quais arquivos serão incluidos?
- Qual a mensagem do commit?
- O usuário confirmou que quer fazer commit?

Exemplo de como perguntar:
> "Desejo fazer `git add` nos arquivos X, Y, Z e `git commit -m 'mensagem'`. Confirmo?"

#### `git push`

Antes de fazer push, pergunte apenas:
- O usuário confirmou que quer fazer push?

Exemplo de como perguntar:
> "Desejo fazer `git push`. Confirmo?"

#### `git pull`

Antes de fazer pull, pergunte:
- De qual branch?
- Qual estratégia de merge/rebase?
- O usuário confirmou que quer fazer pull?

Exemplo de como perguntar:
> "Desejo fazer `git pull origin main` com merge. Confirmo?"

#### `git reset`

Antes de fazer qualquer reset, pergunte:
- Qual tipo de reset? (`--soft`, `--mixed`, `--hard`)
- Para qual commit?
- O usuário está ciente das possíveis perdas?

Exemplo de como perguntar:
> "Desejo fazer `git reset --hard HEAD~1`. Isso irá descartar todas as mudanças locais. Confirmo?"

---

## ✅ Boas Práticas

- Sempre verificar `git status` antes de qualquer operação
- Usar `git diff` para revisar mudanças antes de commitar
- Preferir `git push --force-with-lease` ao invés de `git push --force` (sempre com aprovação)
- Manter commits atômicos e com mensagens descritivas
- Sempre seguir as regras de Conventional Commits ao commitar

---

## 🔄 Fluxo Recomendado

```
1. git status          → Verificar estado atual
2. git diff            → Revisar mudanças
3. git add             → Adicionar arquivos (NUNCA ignorados)
4. git commit          → ⚠️ PERGUNTAR ANTES (a menos que o prompt diga explicitamente)
5. git pull            → ⚠️ PREGUNTAR ANTES
6. git push            → ⚠️ PERGUNTAR ANTES (a menos que o prompt diga explicitamente)
```
