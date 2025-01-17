# Database Schema Concept for a Mechanical Workshop

This exercise consists of creating a conceptual schema for a service order control and management system in a mechanical workshop. The model covers the main entities, their attributes and the relationships between them, in addition to including an ER diagram in Mermaid format to visualize the interactions.

## Entidades e Atributos

### Cliente
- ID_Cliente
- Nome
- Endereço
- Telefone

### Veículo
- ID_Veículo
- Modelo
- Marca
- Ano
- Placa
- ID_Cliente

### Mecânico
- ID_Mecânico
- Nome
- Endereço
- Especialidade

### Equipe
- ID_Equipe
- Nome
- ID_Mecânico

### Ordem de Serviço (OS)
- ID_OS
- Data_Emissao
- Valor_Total
- Status
- Data_Entrega_Estimada
- Data_Inicio
- Observações

### Serviço
- ID_Serviço
- Descrição
- Valor_Unitario
- Categoria
- Tempo_Estimado

### Peça
- ID_Peça
- Descrição
- Valor_Unitario

### Pagamento
- ID_Pagamento
- ID_OS
- Data_Pagamento
- Valor_Pago
- Método_Pagamento

## Relacionamentos entre Entidades

1. **Cliente - Veículo**
   - Tipo: 1:N (Um para Muitos)
   - Descrição: Um cliente pode ter vários veículos registrados na oficina. Cada veículo está associado a um único cliente.

2. **Veículo - Ordem de Serviço (OS)**
   - Tipo: 1:N (Um para Muitos)
   - Descrição: Um veículo pode ter várias ordens de serviço ao longo do tempo, refletindo diferentes serviços realizados ou revisões periódicas. Cada ordem de serviço está vinculada a um único veículo.

3. **Ordem de Serviço (OS) - Serviço**
   - Tipo: 1:N (Um para Muitos)
   - Descrição: Uma ordem de serviço pode incluir vários serviços a serem executados. Cada serviço pode ser parte de apenas uma ordem de serviço específica.

4. **Serviço - Ordem de Serviço**
   - Tipo: N:M (Muitos para Muitos)
   - Descrição: Um serviço pode ser realizado em várias ordens de serviço diferentes, especialmente se for um serviço comum, como troca de óleo ou alinhamento. Da mesma forma, uma ordem de serviço pode incluir múltiplos serviços.

5. **Ordem de Serviço (OS) - Peça**
   - Tipo: 1:N (Um para Muitos)
   - Descrição: Uma ordem de serviço pode incluir várias peças necessárias para realizar os serviços. Cada peça pode ser utilizada em apenas uma ordem de serviço específica.

6. **Peça - Ordem de Serviço**
   - Tipo: N:M (Muitos para Muitos)
   - Descrição: Uma peça pode ser utilizada em várias ordens de serviço diferentes, especialmente se for uma peça comum que é frequentemente substituída ou reparada.

7. **Ordem de Serviço (OS) - Pagamento**
   - Tipo: 1:1 (Um para Um)
   - Descrição: Cada ordem de serviço deve ter um registro correspondente de pagamento, que indica que o cliente pagou pelos serviços e peças fornecidos. Não deve haver múltiplos pagamentos para uma única ordem de serviço.

8. **Mecânico - Equipe**
   - Tipo: N:M (Muitos para Muitos)
   - Descrição: Um mecânico pode fazer parte de várias equipes, e uma equipe pode conter vários mecânicos. Isso permite flexibilidade na alocação dos mecânicos às ordens de serviço conforme necessário.

9. **Equipe - Ordem de Serviço (OS)**
   - Tipo: N:M (Muitos para Muitos)
   - Descrição: Uma equipe pode ser designada para trabalhar em várias ordens de serviço, e uma ordem de serviço pode envolver várias equipes, especialmente em casos mais complexos que exigem diferentes especializações.

## Diagrama ER

```mermaid
erDiagram
    CLIENTE {
        int ID_Cliente PK
        string Nome
        string Endereço
        string Telefone
    }

    VEICULO {
        int ID_Veículo PK
        string Modelo
        string Marca
        int Ano
        string Placa
        int ID_Cliente FK
    }

    MECANICO {
        int ID_Mecânico PK
        string Nome
        string Endereço
        string Especialidade
    }

    EQUIPE {
        int ID_Equipe PK
        string Nome
    }

    ORDEM_SERVICO {
        int ID_OS PK
        date Data_Emissao
        float Valor_Total
        string Status
        date Data_Entrega_Estimada
        date Data_Inicio
        string Observações
    }

    SERVIÇO {
        int ID_Serviço PK
        string Descrição
        float Valor_Unitario
        string Categoria
        int Tempo_Estimado
    }

    PEÇA {
        int ID_Peça PK
        string Descrição
        float Valor_Unitario
    }

    PAGAMENTO {
        int ID_Pagamento PK
        int ID_OS FK
        date Data_Pagamento
        float Valor_Pago
        string Método_Pagamento
    }

    CLIENTE ||--o{ VEICULO : possui
    VEICULO ||--o{ ORDEM_SERVICO : tem
    ORDEM_SERVICO ||--o{ SERVIÇO : inclui
    SERVIÇO }o--o{ ORDEM_SERVICO : realiza 
    ORDEM_SERVICO ||--o{ PEÇA : inclui 
    PEÇA }o--o{ ORDEM_SERVICO : utiliza 
    ORDEM_SERVICO ||--|| PAGAMENTO : gera 
    MECANICO }o--o{ EQUIPE : fazParte 
    EQUIPE }o--o{ ORDEM_SERVICO : designadaPara 