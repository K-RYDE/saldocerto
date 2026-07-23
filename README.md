# SaldoCerto — App de Controle Financeiro (PWA)

App completo de controle financeiro pessoal, feito pra rodar como um app de
celular (PWA — Progressive Web App), sem precisar de loja de aplicativos.
Suporta vários usuários, cada um com login próprio.

## O que o app faz

- **Cadastro multi-usuário** (e-mail e senha) — cada pessoa só vê os próprios dados
- **Renda mensal** informada no cadastro, usada para personalizar os alertas
- **Cadastro de receitas e despesas** por categoria e data
- **Alertas inteligentes**: compara o gasto do usuário em cada categoria
  (Alimentação, Moradia, Transporte, Saúde, Educação, Lazer) com a média
  esperada para a **faixa de renda dele**, com base na Pesquisa de
  Orçamentos Familiares (POF) do IBGE
- **Instalável no celular** — abre a tela de "Adicionar à tela inicial" como
  se fosse um app normal
- **Painel de administrador** (opcional) — números agregados da plataforma
  (usuários, lançamentos, volume), sem acesso a dados individuais
- Visual mobile-first, com barra de navegação inferior

---

## 1. Testando agora (rápido, sem instalar nada)

Dê um duplo clique no arquivo `index.html` — ele abre no navegador. Sem
configurar o Firebase (próximo passo), você vai ver um aviso amarelo e não
vai conseguir logar, mas dá pra ver todo o visual.

## 2. Configurando o Firebase (obrigatório, gratuito)

1. Acesse [console.firebase.google.com](https://console.firebase.google.com), crie um projeto.
2. **Build → Authentication → Sign-in method** → ative **E-mail/senha**.
3. **Build → Firestore Database** → criar banco → modo produção → região
   `southamerica-east1` (Brasil).
4. Na aba **Regras** do Firestore, cole o conteúdo de `firestore.rules`
   (arquivo incluso aqui), substituindo o texto padrão.
5. Configurações do projeto (ícone de engrenagem) → **Seus apps** → `</>`
   (Web) → registrar app → copie o bloco `firebaseConfig`.
6. Abra `index.html` em um editor de texto, procure por `firebaseConfig`
   (perto do fim do arquivo) e cole os valores no lugar de `"COLE_AQUI"`.
7. Salve e abra o arquivo de novo no navegador. Crie sua conta normalmente
   pela tela de cadastro (nome, e-mail, senha e renda).

> Você mesmo pode ser o primeiro usuário — não existe mais um admin fixo;
> qualquer pessoa pode se cadastrar com o próprio e-mail.

## 2.1 Virando administrador (opcional)

O painel de admin mostra só números agregados (total de usuários, total de
lançamentos, volume movimentado) — nunca o extrato individual de ninguém.

1. Crie sua conta normal pelo app (cadastro com e-mail/senha).
2. No Console do Firebase, vá em **Build → Authentication → Users** e copie
   o **User UID** da sua conta.
3. Vá em **Build → Firestore Database → Dados** → clique em **"Iniciar
   coleção"** → nome da coleção: `admins`.
4. No **ID do documento**, cole o UID que você copiou (deixe os campos em
   branco ou adicione um campo qualquer, ex: `desde: hoje`) → Salvar.
5. Saia e entre de novo no app — vai aparecer um selinho "⭐ admin" no topo e
   uma aba nova "Admin" na barra inferior.

Pra promover outra pessoa a admin no futuro, repita o passo 3-4 com o UID
dela.

## 3. Testando no celular como app (PWA)

Pra "Adicionar à tela inicial" funcionar de verdade, o navegador do celular
precisa acessar o app via **https** (não funciona direto do arquivo local).
Formas rápidas e gratuitas de testar:

- **Netlify Drop**: acesse [app.netlify.com/drop](https://app.netlify.com/drop)
  e arraste a pasta inteira do projeto. Em segundos ele te dá um link
  `https://...netlify.app` — abra esse link no celular, e o navegador vai
  sugerir "Adicionar à tela inicial".
- Depois de subir pro GitHub (próximo passo), qualquer serviço de hospedagem
  gratuita (Vercel, Firebase Hosting, GitHub Pages) também funciona.

## 4. Subindo para o GitHub

```bash
git init
git add .
git commit -m "SaldoCerto - app completo com alertas por renda"
git branch -M main
git remote add origin https://github.com/SEU-USUARIO/saldocerto.git
git push -u origin main
```

## 5. Colocando no ar (deploy definitivo)

**Vercel** (mais simples): crie conta em [vercel.com](https://vercel.com) com
GitHub, importe o repositório, e clique em Deploy — não precisa de nenhuma
configuração especial, pois é um site estático.

---

## Sobre a base dos alertas (IBGE/POF)

A tabela de referência usada no app (`FAIXAS_IBGE`, dentro do `index.html`)
foi construída a partir de dados públicos da Pesquisa de Orçamentos
Familiares do IBGE, que mostra o quanto famílias em diferentes faixas de
renda costumam gastar, proporcionalmente, em cada categoria. São médias
nacionais — úteis como referência geral, mas não substituem orientação
financeira profissional personalizada.

## Estrutura do projeto

```
index.html         app inteiro (telas, estilo e lógica)
manifest.json       metadados do PWA (nome, ícone, cor)
sw.js                service worker, permite instalar o app
icon-192.png         ícone do app (tela inicial)
icon-512.png         ícone do app (versão maior)
firestore.rules      regras de segurança do banco de dados
```
