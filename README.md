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
