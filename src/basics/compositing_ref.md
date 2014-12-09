### 关于合成操作（Compositing）

Compositing operations affect the way colors and textures of different elements and styles interact with each other.

Without any compositing operations on a source it will just be painted directly over the destination – compositing operations allow us to change this. There are 33 compositing operations available in CartoCSS:

| Compositing operations in CartoCSS    |||
|-------------|---------------|----------|
| plus        | difference    | src      |
| minus       | exclusion     | dst      |
| multiply    | contrast      | src-over |
| screen      | invert        | dst-over |
| overlay     | invert-rgb    | src-in   |
| darken      | grain-merge   | dst-in   |
| lighten     | grain-extract | src-out  |
| color-dodge | hue           | dst-out  |
| color-burn  | saturation    | src-atop |
| hard-light  | color         | dst-atop |
| soft-light  | value         | xor      |
