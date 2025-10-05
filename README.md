# 🧰 Sistema de Controle de Ordens de Serviço - Oficina Mecânica

## 📘 Descrição do Projeto

Este projeto tem como objetivo modelar o **esquema conceitual** de um sistema para controle e gerenciamento de **ordens de serviço (OS)** em uma **oficina mecânica**.  

O sistema deve permitir o acompanhamento de clientes, veículos, equipes de mecânicos, serviços realizados e peças utilizadas em cada ordem de serviço — garantindo a rastreabilidade completa do processo de manutenção.

---

## 🚗 Contexto Narrativo

- Clientes levam **veículos** à oficina mecânica para **consertos** ou **revisões periódicas**.  
- Cada veículo é designado a uma **equipe de mecânicos**, que identifica os **serviços** a serem executados e preenche uma **Ordem de Serviço (OS)** com **data de entrega**.  
- A partir da OS, calcula-se o **valor de cada serviço**, com base em uma **tabela de referência de mão de obra**.  
- O valor das **peças** utilizadas também compõe o **valor total da OS**.  
- O **cliente autoriza** a execução dos serviços.  
- A **mesma equipe** que avaliou o veículo é a responsável pela execução dos serviços.  
- Os **mecânicos** possuem **código, nome, endereço e especialidade**.  
- Cada **OS** possui:  
  - Número identificador  
  - Data de emissão  
  - Valor total  
  - Status (em análise, aprovado, em execução, concluído, cancelado)  
  - Data de conclusão dos trabalhos  
- Uma OS pode conter **vários serviços**, e um mesmo serviço pode aparecer em várias OS.  
- Uma OS pode incluir **várias peças**, e uma mesma peça pode estar presente em diferentes OS.

---

## 🧩 Entidades Identificadas

| Entidade | Atributos Principais | Observações |
|-----------|----------------------|--------------|
| **Cliente** | id_cliente, nome, telefone, endereço | Pode possuir mais de um veículo |
| **Veículo** | id_veiculo, placa, modelo, marca, ano, id_cliente | Relacionado a um único cliente |
| **Equipe** | id_equipe, nome_equipe | Responsável por várias OS |
| **Mecânico** | id_mecanico, nome, endereço, especialidade, id_equipe | Um mecânico pertence a uma equipe |
| **Ordem_Servico (OS)** | id_os, data_emissao, valor_total, status, data_conclusao, id_veiculo, id_equipe | Centraliza todas as informações da execução |
| **Serviço** | id_servico, descricao, valor_mao_obra | Cadastrado em tabela de referência |
| **Peça** | id_peca, descricao, valor_unitario, fabricante | Usadas em diversas OS |
| **OS_Serviço** | id_os, id_servico, quantidade, subtotal | Tabela associativa entre OS e Serviço |
| **OS_Peça** | id_os, id_peca, quantidade, subtotal | Tabela associativa entre OS e Peça |

---

## 🔗 Relacionamentos

| Relacionamento | Tipo | Descrição |
|----------------|------|------------|
| Cliente – Veículo | 1:N | Um cliente pode ter vários veículos |
| Veículo – OS | 1:N | Um veículo pode gerar várias ordens de serviço |
| Equipe – OS | 1:N | Uma equipe pode atender várias ordens |
| Equipe – Mecânico | 1:N | Uma equipe é formada por vários mecânicos |
| OS – Serviço | N:M | Uma OS pode conter vários serviços |
| OS – Peça | N:M | Uma OS pode conter várias peças |

---

## 📊 Modelo Conceitual (Resumo em Texto)

```
CLIENTE (id_cliente, nome, telefone, endereço)
    1 ────< VEÍCULO (id_veiculo, placa, modelo, marca, ano, id_cliente)
                1 ────< ORDEM_SERVICO (id_os, data_emissao, valor_total, status, data_conclusao, id_veiculo, id_equipe)
                            N ────< OS_SERVIÇO (id_os, id_servico, quantidade, subtotal)
                                        >──── N SERVIÇO (id_servico, descricao, valor_mao_obra)
                            N ────< OS_PEÇA (id_os, id_peca, quantidade, subtotal)
                                        >──── N PEÇA (id_peca, descricao, valor_unitario, fabricante)
EQUIPE (id_equipe, nome_equipe)
    1 ────< MECÂNICO (id_mecanico, nome, endereço, especialidade, id_equipe)
```

---

## 🧠 Decisões de Modelagem

- A **Equipe** é uma entidade própria para permitir a alocação de múltiplos mecânicos em diferentes OS.  
- Criaram-se tabelas associativas **OS_Serviço** e **OS_Peça**, já que há relacionamento **N:N** entre OS e esses itens.  
- Os valores de serviços e peças são armazenados também nas tabelas associativas, permitindo registrar **quantidade** e **subtotal**.  
- O **valor total da OS** pode ser calculado dinamicamente (somando subtotais) ou armazenado como atributo.  
- O **status** da OS facilita o acompanhamento do fluxo (Ex: “Aguardando autorização”, “Em execução”, “Concluído”).  

---

## 💾 Tecnologias sugeridas (para modelagem)

- Ferramenta de diagrama: **Lucidchart**, **Draw.io**, **BrModelo** ou **Diagrams.net**  
- Banco de dados alvo: **MySQL**, **PostgreSQL** ou **SQL Server**  
- Linguagem de apoio: **SQL** para criação física posterior

---

## ✍️ Autor

**Hamilton Junior**  
Analista de Tecnologia da Informação  
Instituto Federal do Sertão Pernambucano  
