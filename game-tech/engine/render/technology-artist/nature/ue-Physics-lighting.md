
# UE 中的物理光照系统

UE4 里的灯光单位

- 定向光使用Lux为单位，并且无法切换灯光单位。
- 天光使用cd/m²为单位，也无法切换灯光单位。
- 点光，聚光灯，面光默认单位为cd，并可以切换Unitless以及Lumens（lm）。

点光：
- 1 lm ≈ 49.7 * 1 unitless
- 1 cd ≈ 12.6 * 1 lm

面光：
- 1 lm ≈ 199 * 1 unitless
- 1 cd ≈ 3.14 * 1 lm