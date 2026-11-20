# Análise dos 3 Casos de Teste — Sistema Especialista de Triagem de TI

Para validar o comportamento do **Motor de Inferência**, a consistência da **Base de Conhecimento** e o **Módulo de Explicação**, foram estruturados e executados **3 casos de teste representativos** da rotina de suporte de infraestrutura de TI.

---

## Caso 1: Falha Crítica de Storage com Encadeamento Dedutivo em Duas Etapas
* **Contexto Operacional:** Um servidor de arquivos corporativo apresenta alerta luminoso laranja em um dos discos físicos, o controlador reporta que o array RAID entrou em modo degradado e o volume compartilhado ficou inacessível (offline) para os departamentos.
* **Fatos de Entrada:** `disco_falhou = True`, `raid_degradado = True`, `volume_offline = True`.
* **Comportamento do Motor de Inferência:**
  1. **Encadeamento para Frente (*Forward Chaining*):**
     * **Ciclo 1:** A regra `R1` (`SE disco_falhou E raid_degradado ENTÃO raid_em_risco`) é satisfeita e insere o **fato intermediário** `raid_em_risco` na memória de trabalho.
     * **Ciclo 2:** Com o novo fato ativo, a regra `R2` (`SE raid_em_risco E volume_offline ENTÃO triagem_storage_critica`) é disparada.
  2. **Encadeamento para Trás (*Backward Chaining*):** Ao testar diretamente a hipótese `triagem_storage_critica`, o motor retrocede exigindo apenas `volume_offline`, `disco_falhou` e `raid_degradado`, ignorando perguntas irrelevantes sobre rede local ou usuários isolados.
* **Resultado da Triagem:**
  * **Categoria:** Storage / Armazenamento | **Prioridade:** CRÍTICA
  * **SLA (Referência ITIL):** Primeira resposta em 15 min | Solução em até 4 horas
  * **Equipe:** Especialistas de Storage / Infraestrutura N3
* **Análise Crítica:** Demonstra o poder do **encadeamento multinível** (uso de fato intermediário `raid_em_risco`). Em vez de tratar o problema como um simples chamado de suporte comum, o sistema reconhece o risco iminente de perda de dados e escala imediatamente para N3.

---

## Caso 2: Indisponibilidade de Rede Setorial vs. Estação Isolada (Eficiência do Raciocínio)
* **Contexto Operacional:** Colaboradores de todo o setor financeiro relatam perda simultânea de acesso à rede interna e à internet, enquanto os indicadores físicos do storage permanecem normais.
* **Fatos de Entrada:** `sem_acesso_rede = True`, `varios_usuarios = True` (demais sintomas falsos).
* **Comportamento do Motor de Inferência:**
  * **Encadeamento para Frente:** Avalia as regras de storage (descartadas) e dispara `R4` (`SE sem_acesso_rede E varios_usuarios ENTÃO triagem_rede_alta`).
  * **Encadeamento para Trás (Comparativo de Eficiência):** Se o operador suspeita de falha de rede setorial (`triagem_rede_alta`), o motor faz apenas **2 perguntas** (`sem_acesso_rede` e `varios_usuarios`) em vez de percorrer todas as 7 perguntas do questionário completo — reduzindo o tempo de triagem em ~71%.
* **Resultado da Triagem:**
  * **Categoria:** Redes | **Prioridade:** ALTA
  * **SLA (Referência ITIL):** Primeira resposta em 30 min | Solução em até 8 horas
  * **Equipe:** Equipe de Redes
* **Análise Crítica:** Evidencia a diferença de impacto organizacional capturada pelas regras `R4` (incidente coletivo -> Prioridade Alta, Equipe de Redes) e `R5` (usuário único -> Prioridade Média, Suporte N1), evitando o envio indevido de técnicos de campo às estações individuais quando a causa provável está no switch ou gateway do setor.

---

## Caso 3: Sintomas Ambíguos / Fora da Base de Conhecimento (Tratamento de Incerteza e Ética)
* **Contexto Operacional:** Um analista abre chamado relatando apenas *"lentidão intermitente no acesso a pastas de rede durante o horário de almoço"*, sem falha física de disco, sem RAID degradado, sem volume offline, sem alerta SMART e sem queda total de rede.
* **Fatos de Entrada:** Todos os indicadores críticos/binários da base marcados como `False`.
* **Comportamento do Motor de Inferência:**
  * Nenhuma regra (`R1` a `R5`) tem suas condições satisfeitas.
  * A trilha de regras disparadas permanece vazia (`[]`).
* **Resultado da Triagem:**
  * **Status:** `INCONCLUSIVO / ESCALONAMENTO MANUAL`
  * **Ação Recomendada:** Encaminhar o chamado para triagem assistida por analista N2 com registro dos sintomas verificados.
* **Análise Crítica:** Este caso testa a **robustez e a ética operacional** do Sistema Especialista. Diferente de modelos generativos que podem fabricar uma causa provável mesmo sem evidências, um sistema baseado em regras bem projetado **reconhece a fronteira do seu conhecimento**. Além disso, aponta uma oportunidade concreta de evolução da base: incluir regras futuras voltadas a *gargalo de IOPS / saturação de banda* mediante análise de histórico.
