# 📇 CRM Automatizado — Custo R$ 0

Automação criada para corretores de imóveis (ou qualquer profissional com carteira de clientes) que precisam manter relacionamento ativo com centenas de contatos sem custo e sem esforço manual.

Construído com ferramentas 100% gratuitas e self-hosted.

---

## 💡 O problema

Manter contato regular com uma carteira grande de clientes é inviável manualmente. Contatos antigos esfriam, oportunidades escapam. As ferramentas de CRM prontas no mercado cobram mensalidades altas.

## ✅ A solução

Um sistema que roda sozinho todo dia, enviando mensagens personalizadas no WhatsApp para 30 contatos, segmentados por perfil, sem repetir quem já foi contactado.

---

## ⚙️ Como funciona

O projeto é composto por dois fluxos no n8n:

### Fluxo 1 — `alimentar_planilha_contato.json`
Importa contatos do Gmail e alimenta automaticamente a planilha no Google Sheets, evitando duplicatas.

### Fluxo 2 — `ver_planilha.json`
Todo dia:
1. Lê a planilha e separa os contatos por etiqueta: **Novo Cliente**, **Antigo**, **Importante**, **Ficar de Olho**, **Pegar Opção**, **Comprou**, **Vendeu**
2. Sorteia até 30 contatos priorizando os perfis mais relevantes
3. Para cada contato, gera uma mensagem personalizada de acordo com a etiqueta
4. Envia a mensagem no WhatsApp com intervalo aleatório entre envios (para parecer natural)
5. Marca o contato como "enviado" na planilha — ele não será sorteado novamente

---

## 🛠️ Stack (100% gratuita)

| Ferramenta | Função | Custo |
|---|---|---|
| [n8n](https://n8n.io) (self-hosted) | Motor de automação | Grátis |
| Google Sheets | Banco de dados dos contatos | Grátis |
| Gmail API | Importação de contatos | Grátis |
| [WAHA](https://waha.devlike.pro) (Docker) | Envio de mensagens WhatsApp | Grátis |

> **Por que WAHA e não a API oficial do WhatsApp?**
> A API oficial do WhatsApp Business cobra por mensagem enviada. O WAHA roda localmente via Docker e não tem custo de uso.

---

## 🚀 Como usar

### Pré-requisitos
- [n8n self-hosted](https://docs.n8n.io/hosting/) instalado
- [Docker](https://www.docker.com/) instalado
- [WAHA](https://waha.devlike.pro/docs/overview/quick-start/) rodando no Docker
- Conta Google com acesso ao Google Sheets e Gmail

### Passo a passo

1. **Clone o repositório**
```bash
git clone https://github.com/luizozoriofilho/n8n-whatsapp-crm
```

2. **Importe os fluxos no n8n**
   - Acesse seu n8n → Menu → Import workflow
   - Importe `alimentar_planilha_contato.json`
   - Importe `ver_planilha.json`

3. **Configure as credenciais no n8n**
   - Google Sheets: conecte sua conta Google
   - Gmail: conecte sua conta Google
   - WAHA: aponte para `http://localhost:3000` (ou o endereço do seu Docker)

4. **Configure a planilha**
   - Crie uma planilha no Google Sheets com as colunas:
     `nome_whats | telefone | etiqueta | enviado`
   - Etiquetas disponíveis: `NOVO CLIENTE`, `ANTIGO`, `IMPORTANTE`, `FICAR DE OLHO`, `PEGAR OPÇÃO`
   - Aponte o ID da planilha nos dois fluxos

5. **Ative os fluxos**
   - `alimentar_planilha_contato.json` — roda automaticamente conforme agendamento
   - `ver_planilha.json` — roda uma vez por dia no horário configurado

---

## 📁 Estrutura do repositório

```
📦 crm-automatizado
 ┣ 📄 alimentar_planilha_contato.json   # Fluxo de importação de contatos
 ┣ 📄 ver_planilha.json                 # Fluxo de envio diário
 ┗ 📄 README.md
```

---

## ⚠️ Atenção antes de usar

- Respeite os limites de mensagens do WhatsApp para evitar bloqueio de número
- O projeto envia no máximo 30 mensagens por dia por design — não aumente esse limite sem cuidado
- Não use para spam — o sistema foi criado para relacionamento genuíno com sua carteira de clientes

---

## 🤝 Contribuições

Sugestões e melhorias são bem-vindas. Abra uma issue ou pull request.

---

## 📬 Contato

Feito por **Luiz Eduardo Ozório** — Corretor de Imóveis & Analista de Dados

[LinkedIn](https://www.linkedin.com/in/luizeduardoozorio/) · [GitHub](https://github.com/luizozoriofilho)
