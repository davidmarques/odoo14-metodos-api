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
        1565
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
    "res_id": 115,
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
