# 🎯 DESAFIO TÉCNICO SALESFORCE
## Implementação de Funil Comercial B2B - Lead to Opportunity

---

## 📋 CONTEXTO DA EMPRESA

### Empresa Fictícia: **TechFlow Solutions**

**Segmento:** Empresa SaaS de transformação digital especializada em automação de processos comerciais

**Localização:** São Paulo, Brasil (com expansão para mercado latino-americano)

**Tamanho:** 150+ colaboradores | 50+ clientes ativos | Faturamento: R$ 15M anuais

**Modelo de Negócio:** 
- Venda de licenças SaaS (anual e mensal)
- Serviços de implementação e customização
- Suporte técnico dedicado

---

## 👥 EQUIPES E INTERLOCUTORES

### **Diretoria Comercial**
- **Gerente Comercial (Carla Mendes)**: Define metas, aprova desconto e prioridades
- **Gerente de Operações de Vendas (Roberto Silva)**: Supervisiona processos, garante compliance

### **Time de Vendas**
- **AE (Account Executive) - Segmento Enterprise (Marcelo)**: Vendedor sênior, closes > R$ 500K
- **AE - Segmento Mid-Market (Juliana)**: Vendedor mid-level, closes R$ 100K - 500K
- **Sales Development Rep (João)**: Prospecção, qualificação de leads
- **Sales Development Rep (Fernanda)**: Prospecção, especializada em inbound

### **Operações e Suporte**
- **Specialist em Dados (Ana Paula)**: Responsável por integridade e qualidade de dados
- **DevOps/Admin Salesforce (Marcus)**: Mantém plataforma, configura ferramentas

### **Marketing e Geração de Leads**
- **Gerente de Marketing (Patricia)**: Define estratégia de geração de leads
- **Especialista em Marketing Digital (Felipe)**: Manages campanhas, web-to-lead

---

## 🎭 ESTÁGIOS DO FUNIL COMERCIAL

### **1. LEAD (Prospecto)**
- **Origem:** Web-to-Lead, eventos, referências, cold outreach
- **Status:** Raw Lead → Qualified Lead
- **Responsável:** SDR (João ou Fernanda)
- **Tempo esperado:** 2-5 dias
- **Meta:** Score mínimo 50 pontos para passar para oportunidade

### **2. QUALIFIED LEAD**
- **Critério:** Empresa com mínimo 10 funcionários, orçamento > R$ 50K
- **Validação:** CNPJ verificado, dados da empresa confirmados
- **Responsável:** SDR ou AE
- **Ação:** Conversão para Account + Contact + Opportunity

### **3. OPPORTUNITY (Oportunidade)**
- **Etapas:** 
  - Prospect (novo lead qualificado)
  - Qualification (reunião de descoberta realizada)
  - Needs Analysis (análise de necessidades)
  - Value Proposition (proposta em discussão)
  - Decision (em processo de aprovação)
  - Negotiation (negociação de termos)
  - Closed Won / Closed Lost
- **Valor mínimo:** R$ 50.000
- **Valor máximo:** R$ 5.000.000
- **Probabilidade associada:** Define forecast comercial

### **4. CUSTOMER (Cliente)**
- **Gerado via:** Conversão de Opportunity (Closed Won)
- **Status:** Active ou At Risk
- **Responsável:** Customer Success Manager

---

## ⚙️ DESAFIOS COMERCIAIS REAIS

### **DESAFIO 1: Validação de Dados via Integração CNPJ**
**Cenário:** 
- SDR recebe lead de "Consultoria ABC Ltda" sem documento fiscal
- Sistema deve buscar na Receita Federal os dados reais: razão social correta, endereço, regime tributário
- Se não encontrar, marcar como "Dados não verificados"
- Se encontrar divergências, alertar para revisão

**Objetivo:** 
- Garantir integridade de dados desde o início
- Evitar duplicatas de empresa com nomes diferentes
- Preparar base para integrações futuras (Service Cloud, Data Cloud)

**Pontos de Avaliação:**
- Como estruturar a chamada à API da Receita Federal?
- Tratamento de erros e timeouts
- Validação de respostas
- Atualização automática de campos na conta

---

### **DESAFIO 2: Scoring Automático de Leads**
**Cenário:**
- Lead chega via web-to-lead com: email, empresa, telefone, tamanho de empresa
- Sistema deve calcular automaticamente um score com base em:
  - ✅ Email corporativo (10 pts)
  - ✅ Empresa verificada via CNPJ (20 pts)
  - ✅ Tamanho de empresa > 10 func. (15 pts)
  - ✅ Segmento industrial alvo (25 pts)
  - ✅ Orçamento informado (20 pts)
  - ✅ Resposta a formulário específico (10 pts)

**Score mínimo para qualificação:** 50 pontos

**Objetivo:**
- Priorizar leads com maior probabilidade de conversão
- Distribuir leads apenas aos SDRs quando qualificados
- Criar fila de trabalho eficiente

**Pontos de Avaliação:**
- Recalculagem ao atualizar lead
- Visualização clara do score e justificativa
- Performance com volume alto (1000+ leads/dia)

---

### **DESAFIO 3: Prevenção de Duplicatas**
**Cenário:**
- "Empresa XYZ" chega como lead via formulário web
- Mesma empresa já existe como Account com 2 contatos
- Sistema deve detectar e oferecer opção de merge
- Se não fizer merge, lead fica em quarentena até revisão manual

**Objetivo:**
- Manter base limpa e única
- Evitar múltiplas oportunidades da mesma empresa
- Facilitar view 360° do cliente

**Pontos de Avaliação:**
- Uso de matching rules e duplicate rules
- Validação de CNPJ como campo único
- Fluxo de revisão para dúvidas
- Auditoria de mudanças

---

### **DESAFIO 4: Distribuição Automática de Leads**
**Cenário:**
- Lead qualificado deve ser atribuído ao AE correto:
  - Enterprise (>1000 func.) → Marcelo
  - Mid-Market (50-1000 func.) → Juliana
  - SMB (<50 func.) → Fila compartilhada (João + Fernanda)
- Considerar carga de trabalho atual (oportunidades abertas por AE)
- Considerar especialidade (vertical: financeiro, manufatura, etc)

**Objetivo:**
- Maximizar conversão atribuindo ao especialista correto
- Balancear carga de trabalho
- Reduzir tempo manual de distribuição

**Pontos de Avaliação:**
- Lógica de roteamento automático
- Cálculo de carga por vendedor
- Configuração de regras de atribuição
- Auditoria de distribuição

---

### **DESAFIO 5: Validação de Integridade de Dados**
**Cenário:**
- Lead criado SEM telefone e empresa pequena (8 funcionários)
- Opportunity criada SEM valor, SEM data de close estimada
- Sistema deve marcar como "Incompleta" e bloquear certas ações
- Gerar relatório de qualidade de dados diário

**Objetivo:**
- Garantir base limpa antes de integrações futuras
- Evitar dados "lixo" nas integrações com Service/Marketing/Data Cloud
- Identificar treinamento necessário para o time

**Pontos de Avaliação:**
- Validação de campos obrigatórios por etapa
- Alertas e bloqueios claros
- Relatórios de qualidade (dashboard)
- Permissões para ignorar validações (com auditoria)

---

### **DESAFIO 6: Path (Caminho Guiado) de Vendas**
**Cenário:**
- AE abre opportunity "Qualification" e vê Path visual
- Path guia: "Agende reunião → Documente necessidades → Prepare proposta → Obtenha assinatura"
- Cada etapa tem checkbox, campo obrigatório, próxima ação sugerida
- Ao completar etapa, automático avança para próxima
- Histórico de progresso visível
- Impossibilitar mudanças de estágios de forma manual

**Objetivo:**
- Consistência no processo de vendas
- Reduzir ciclo de vendas
- Facilitar onboarding de novos vendedores
- Melhorar forecast (dados completos)

**Pontos de Avaliação:**
- Configuração do Path com lógica clara
- Automação de transição de etapas
- Campos dinâmicos por etapa
- Validação antes de avançar

---

### **DESAFIO 7: Sharing Settings e Visibilidade**
**Cenário:**
- SDR cria Lead e o converte para Opportunity
- Lead deve ser visível para: criador + AE atribuído + Gerente
- Opportunity deve ser visível para: criador + AE + Gerente + CSM (quando Closed Won)
- Outras empresas NÃO devem ver dados de concorrentes
- Relatórios devem respeitar hierarquia de visibilidade

**Objetivo:**
- Segurança de dados comerciais
- Evitar acesso não autorizado
- Preparar para múltiplas regiões/filiais futuras

**Pontos de Avaliação:**
- Configuração de Sharing Settings
- Profiles e Permission Sets corretos
- Testes de visibilidade
- Auditoria de acesso (Field Audit Trail)

---

### **DESAFIO 8: Integração Web-to-Lead Customizada**
**Cenário:**
- Marketing quer novo formulário web com campos customizados:
  - Orçamento (picklist: <50K, 50-100K, >100K)
  - Vertical (picklist: Financeiro, Manufatura, Saúde, Outro)
  - Tem projeto iniciado? (SIM/NÃO)
  - Prioridade de implementação (Alta, Média, Baixa)
- Formulário deve usar branded landing page da empresa
- Respostas devem criar Lead + registrar valores customizados
- Após 24h sem resposta, enviar email automático

**Objetivo:**
- Capturar informações de qualidade desde o início
- Alimentar scoring e distribuição
- Reduzir follow-up manual

**Pontos de Avaliação:**
- Customização de campos Web-to-Lead
- Validação de dados do formulário
- Tratamento de respostas duplicadas
- Integração com email marketing

---

### **DESAFIO 9: Named Credentials e Segurança**
**Cenário:**
- Integração com API Receita Federal requer autenticação (OAuth 2.0)
- Credenciais NÃO devem estar em código
- Múltiplos ambientes: dev, staging, production
- Diferentes permissões por ambiente

**Objetivo:**
- Implementar segurança de credenciais
- Facilitar deployment entre ambientes
- Evitar exposição acidental de secrets

**Pontos de Avaliação:**
- Uso correto de Named Credentials vs External Credentials
- Configuração de OAuth 2.0
- Refresh tokens
- Tratamento de expiração

---

### **DESAFIO 10: Relatórios e Insights Comerciais**
**Cenário:**
- Diretoria precisa de:
  - Leads por origem (web, evento, referência) - últimos 30 dias
  - Taxa de conversão Lead → Opportunity por AE
  - Pipeline atual por etapa
  - Forecast de receita para próximos 90 dias
  - Leads "em risco" (sem contato há 7 dias)
- Relatórios devem ser atualizados em tempo real
- Exportável para PowerBI ou Dashboard executivo

**Objetivo:**
- Visibilidade do pipeline comercial
- Suportar decisões data-driven
- Facilitar acompanhamento de metas

**Pontos de Avaliação:**
- Criação de relatórios customizados
- Uso de SOQL complexos (agregação, grouping)
- Dashboard interativo
- Performance com volume de dados
