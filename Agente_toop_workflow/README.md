# Agente Toop - Workflow de Atendimento

Sistema de atendimento automatizado para o Toop Chat utilizando n8n, inteligência artificial e integração com Google Calendar.

## 📋 Descrição

O Agente Toop é um assistente virtual que gerencia o atendimento de clientes do Toop Chat, realizando:

- ✅ Recepção e triagem de clientes
- 📅 Agendamento de reuniões via Google Calendar
- 🔍 Busca em base de conhecimento
- 🎯 Transferência para suporte quando necessário
- 💬 Gestão de conversas com memória contextual

## 🏗️ Arquitetura

### Workflow Principal
- **Agente_toop.json**: Workflow principal com IA Agent (GPT-4.1-mini)

### Subworkflows
- **busca_calendario.json**: Busca eventos no Google Calendar
- **cria_evento.json**: Cria novos eventos e envia notificações
- **data_toop.json**: Consulta base de conhecimento
- **transferir_suporte.json**: Transfere atendimento para equipe de suporte

## 🔧 Tecnologias

- **n8n**: Plataforma de automação de workflows
- **OpenAI GPT-4.1-mini**: Modelo de linguagem para IA conversacional
- **Google Calendar API**: Gerenciamento de agendamentos
- **PostgreSQL**: Armazenamento de histórico de conversas e vetores
- **Google Sheets**: Controle de fila de mensagens
- **Evolution API**: Integração com WhatsApp/Toop Chat

## ⚙️ Configuração

### Pré-requisitos

1. Instância do n8n configurada
2. Credenciais necessárias:
   - OpenAI API Key
   - Google Calendar API (Service Account)
   - Google Sheets API
   - PostgreSQL Database
   - Toop Chat API Token
   - Evolution API Token

### Instalação

1. Clone o repositório:
```bash
git clone https://github.com/aquiladmitruk/Agente_atendimento_rag.git
cd Agente_toop_workflow
```

2. **IMPORTANTE**: Copie os arquivos de exemplo e configure suas credenciais:
```bash
# Os arquivos .json contêm credenciais sensíveis e não devem ser commitados
# Use os arquivos .example.json como referência
```

3. Importe os workflows no n8n:
   - Importe primeiro os subworkflows da pasta `Subworkflows/`
   - Depois importe o workflow principal `Agente_toop.json`

4. Configure as credenciais no n8n:
   - **OpenAI API**: Adicione sua API key
   - **Google Calendar**: Configure Service Account
   - **PostgreSQL**: Configure conexão com banco
   - **Toop Chat**: Adicione Bearer Token
   - **Evolution API**: Configure API key

5. Ajuste os IDs dos workflows:
   - No workflow principal, atualize os IDs dos subworkflows referenciados

### Variáveis de Ambiente

Configure as seguintes credenciais no n8n:

| Credencial | Tipo | Descrição |
|------------|------|-----------|
| OpenAI API | API Key | Chave de acesso à API da OpenAI |
| Google Calendar | Service Account | Credenciais do Google Calendar |
| PostgreSQL | Database | Conexão com banco de dados |
| Toop Chat | Bearer Token | Token de autenticação Toop |
| Evolution API | API Key | Chave da Evolution API |

## 📊 Banco de Dados

### Tabelas PostgreSQL

- `evol7_core_n8n.n8n_chat_histories`: Histórico de conversas
- `evol7_core_desenvolvimento.memoria_toop`: Vetores para busca semântica

## 🚀 Uso

### Fluxo de Atendimento

1. **Recepção**: Cliente envia mensagem via webhook
2. **Triagem**: Sistema verifica se é cliente novo ou existente
3. **Processamento**: IA analisa mensagem e consulta base de conhecimento
4. **Ação**: 
   - Responde dúvidas
   - Agenda reuniões
   - Transfere para suporte
5. **Memória**: Conversa é armazenada para contexto futuro

### Horários de Agendamento

- **Manhã**: 09:00, 10:00, 11:00
- **Tarde**: 14:00, 15:00, 16:00, 17:00
- **Duração**: 1 hora por reunião
- **Fuso horário**: America/Sao_Paulo (Brasília)

## 🔒 Segurança

⚠️ **ATENÇÃO**: Este projeto contém dados sensíveis!

- Nunca commite arquivos `.json` com credenciais
- Use variáveis de ambiente para dados sensíveis
- Mantenha o `.gitignore` atualizado
- Revise o histórico do Git antes de tornar o repositório público

## 📝 Estrutura de Dados

### Formato de Mensagem (Webhook)
```json
{
  "body": {
    "type": "message_n8n",
    "contact": {
      "number": "5544988243253",
      "tags": { "id": 1 }
    },
    "content": {
      "text": "Mensagem do cliente",
      "messageId": "unique-id"
    }
  }
}
```

## 🤝 Contribuindo

1. Faça fork do projeto
2. Crie uma branch para sua feature (`git checkout -b feature/AmazingFeature`)
3. Commit suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. Push para a branch (`git push origin feature/AmazingFeature`)
5. Abra um Pull Request

## 📄 Licença

Este projeto é privado e proprietário.

## 👥 Autores

- **Evol7 Team** - Desenvolvimento e manutenção

## 📞 Suporte

Para suporte, entre em contato através do Toop Chat ou abra uma issue no repositório.

---

**Nota**: Este é um sistema em produção. Teste todas as alterações em ambiente de desenvolvimento antes de aplicar em produção.
