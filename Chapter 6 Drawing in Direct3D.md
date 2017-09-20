# <element id = "6"> Chapter 6 DRAWING IN DIRECT3D </element>

��֮ǰ���½��У�������Ҫ����������Ⱦ�ܵ��е�һЩ�����һЩ��ѧ֪ʶ��
�ڱ����У����Ǿ�תΪ��Ҫ����`Direct3D API`���Ӷ���������Ⱦ�ܵ�����д������ɫ����������ɫ�����Լ��ύͼ�ε���Ⱦ�ܵ����һ�������
�ڱ��½�����ʱ�����Ǿ��ܹ��ɹ�����һ���������ˡ�

**Ŀ��:**
- �˽�һЩ���ڶ��壬�洢����ͼ�ε�`Direct3D API`��
- �˽�ѧϰ��α�д�����Ķ�����ɫ����������ɫ�����롣
- �˽����ʹ�ùܵ�״̬��������Ⱦ�ܵ���
- �˽���δ���һ�������岢�ҽ���󶨵���Ⱦ�ܵ���ȥ��������Ϥ`Root Signature`(���ҽ�������������Ҫ�����ڶ�������ɫ��ʹ����Щ��Դ)��

## <element id = "6.1"> 6.1 VERTICES AND INPUT LAYOUTS </element>

�ع�5.5.1����`Direct3D`�У������������λ�����������Ը���һЩ�������ݡ�
Ϊ�˴���һ���Զ���Ķ����ʽ��������Ҫ����һ���ṹ������������Ҫ�����㸽�ӵ����ݡ�
����;ٳ�������ͬ�Ķ����ʽ�����ӣ�����һ��������λ�ú���ɫ���ݣ�����һ��������λ�ã����������Լ����������������ݡ�

```C++

struct Vertex1
{
    Float3 Pos;
    Float4 Color;
};

struct Vertex2
{
    Float3 Pos;
    Float3 Normal;
    Float2 Tex0;
    Float2 Tex1;
};

```

������Ȼ�����˶����ʽ����������ͬ����Ҫ����`Direct3D`���ǵĶ����ʽ����Ȼ�Ļ�`Direct3D`��û��֪�����Ǹ����㸽������Щ��������ݡ�
����ʹ��`D3D12_INPUT_LAYOUT_DESC`��������¡�

```C++

struct D3D12_INPUT_LAYOUT_DESC 
{
    const D3D12_INPUT_ELEMENT_DESC *pInputElementDescs;
    UINT NumElements;
};

```

- `pInputElementDescs`: һ�����飬����`Direct3D`���ǵĶ��㸽�ӵ�������Ϣ��
- `NumElements`: ����Ĵ�С��

`pInputElementDescs`�����ÿһ��Ԫ�ض��൱�ڶ����ʽ��һ������������Ϣ��
���������ǵ�һ�������ʽ������������������Ϣ�Ļ�����ô�������Ĵ�С����Ҫ��������

```C++
struct D3D12_INPUT_ELEMENT_DESC
{
    LPCSTR SemanticName;
    UINT SemanticIndex;
    DXGI_FORMAT Format;
    UINT InputSlot;
    UINT AlignedByteOffset;
    D3D12_INPUT_CLASSIFICATION InputSlotClass;
    UINT InstanceDataStepRate;
};
```

- `SemanticName`: һ��������ϵԪ�ص��ַ���������ֵ�����ںϷ��ķ�Χ�ڡ�����ʹ��������������е�Ԫ��ӳ�䵽��ɫ��������������ȥ���μ�ͼƬ[6.1](#Image6.1)��
<img src="Images/6.1.png" id = "Image6.1"> </img>
- `SemanticIndex`: ����ֵ��������Բμ�ͼƬ[6.1](#Image6.1)������һ��������ܻ��в�ֹһ���������꣬���Ǳ���������Щ�������꣬������Ǿͼ���������ֵ������һ����������Ķ���������ꡣ���һ��`semanticName`û�м��������Ļ���Ĭ��Ϊ����ֵΪ0������`POSITION`����`POSITION0`��
- `Format`: ���ӵ�������Ϣ�ĸ�ʽ��������`DXGI_FORMAT`��
- `InputSlot`: ָ�����Ԫ�ش��ĸ�����ڽ��룬`Direct3D`֧��16�������(0-15)���붥�����ݡ�����������˵������ֻʹ��0����ڡ�
- `AlignedByteOffset`: �ڴ�ƫ��������λ���ֽڡ��Ӷ���ṹ�Ŀ��˵����Ԫ�صĿ��˵��ֽڴ�С��������һ�����ӡ�
    ```C++
    struct Vertex
    {
        Float3 Pos; // 0-byte offset
        Float3 Normal; // 12-byte offset
        Float2 Tex0; // 24-byte offset
        Float2 Tex1; // 32-byte offset
    };
    ```
- `InputSlotClass`: ����Ĭ��ʹ��`D3D12_INPUT_PER_VERTEX_DATA`�������һ������������`Instancing`�����ġ�

�������Ǹ�һ������: 

```C++
struct Vertex
{
    Float3 Pos; // 0-byte offset
    Float3 Normal; // 12-byte offset
    Float2 Tex0; // 24-byte offset
    Float2 Tex1; // 32-byte offset
};

D3D12_INPUT_ELEMENT_DESC desc [] = 
{
    	{��POSITION��, 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D12_INPUT_PER_VERTEX_DATA, 0},
        {��NORMAL��,   0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D12_INPUT_PER_VERTEX_DATA, 0},
        {��TEXCOORD��, 0, DXGI_FORMAT_R32G32_FLOAT,    0, 24, D3D12_INPUT_PER_VERTEX_DATA, 0},
        {��TEXCOORD��, 1, DXGI_FORMAT_R32G32_FLOAT,    0, 32, D3D12_INPUT_PER_VERTEX_DATA, 0}     
};
```
 
 ## <element id = "6.2"> 6.2 VERTEX BUFFERS </element>

 Ϊ���ܹ���`GPU`����һ�鶥���������Ϣ��������Ҫ��������Ϣ���뵽`GPU`��Դ��ȥ(**ID3D12Resource**)�����ǳ�֮Ϊ���塣
 ���ǽ��洢�������ݵĻ����֮Ϊ���㻺��(`Vertex Buffer`)��
 ����������һЩ����ֻ��һά��������û��������ϸ(`MipMaps`)��������(`filters`)�����ز���(`multisampling`)��Щ������
 ������ʲôʱ���������Ҫ��`GPU`�ṩһ��������Ϣ���綥��������Ϣ�����Ƕ���ʹ�û�����ʵ�֡�

 ������֮ǰ����ͨ�����`D3D12_RESOURCE_DESC`����������һ��`ID3D12Resource`��
 �������Ҳ��ͨ�������ķ�ʽ������һ�����壬�����`D3D12_RESOURCE_DESC`�ṹ����������Ҫ�����Ļ�������ԣ�Ȼ��ʹ��`ID3D12Device::CreateCommittedResource`�������塣

 ��`d3dx12.h`���ṩ��һ����������`CD3D12_RESOURCE_DESC`��������Դ���������ȥ�μ�`d3dx12.h`��

 **ע��������`Direct3D 12`�в�û�ж���һ���������������ʾ�����Դ��һ�����������һ������(`Direct3D 11`������������)��
 ��������ڴ�����Դ��ʱ����Ҫͨ��`	D3D12_RESOURCE_DESC::D3D12_RESOURCE_DIMENSION`��ָ����Դ�����͡�**

 ���ھ�̬��ͼ��(��ÿһ֡��ͨ������ı�)�����ǻὫ���Ķ��㻺�����Ĭ�϶���(**Default Heap**)�Ա������ŵ����ܣ�ͨ���󲿷���Ϸ�е�ͼ�ζ������Ĭ�϶��У������������������Σ�����ȡ�
 ��Ϊ�����ڴ��������Ķ��㻺���`GPU`ֻ���ȡ����Ķ�����Ϣ���������������������������飬��˽������Ĭ�϶���������õġ�
 Ȼ������`GPU`�����ܽ�����д�뵽����Ĭ�϶��������Դ�����Ǹ���ν���ʼ�Ķ������ݷŵ�������ȥ��

 Ϊ��ʵ�������������Ҫ����һ���ϴ�������Դ(`Upload Buffer`)�����ᱻ�����ϴ�����(**Upload Heap**)��
 �ع�4.3.8�½ڣ�������Ҫ�����ݴ��ڴ濽�����Դ���ȥ��ʱ�������ύ��һ����Դ���ϴ����С�
 �����Ǵ�����һ���ϴ���������ǽ��������ݿ������ϴ������У�Ȼ�����Ǵ��ϴ������п����������ݵ����ǵĶ��㻺����ȥ��

 ֮���������һ�����ӣ���������Լ�ȥ�����롣��ֻ��������Ҫʹ�õĽṹ�����ͽ����¡�

 ```C++
 struct D3D12_SUBRESOURCE_DATA
 {
    const void *pData;
    LONG_PTR RowPitch;
    LONG_PTR SlicePitch;
 };
 ```

 - `pData`: �������Ԫ��ָ�롣
 - `RowPitch`: ���ڻ�����˵���������ǿ��������ݵĴ�С����λ�ֽڡ�
 - `SlicePitch`: ���ڻ�����˵���������ǿ��������ݵĴ�С����λ�ֽڡ�

ע����ǣ����ڶ��㻺����˵�����Ǵ��������������ǲ���Ҫ����������������еġ�

```C++
struct D3D12_VERTEX_BUFFER_VIEW
{
    D3D12_GPU_VIRTUAL_ADDRESS BufferLocation;
    UINT SizeInBytes;
    UINT StrideInBytes;
};
```

- `BufferLocation`: ������Ҫʹ�õĶ��㻺��������ַ�����ǿ���ʹ��`ID3D12Resource::GetGPUVirtualAddress`����ȡ��ַ��
- `SizeInBytes`: ����������ֻ������ֻʹ�û����е�һ�������ݣ������������������������Ҫʹ�õĻ����С����λ�ֽڣ���`BufferLocation`λ�ÿ�ʼ����ƫ�ơ�
- `StrideInBytes`: ÿ������Ԫ�صĴ�С����λ�ֽڡ�

�����Ǵ����껺��ͻ���������������ǿ��Խ����ǰ󶨵���Ⱦ�ܵ�������ڣ�Ȼ��������װ��׶ν����㻺�������ȥ��


