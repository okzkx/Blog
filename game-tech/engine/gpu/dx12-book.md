# DirectX 12 3D游戏开发实战指南

Direct3D 12 编程知识库

## 前言

DirectX 12代表了图形编程的重大演进，为开发者提供了对现代GPU硬件的底层访问能力。本指南涵盖了DirectX 12开发的基础概念和实用实现技术。

### 开发环境配置
- **IDE**: Visual Studio 2019或更高版本（推荐）
- **SDK**: Windows 10 SDK（最低版本 10.0.17763.0）
- **图形调试器**: PIX for Windows 或 Visual Studio 图形调试器
- **示例代码**: [Microsoft DirectX 12 示例](https://github.com/Microsoft/DirectX-Graphics-Samples)

### 项目配置
```cpp
// 必需的头文件
#include <d3d12.h>
#include <dxgi1_6.h>
#include <D3Dcompiler.h>
#include <DirectXMath.h>

// 必需的库文件
#pragma comment(lib, "d3d12.lib")
#pragma comment(lib, "dxgi.lib")
#pragma comment(lib, "d3dcompiler.lib")
```

## 必备的数学基础

### 向量代数
向量是3D图形编程的基础，用于表示位置、方向和其他几何属性。

**核心概念：**
- **向量运算**: 加法、减法、标量乘法
- **点积**: `a·b = |a||b|cos(θ)` - 用于光照计算、角度确定
- **叉积**: `a×b` - 产生垂直向量，用于计算表面法线
- **标准化**: 将向量转换为单位长度，用于方向计算

```cpp
// DirectXMath 向量运算
XMVECTOR v1 = XMVectorSet(1.0f, 2.0f, 3.0f, 0.0f);
XMVECTOR v2 = XMVectorSet(4.0f, 5.0f, 6.0f, 0.0f);

// 点积
XMVECTOR dotResult = XMVector3Dot(v1, v2);

// 叉积
XMVECTOR crossResult = XMVector3Cross(v1, v2);

// 标准化
XMVECTOR normalized = XMVector3Normalize(v1);
```

### 矩阵代数
矩阵表示3D空间中的变换和坐标系转换。

**矩阵类型：**
- **单位矩阵**: 无变换
- **平移矩阵**: 位置变化
- **旋转矩阵**: 方向变化
- **缩放矩阵**: 大小变化
- **投影矩阵**: 3D到2D转换

```cpp
// 创建变换矩阵
XMMATRIX translation = XMMatrixTranslation(x, y, z);
XMMATRIX rotation = XMMatrixRotationRollPitchYaw(pitch, yaw, roll);
XMMATRIX scaling = XMMatrixScaling(sx, sy, sz);

// 组合变换
XMMATRIX world = scaling * rotation * translation;
```

### 坐标变换
图形管线涉及多个坐标空间的变换：

1. **模型空间**: 物体的局部坐标
2. **世界空间**: 全局场景坐标
3. **观察空间**: 相对于摄像机的坐标
4. **投影空间**: 透视校正后的坐标
5. **屏幕空间**: 最终的2D坐标

**变换管线：**
```
模型 → 世界 → 观察 → 投影 → 屏幕
```

## Direct3D 基础知识

### 预备知识

#### 概述
Direct3D 12是一个底层图形API，提供对现代GPU硬件功能的直接访问。与以前的版本不同，DX12提供了：

- **显式多线程**: 应用程序控制GPU工作提交
- **降低CPU开销**: 最小化驱动程序干预
- **内存管理**: 直接控制GPU内存分配
- **管线状态对象**: 不可变状态以获得最优性能

**核心架构组件：**
```cpp
// DX12核心接口
ID3D12Device*           device;         // GPU设备抽象
ID3D12CommandQueue*     commandQueue;   // GPU工作提交
ID3D12CommandAllocator* cmdAllocator;   // 命令内存管理
ID3D12GraphicsCommandList* commandList; // 记录GPU命令
ID3D12Fence*            fence;          // CPU-GPU同步
```

#### 组件对象模型 (COM)
DirectX 12使用COM进行对象生命周期管理和接口访问。

**使用ComPtr的智能指针管理：**
```cpp
#include <wrl/client.h>
using Microsoft::WRL::ComPtr;

// 智能指针声明
ComPtr<ID3D12Device> device;
ComPtr<ID3D12CommandQueue> commandQueue;

// 关键方法：
// 1. Get() - 返回原始指针用于API调用
ID3D12Device* rawDevice = device.Get();

// 2. GetAddressOf() - 获取地址用于输出参数
HRESULT hr = D3D12CreateDevice(nullptr, D3D_FEATURE_LEVEL_11_0, 
                               IID_PPV_ARGS(device.GetAddressOf()));

// 3. Reset() - 释放当前对象并重置为nullptr
device.Reset();
```

**错误处理：**
```cpp
#define ThrowIfFailed(x) \
{ \
    HRESULT hr__ = (x); \
    if(FAILED(hr__)) { throw std::exception(); } \
}
```

#### 纹理格式
DX12支持针对不同用例优化的各种纹理格式：

**常见颜色格式：**
- **DXGI_FORMAT_R8G8B8A8_UNORM**: 32位RGBA（最常见）
- **DXGI_FORMAT_R16G16B16A16_FLOAT**: 64位HDR格式
- **DXGI_FORMAT_R32G32B32A32_FLOAT**: 128位高精度
- **DXGI_FORMAT_BC1_UNORM**: DXT1压缩（无alpha）
- **DXGI_FORMAT_BC3_UNORM**: DXT5压缩（带alpha）

**深度/模板格式：**
- **DXGI_FORMAT_D32_FLOAT**: 32位深度
- **DXGI_FORMAT_D24_UNORM_S8_UINT**: 24位深度 + 8位模板
- **DXGI_FORMAT_D16_UNORM**: 16位深度（移动端友好）

#### 交换链和页面翻转
交换链管理渲染帧到显示器的呈现。

**缓冲区类型：**
- **前缓冲区**: 当前显示的帧
- **后缓冲区**: 正在渲染的帧
- **呈现**: 使后缓冲区可见

**缓冲策略：**
```cpp
// 双缓冲（2个缓冲区）
DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {};
swapChainDesc.BufferCount = 2;
swapChainDesc.Width = width;
swapChainDesc.Height = height;
swapChainDesc.Format = DXGI_FORMAT_R8G8B8A8_UNORM;
swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_DISCARD;

// 三缓冲（3个缓冲区）- 减少输入延迟
swapChainDesc.BufferCount = 3;
```

#### 深度缓冲
深度缓冲通过存储每像素深度信息来实现正确的3D对象可见性。

**关键概念：**
- **深度范围**: 标准化设备坐标中的0.0（近）到1.0（远）
- **视锥体**: 定义可见空间的3D体积
- **Z缓冲区**: 深度缓冲区的别名
- **深度测试**: 确定像素可见性的比较

**深度缓冲区设置：**
```cpp
// 创建深度缓冲区纹理
D3D12_RESOURCE_DESC depthStencilDesc = {};
depthStencilDesc.Dimension = D3D12_RESOURCE_DIMENSION_TEXTURE2D;
depthStencilDesc.Width = width;
depthStencilDesc.Height = height;
depthStencilDesc.DepthOrArraySize = 1;
depthStencilDesc.MipLevels = 1;
depthStencilDesc.Format = DXGI_FORMAT_D24_UNORM_S8_UINT;
depthStencilDesc.SampleDesc.Count = 1;
depthStencilDesc.Flags = D3D12_RESOURCE_FLAG_ALLOW_DEPTH_STENCIL;

ThrowIfFailed(device->CreateCommittedResource(
    &CD3DX12_HEAP_PROPERTIES(D3D12_HEAP_TYPE_DEFAULT),
    D3D12_HEAP_FLAG_NONE,
    &depthStencilDesc,
    D3D12_RESOURCE_STATE_DEPTH_WRITE,
    &depthOptimizedClearValue,
    IID_PPV_ARGS(&depthStencilBuffer)));
```

#### 资源与描述符
DX12使用描述符为GPU提供对纹理和缓冲区等资源的访问。

**核心概念：**
- **绘制调用**: 渲染几何体的GPU命令
- **资源绑定**: 将资源连接到着色器阶段
- **描述符**: 描述GPU资源属性的轻量级结构
- **描述符堆**: 包含描述符表的GPU可见内存

**描述符类型：**
```cpp
// 着色器资源视图（SRV）- 只读纹理/缓冲区
D3D12_SHADER_RESOURCE_VIEW_DESC srvDesc = {};
srvDesc.Format = DXGI_FORMAT_R8G8B8A8_UNORM;
srvDesc.ViewDimension = D3D12_SRV_DIMENSION_TEXTURE2D;
srvDesc.Texture2D.MipLevels = 1;

// 渲染目标视图（RTV）- 输出颜色目标
D3D12_RENDER_TARGET_VIEW_DESC rtvDesc = {};
rtvDesc.Format = DXGI_FORMAT_R8G8B8A8_UNORM;
rtvDesc.ViewDimension = D3D12_RTV_DIMENSION_TEXTURE2D;

// 常量缓冲区视图（CBV）- 着色器常量
D3D12_CONSTANT_BUFFER_VIEW_DESC cbvDesc = {};
cbvDesc.BufferLocation = constantBuffer->GetGPUVirtualAddress();
cbvDesc.SizeInBytes = (sizeof(ConstantBufferData) + 255) & ~255; // 256字节对齐
```

#### 多重采样抗锯齿（MSAA）
MSAA通过对每像素采样多个点来减少视觉伪影（锯齿）。

**抗锯齿技术：**
- **超采样（SSAA）**: 以更高分辨率渲染，然后降采样
- **多重采样抗锯齿（MSAA）**: 每像素多个覆盖采样
- **时间抗锯齿（TAA）**: 使用跨帧的时间信息
- **快速近似抗锯齿（FXAA）**: 后处理边缘平滑

```cpp
// MSAA采样数（必须是2的幂）
const UINT sampleCount = 4; // 4x MSAA

// 检查MSAA支持
D3D12_FEATURE_DATA_MULTISAMPLE_QUALITY_LEVELS msQualityLevels = {};
msQualityLevels.Format = DXGI_FORMAT_R8G8B8A8_UNORM;
msQualityLevels.SampleCount = sampleCount;
device->CheckFeatureSupport(D3D12_FEATURE_MULTISAMPLE_QUALITY_LEVELS,
                           &msQualityLevels, sizeof(msQualityLevels));
```

### CPU和GPU之间的交互

高效的CPU-GPU交互对于高性能DirectX 12应用程序至关重要。关键原则是最大化并行执行同时最小化同步点。

#### 命令队列和命令列表
DX12使用基于命令的架构进行GPU工作提交：

**核心概念：**
- **命令队列**: GPU上处理命令的环形缓冲区
- **命令列表**: CPU端用于记录GPU命令的容器
- **命令分配器**: 管理命令列表的内存

```cpp
// 创建命令队列
D3D12_COMMAND_QUEUE_DESC queueDesc = {};
queueDesc.Type = D3D12_COMMAND_LIST_TYPE_DIRECT;
queueDesc.Flags = D3D12_COMMAND_QUEUE_FLAG_NONE;

ComPtr<ID3D12CommandQueue> commandQueue;
device->CreateCommandQueue(&queueDesc, IID_PPV_ARGS(&commandQueue));

// 记录命令
commandList->SetGraphicsRootSignature(rootSignature.Get());
commandList->SetPipelineState(pipelineState.Get());
commandList->DrawInstanced(vertexCount, 1, 0, 0);

// 关闭并执行
commandList->Close();
ID3D12CommandList* cmdLists[] = { commandList.Get() };
commandQueue->ExecuteCommandLists(_countof(cmdLists), cmdLists);
```

#### CPU与GPU同步
正确的同步可以防止资源冲突同时保持性能：

**基于围栏的同步：**
```cpp
class FenceManager {
public:
    void Initialize(ID3D12Device* device, ID3D12CommandQueue* queue) {
        device->CreateFence(0, D3D12_FENCE_FLAG_NONE, IID_PPV_ARGS(&fence));
        fenceEvent = CreateEvent(nullptr, FALSE, FALSE, nullptr);
        commandQueue = queue;
        fenceValue = 1;
    }
    
    UINT64 Signal() {
        UINT64 currentFenceValue = fenceValue;
        commandQueue->Signal(fence.Get(), currentFenceValue);
        fenceValue++;
        return currentFenceValue;
    }
    
    void WaitForValue(UINT64 value) {
        if (fence->GetCompletedValue() < value) {
            fence->SetEventOnCompletion(value, fenceEvent);
            WaitForSingleObject(fenceEvent, INFINITE);
        }
    }
    
private:
    ComPtr<ID3D12Fence> fence;
    HANDLE fenceEvent;
    ID3D12CommandQueue* commandQueue;
    UINT64 fenceValue;
};
```

#### 资源转换
DX12需要显式的资源状态管理来防止资源冒险：

```cpp
// 资源状态转换
void TransitionResource(ID3D12GraphicsCommandList* cmdList,
                       ID3D12Resource* resource,
                       D3D12_RESOURCE_STATES stateBefore,
                       D3D12_RESOURCE_STATES stateAfter) {
    D3D12_RESOURCE_BARRIER barrier = {};
    barrier.Type = D3D12_RESOURCE_BARRIER_TYPE_TRANSITION;
    barrier.Transition.pResource = resource;
    barrier.Transition.StateBefore = stateBefore;
    barrier.Transition.StateAfter = stateAfter;
    
    cmdList->ResourceBarrier(1, &barrier);
}
```

### 初始化 Direct3D

正确的初始化对于DX12应用程序至关重要：

```cpp
class D3D12Renderer {
public:
    void Initialize(HWND hwnd, int width, int height) {
        EnableDebugLayer();
        CreateDevice();
        CreateCommandObjects();
        CreateSwapChain(hwnd, width, height);
        CreateDescriptorHeaps();
        CreateRenderTargets();
        CreateDepthStencil(width, height);
    }
    
private:
    void CreateDevice() {
        // 创建DXGI工厂
        ThrowIfFailed(CreateDXGIFactory2(0, IID_PPV_ARGS(&factory)));
        
        // 查找硬件适配器
        ComPtr<IDXGIAdapter1> adapter;
        for (UINT adapterIndex = 0; ; ++adapterIndex) {
            if (DXGI_ERROR_NOT_FOUND == factory->EnumAdapters1(adapterIndex, &adapter)) {
                break;
            }
            
            // 检查适配器是否支持D3D12
            if (SUCCEEDED(D3D12CreateDevice(adapter.Get(), D3D_FEATURE_LEVEL_11_0,
                                          _uuidof(ID3D12Device), nullptr))) {
                break;
            }
        }
        
        // 创建设备
        ThrowIfFailed(D3D12CreateDevice(adapter.Get(), D3D_FEATURE_LEVEL_11_0,
                                       IID_PPV_ARGS(&device)));
    }
};
```

### 计时和动画

准确的计时对于流畅的动画和一致的帧率至关重要：

```cpp
class GameTimer {
public:
    float TotalTime() const {
        if (stopped) {
            return (float)(((stopTime - pausedTime) - baseTime) * secondsPerCount);
        } else {
            return (float)(((currTime - pausedTime) - baseTime) * secondsPerCount);
        }
    }
    
    float DeltaTime() const {
        return (float)deltaTime;
    }
    
    void Tick() {
        if (stopped) {
            deltaTime = 0.0;
            return;
        }
        
        QueryPerformanceCounter((LARGE_INTEGER*)&currTime);
        deltaTime = (currTime - prevTime) * secondsPerCount;
        prevTime = currTime;
        
        if (deltaTime < 0.0) {
            deltaTime = 0.0;
        }
    }
    
private:
    double secondsPerCount;
    double deltaTime;
    __int64 baseTime;
    __int64 pausedTime;
    __int64 currTime;
    bool stopped;
};
```

### 调试DirectX 12应用程序

DX12提供了强大的调试工具：

#### 调试层配置
```cpp
void EnableAdvancedDebugFeatures() {
#ifdef _DEBUG
    // 启用调试层
    ComPtr<ID3D12Debug> debugController;
    if (SUCCEEDED(D3D12GetDebugInterface(IID_PPV_ARGS(&debugController)))) {
        debugController->EnableDebugLayer();
        
        // 启用基于GPU的验证（较慢但更全面）
        ComPtr<ID3D12Debug1> debugController1;
        if (SUCCEEDED(debugController->QueryInterface(IID_PPV_ARGS(&debugController1)))) {
            debugController1->SetEnableGPUBasedValidation(true);
        }
    }
#endif
}
```

#### PIX for Windows
- **GPU捕获**: 分析单个帧
- **CPU采样**: 分析应用程序性能
- **内存使用**: 跟踪GPU内存分配

#### 常见调试模式
```cpp
// 验证资源状态
void ValidateResourceState(ID3D12Resource* resource, D3D12_RESOURCE_STATES expectedState) {
#ifdef _DEBUG
    // 实现取决于调试工具
#endif
}

// 为调试命名对象
resource->SetName(L"主渲染目标");
commandList->SetName(L"主命令列表");
```

## 总结

DirectX 12引入了图形编程的范式转变：

**主要优势：**
- **更低的CPU开销**: 减少驱动程序干预
- **多线程**: 显式控制GPU工作提交
- **内存管理**: 直接资源控制
- **性能**: 更好地利用现代硬件

**需要掌握的核心概念：**
1. **资源管理**: 屏障、状态和生命周期
2. **命令记录**: 列表、分配器和队列
3. **同步**: 围栏和GPU/CPU协调
4. **描述符管理**: 堆和资源绑定
5. **管线状态**: 不可变状态对象

**最佳实践：**
- 最小化CPU-GPU同步点
- 使用多个帧资源进行流畅渲染
- 实现适当的资源状态跟踪
- 利用多线程进行命令记录
- 使用适当的工具进行早期和频繁的性能分析

**学习资源：**
- [Microsoft DirectX 12编程指南](https://docs.microsoft.com/en-us/windows/win32/direct3d12/directx-12-programming-guide)
- [GitHub上的DirectX 12示例](https://github.com/Microsoft/DirectX-Graphics-Samples)
- [GPU Open文档](https://gpuopen.com/learn/)
- [实时渲染（第4版）](https://www.realtimerendering.com/)

DirectX 12代表了高性能图形编程的未来，提供了对现代GPU硬件前所未有的控制，同时要求开发者管理以前由驱动程序处理的复杂性。