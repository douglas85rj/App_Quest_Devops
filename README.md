# App_Quest_Devops

README do repositório App_Quest_Devops — coleção de front- e backends usados no projeto.

Este repositório contém três subprojetos principais:

- `backend-scm/`  — serviços relacionados à integração com SCM/GitHub.
- `backend-user/` — API de usuário, autenticação, modelos e rotas.
- `frontend/`     — aplicação React (UI) conectada aos backends.

## Visão geral da arquitetura

O projeto segue uma arquitetura simples de micro-serviços locais:

- Frontend (React) consome as APIs expostas pelos backends.
- `backend-user` fornece autenticação (Passport + JWT), perfis, posts e endpoints do usuário.
- `backend-scm` oferece integrações com repositórios (GitHub) e endpoints auxiliares.

Cada subdiretório contém um `package.json` e scripts auxiliares (ex.: `read-package-version.sh`).

## Estrutura (resumo)

Raiz:

```
README.md
backend-scm/
backend-user/
frontend/
```

Dentro de cada backend há uma pasta `config/` com `keys-env.js` e `keys.js`.

## Requisitos

- Node.js (recomendo 12.x ou 14.x+)
- npm ou yarn
- MongoDB (local ou atlas) para `backend-user`

No macOS com zsh, os comandos abaixo funcionam diretamente no terminal.

## Configuração das variáveis de ambiente

Os backends usam um arquivo `config/keys.js` que carrega `keys-env.js` em produção/staging ou `keys-dev` em desenvolvimento.

Opções:

1) Definir variáveis de ambiente (recomendado para produção):

```bash
export MONGO_URI="sua_mongo_uri"
export SECRET_OR_KEY="uma_chave_secreta"
export GITHUB_CLIENT_ID="<client_id>"
export GITHUB_CLIENT_SECRET="<client_secret>"
```

2) Para desenvolvimento rápido, você pode criar um arquivo `config/keys-dev.js` em cada backend com o conteúdo:

```javascript
module.exports = {
	mongoURI: 'mongodb://localhost:27017/seu_banco',
	secretOrKey: 'uma_chave_local',
	clientId: '',
	clientSecret: ''
};
```

Ou copie `config/keys-env.js` para `config/keys.js` e adapte conforme necessário (não commit em VCS se contiver segredos).

## Como rodar (local)

1) Backend de usuário (`backend-user`)

```bash
cd backend-user
npm install
# Para desenvolvimento (com nodemon):
npm run server
# Em produção (ou para usar o script start do package.json):
npm start
```

Observação: o `package.json` tem um script `server` que usa `nodemon server.js` (útil em dev) e `start` que executa o servidor com node.

2) Backend SCM (`backend-scm`)

```bash
cd backend-scm
npm install
# Não há script de start no package.json; rode direto:
node server.js
```

3) Frontend (`frontend`)

```bash
cd frontend
npm install
npm start
```

O `frontend` usa `react-scripts` (create-react-app) — a página abrirá em `http://localhost:3000` por padrão.

## Scripts úteis

- `read-package-version.sh` (existente em cada subprojeto) — imprime a versão definida no `package.json`.

Exemplo (no macOS/zsh):

```bash
./read-package-version.sh
```

## Desenvolvimento e contribuições

- Crie uma branch para cada feature/bugfix: `git checkout -b feature/nome-da-feature`.
- Faça commits pequenos e claros.
- Não faça commit de arquivos com segredos (`config/keys-dev.js` ou `.env` que contenham chaves reais).

## Dicas rápidas

- Se o backend não iniciar, verifique variáveis de ambiente e string de conexão do MongoDB.
- Para desenvolvimento do backend-user, prefira `npm run server` para auto-reload via `nodemon`.
