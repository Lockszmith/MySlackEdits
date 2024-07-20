# Layered DAX calculation
## My starting point
| TrnsctID | LineID | Month  | ContextID | LineStart  | LineEnd    | Cat  | ItemSKU | Quantity |
| -------: | ------ | ------ | --------- | ---------- | ---------- | ---- | --------- | ---: |
|     1001 | 100101 | 202401 | Context01 | 2024-01-01 | 2024-07-15 | CatA | CatA-Sku1 | 10 |
| 1001 | 100102 | 202401 | Context01 | 2024-01-01 | 2024-07-15 | CatA | CatA-Sku1 | 10 |
| 1001 | 100101 | 202401 | Context01 | 2024-01-01 | 2024-07-15 | CatA | CatA-Sku1 | 10 |
| 1001 | 100101 | 202401 | Context01 | 2024-01-01 | 2024-07-15 | CatA | CatA-Sku1 | 10 |
| 1001 | 100101 | 202401 | Context01 | 2024-01-01 | 2025-01-01 | CatA | CatA-Sku1 | 10 |
| 1001 | 100101 | 202401 | Context01 | 2024-01-01 | 2025-01-01 | CatA | CatA-Sku1 | 10 |
| 1001 | 100101 | 202401 | Context01 | 2024-01-01 | 2025-01-01 | CatA | CatA-Sku1 | 10 |
| 1001 | 100101 | 202401 | Context01 | 2024-01-01 | 2025-01-01 | CatA | CatA-Sku1 | 10 |
| 1001 | 100101 | 202401 | Context01 | 2024-01-01 | 2025-01-01 | CatA | CatA-Sku1 | 10 |
| 1001 | 100101 | 202401 | Context01 | 2024-01-01 | 2025-01-01 | CatA | CatA-Sku1 | 10 |
| 1001 | 100101 | 202401 | Context01 | 2024-01-01 | 2025-01-01 | CatA | CatA-Sku1 | 10 |
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTg0NDE3NzU2M119
-->