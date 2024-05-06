# Modelo de Entidade e Relacionamento para o Sistema de Transporte e Produtos Médicos

Esta documentação descreve o modelo de entidade e relacionamento para um Sistema de Transporte e Produtos Médicos, incluindo entidades principais, atributos, relacionamentos, otimização e avaliação, bem como relacionamentos N para N e 1 para N. Além disso, foram criadas tabelas intermediárias para gerenciar relacionamentos de N para N entre pacientes, medicamentos e veículos.

## Entidades Principais

1. **Paciente:**
   - ID do paciente
   - Nome
   - Data de nascimento
   - Endereço
   - Condição médica

2. **Produto Médico:**
   - ID do produto
   - Nome
   - Descrição
   - Quantidade em estoque

3. **Unidade de Saúde:**
   - ID da unidade
   - Nome
   - Endereço
   - Tipo (hospital, clínica, etc.)

4. **Veículo Médico:**
   - ID do veículo
   - Tipo (ambulância, van, etc.)
   - Capacidade de passageiros
   - Capacidade de carga

## Relacionamentos

1. **Transporte de Pacientes:**
   - Relaciona o paciente com o veículo médico usado para transporte.
   - Atributos adicionais: data/hora de partida, data/hora de chegada, motorista responsável.

2. **Entrega de Produtos Médicos:**
   - Relaciona o produto médico com o veículo usado para entrega.
   - Atributos adicionais: data/hora de saída, data/hora de entrega, responsável pela entrega.

3. **Associação entre Pacientes e Unidades de Saúde:**
   - Pacientes são associados à unidade de saúde onde receberão tratamento.

4. **Associação entre Produtos Médicos e Unidades de Saúde:**
   - Produtos médicos são associados à unidade de saúde onde serão utilizados.

## Otimização e Avaliação

- Para otimizar as rotas de transporte de pacientes, considere algoritmos de roteirização, como o algoritmo de Dijkstra ou o algoritmo genético.
- Avalie continuamente a eficácia do sistema monitorando métricas como tempo de transporte, disponibilidade de produtos médicos e satisfação dos pacientes.

## Relacionamentos N para N (Muitos-para-Muitos)

1. **Pacientes e Produtos Médicos:**
   - Um paciente pode precisar de vários produtos médicos (por exemplo, medicamentos).
   - Um produto médico pode ser usado por vários pacientes.
   - Relacionamento N para N entre pacientes e produtos médicos.

2. **Unidades de Saúde e Produtos Médicos:**
   - Uma unidade de saúde pode usar vários produtos médicos (por exemplo, equipamentos, suprimentos).
   - Um produto médico pode ser utilizado em várias unidades de saúde.
   - Relacionamento N para N entre unidades de saúde e produtos médicos.

## Relacionamentos 1 para N (Um-para-Muitos)

1. **Pacientes e Unidades de Saúde:**
   - Um paciente é atendido por uma única unidade de saúde (hospital, clínica, etc.).
   - No entanto, uma unidade de saúde pode atender a vários pacientes.
   - Relacionamento 1 para N entre pacientes e unidades de saúde.

2. **Veículos Médicos e Pacientes/Produtos Médicos:**
   - Um veículo médico pode transportar vários pacientes ou entregar vários produtos médicos.
   - Cada paciente ou produto médico está associado a um único veículo médico.
   - Relacionamento 1 para N entre veículos médicos e pacientes/produtos médicos.

## Tabelas Intermediárias

1. **Tabela Intermediária "Receituário":**
   - Relaciona pacientes e medicamentos.
   - Campos: ID do receituário, ID do paciente, ID do medicamento, Quantidade do medicamento.

2. **Tabela Intermediária "Entrega":**
   - Relaciona medicamentos e veículos.
   - Campos: ID da entrega, ID do medicamento, ID do veículo, Data e hora da entrega.

## Consulta Realizada

A seguinte consulta foi realizada para calcular o número médio de pacientes transportados por veículo por mês:

```sql
SELECT 
    MONTH(data_partida) AS mes,
    YEAR(data_partida) AS ano,
    veiculo_id,
    COUNT(DISTINCT paciente_id) AS pacientes_transportados,
    COUNT(DISTINCT paciente_id) / COUNT(DISTINCT DAY(data_partida)) AS media_pacientes_por_dia,
    COUNT(DISTINCT paciente_id) / COUNT(DISTINCT MONTH(data_partida)) AS media_pacientes_por_mes
FROM 
    transporte_pacientes
GROUP BY 
    YEAR(data_partida),
    MONTH(data_partida),
    veiculo_id;
```

Os resultados obtidos foram:

| mes | ano  | veiculo_id | pacientes_transportados | media_pacientes_por_dia | media_pacientes_por_mes |
|-----|------|------------|------------------------|-------------------------|-------------------------|
| 5   | 2024 | 1          | 1                      | 1.0000                  | 1.0000                  |
| 5   | 2024 | 2          | 1                      | 1.0000                  | 1.0000                  |

Este documento fornece uma visão completa do modelo de banco de dados e das consultas realizadas para análise de dados no Sistema de Transporte e Produtos Médicos.