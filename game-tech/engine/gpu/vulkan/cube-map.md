# Vulkan 创建 CubeMap

### 参考
[Vulkan Adventures: Cube map tutorial! - satellitnorden](https://satellitnorden.wordpress.com/2018/01/23/vulkan-adventures-cube-map-tutorial/)

### 摘要
需要在 Create Image 和 Create Image View 进行配置

**VkImageCreateInfo**
1. **flags** : VK_IMAGE_CREATE_CUBE_COMPATIBLE_BIT
2. **arrayLayers** : 6
3. **imageType** : **VK_IMAGE_TYPE_2D**
4. **subresourceRange** **layerCount** : 6

**VkImageViewCreateInfo**
1. **viewType** : **VK_IMAGE_VIEW_TYPE_CUBE**
2. **arrayLayers** : 6
3. **layerCount** : 6
 
```glsl
layout (binding = X) uniform samplerCube cubeMapTexture;
vec4 textureSample = texture(cubeMapTexture, directionVector);
```
