# W01 Principal — Automação NOC com Zabbix, n8n, GLPI e Microsoft Teams

> Projeto de automação para tratamento inteligente de alertas de monitoramento, correlação com tickets e notificação operacional.

Este repositório apresenta uma visão arquitetural e funcional do workflow **W01 Principal**, desenvolvido em **n8n** para integrar **Zabbix**, **GLPI** e **Microsoft Teams**.

Por questões de segurança e propriedade intelectual, o workflow completo não é publicado neste repositório.

---

## Visão geral

O W01 Principal automatiza parte do processo operacional do NOC, reduzindo atividades manuais no tratamento de alertas e evitando abertura de chamados duplicados.

A automação recebe eventos do Zabbix, valida o payload, classifica o alerta, consulta histórico, verifica chamados existentes no GLPI e decide automaticamente o próximo passo.

---

## Arquitetura visual

![Visão geral do workflow](assets/screenshots/w01-workflow-overview.png)

---

## Stack utilizada

- Zabbix
- n8n
- GLPI
- Microsoft Teams
- JavaScript em nós do n8n

---

## Fluxo macro

Zabbix → Webhook n8n → Validação do Payload → Classificação do Alerta → Consulta Histórico Zabbix → Análise de Recorrência → Consulta GLPI → Decisão → GLPI / Teams / ACK Zabbix

---

## Decisões automatizadas

O workflow trabalha com quatro caminhos principais:

### 1. Abrir chamado

Quando não existe ticket aberto relacionado ao alerta.

Resultado esperado:

- Criação automática no GLPI
- Notificação no Microsoft Teams
- Acknowledge no Zabbix

### 2. Atualizar chamado existente

Quando já existe ticket aberto para o mesmo host e trigger.

Resultado esperado:

- Não abre chamado duplicado
- Registra acompanhamento no ticket existente
- Faz acknowledge no Zabbix

### 3. Validação humana

Quando existe chamado aberto para o host, mas a correlação não é exata.

Resultado esperado:

- Envia card interativo no Teams
- Analista decide se abre novo ticket, atualiza existente ou ignora
- Fluxo continua conforme decisão

### 4. Aguardar

Quando o evento ainda está em etapa inicial de escalonamento.

Resultado esperado:

- Evita abertura prematura de chamado
- Registra acknowledge informativo no Zabbix

---

## Capacidades técnicas

- Webhook autenticado
- Validação de payload
- Classificação de eventos
- Consulta de histórico no Zabbix
- Detecção de recorrência/flapping
- Deduplicação antes da abertura de tickets
- Correlação entre alerta e chamado
- Notificação via Teams
- Acknowledge automático no Zabbix
- Tratamento de falhas em pontos críticos
- Estratégia anti-colisão para alertas simultâneos

---

## Tipos de eventos tratados

Exemplos de classificações usadas na automação:

- HOST_DOWN
- VPN_DOWN
- LINK_DOWN
- CPU_ALERT
- MEMORY_ALERT
- SNMP_ALERT
- GENERIC_ALERT

---

## Benefícios esperados

- Redução de abertura manual de chamados
- Menos chamados duplicados
- Mais rastreabilidade entre alerta e ticket
- Menor tempo de resposta operacional
- Padronização no tratamento de incidentes
- Melhor visibilidade para o time NOC
- Base pronta para evolução em SRE e observabilidade

---

## Segurança

Este repositório não contém:

- URLs internas
- Tokens
- Credenciais
- IDs reais de workflows
- Payloads reais de produção
- Export completo do workflow n8n
- Regras sensíveis em nível operacional

---

## Demonstração

A solução pode ser demonstrada por meio de:

1. Print da arquitetura do workflow
2. Simulação de alerta do Zabbix
3. Criação automática de ticket
4. Notificação no Teams
5. Acknowledge no Zabbix
6. Cenário de deduplicação
7. Cenário de validação humana

---

## Status

Projeto em evolução contínua.

Atualmente o workflow está sendo refinado com foco em:

- Modularização
- Observabilidade do próprio workflow
- Tratamento centralizado de erros
- Melhorias de segurança
- Separação de responsabilidades por domínio

---

## Autor

Desenvolvido por **Welbert Simões dos Santos**.

Projeto voltado para automação de NOC, monitoramento, observabilidade e gestão de tickets.
