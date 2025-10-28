# aws-step-functions-guia
Resumo completo sobre AWS Step Functions

# 🌀 AWS Step Functions — Guia Completo

O **AWS Step Functions** é um serviço *serverless* de orquestração visual que permite aos desenvolvedores **coordenar componentes de aplicações distribuídas e microsserviços** por meio de fluxos de trabalho (*workflows*) compostos por várias etapas chamadas **máquinas de estado**.  

Ele integra serviços da AWS (como Lambda, ECS, DynamoDB, S3, entre outros) em processos automatizados, garantindo **controle, rastreabilidade e resiliência** em cada execução.

---

## 🔍 Conhecendo o AWS Step Functions

### **O que é**
Um serviço de **orquestração sem servidor (serverless)** que automatiza processos e coordena múltiplas etapas de um fluxo de trabalho. Cada etapa é definida em uma **máquina de estado**, responsável por determinar a ordem de execução e a lógica de decisões.

### **Como funciona**
Os fluxos são definidos em formato **JSON**, utilizando a **Amazon States Language (ASL)** — uma linguagem declarativa que descreve cada estado, suas transições e condições.  
O Step Functions executa essa definição, garantindo que cada etapa ocorra na sequência correta e que erros, exceções e ramificações sejam tratadas automaticamente.

### **Visualização**
O **Workflow Studio** oferece uma interface gráfica para criar e visualizar fluxos de forma intuitiva (*drag-and-drop*), permitindo acompanhar execuções em tempo real — funcionando como o “maestro” da orquestra de serviços AWS.

---

## ⚙️ Tipos de Máquinas de Estado

O Step Functions oferece dois tipos principais de execução, adaptados a diferentes cenários:

- **Standard Workflows:**  
  Projetados para execuções **de longa duração** (até 1 ano), com histórico detalhado e rastreabilidade completa.  
  Ideais para processos críticos, como pipelines de dados e aprovações manuais.

- **Express Workflows:**  
  Otimizados para **execuções de alta frequência e curta duração** (segundos ou minutos).  
  Oferecem **baixo custo e menor latência**, sendo ideais para processamento de eventos em tempo real, integrações rápidas ou automações leves.

---

## 🚀 Benefícios do AWS Step Functions

- **Orquestração Simplificada:**  
  Coordena serviços da AWS e microsserviços complexos sem precisar codificar a lógica de controle manualmente.

- **Resiliência e Confiabilidade:**  
  Inclui nativamente **tratamento de erros**, **retries**, **timeouts** e **compensações**, tornando fluxos tolerantes a falhas.

- **Design Visual:**  
  O *Workflow Studio* permite criar e modificar fluxos facilmente com arrastar e soltar.

- **Escalabilidade e Desempenho:**  
  Totalmente *serverless*, escala automaticamente de acordo com a carga de trabalho.

- **Integração Extensa:**  
  Conecta-se nativamente a mais de **220 serviços AWS** e, com o **AWS SDK Integrations**, pode chamar APIs da AWS diretamente — sem precisar de Lambda intermediário.

- **Custos Flexíveis:**  
  Cobrados por transição de estado (Standard) ou por tempo e memória (Express), garantindo previsibilidade e controle de gastos.

---

## 🧱 Estruturas de Controle e Lógica

O Step Functions usa diferentes **tipos de estados** para definir o comportamento do fluxo:

| Tipo de Estado | Função |
|-----------------|--------|
| `Task` | Executa uma ação (ex: chamar uma função Lambda) |
| `Choice` | Cria ramificações condicionais (`if/else`, `switch/case`) |
| `Parallel` | Executa várias tarefas simultaneamente |
| `Wait` | Pausa o fluxo por um tempo definido |
| `Succeed` / `Fail` | Finaliza o fluxo com sucesso ou erro |
| `Map` | Itera sobre listas de itens de forma paralela |

Além disso, o Step Functions permite **transformar e filtrar dados JSON** entre etapas, garantindo que a entrada e saída de cada estado estejam no formato correto.

---

## 🧠 Boas Práticas de Projeto

- Mantenha fluxos **modulares e reutilizáveis** (ex: subfluxos com *Nested Workflows*).  
- Centralize **a orquestração no Step Functions** e **a lógica de negócio no Lambda**.  
- Defina **timeouts e políticas de retry** para evitar loops ou travamentos.  
- Utilize o **estado Parallel** para tarefas independentes.  
- Adicione **logs e métricas** com o **Amazon CloudWatch** e **AWS X-Ray** para monitorar desempenho e identificar gargalos.  
- Use **Express Workflows** para tarefas rápidas e de alta frequência.

---

## 🧩 Criando e Executando Lambdas no Step Functions

O **AWS Lambda** é um dos serviços mais usados dentro do Step Functions. Ele é normalmente invocado via **estado de Tarefa (`Task`)**.

### **Fluxo de Execução:**
1. O Step Functions **inicia uma execução** da máquina de estado (manualmente, via API ou através do Amazon EventBridge).  
2. Ao atingir um estado `Task`, ele **invoca a função Lambda** correspondente.  
3. A Lambda **executa sua lógica** e retorna o resultado.  
4. O Step Functions **usa essa saída** como entrada para o próximo estado.

### **Padrões de Integração:**
- **Request-Response (`.sync`)** → executa e aguarda o resultado.  
- **Run a Job** → aguarda conclusão de tarefas longas.  
- **Wait for Callback (`.waitForTaskToken`)** → pausa até receber um retorno externo (ideal para fluxos com intervenção humana ou sistemas externos).

---

## 🧰 Exemplo Prático Simplificado

Um exemplo básico de máquina de estado para um processo ETL (Extração, Transformação e Carga):

```json
{
  "StartAt": "ExtrairDados",
  "States": {
    "ExtrairDados": {
      "Type": "Task",
      "Resource
