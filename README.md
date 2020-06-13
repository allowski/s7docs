# s7docs
Documentação S7 Digital para integrações.

# Descrição geral
O presente documento explica como funcionam o serviço de integrações com terceiros da plataforma S7. Com este documento você irá entender os fundamentos de funcionamento do serviço de pagamentos e contas:
- Como criar um pagamento
- Como criar regra de split para um pagamento especifico.
- Como criar contas e visualizar as contas existentes.

# Definição de uma conta
Uma conta S7 é uma carteira digital onde pessoas fisicas e juridicas podem receber pagamentos usando diferentes meios de pagamentos (Cartães, Boletos e outros) sendo estes considerados *CREDITOS* (Recebiveis), e realizar pagamentos de contas ou TED para terceiros sendo considerados *DEBITOS* (Pagamentos). 

Toda conta S7 possui um token únito hex composto por 6 dígitos, que identifica cada conta, este token será necessário para poder criar pagamentos, regras de split e acessar dados da conta.

# O que é uma transação?
Uma transação é qualquer movimentação realizada na conta do cliente.

# O que é um pagamento?
Um pagamento é uma transação de crédito em uma conta S7, é esta composto pelos seguintes campos:
- token: O token da conta principal (owner) da transação principal.
- amount: O valor bruto a ser pago pelo pagador (payer).
- description: Uma descrição dos da venda.

# O que é split?
Split é a divisão de um recebivel entre várias contas S7.

# O que são regras de split?
As regras de split definem como será realizado a divisão do valor do recebível, uma vez que o recebível seja efetuado (Pago pelo pagador) utilizando qualquer meio de pagamento.

Estas regras são compostas dos seguintes campos:
- amount: Valor a ser repassado.
- token: Token da conta á ser creditada.
- description: Descrição que ira aparecer no extrato da conta.
