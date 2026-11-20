# Sistema Especialista: Triagem Inteligente de Chamados de Infraestrutura de TI

Projeto desenvolvido para a disciplina de **Inteligência Artificial** (Profa. Kadidja Valéria), integrando os fundamentos de **Sistemas Especialistas baseados em Regras**, **Engenharia do Conhecimento (Referencial IBM)**, **Boas Práticas de SLA (ITIL)** e **Aprendizado Supervisionado**.

---

## Estrutura do Repositório

```text
sistema-especialista-triagem-ti/
├── README.md                              # Visão geral, arquitetura, regras e instruções
├── sistema_especialista_triagem.ipynb     # Notebook executável com motor de inferência e testes
├── estudo_inicial.md                      # Respostas teóricas baseadas nos guias da IBM
├── analise_3_casos.md                     # Estudo detalhado dos 3 cenários de validação
└── requirements.txt                       # Dependências opcionais (Jupyter e scikit-learn)
```

---

## 1. Problema e Objetivo
Chamados de infraestrutura frequentemente chegam ao *Service Desk* com descrições genéricas (ex.: *"rede lenta"* ou *"pasta não abre"*), gerando filas indevidas e atrasos em incidentes críticos.

Este **Sistema Especialista** realiza a **triagem automatizada e explicável** antes do encaminhamento do chamado, definindo:
* **Categoria técnica** (Storage, Redes ou Estação de Trabalho);
* **Prioridade operacional** (Crítica, Alta ou Média);
* **SLA de referência (ITIL)** para primeira resposta e solução;
* **Equipe especialista responsável** e **primeira ação recomendada**.

---

## 2. Arquitetura (Separação entre Conhecimento e Inferência)
* **Base de Conhecimento (`PERGUNTAS`, `REGRAS`, `DIAGNOSTICOS`):** Armazena declarativamente os sintomas observáveis, as regras de produção (`SE... ENTÃO...`) e os parâmetros de SLA.
* **Motor de Inferência (`encadeamento_para_frente` e `encadeamento_para_tras`):** Opera de forma independente do domínio, suportando tanto dedução guiada por sintomas (*forward chaining*) quanto validação guiada por hipótese (*backward chaining*).
* **Módulo de Explicação:** Exibe a trilha exata de regras disparadas (`R1` -> `R2`, etc.), garantindo auditabilidade completa.

---

## 3. Base de Regras e SLAs de Referência (ITIL)

| Regra | Condição (`SE`) | Conclusão (`ENTÃO`) | Prioridade / SLA |
| :--- | :--- | :--- | :--- |
| **R1** | `disco_falhou` **E** `raid_degradado` | `raid_em_risco` *(fato intermediário)* | — |
| **R2** | `raid_em_risco` **E** `volume_offline` | `triagem_storage_critica` (Storage N3) | **Crítica** (Resp: 15 min \| Sol: 4 h) |
| **R3** | `alerta_smart` | `triagem_storage_media` (Infra N2) | **Média** (Resp: 2 h \| Sol: 24 h) |
| **R4** | `sem_acesso_rede` **E** `varios_usuarios` | `triagem_rede_alta` (Redes) | **Alta** (Resp: 30 min \| Sol: 8 h) |
| **R5** | `sem_acesso_rede` **E** `apenas_um_usuario` | `triagem_suporte_media` (Suporte N1) | **Média** (Resp: 2 h \| Sol: 24 h) |

---

## 4. Resumo dos Documentos Complementares
* **[`estudo_inicial.md`](./estudo_inicial.md):** Fundamentação baseada nos guias da **IBM** sobre sistemas baseados em regras, comparação com ML e IA Generativa, gargalo da aquisição de conhecimento e princípios de *Trustworthy AI*.
* **[`analise_3_casos.md`](./analise_3_casos.md):** Discussão técnica dos 3 casos executados no notebook (Storage Crítico multinível, Rede Setorial com redução de 71% nas perguntas via encadeamento para trás, e Caso Ambíguo com escalonamento ético para humano).

---

## 5. Como Executar

```bash
# 1. Instalar dependências (opcional, apenas para rodar o notebook e a árvore de decisão)
pip install -r requirements.txt

# 2. Abrir o Jupyter Notebook
jupyter notebook sistema_especialista_triagem.ipynb
```
*(Observação: o arquivo `.ipynb` neste repositório já está salvo com todas as células executadas e suas respectivas saídas visíveis diretamente no GitHub).*
