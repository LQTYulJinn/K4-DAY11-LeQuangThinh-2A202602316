# So sánh L với R

Chỉ số L/R là thứ tự box cao ≥ H=40 trong từng frame, theo thứ tự XML; bắt đầu từ 1.
Box L trong ignore_region được báo IGNORE_SCOPE, không tính SPURIOUS.

## adasind_001320.jpg
- L2 edge IGNORE_SCOPE
## adasind_014670.jpg
- L7 edge IGNORE_SCOPE
- L1+R5 mid WRONG_CLASS
- L2 center SPURIOUS
## adasind_034080.jpg
- L9 edge IGNORE_SCOPE
- R9 center MISSING

## Theo zone
| zone | n_ref | matched | missing | spurious |
|---|---|---|---|---|
| center | 7 | 6 | 1 | 1 |
| mid | 9 | 8 | 1 | 1 |
| edge | 4 | 4 | 0 | 0 |
