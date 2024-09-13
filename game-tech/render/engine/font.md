## bit-font

位图文字

## sdf-font

基于纹理的 SDF 文字

1. 原理
	1. [Distance field fonts](https://libgdx.com/wiki/graphics/2d/fonts/distance-field-fonts)
2. 使用 FontAsset 创建 SDF font
	1.  [Unity - Manual: Introduction to font assets](https://docs.unity3d.com/Manual/UIE-font-asset.html#:~:text=SDF%20fonts,assets%20contain%20contour%20distance%20information.)
	2. [Unity - Manual: Font Asset properties reference](https://docs.unity3d.com/Manual/UIE-font-asset-properties.html)
3. 创建文字 Mesh
	1.  [GPOS — Glyph Positioning Table (OpenType 1.8.1) - Typography | Microsoft Learn](https://learn.microsoft.com/en-us/typography/opentype/otspec181/gpos)
	2. [Unity - Scripting API: MeshData](https://docs.unity3d.com/2020.3/Documentation/ScriptReference/Mesh.MeshData.html)

纯数学矢量 SDF
- [Direct font → SDF rendering](https://www.shadertoy.com/view/dls3Wr)