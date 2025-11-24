# 🤖 Scrum Master AI - Planejamento e Refinamento de Sprints

## 🎯 Visão Geral do Projeto

O **Scrum Master AI** é um agente de IA especialista, projetado no Azure AI Foundry (ou Azure Functions), que atua como um facilitador estruturado para o **Planejamento e Refinamento de Sprints**. Seu foco é eliminar a subjetividade da estimativa e fornecer dados de capacidade tangíveis.

### Problema Resolvido

* ❌ **Estimativa Subjetiva:** Conversão manual e inconsistente entre dias e Story Points (SP).
* ❌ **Falta de Capacidade:** Ausência de um cálculo rápido e líquido da capacidade da equipe por Sprint.

### Solução

✨ **Automação Completa:** Conversão automática de estimativas (dias ou T-Shirt Size) para a escala Fibonacci.  
✨ **Cálculo Estruturado:** Uso da função **`calculate_sprint_capacity`** para aplicar a regra de 80% de produtividade.  
✨ **Relatório Compilado:** Geração de um resumo final com **três métricas essenciais** (Dias, T-Shirt, SP), pronto para o compromisso da equipe.   

## 🧠 Agent em Ação

O agente segue um **Workflow Mandatório de 5 Etapas** (conforme definido no System Prompt), garantindo que a capacidade seja calculada antes da estimativa.

* **Ação Funcional 1 (Capacidade):** O agente inicia a sessão chamando a ferramenta **`calculate_sprint_capacity`** para determinar as horas líquidas da Sprint.
* **Ação Funcional 2 (Estimativa):** Em seguida, ele chama a ferramenta **`convert_to_fibonacci_story_points`** (ou mapeamento interno) para converter a estimativa do usuário em Story Points.

---

## 🛠️ Arquitetura e Componentes Essenciais

| Componente | Tecnologia / Lógica | Função Principal |
| :--- | :--- | :--- |
| **Agent IA (Cérebro)** | Azure AI Foundry / GPT-4o Mini | Orquestra as ferramentas e mantém o contexto da sessão. |
| **Tool 1 (Cálculo SP)** | Python `convert_to_fibonacci_story_points` | Converte estimativas em **dias** para Fibonacci (Regra: Arredondamento para cima). |
| **Tool 2 (Capacidade)** | Python `calculate_sprint_capacity` | Calcula a capacidade líquida da Sprint (descontando 20% de overhead). |
| **Lógica Interna** | System Prompt Mapping | Mapeia o T-Shirt Size (P, M, G, GG) para os SPs numéricos (1, 3, 5, 8). |

---

## 📚 Prova de Execução e Lógica

### 1. Lógica Funcional (Print 1)
O código Python demonstra a implementação das duas regras de negócio (Fibonacci e Capacidade).
**** (Insira o **PRINT 1**)

```python
from promptflow.core import tool

@tool
def convert_to_fibonacci_story_points(effort_estimate: float) -> int:
    """
    Converts estimated effort (days or hours) to the nearest upward Story Point 
    on the Fibonacci scale, following Agile estimation rules.
    """
    # A sequência de Fibonacci padrão para Story Points
    fibonacci = [1, 2, 3, 5, 8, 13, 21]
    
    if effort_estimate <= 0:
        return 0
        
    # Itera para encontrar o primeiro ponto de Fibonacci maior ou igual à estimativa.
    # Isso simula o arredondamento para cima (Round Up) baseado na incerteza.
    for point in fibonacci:
        if point >= effort_estimate:
            return point
            
    # Se a estimativa for maior que 21, retorna 21 como teto (um "épico" ou "spike").
    return fibonacci[-1]
```  
```python
from promptflow.core import tool

@tool
def calculate_sprint_capacity(num_weeks: int, num_developers: int, available_hours_per_day: int) -> int:
    """
    Calculates the total available working hours for a sprint, excluding meetings/overhead.
    Assumes 5 working days per week.
    """
    TOTAL_DAYS = num_weeks * 5  # 5 working days per week
    
    # Capacidade total em horas (considerando a equipe)
    total_capacity = TOTAL_DAYS * num_developers * available_hours_per_day
    
    # Reduz 20% para reuniões, burocracia e overhead (boa prática ágil)
    net_capacity = int(total_capacity * 0.80) 
    
    return net_capacity
```


### 2. Design e Instruções do Agente (Print 2)
O System Prompt força o fluxo de trabalho obrigatório (1. Perguntar Capacidade, 2. Estimar).
**** (Insira o **PRINT 2**)

### 3. Resultado Final Compilado (Print 3)
A execução do teste final prova que o agente utilizou as duas ferramentas e gerou o relatório compilado com as três métricas essenciais.

**** (Insira o seu **PRINT 3**)

---

## 📊 Conclusão

O projeto entrega um agente com alta capacidade analítica, validando a capacidade de construir e orquestrar múltiplas funções customizadas em um ambiente serverless de IA.

---
**Autor:** [Seu Nome Completo Aqui]
