# Projeto-Integrador-2-DegustaAi
chatbot sobre recomendacao gastronomica 
gustaAI – Workflow n8n  
Assistente gastronômico inteligente integrado ao WhatsApp via Evolution API.

## Visão Geral
*DegustaAI* é um fluxo avançado desenvolvido em *n8n* que recebe mensagens do WhatsApp via Evolution API, processa texto ou áudio, consulta um agente de IA altamente especializado (baseado em GPT-4.1-mini com prompt estruturado) e envia recomendações personalizadas de restaurantes em *Belém – PA*.

O fluxo inclui:
•⁠  ⁠Webhook de entrada (WhatsApp)
•⁠  ⁠Processamento e normalização dos dados
•⁠  ⁠Detecção do tipo de mensagem (texto / áudio)
•⁠  ⁠Transcrição de áudio usando OpenAI
•⁠  ⁠Memória conversacional persistente via Redis
•⁠  ⁠Agente de IA com prompt extenso (“DegustaAI”)
•⁠  ⁠Envio automático de resposta via Evolution API

---

## Estrutura do Workflow (Resumo Técnico)

### *1. Webhook*
Recebe eventos do Evolution API (mensagens enviadas pelo usuário).
•⁠  ⁠Tipo: ⁠ POST ⁠
•⁠  ⁠Entrada: event, instance, remoteJid, pushName, conversation, messageType, timestamps, etc.

### *2. Dados (Set node)*
Extrai e salva os campos relevantes do JSON enviado pelo Evolution:
•⁠  ⁠event  
•⁠  ⁠instance  
•⁠  ⁠remoteJid  
•⁠  ⁠pushName  
•⁠  ⁠conversation  
•⁠  ⁠messageType  
•⁠  ⁠timestamp  
•⁠  ⁠idMensagem  

### *3. IF – Filtragem de remetente*
Verifica se o número é ou não o número de teste:
•⁠  ⁠Se for o número específico → fluxo é interrompido  
•⁠  ⁠Caso contrário → segue para processamento  

### *4. Switch – Detecção do tipo de mensagem*
Identifica:
•⁠  ⁠⁠ conversation ⁠ → texto
•⁠  ⁠⁠ audioMessage ⁠ → áudio

### *5. Fluxo de Texto*
•⁠  ⁠Captura a mensagem textual
•⁠  ⁠Redireciona para o agente de IA

### *6. Fluxo de Áudio*
1.⁠ ⁠Baixa áudio em Base64 da Evolution API  
2.⁠ ⁠Converte para binário  
3.⁠ ⁠Envia para transcrição (OpenAI Whisper)  
4.⁠ ⁠Captura o texto transcrito  
5.⁠ ⁠Redireciona para o agente de IA  

### *7. AI Agent*
O cérebro do DegustaAI — nó que contém:
•⁠  ⁠Grande prompt de instruções
•⁠  ⁠Base do agente gastronômico de Belém
•⁠  ⁠Regras de segurança
•⁠  ⁠Processo de coleta de preferências
•⁠  ⁠Procedimentos passo a passo
•⁠  ⁠Exemplos de interação
•⁠  ⁠Ferramentas internas

### *8. Chat Model (OpenAI)*
Modelo operacional:
•⁠  ⁠⁠ gpt-4.1-mini ⁠  
•⁠  ⁠Usa credenciais OpenAI  

### *9. Memória via Redis*
Mantém contexto da conversa por *1 hora*, usando chave 
