# App de Avaliação Corporal | Joyce Fernandes

Guia de instalação com **Firebase** (login e banco de dados) e **GitHub Pages** (publicação do site). Tudo no plano gratuito. Tempo estimado: 30 a 40 minutos.

## Conteúdo do pacote

| Pasta | Arquivo | Para que serve |
|---|---|---|
| `site/` | `index.html`, `logo.png`, `config.js`, `.nojekyll` | O app. É o que vai para o GitHub. |
| `firebase/` | `firestore.rules` | Regras de segurança do banco de dados. |

Antes de configurar, dá para testar o app abrindo `site/index.html` no navegador. Ele roda em **modo demonstração**, com os dados salvos só naquele navegador.

---

## 1. Criar o projeto no Firebase

1. Acesse https://console.firebase.google.com com a conta Google que vai administrar o app.
2. Clique em **Criar um projeto** (ou **Adicionar projeto**), dê um nome (ex.: `joyce-avaliacao`) e avance. O Google Analytics pode ser desativado.

## 2. Registrar o app Web e preencher o `config.js`

1. Na página inicial do projeto, clique no ícone **Web** (`</>`).
2. Dê um apelido (ex.: `app-avaliacao`). **Não** marque "Firebase Hosting". Clique em **Registrar app**.
3. Vai aparecer um bloco `const firebaseConfig = { ... }`. Abra o arquivo `site/config.js` num editor de texto (Bloco de Notas serve) e troque cada `COLE_AQUI` pelo valor correspondente:

```js
window.FIREBASE_CONFIG = {
  apiKey: "AIza...",
  authDomain: "joyce-avaliacao.firebaseapp.com",
  projectId: "joyce-avaliacao",
  storageBucket: "joyce-avaliacao.appspot.com",
  messagingSenderId: "1234567890",
  appId: "1:1234567890:web:abc123"
};
```

Esses dados não são senha: o Firebase foi feito para que eles fiquem visíveis no site. Quem protege os dados são as regras do passo 4.

## 3. Ativar os tipos de login

1. Menu **Criação > Authentication > Vamos começar**.
2. Aba **Método de login**:
   - **E-mail/senha**: ative a primeira opção e salve.
   - **Google**: ative, escolha o e-mail de suporte e salve.
3. Aba **Templates** (opcional): em "Redefinição de senha", troque o idioma para Português.

## 4. Criar o banco de dados e colar as regras

1. Menu **Criação > Firestore Database > Criar banco de dados**.
2. Local: **southamerica-east1 (São Paulo)**. Modo: **produção**.
3. Depois de criado, abra a aba **Regras**, apague o conteúdo, cole o texto do arquivo `firebase/firestore.rules` e clique em **Publicar**.

Com essas regras, cada conta só enxerga as próprias clientes e avaliações. Ninguém lê dados de outra conta.

## 5. Publicar o site no GitHub Pages

1. Entre em https://github.com e clique em **New repository**.
   - Nome: ex. `avaliacao-joyce`.
   - Visibilidade: **Public** (o GitHub Pages gratuito exige repositório público; os dados das clientes **não** ficam no repositório, ficam no Firebase).
   - Clique em **Create repository**.
2. Na página do repositório, clique em **uploading an existing file** (ou **Add file > Upload files**) e arraste **os 4 arquivos da pasta `site/`**: `index.html`, `logo.png`, `config.js` e `.nojekyll`.
   - O `.nojekyll` fica oculto em alguns computadores. Se não aparecer, tudo bem: o app funciona sem ele.
   - Clique em **Commit changes**.
3. Vá em **Settings > Pages**. Em **Source**, escolha **Deploy from a branch**; em **Branch**, escolha **main** e a pasta **/(root)**. Clique em **Save**.
4. Em 1 a 2 minutos o endereço aparece no topo da página, no formato:
   `https://SEU-USUARIO.github.io/avaliacao-joyce/`

## 6. Autorizar o endereço do site no Firebase

Sem este passo, o login com Google mostra o erro "endereço não autorizado".

1. Firebase > **Authentication > Configurações > Domínios autorizados > Adicionar domínio**.
2. Digite só o domínio, sem `https://` e sem a pasta: `SEU-USUARIO.github.io`.

## 7. Primeiro acesso

1. Abra o endereço do site.
2. Clique em **Criar novo login** (ou **Entrar com Google**).
3. Depois da tela de boas-vindas, vá em **Configurações > Importar backup** e escolha `dados/backup-inicial.json`. As 3 avaliações já lançadas aparecem na lista.
4. Ainda em Configurações, confira nome, registros, WhatsApp e a mensagem de boas-vindas.

### Instalar no celular

Abra o site no celular e use **Adicionar à tela inicial** (Android: menu do Chrome; iPhone: botão Compartilhar no Safari). O app fica com ícone próprio.

---

## Segurança e cuidados

- **Quem pode criar login:** qualquer pessoa que acesse o endereço pode criar uma conta, mas vê apenas uma lista vazia, porque cada conta só acessa os próprios dados. Para impedir novos cadastros depois de criar as contas que vão usar o app, procure em Firebase > **Authentication > Configurações > Ações do usuário** a opção **Ativar a criação (inscrição)** e desmarque. Se essa opção não aparecer no seu console, é possível restringir o acesso pelas regras do banco a uma lista de e-mails.
- **Backup:** em Configurações, use **Baixar backup (.json)** de tempos em tempos e guarde o arquivo fora do GitHub. Ele contém dados de saúde das clientes (LGPD).
- **Sem internet:** o app guarda uma cópia local e sincroniza quando a conexão volta.

## Atualizar o app

Quando houver uma versão nova do `index.html`, abra o arquivo no repositório do GitHub, clique no lápis ou em **Add file > Upload files**, envie o arquivo novo e confirme. O `config.js` não precisa ser alterado de novo.

## Problemas comuns

| Mensagem | Solução |
|---|---|
| "Modo demonstração" no topo | O `config.js` ainda está com `COLE_AQUI` ou não foi enviado ao GitHub (passo 2 e 5). |
| "Este endereço não está autorizado no Firebase" | Faça o passo 6. |
| "Este tipo de login não está ativado" | Faça o passo 3. |
| "Erro ao ler os dados: Missing or insufficient permissions" | As regras do passo 4 não foram publicadas. |
| Login com Google não abre no celular | Permita pop-ups para o site ou tente de novo; o app usa redirecionamento quando o pop-up é bloqueado. |

## Limites do plano gratuito (Spark)

Firestore: 1 GB de armazenamento e 50 mil leituras por dia. Login por e-mail e Google não tem custo neste uso. Para um estúdio, sobra muito.
