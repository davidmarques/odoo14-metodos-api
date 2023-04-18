# odoo14-metodos-api
Este repositório tem documentada a estrutura das requisições do Odoo14

## Obter fatura(s) de um pedido
```
{
  "jsonrpc": "2.0",
  "method": "call",
  "params": {
    "args": [
      [
        NUMERO_DO_PEDIDO
      ]
    ],
    "kwargs": {
      "context": {
        "lang": "pt_BR",
        "tz": "America/Sao_Paulo",
        "uid": 2,
        "allowed_company_ids": [
          1
        ],
        "search_default_my_quotation": 1
      }
    },
    "method": "action_view_invoice",
    "model": "sale.order"
  },
  "id": 92236515
}
```

Um exemplo de resposta para esta requisição seria:

```
{
  "jsonrpc": "2.0",
  "id": 92236515,
  "result": {
    "id": 208,
    "name": "Faturas",
    "type": "ir.actions.act_window",
    "view_id": [
      633,
      "account.out.invoice.tree"
    ],
    "domain": "[('move_type', '=', 'out_invoice')]",
    "context": {
      "default_move_type": "out_invoice",
      "default_partner_id": 7947,
      "default_partner_shipping_id": 7947,
      "default_invoice_payment_term_id": 1,
      "default_invoice_origin": "S01565",
      "default_user_id": 8
    },
    "res_id": ID_DA_FATURA,
    "res_model": "account.move",
    "target": "current",
    "view_mode": "list,kanban,form",
    "views": [
      [
        636,
        "form"
      ],
      [
        633,
        "list"
      ],
      [
        false,
        "kanban"
      ]
    ],
    "limit": 80,
    "groups_id": [],
    "search_view_id": [
      638,
      "account.invoice.select"
    ],
    "filter": false,
    "search_view": "LONGO CONTEUDO SUPRIMIDO",
    "binding_model_id": false,
    "binding_type": "action",
    "binding_view_types": "list,form",
    "display_name": "Faturas"
  }
}
```


## Criar uma fatura
Para a criação de uma fatura, são necessárias 3 requisições
###1. Método: create | Módulo: sale.advance.payment.inv
Inicia o processo de criação da fatura
```
{
  "jsonrpc": "2.0",
  "method": "call",
  "params": {
    "args": [
      {
        "advance_payment_method": "delivered",
        "deduct_down_payments": true,
        "product_id": 15,
        "currency_id": 6,
        "fixed_amount": 0,
        "amount": 0,
        "deposit_account_id": 168,
        "deposit_taxes_id": [
          [
            6,
            false,
            []
          ]
        ]
      }
    ],
    "model": "sale.advance.payment.inv",
    "method": "create",
    "kwargs": {
      "context": {
        "lang": "pt_BR",
        "tz": "America/Sao_Paulo",
        "uid": 2,
        "allowed_company_ids": [
          1
        ],
        "active_model": "sale.order",
        "active_id": 1499,
        "active_ids": [
          1499
        ]
      }
    }
  },
  "id": 113470903
}
```
A resposta será simplesmente um ID

```
{
  "jsonrpc": "2.0",
  "id": 113470903,
  "result": 122
}
```

###2. Método: create_invoices | Módulo: sale.advance.payment.inv
Cria uma fatura provisória
O parâmetro enviado em args, será campo **result** recebido na requisição anterior

```
{
  "jsonrpc": "2.0",
  "method": "call",
  "params": {
    "args": [
      [
        122
      ]
    ],
    "kwargs": {
      "context": {
        "lang": "pt_BR",
        "tz": "America/Sao_Paulo",
        "uid": 2,
        "allowed_company_ids": [
          1
        ],
        "active_model": "sale.order",
        "active_id": 1499,
        "active_ids": [
          1499
        ],
        "open_invoices": true
      }
    },
    "method": "create_invoices",
    "model": "sale.advance.payment.inv"
  },
  "id": 245569960
}
```

A resposta segue o padrão a seguir

```
{
  "jsonrpc": "2.0",
  "id": 245569960,
  "result": {
    "id": 208,
    "name": "Faturas",
    "type": "ir.actions.act_window",
    "view_id": [
      633,
      "account.out.invoice.tree"
    ],
    "domain": "[('move_type', '=', 'out_invoice')]",
    "context": {
      "default_move_type": "out_invoice",
      "default_partner_id": 7370,
      "default_partner_shipping_id": 7370,
      "default_invoice_payment_term_id": 1,
      "default_invoice_origin": "S01499",
      "default_user_id": 8
    },
    "res_id": 119,
    "res_model": "account.move",
    "target": "current",
    "view_mode": "list,kanban,form",
    "views": [
      [
        636,
        "form"
      ],
      [
        633,
        "list"
      ],
      [
        false,
        "kanban"
      ]
    ],
    "limit": 80,
    "groups_id": [],
    "search_view_id": [
      638,
      "account.invoice.select"
    ],
    "filter": false,
    "search_view": "LONGO CONTEUDO SUPRIMIDO",
    "binding_model_id": false,
    "binding_type": "action",
    "binding_view_types": "list,form",
    "display_name": "Faturas"
  }
}
```
Será utilizado o valor encontrado em **res_id** na resposta da requisição anterior.
Atente-se ao conteúdo em **context** na resposta anterior, ele será utilizado na próxima

###3. Método: action_post | Módulo account.move
Confirma a fatura criada
```
{
  "jsonrpc": "2.0",
  "method": "call",
  "params": {
    "args": [
      [
        119
      ]
    ],
    "kwargs": {
      "context": {
        "lang": "pt_BR",
        "tz": "America/Sao_Paulo",
        "uid": 2,
        "allowed_company_ids": [
          1
        ],
        "active_model": "sale.advance.payment.inv",
        "open_invoices": true,
        "active_id": 122,
        "active_ids": [
          122
        ],
        "default_move_type": "out_invoice",
        "default_partner_id": 7370,
        "default_partner_shipping_id": 7370,
        "default_invoice_payment_term_id": 1,
        "default_invoice_origin": "S01499",
        "default_user_id": 8
      }
    },
    "method": "action_post",
    "model": "account.move"
  },
  "id": 34422302
}
```
