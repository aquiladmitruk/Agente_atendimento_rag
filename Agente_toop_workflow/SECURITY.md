# 🔒 Guia de Segurança - Agente Toop

## ⚠️ IMPORTANTE: Credenciais Removidas

Os arquivos JSON originais com credenciais foram **removidos do repositório** por questões de segurança.

## 📋 Credenciais Necessárias

Para configurar o projeto, você precisará das seguintes credenciais:

### 1. OpenAI API
- **Tipo**: API Key
- **Onde configurar**: n8n → Credentials → OpenAI API
- **Nome da credencial no workflow**: `OpenAi account`
- **ID no workflow**: `9NMzkbYbg0eVa5HZ`

### 2. Google Calendar API
- **Tipo**: Service Account (OAuth2)
- **Onde configurar**: n8n → Credentials → Google Calendar
- **Nome da credencial no workflow**: `Google Calendar`
- **ID no workflow**: `YTjmE3wr5brPyXI7`
- **Email da conta**: `renato.chacon@evol7.com.br`

### 3. Google Sheets API
- **Tipo**: Service Account (OAuth2)
- **Onde configurar**: n8n → Credentials → Google Sheets
- **Nome da credencial no workflow**: `Google Sheets account`
- **ID no workflow**: `X6l1fLEmSpjGnhuZ`
- **Planilhas usadas**:
  - `fila_de_mensagens` (ID: 1LWTEJm_EAiIsNoK_Q8rHw8BfO4vU5EgH7C488z2M0dA)
  - `reunioes_agendadas` (ID: 1zx-XrAsGLotWQ46wweebq-dd5D6ffCPIgziLUgeVl9w)

### 4. PostgreSQL Database
- **Tipo**: Database Connection
- **Onde configurar**: n8n → Credentials → PostgreSQL
- **Nome da credencial no workflow**: `Postgres account`
- **ID no workflow**: `8xnZYRxBdrW6INBo`
- **Schemas usados**:
  - `evol7_core_n8n.n8n_chat_histories`
  - `evol7_core_desenvolvimento.memoria_toop`

### 5. Toop Chat API
- **Tipo**: Bearer Token (HTTP Bearer Auth)
- **Onde configurar**: n8n → Credentials → HTTP Bearer Auth
- **Nome da credencial no workflow**: `Toop`
- **ID no workflow**: `Fh1EeyH6sWNFjd0L`
- **Base URL**: `https://api.toop.one/v2/api/external/`
- **Tenant ID**: `01f97b45-463b-49ad-a9c1-1937751b81e1`

### 6. Evolution API
- **Tipo**: API Key (Header)
- **Onde configurar**: n8n → HTTP Request → Headers
- **Base URL**: `https://evolution-api.evol7.digital`
- **Instance**: `evolzpro3`

## 🔧 Como Configurar

### Passo 1: Configurar Credenciais no n8n

1. Acesse seu n8n
2. Vá em **Settings → Credentials**
3. Adicione cada credencial listada acima
4. **Anote os IDs** gerados pelo n8n

### Passo 2: Atualizar IDs nos Workflows

Após importar os workflows no n8n, você precisará atualizar os IDs das credenciais:

1. Abra cada workflow no n8n
2. Para cada nó que usa credenciais:
   - Clique no nó
   - Selecione a credencial correta
   - Salve o workflow

### Passo 3: Configurar Webhooks

O webhook principal está configurado em:
- **Path**: `/response/02`
- **Webhook ID**: `c94081d9-06d1-4a10-bd72-a7a70a101d69`

Atualize este webhook conforme sua instância do n8n.

### Passo 4: Configurar Subworkflows

Os subworkflows precisam ser importados primeiro e seus IDs atualizados no workflow principal:

| Subworkflow | ID Original | Onde Atualizar |
|-------------|-------------|----------------|
| data_toop | 2N4DnSUK5Nr7Pbkl | Nó "data_toop" |
| busca_calendario | sBY6KBx9cr3Crsgi | Nó "busca_evento" |
| cria_evento | e0WMZ3z8Lz5mqWZC | Nó "cria_evento" |
| transferir_suporte | (ID necessário) | Nó "transferir_suporte" |

## 🗄️ Estrutura do Banco de Dados

### Tabela: n8n_chat_histories
```sql
CREATE TABLE evol7_core_n8n.n8n_chat_histories (
    id SERIAL PRIMARY KEY,
    session_id VARCHAR(255),
    message JSONB,
    created_at TIMESTAMP DEFAULT NOW()
);
```

### Tabela: memoria_toop (PGVector)
```sql
CREATE TABLE evol7_core_desenvolvimento.memoria_toop (
    id SERIAL PRIMARY KEY,
    content TEXT,
    embedding vector(1536),
    metadata JSONB
);
```

## 🚨 Boas Práticas de Segurança

### ✅ O que FAZER:
- Manter credenciais apenas no n8n
- Usar variáveis de ambiente quando possível
- Rotacionar tokens periodicamente
- Limitar permissões de API ao mínimo necessário
- Fazer backup das credenciais em local seguro (ex: 1Password, Vault)

### ❌ O que NÃO fazer:
- Nunca commitar arquivos `.json` com credenciais
- Nunca compartilhar tokens em chat/email
- Nunca expor webhooks publicamente sem autenticação
- Nunca usar credenciais de produção em desenvolvimento

## 🔄 Rotação de Credenciais

Se você suspeitar que alguma credencial foi exposta:

1. **Imediatamente**:
   - Revogue a credencial comprometida
   - Gere uma nova credencial
   - Atualize no n8n

2. **OpenAI**: https://platform.openai.com/api-keys
3. **Google APIs**: https://console.cloud.google.com/apis/credentials
4. **Toop Chat**: Contate o suporte
5. **Evolution API**: Regenere a API key

## 📞 Suporte

Em caso de dúvidas sobre segurança:
- Abra uma issue **privada** no repositório
- Contate a equipe de DevOps da Evol7

---

**Última atualização**: 2026-05-22
