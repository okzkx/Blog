
[AoS and SoA](https://en.wikipedia.org/wiki/AoS_and_SoA#:~:text=Structure%20of%20arrays%20(SoA)%20is,one%20parallel%20array%20per%20field.)

Structure of arrays
- easier manipulation with packed [SIMD instructions](https://en.wikipedia.org/wiki/SIMD_instruction "SIMD instruction") in most [instruction set architectures](https://en.wikipedia.org/wiki/Instruction_set_architecture "Instruction set architecture")
- allowing more data to fit onto a single cache line.

```
struct pointlist3D {
    float x[N];
    float y[N];
    float z[N];
};
struct pointlist3D points;
float get_point_x(int i) { return points.x[i]; }
```
Array of structures

```
struct point3D {
    float x;
    float y;
    float z;
};
struct point3D points[N];
float get_point_x(int i) { return points[i].x; }
```