# So sánh L với R

Chỉ số L/R là thứ tự box cao ≥ H=40 trong từng frame, theo thứ tự XML; bắt đầu từ 1.
Box L trong ignore_region được báo IGNORE_SCOPE, không tính SPURIOUS.

## adasind_062370.jpg
- L1+R1 mid ATTRIBUTE
- L3+R5 center WRONG_CLASS
- R7 center MISSING
- R8 center MISSING
## adasind_069450.jpg
## adasind_117120.jpg
- L1 center SPURIOUS
- L3 center SPURIOUS
- L4 center SPURIOUS

## Theo zone
| zone | n_ref | matched | missing | spurious |
|---|---|---|---|---|
| center | 13 | 10 | 3 | 4 |
| mid | 5 | 5 | 0 | 0 |
| edge | 2 | 2 | 0 | 0 |
