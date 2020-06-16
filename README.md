# s7docs
Documentação S7 Digital para integrações.

# 1. Descrição geral
O presente documento explica como funcionam o serviço de integrações com terceiros da plataforma S7. Com este documento você irá entender os fundamentos de funcionamento do serviço de pagamentos e contas:
- Como criar um pagamento
- Como criar regra de split para um pagamento especifico.
- Como criar contas e visualizar as contas existentes.

# 2. Definição de uma conta
Uma conta S7 é uma carteira digital onde pessoas fisicas e juridicas podem receber pagamentos usando diferentes meios de pagamentos (Cartães, Boletos e outros) sendo estes considerados *CREDITOS* (Recebiveis), e realizar pagamentos de contas ou TED para terceiros sendo considerados *DEBITOS* (Pagamentos). 

Toda conta S7 possui um token únito hex composto por 6 dígitos, que identifica cada conta, este token será necessário para poder criar pagamentos, regras de split e acessar dados da conta.

# 3. O que é uma transação?
Uma transação é qualquer movimentação realizada na conta do cliente.

# 4. O que é um pagamento?
Um pagamento é uma transação de crédito em uma conta S7, é esta composto pelos seguintes campos:
- token: O token da conta principal (owner) da transação principal.
- amount: O valor bruto a ser pago pelo pagador (payer).
- description: Uma descrição dos da venda.
- parties: Lista das regras de split, como descritas.
- reference: Código de referencia externa (exemplo: COD12345).
- notificationUrl: Uma url para informar quando for concluía a transação [Via POST].

# 5. O que é split?
Split é a divisão de um recebivel entre várias contas S7.

# 6. O que são regras de split?
As regras de split definem como será realizado a divisão do valor do recebível, uma vez que o recebível seja efetuado (Pago pelo pagador) utilizando qualquer meio de pagamento.

## 6.1 Estas regras são compostas dos seguintes campos:
- amount: Valor a ser repassado.
- token: Token da conta á ser creditada.
- description: Descrição que ira aparecer no extrato da conta.

## 6.2 Exemplo de transação:
A padaria do dom João (token: HE123) irá realizar uma venda de R$ 100,00 para o Daniel, nesta venda terá um custo de entrega de R$ 8,00 feito pelo Pedro, quem tem uma conta com o token ABC12. Seguindo esta lógica de negócios, teremos que declarar as seguintes variáveis:

- Valor Bruto: R$ 100,00
- Valor do Envio: R$ 8,00
- Taxa da transação: 4% => R$ 100,00 * 4% = R$ 4,00

Considerando todos os valores que insidem sobre a transação o valor líquido a ser lançado na conta do cliente será de:
R$ 100,00 – Valor bruto da venda.
- R$ 8,00 – Valor do pedro.
- R$ 4,00 – Esta taxa é calculada de forma automática com base no contrato.
__________
R$ 88,00  – Valor líquido a ser lançado como crédito na conta do cliente.


Será definida a seguinte regra para esta transação:
```JS
{
  "amount": 100.0,
  "description": "Venda 1234",
  "token": "HE123",
  "reference": "1234",
  "notificationUrl": "http://minhaloja.net/order/1234",
  "parties": [
    "description": "Entrega venda 1234",
    "amount": 8.0,
    "token": "ABC12"
  ]
}
```
Uma vez que o pagamento for concluído, os valores serão lançados nas contas dos clientes. 

Lembrando que, o valor líquido não pode ser negativo, em cujo caso o sistema irá retornar um erro do tipo 400.

# Rateio da taxa
Para cada recebivel insidira uma taxa que será parametrizada no momento da criação da conta do estabelecimento, esta taxa esta composta por duas partes:
- Taxa administrativa S7. – Esta é a taxa que foi negociada entre a S7 e a representante.
- Taxa do representante. – Este é o overprice definido pelo representante.

Á taxa a ser cobrada é definida no momento da criação da conta, como mencionado acima. Porem algumas transações podem ter divergência de taxa devído à bandeira do cartão (Amex e outros) é qualquer transação que não seja enquadrada como presencial, por exemplo: tarja magnética. Nesse cenário o cliente é quem paga o over gerado, pelo que pode ficar menor o valor líquido.


