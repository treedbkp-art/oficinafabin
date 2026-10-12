# Fabinho Motors — guia para o GitHub

Este pacote contém o código atual da versão 7, incluindo dashboard financeiro,
gastos, histórico importado, correção de placas, composição de valores e envio
de ordens de serviço em imagem pelo WhatsApp.

## Repositório do projeto

O código está em https://github.com/treedbkp-art/oficinafabin, na branch `main`.
Para obter uma cópia local, execute:

```sh
git clone https://github.com/treedbkp-art/oficinafabin.git
cd oficinafabin
```

Para enviar alterações, use a autenticação da sua conta do GitHub;
não use a senha de acesso da oficina.

Alternativa sem terminal: abra o repositório no GitHub Desktop e use
Clone repository.

## O que está incluído

- Aplicação, interface, API, importadores e testes.
- Estrutura e migrations SQL do banco em `drizzle/`.
- `package.json` e `pnpm-lock.yaml` com as dependências do projeto.
- Modelo vazio `.env.example` e arquivos de configuração necessários.
- Configuração atual do Sites em `.openai/hosting.json` (sem credenciais).

O repositório não inclui banco de produção, dados dos clientes, planilhas,
backups, senhas, tokens, dependências instaladas nem histórico interno do Git.
Os nomes e demais registros nos testes são dados de teste.

A fonte corresponde ao commit informado em `VERSAO.txt`. Apenas a documentação
e o `.gitignore` receberam ajustes para esta entrega: instruções de exportação,
proteção de arquivos privados e remoção de números financeiros da documentação.
O código da aplicação foi preservado.

## Desenvolvimento

Requisitos declarados no projeto: Node.js >= 22.13 e pnpm 11.25.0.

```sh
pnpm install --frozen-lockfile
pnpm build
```

Verificações locais que não usam dados de produção:

```sh
node node_modules/typescript/bin/tsc --noEmit
node tests/integration.mjs
node tests/finance.mjs
node tests/plate-income.mjs
node tests/order-share.mjs
```

Os testes de importação comparam planilhas e manifestos privados e exigem
arquivos e variáveis adicionais descritos no README. Os scripts Python de
leitura de Excel usam `openpyxl`.

## Banco, acesso e hospedagem

O sistema usa Cloudflare Workers, D1 (banco) e R2 (planilhas). Subir o código
no GitHub armazena a fonte; não transfere o banco nem publica a aplicação
completa no GitHub Pages.

Para uma nova hospedagem, é necessário configurar D1/R2, aplicar as migrations
e cadastrar os segredos de ambiente `ADMIN_USERNAME`, `ADMIN_PASSWORD_HASH`
e `APP_ORIGIN`. O hash da senha é gerado com scrypt conforme `lib/password.ts`.
O campo `APP_ORIGIN` deve corresponder à origem HTTPS da instalação.
Não coloque os valores reais no código nem no arquivo `.env.example`.

O endereço atual continua sendo:
https://oficina-moto.creamy-book-0150.chatgpt.site

O envio do código ao GitHub não altera o sistema publicado. A migração dos dados para
outra hospedagem deve ser feita separadamente; os dados privados não pertencem
ao repositório. O aplicativo possui exportação JSON dos registros e download
das planilhas originais; a cópia JSON não tem restauração automática pela interface.
