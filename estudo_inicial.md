# Estudo Inicial — Sistemas Especialistas e Engenharia do Conhecimento (Referencial IBM)

Este documento reúne as respostas fundamentais do estudo inicial sobre **Sistemas Especialistas (Expert Systems)**, tomando como base conceitual os guias técnicos e educacionais da **IBM** (*IBM Think / IBM Cloud Learn Hub — Rule-Based Systems, Expert Systems, Knowledge Engineering e Trustworthy AI*) articulados aos conteúdos da disciplina de Inteligência Artificial.

---

## 1. O que é um Sistema Especialista segundo os guias da IBM?
De acordo com a documentação conceitual da IBM sobre Inteligência Artificial simbólica e sistemas baseados em conhecimento, um **Sistema Especialista** é um programa de computador projetado para emular a capacidade de tomada de decisão de um especialista humano em um domínio específico e bem delimitado (como diagnóstico médico, análise de crédito ou suporte técnico de infraestrutura de TI).

Diferente de algoritmos convencionais — onde a lógica de decisão fica misturada ao código procedural — um Sistema Especialista separa explicitamente **o que se sabe sobre o problema** (fatos e regras) de **como se raciocina sobre esses dados**.

---

## 2. Quais são os componentes essenciais da arquitetura?
Conforme a arquitetura clássica descrita nos guias da IBM:

1. **Base de Conhecimento (*Knowledge Base*):** Repositório estruturado que armazena o conhecimento especializado do domínio. Divide-se em:
   * **Base de Fatos (Memória de Trabalho):** Dados e sintomas observados no caso atual (ex.: `disco_falhou = Verdadeiro`, `volume_offline = Verdadeiro`).
   * **Base de Regras (Regras de Produção):** Conhecimento heurístico e técnico codificado no formato `SE <condições> ENTÃO <conclusão/ação>`.
2. **Motor de Inferência (*Inference Engine*):** O mecanismo lógico que cruza os fatos conhecidos com as regras da base para deduzir novas conclusões. Opera principalmente por duas estratégias:
   * **Encadeamento para Frente (*Forward Chaining*):** Raciocínio guiado pelos dados (*data-driven*). Parte dos sintomas iniciais e dispara regras sucessivamente até atingir uma conclusão.
   * **Encadeamento para Trás (*Backward Chaining*):** Raciocínio guiado por objetivos (*goal-driven*). Parte de uma hipótese diagnóstica e retrocede verificando apenas as pré-condições necessárias para confirmá-la ou descartá-la.
3. **Módulo de Explicação (*Explanation Facility*):** Componente crítico para auditabilidade e confiança, responsável por registrar e exibir a trilha de regras disparadas (*"por que o sistema chegou a essa conclusão?"*).
4. **Interface com o Usuário (*User Interface*):** Camada de interação que coleta os sintomas do operador/atendente e apresenta a triagem final e as recomendações operacionais.

---

## 3. Como os Sistemas Especialistas se comparam ao Machine Learning e à IA Generativa (Visão IBM)?
Os guias da IBM diferenciam três paradigmas complementares de IA corporativa:

| Critério | Sistemas Especialistas (Regras) | Machine Learning Supervisionado | IA Generativa (LLMs) |
| :--- | :--- | :--- | :--- |
| **Fonte do Conhecimento** | Regras explícitas formalizadas com especialistas humanos. | Padrões estatísticos extraídos de grandes volumes de dados históricos. | Treinamento massivo em linguagem natural e multimodal. |
| **Explicabilidade** | **Total (Caixa-Branca):** cada decisão aponta exatamente quais regras dispararam. | **Parcial a Baixa:** árvores são legíveis, mas redes profundas são opacas. | **Baixa (Caixa-Preta):** gera texto fluido, mas pode sofrer alucinações. |
| **Determinismo** | **100% determinístico:** mesmas entradas sempre geram a mesma saída. | Probabilístico (baseado em escores e limiares). | Probabilístico / estocástico. |
| **Ponto Fraco Principal** | **Gargalo da Aquisição de Conhecimento:** manutenção manual difícil quando o domínio cresce. | Exige bases históricas limpas, rotuladas e representativas. | Alto custo computacional e risco de inventar procedimentos técnicos inexistentes. |

**Síntese aplicada ao problema:** Na triagem de incidentes críticos de infraestrutura de TI (como falhas de *storage* e RAID), o determinismo e a auditabilidade dos Sistemas Especialistas evitam respostas inventadas e garantem conformidade com acordos de nível de serviço (SLAs). Além disso, técnicas de Aprendizado Supervisionado (como Árvores de Decisão) podem ser usadas como apoio na fase de **Engenharia do Conhecimento** para minerar regras a partir de chamados históricos.

---

## 4. O que é o "Gargalo da Aquisição de Conhecimento" e como mitigá-lo?
Nos guias da IBM sobre Engenharia do Conhecimento, destaca-se que extrair o conhecimento tácito de engenheiros seniores e traduzi-lo em regras lógicas consistentes (sem contradições ou lacunas) é a etapa mais demorada e custosa do projeto. Para mitigá-lo, adota-se:
* **Escopo bem delimitado:** Focar nos incidentes de maior impacto operacional (storage, RAID e conectividade de rede).
* **Documentação de padrões operacionais (ITIL / Runbooks):** Estruturar prioridades e SLAs a partir de manuais técnicos consolidados.
* **Mineração híbrida de regras:** Utilizar árvores de decisão sobre logs históricos de chamados para sugerir novas regras `SE... ENTÃO...` que são posteriormente validadas por especialistas humanos.

---

## 5. Princípios de IA Confiável (*Trustworthy AI* — IBM) aplicados ao Sistema
Seguindo os pilares de governança e ética da IBM (*Explicabilidade, Justiça, Robustez, Transparência e Privacidade*):
* **Transparência e Explicabilidade:** O operador visualiza passo a passo quais regras (`R1`, `R2`, etc.) justificaram a prioridade atribuída.
* **Human-in-the-Loop (Supervisão Humana):** O sistema atua como **apoio à triagem**, nunca executando ações destrutivas (como reconstrução de array RAID ou desligamento de volumes) sem autorização humana.
* **Reconhecimento de Incerteza (Robustez):** Quando os sintomas informados não satisfazem nenhuma regra da base, o sistema declara explicitamente a insuficiência de dados e encaminha o chamado para análise humana, em vez de forçar uma classificação incorreta.
