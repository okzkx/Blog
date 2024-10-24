Homogeneous Clip Coordinates (HClip)

世界坐标 -> 视口坐标 -> 裁剪空间(ClipSpace, NDC) -> 其次裁剪空间(HClip)

- **Model Space to World Space**: Transforming the object's vertices from the object's local coordinate system (model space) to a common world coordinate system.    
- **World Space to View Space**: Applying the camera transformation to move the vertices from world space to view space.
- **View Space to Clip Space**: Performing perspective division and projection to map the view space coordinates to normalized device coordinates, where the visible space is defined as a cube with coordinates ranging from -1 to 1 in each dimension.
- **Clip Space to Homogeneous Clip Space**: Adding the homogeneous coordinate (w) to the clip space coordinates, creating homogeneous clip coordinates. This step prepares the vertices for further processing in the graphics pipeline.