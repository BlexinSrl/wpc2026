# Data Model: Rimuovere lo sconto

## Order

| Campo | Tipo | Regole |
| --- | --- | --- |
| Id | Guid | obbligatorio, non vuoto |
| Subtotal | decimal | non negativo, immutabile |
| DiscountPercentage | decimal | tra 0 e 30; 0 senza sconto |
| DiscountCode | string? | null senza sconto; non vuoto quando presente |
| Total | decimal | calcolato: Subtotal × (1 − DiscountPercentage / 100), arrotondato a 2 decimali |

## Transizioni

| Stato iniziale | Operazione | Stato finale |
| --- | --- | --- |
| senza sconto | ApplyDiscount("SAVE20", 20) | 20%, "SAVE20", totale 80 |
| con sconto | RemoveDiscount() | 0%, null, totale = subtotale |
| senza sconto | RemoveDiscount() | invariato |

## Invarianti

- DiscountCode è null se e solo se DiscountPercentage è 0 dopo una rimozione.
- Il totale non è mai negativo.
- Subtotal non cambia con nessuna operazione sullo sconto.
