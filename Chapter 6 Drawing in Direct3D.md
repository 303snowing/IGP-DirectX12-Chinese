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
- `AlignedByteOffset`: �ڴ�ƫ��������λ���ֽڡ��Ӷ���ṹ�Ŀ��˵����Ԫ�صĿ��˵��ֽڴ�С��

 