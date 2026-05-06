# 🍫 Calendário de Cursos — Maria Chocolate

> Sistema de agendamento de cursos presenciais para instrutores, com painel administrativo de aprovação, notificações por e-mail e armazenamento em nuvem.

---

## 📋 Visão Geral

Este projeto é uma aplicação web completa para gerenciar o agendamento de datas de cursos ministrados por instrutores parceiros da **Maria Chocolate**. O sistema permite que instrutores solicitem datas diretamente pelo calendário, enquanto a equipe administrativa revisa, aprova ou reprova cada solicitação — tudo com notificações automáticas por e-mail.

> ⚠️ Este calendário é exclusivo para **cursos presenciais**. Lives, aulas show e degustações continuam sendo agendadas pelo WhatsApp.

---

## ✨ Funcionalidades

### Portal do Instrutor (`instrutores.html`)
- Calendário visual com disponibilidade em tempo real
- Seleção de período: **manhã**, **tarde**, **dia inteiro** ou **2 dias consecutivos**
- Regras automáticas por dia da semana (segundas de manhã bloqueadas para lives)
- Prazo mínimo de **20 dias** para agendamento
- Limite de **5 cursos por instrutor por mês**
- Bloqueio visual de datas com indicação de status:
  - 🟠 Laranja — pendente de aprovação
  - 🟢 Verde — confirmado
  - 🔴 Vermelho — data bloqueada pelo admin
- Upload de **2 a 5 fotos** de divulgação com validação de resolução mínima (1080×1080 px)
- Descrição do curso com mínimo de **500 caracteres**
- Notificação automática por e-mail ao enviar solicitação

### Painel Administrativo (`admin.html`)
- Acesso protegido por **autenticação Firebase** (e-mail + senha)
- Listagem de solicitações com filtros por status, categoria e mês
- Aprovação e reprovação com e-mail automático para o instrutor
- Cancelamento de aprovações com liberação imediata da data
- Galeria de fotos com download individual ou em lote
- **Exportação de planilha CSV** com os cursos aprovados do mês
- Calendário visual idêntico ao do instrutor para visão geral do mês
- Bloqueio de datas específicas (feriados, eventos) — salvo no Firestore
- Liberação antecipada de datas dentro do prazo de 20 dias

---

## 🛠️ Tecnologias Utilizadas

| Tecnologia | Uso |
|---|---|
| **HTML / CSS / JavaScript** | Interface completa sem frameworks |
| **Firebase Firestore** | Banco de dados em tempo real |
| **Firebase Authentication** | Autenticação segura do painel admin |
| **Cloudinary** | Armazenamento e entrega de imagens |
| **EmailJS** | Envio de e-mails automáticos sem backend |
| **GitHub Pages** | Hospedagem estática gratuita |

---

## 📁 Estrutura do Projeto

```
calendario-cursos-maria-chocolate/
├── instrutores.html     Portal de agendamento para instrutores
├── admin.html           Painel administrativo
├── logo.png             Logomarca Maria Chocolate
└── README.md            Este arquivo
```

---

## ⚙️ Configuração e Deploy

### 1. Firebase (Firestore + Authentication)
1. Crie um projeto no Firebase Console
2. Ative o **Firestore Database** na região `southamerica-east1`
3. Crie as coleções: `agendamentos`, `datas_bloqueadas`, `datas_liberadas`
4. Ative o **Authentication → E-mail/senha** e cadastre o usuário admin
5. Configure as **regras de segurança do Firestore**:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /agendamentos/{id} {
      allow create: if true;
      allow read: if true;
      allow update, delete: if request.auth != null;
    }
    match /datas_bloqueadas/{id} {
      allow read: if true;
      allow write: if request.auth != null;
    }
    match /datas_liberadas/{id} {
      allow read: if true;
      allow write: if request.auth != null;
    }
  }
}
```

6. Registre um app web e copie as credenciais para os arquivos HTML

### 2. Cloudinary
1. Crie uma conta no Cloudinary
2. Em **Settings → Upload**, crie um upload preset:
   - **Nome:** `maria_chocolate_unsigned`
   - **Signing mode:** `Unsigned`
   - **Pasta:** `maria-chocolate`
3. Restrinja a chave de API ao domínio do GitHub Pages
4. Atualize `CLOUDINARY_CLOUD` em `instrutores.html`

### 3. EmailJS
1. Crie uma conta no EmailJS e conecte seu Gmail
2. Crie 2 templates: notificação para admin e resultado para instrutor
3. Atualize `service_id`, `template_id` e `public_key` nos arquivos HTML

### 4. GitHub Pages
1. Suba os arquivos `instrutores.html`, `admin.html` e `logo.png` no repositório
2. Vá em **Settings → Pages**, selecione branch `main` e pasta `/ (root)`
3. Acesse via `https://usuario.github.io/calendario-cursos-maria-chocolate/`

---

## 🔐 Segurança

- Painel admin protegido por **Firebase Authentication** (e-mail + senha)
- Regras do Firestore garantem que apenas usuários autenticados modificam dados
- Upload preset do Cloudinary é `unsigned` com pasta restrita
- Restrinja a `apiKey` do Firebase ao domínio do GitHub Pages no Google Cloud Console
- Nunca compartilhe as credenciais do Firebase, Cloudinary ou EmailJS publicamente

---

## 📅 Regras de Agendamento

| Período | Horário | Dias disponíveis |
|---|---|---|
| Somente manhã | 09h às 12h30 | Terça a Sábado |
| Somente tarde | 13h30 às 17h | Segunda a Sexta |
| Dia inteiro | 09h às 17h | Segunda a Sexta |
| 2 dias consecutivos | 09h às 17h | 1º dia: Segunda a Quinta |

> ⚠️ Segundas-feiras de manhã sempre bloqueadas (lives) · Prazo mínimo: 20 dias · Limite: 5 cursos/mês por instrutor · É possível agendar manhã e tarde no mesmo dia em solicitações separadas · Cursos de fornecedores: 2h30 de duração

---

## 📬 Fluxo de Notificações por E-mail

```
Instrutor envia solicitação
        ↓
Admin recebe e-mail com os dados do curso
        ↓
Admin aprova ou reprova no painel (prazo: 5 dias úteis)
        ↓
Instrutor recebe e-mail com o resultado
```

> Se aprovado, o instrutor deve chegar com **1 hora de antecedência** para organizar a sala.

---

## 🗂️ Categorias · 📸 Fotos de Divulgação

**Categorias:** 🍫 Chocolateria · 🎂 Confeitaria · 🥐 Salgados · 🏪 Fornecedor

**Fotos:** Formato JPG ou PNG · Resolução mínima 1080×1080 px · Mínimo 2, máximo 5 fotos por solicitação

---

## ⚠️ Limitações dos Planos Gratuitos

| Plataforma | Limite | Impacto |
|---|---|---|
| **🔥 Firebase Firestore** | 1 GB · 50k leituras/dia · 20k escritas/dia | Suficiente para centenas de agendamentos |
| **🖼️ Cloudinary** | ~10 GB · 25 créditos mensais | 12 instrutores × 2 cursos ≈ 240 MB/mês |
| **📧 EmailJS** | 200 e-mails/mês · 2 templates | ~48 e-mails/mês estimados |
| **🐙 GitHub Pages** | 1 GB repositório · 100 GB banda/mês | Sem impacto |

---

## 👩‍💼 Sobre

Desenvolvido por **Heberth Augusto** para **Maria Chocolate** utilizando a **Claude IA**.

*Ensinar, criar, celebrar.*
