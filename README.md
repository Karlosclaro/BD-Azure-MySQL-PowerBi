# BD-Azure-MySQL-PowerBi
Projeto de integração de BD, utilizando técnicas ETL.

# 📊 Tratamento e Modelagem de Dados — Azure + Power BI

Este repositório documenta as **alterações realizadas no banco de dados `azure_company`** e os **processos de tratamento, limpeza e modelagem de dados** aplicados no **Power BI / Power Query**, com o objetivo de **evitar imprecisão nos dashboards** e **melhorar a confiabilidade das análises**.

---

## 🗂️ Alterações no Banco de Dados

### 🏢 Tabela `azure_company.department`
- 🔗 **Removidas ligações** com:
  - `dept_locations`
  - `employee`
  - `project`
- ✅ Motivo: o **Power BI realiza os relacionamentos dinamicamente**, evitando duplicidade e imprecisão nos gráficos.
- 🧩 Tipo de uso: `TABLE`
- 🔄 **Tipos de dados ajustados** com a função **Detectar Tipo de Dado**.

---

### 📍 Tabela `azure_company.dept_locations`
- 🔗 **Removida ligação** com:
  - `department`
- 🧩 Tipo de uso: `VALUE`
- ✅ Relacionamentos tratados diretamente no Power BI.

---

### 👨‍💼 Tabela `azure_company.employee`
- 🔗 **Removidas ligações** com:
  - `department`
  - `dependent`
  - `employee (Ssn)`
  - `employee (Super_ssn)`
  - `works_on`
- 🧩 Tipos de uso:
  - `TABLE`
  - `VALUE`
- 🔄 Tipos de dados ajustados utilizando **Detectar Tipo de Dado**.
- 💰 Colunas utilizadas:
  - `Salary`
  - `Super_ssn`

---

### 👪 Tabela `azure_company.dependent`
- 🔗 **Removida ligação** com:
  - `employee`
- 🧩 Tipo de uso: `VALUE`
- 🔄 Tipos de dados ajustados com **Detectar Tipo de Dado**.

---

### 📁 Tabela `azure_company.project`
- 🔗 **Removidas ligações** com:
  - `department`
  - `works_on`
- 🧩 Tipos de uso:
  - `VALUE`
  - `TABLE`
- 🔄 Tipos de dados ajustados com **Detectar Tipo de Dado**.

---

### ⏱️ Tabela `azure_company.works_on`
- 🔗 **Removidas ligações** com:
  - `employee`
  - `project`
- 🧩 Tipos de uso:
  - `VALUE`
  - `TABLE`
- 🔄 Tipos de dados ajustados com **Detectar Tipo de Dado**.

---

## 👔 Gestão de Gerentes

- 📌 A coluna `Super_ssn` indica que o funcionário possui um **gerente**.
- 🔍 Foi feito o **isolamento dos funcionários que possuem gerente** a partir da tabela `employee`.
- 🆕 Criada a tabela **`Gerente`**, contendo:
  - Primeiro Nome
  - Último Nome
  - `Super_ssn`
- 🔗 Realizada uma **mesclagem (Merge)** entre:
  - `azure_company.employee`
  - Tabela `Gerente`

### 🧠 Regra de Negócio (Criada com apoio de IA)
- ✅ **Funcionário Regular** → possui gerente
- ❌ **Error** → funcionário ativo sem gerente

📊 **Resultado**:  
Todos os departamentos possuem **ao menos um gerente válido**.

---

## ⏳ Controle de Horas Trabalhadas

- 🆕 Criada a tabela **`Horas_Por_Employee`**
- 🔗 Mesclagem entre:
  - Funcionários (`Essn`)
  - Tabela `azure_company.works_on`
- ➕ Somatória das horas trabalhadas por funcionário
- 🧠 Filtro inteligente (criado por IA) para validar:
  - ❗ Nenhum funcionário trabalhou **mais de 40 horas** em projetos

---

## 🧾 Simplificação da Tabela `employee`

### ✍️ Nome Completo
- 🔀 Junção das colunas:
  - `Fname`
  - `Minit`
  - `Lname`
- 🆕 Criada a coluna **Nome Completo** para melhor legibilidade.

---

### 🏠 Tratamento de Endereço
- 📌 Coluna `Address` foi **dividida** em:
  - Rua
  - Cidade
  - Estado
- 🛠️ Utilizada a função **Remover Colunas** para organizar os dados.

---

### 🔗 Mesclagens Adicionais
- 🧩 Mesclagem entre:

- `employee[Dno]`
  - `department[Dnumber]`
- 🔁 Realizada também **mesclagem da tabela `employee` com ela mesma**, seguida de:
  - Unificação de nomes por meio de **Mesclar Colunas**.

---

## 🔄 Tipos de Relacionamento

- 🔗 **Mesclagem (Merge)**:
  - Utilizada em relacionamentos **1 para N (ou N)**.
- 🎯 **Atribuição direta**:
  - Utilizada em relacionamentos **1 para 1**.

---

## ✅ Considerações Finais

✔️ Relacionamentos controlados no Power BI  
✔️ Menor risco de duplicidade de dados  
✔️ Melhor desempenho de dashboards  
✔️ Regras de negócio claras e validadas  

---

📌 *Este processo garante uma modelagem de dados mais limpa, confiável e escalável para análises corporativas no Power BI.*
