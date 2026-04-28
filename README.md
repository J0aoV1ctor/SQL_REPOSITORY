# SQL_REPOSITORY

📌 Visão Geral

Este projeto define a estrutura de um banco de dados relacional para um sistema de e-commerce.
O modelo contempla:

Cadastro de clientes (Pessoa Física e Jurídica)
Endereços
Formas de pagamento
Produtos e categorias
Pedidos e itens
Pagamentos
Entregas
🧱 Estrutura do Banco

O banco foi projetado seguindo boas práticas de normalização, separando entidades principais e especializações.

👤 1. Tabela: clientes
Objetivo:

Armazena os dados básicos de todos os clientes.

Campos principais:
id_cliente: Identificador único
tipo: Define se é PF ou PJ
email: Único no sistema
senha: Armazenamento seguro (hash recomendado)
telefone
data_cadastro
Observação:

Essa tabela serve como base para PF e PJ.

🧍 2. Tabela: clientes_pf
Objetivo:

Armazena dados específicos de Pessoa Física.

Campos:
id_cliente: PK e FK
nome
cpf: Único
data_nascimento
Regra:

Só existe se clientes.tipo = 'PF'

🏢 3. Tabela: clientes_pj
Objetivo:

Armazena dados específicos de Pessoa Jurídica.

Campos:
id_cliente: PK e FK
razao_social
cnpj: Único
nome_fantasia
Regra:

Só existe se clientes.tipo = 'PJ'

📍 4. Tabela: enderecos
Objetivo:

Armazena os endereços dos clientes.

Campos:
id_endereco
id_cliente (FK)
rua, numero, cidade, estado, cep, complemento
Relacionamento:
Um cliente pode ter vários endereços (1:N)
💳 5. Tabela: formas_pagamento
Objetivo:

Catálogo de formas de pagamento disponíveis.

Exemplos:
Cartão
Pix
Boleto
👛 6. Tabela: cliente_pagamentos
Objetivo:

Armazena os métodos de pagamento salvos por cada cliente.

Campos:
id_pagamento
id_cliente
id_forma
detalhes
ativo
Relacionamentos:
Cliente → múltiplos pagamentos
Forma → múltiplos registros
🏷️ 7. Tabela: categorias
Objetivo:

Classificar produtos.

Campo:
nome
📦 8. Tabela: produtos
Objetivo:

Armazena os produtos disponíveis.

Campos:
nome
descricao
preco
estoque
id_categoria
Relacionamento:
Produto pertence a uma categoria
🧾 9. Tabela: pedidos
Objetivo:

Registra os pedidos realizados.

Campos:
id_cliente
data_pedido
status
valor_total
Relacionamento:
Cliente → múltiplos pedidos
🛒 10. Tabela: itens_pedido
Objetivo:

Detalhar os produtos dentro de cada pedido.

Campos:
id_pedido
id_produto
quantidade
preco_unitario
Relacionamento:
Pedido → múltiplos itens
Produto → pode estar em vários pedidos
💰 11. Tabela: pagamentos
Objetivo:

Registrar pagamentos dos pedidos.

Campos:
id_pedido
id_cliente_pagamento
valor
status
data_pagamento
Relacionamento:
Pedido → 1 pagamento (ou mais, dependendo da regra)
Usa método salvo (cliente_pagamentos)
🚚 12. Tabela: entregas
Objetivo:

Controlar a logística de entrega.

Campos:
id_pedido
status
codigo_rastreio
data_envio
data_entrega
🔗 Relacionamentos (Resumo)
Cliente → Endereços (1:N)
Cliente → Pedidos (1:N)
Pedido → Itens (1:N)
Produto → Categoria (N:1)
Pedido → Pagamento (1:1 ou 1:N)
Pedido → Entrega (1:1)
Cliente → Métodos de pagamento (1:N)
⚙️ Ordem de Execução do Script

Para evitar erro de chave estrangeira, execute nesta ordem:

clientes
clientes_pf
clientes_pj
enderecos
formas_pagamento
cliente_pagamentos
categorias
produtos
pedidos
itens_pedido
pagamentos
entregas
🧠 Boas Práticas Aplicadas
Uso de chaves primárias auto incrementais
Normalização (até 3FN)
Separação de entidades e especializações (PF/PJ)
Uso de chaves estrangeiras para integridade referencial
Campos UNIQUE para evitar duplicidade (CPF, CNPJ, email)
⚠️ Possíveis Melhorias

Se quiser evoluir o modelo:

Criar tabela de status padronizado (evitar VARCHAR livre)
Implementar soft delete (deleted_at)
Adicionar logs de alteração
Criar tabela de cupons/descontos
Separar estoque por lote ou localização
Indexar colunas usadas em busca (email, cpf, etc.)
🚀 Conclusão

Esse modelo cobre o fluxo completo de um e-commerce:

➡️ Cadastro → Produto → Pedido → Pagamento → Entrega

Ele é escalável, organizado e pronto para uso em sistemas reais ou projetos acadêmicos.
