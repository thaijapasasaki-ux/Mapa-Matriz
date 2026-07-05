# Matriz do Destino — Painel (App real, sem gastar nada pra testar)

Stack: **Next.js + Supabase (login/banco) + Resend (e-mail) + Vercel (hospedagem + cron)**.
Todos têm plano gratuito suficiente pro seu volume inicial.

## 1. Suba o código no GitHub
1. Crie uma conta em github.com (grátis).
2. Crie um repositório **privado** novo, ex: `matriz-do-destino-app`.
3. Suba esta pasta inteira pra esse repositório (pelo site do GitHub mesmo,
   arrastando os arquivos, ou usando `git` se preferir).

## 2. Crie o banco no Supabase (grátis)
1. Crie conta em supabase.com -> "New project".
2. Vá em **SQL Editor** -> New query -> cole o conteúdo de `supabase/schema.sql` -> Run.
3. Vá em **Authentication -> Users -> Add user** e crie o SEU usuário
   (e-mail + senha) — é com isso que você vai entrar no painel.
4. Vá em **Project Settings -> API** e copie:
   - Project URL -> `NEXT_PUBLIC_SUPABASE_URL`
   - anon public key -> `NEXT_PUBLIC_SUPABASE_ANON_KEY`
   - service_role key -> `SUPABASE_SERVICE_ROLE_KEY` (mantenha em segredo)

## 3. Crie a conta no Resend (grátis, pra enviar o e-mail do PDF)
1. Crie conta em resend.com.
2. Verifique um domínio de e-mail (ou use o de testes deles pra começar).
3. Copie a API key -> `RESEND_API_KEY`.

## 4. Suba no Vercel (grátis)
1. Crie conta em vercel.com com o mesmo GitHub.
2. "Add New -> Project" -> selecione o repositório que você criou.
3. Em **Environment Variables**, cole todas as variáveis do `.env.example`
   já preenchidas com os valores reais (Supabase, Resend, e invente os
   dois "secrets": `KIWIFY_WEBHOOK_SECRET` e `CRON_SECRET`).
4. Deploy. Em poucos minutos seu app está em `https://seu-projeto.vercel.app`.

O `vercel.json` já está configurado para rodar a rotina que libera o PDF
**todo dia às 12h** — ela mesma verifica quem já passou de 7 dias.

## 5. Conecte a Kiwify
No produto, em **Webhooks**, cadastre:
```
https://seu-projeto.vercel.app/api/webhook/kiwify?secret=SEU_KIWIFY_WEBHOOK_SECRET
```
(o mesmo valor que você colocou em `KIWIFY_WEBHOOK_SECRET` na Vercel)

⚠️ O nome exato dos campos que a Kiwify envia (order_id, e-mail do
comprador, etc.) pode variar um pouco — dentro do painel da Kiwify, em
Webhooks, normalmente tem um botão de "testar" que mostra o payload real.
Se algo não bater, me manda esse exemplo de payload que eu ajusto o código
em `app/api/webhook/kiwify/route.ts` pra combinar exatamente.

## 6. Use o painel
- Acesse `https://seu-projeto.vercel.app/login` e entre com o e-mail/senha
  que você criou no passo 2.
- Lá você vê todos os clientes, quem já pagou, quem já recebeu o PDF, e
  pode escrever/colar a leitura manual de cada um.

## O que falta pra ficar 100% (posso fazer com você depois)
- Página do cliente pra ele ver a própria leitura online (hoje só existe
  o painel seu de administração).
- Link real de download do PDF dentro do e-mail automático (hoje está
  como placeholder em `app/api/cron/release-pdf/route.ts`).
- Domínio próprio verificado no Resend pra e-mails não caírem em spam.

Tudo isso continua dentro dos planos gratuitos das mesmas ferramentas.
