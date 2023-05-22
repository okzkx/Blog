**VkImageViewCreateInfo**
**viewType** : **VK_IMAGE_VIEW_TYPE_CUBE**
**arrayLayers** : 6
**layerCount** : 6

**VkImageCreateInfo**
**flags** **VK_IMAGE_CREATE_CUBE_COMPATIBLE_BIT**
**arrayLayers** : 6
**imageType** : **VK_IMAGE_TYPE_2D**
**subresourceRange** **layerCount** : 6

layout (binding = X) uniform samplerCube cubeMapTexture;
 
...
 
vec4 textureSample = texture(cubeMapTexture, directionVector);



