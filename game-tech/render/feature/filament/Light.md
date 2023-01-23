# Light

### Units


| Photometric term    | Notation | Unit                   |
| ------------------- | -------- | ---------------------- |
| Luminous power      | Φ       | Lumen (lm)             |
| Luminous intensity  | I        | Candela (cd) or lm/sr  |
| Illuminance         | E        | Lux (lx) or lm/m2      |
| Luminance           | L        | Nit (nt) or cd/m2      |
| Radiant power       | Φe      | Watt (W)               |
| Luminous efficacy   | η       | Lumens per watt (mW/W) |
| Luminous efficiency | V        | Percentage (%)         |


```
vec3 l = normalize(-lightDirection);
float NoL = clamp(dot(n, l), 0.0, 1.0);

// lightIntensity is the illuminance
// at perpendicular incidence in lux
float illuminance = lightIntensity * NoL;
vec3 luminance = BSDF(v, l) * illuminance;
```
