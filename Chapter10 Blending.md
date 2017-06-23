# Chapter10 Blending

�۲��·�ͼƬ[10.1](Images/10.1.png)��
��������һ֡���ʼ��Ⱦ���ǵ��κ�ľ�䣬��˵��κ�ľ������ؾͱ������ں�̨�����С�
Ȼ������ʹ�û�ϼ���������һ��ˮ�浽��̨���壬���ˮ�����غ͵����Լ�ľ��������ں�̨�����еõ��˻�ϣ�����������ľ��͵��δ�����ˮ�档
�ڱ����У����ǳ��Ի�ϼ�������������ܹ����ǻ��(���)�������ڹ�դ��������(���ǳ�֮ΪԴ)��֮ǰ�Ѿ���ɹ�դ�����洢�ں�̨�����е�����(���ǳ�֮ΪĿ��)��
��������ܹ�������ȥ��Ⱦ͸�������壬����ˮ�Ͳ�����

![10.1](Images/10.1.png)


**Ŀ��**

- ������ʹ�û�ϣ������ܹ���`Direct3D`��ʹ������
- ѧϰ`Direct3D`֧�ֵĲ�ͬ�Ļ��ģʽ��
- �������ͨ��ʹ��Alphaȥ����ͼԪ��͸���ȡ�
- ѧϰ���ʹ��HLSL�е�Clip������ֹһ�����ػ��Ƶ���̨�����С�

## 10.1 THE BLENDING EQUATION

������**C<sub>src**</sub>��ʾ�����ǵ�������ɫ��(**PixelShader**)����ĵ�**i**,**j**�����أ�Ҳ�����������ڹ�դ�������أ�Ȼ��������**C<sub>dst**</sub>��ʾ�����ں�̨�����е�**i**,**j**�����ء�
�����ʹ�û�ϼ����Ļ�������**C<sub>src**</sub>�������**C<sub>dst**</sub>(ǰ�������ܹ�ͨ����Ⱥ�ģ�����)��
������ʹ�û�ϵ�����£�**C<sub>src**</sub>��**C<sub>dst**</sub>�����ϳ�һ���µ���ɫ**C**������**C**�������**C<sub>dst**</sub>��Ϊ��̨�������������(�������Ϊ�µ���ɫ**C**���ᱻд�뵽��̨������ȥ)��
`Direct3D`ʹ�������������������Դ��Ŀ�����ص���ɫ:

**<center>C = C<sub>src</sub> �� F<sub>src</sub> ^ C<sub>dst</sub> �� F<sub>dst</sub></center>**

F������������ܣ���Ҫ��ͨ��F���������и���ķ�ʽȥʵ�ָ����Ч����
`��` ���ϱ�ʾ���������ĵ����`^`(���Ǹ������Ҵ򲻳����������ҽ�����ϲ���)������һ��������������������㣬�����������ܡ�

����ķ��������ڴ�����ɫ�����е�**RGB**�ģ�����**Alpha**ֵ���ǵ���ʹ������ķ��̴���������̺�������Ǽ�Ϊ���Ƶģ�

**<center>A = A<sub>src</sub>F<sub>src</sub> ^ A<sub>dst</sub>F<sub>dst</sub></center>**

������̻�������һ���ģ����������ֿ�����Ļ��Ϳ�����`^`������Բ�ͬ�ˡ����ǽ�RGB��Alpha�ֿ�����Ķ��������������ܹ���������RGB��Alphaֵ�������߲�����Ӱ�졣


## 10.2 BLEND OPERATIONTS

- `D3D12_BLEND_OP_ADD`: **C = C<sub>src</sub> �� F<sub>src</sub> + C<sub>dst</sub> �� F<sub>dst</sub>**
- `D3D12_BLEND_OP_SUBTRACT`: **C = C<sub>dst</sub> �� F<sub>dst</sub> - C<sub>src</sub> �� F<sub>src</sub>**
- `D3D12_BLEND_OP_REV_SUBTRACT`: **C = C<sub>src</sub> �� F<sub>src</sub> - C<sub>dst</sub> �� F<sub>dst</sub>** 
- `D3D12_BLEND_OP_MIN`: **C = min(C<sub>src</sub>, C<sub>dst</sub>)**
- `D3D12_BLEND_OP_MAX`: **C = max(C<sub>src</sub>, C<sub>dst</sub>)**

Alpha�Ļ�ϲ���Ҳ��һ���ġ�
��Ȼ�����ʹ�ò�ͬ�Ĳ������ֱ���RGB��Alpha�Ļ�ϡ�
�ٸ�������˵���������RGB�Ļ����ʹ��`+`��Ȼ����Alpha�Ļ����ʹ��`-`��

**<center>C = C<sub>src</sub> �� F<sub>src</sub> + C<sub>dst</sub> �� F<sub>dst</sub></center>** 

**<center>A = A<sub>dst</sub>F<sub>dst</sub> - A<sub>src</sub>F<sub>src</sub></center>**


�����`Direct3D`�м�����һ���µ�����(`D3D12_LOGIC_OP`)�����ǿ���ʹ���߼����������������Ļ�ϲ�����
������Ҿ�û��Ҫ�Ž�ȥ�ˣ��Ͼ��ܼ���⡣

��������Ҫע����ǣ������ʹ���߼�����������ϲ���������Ҫע������߼�������ʹ�ͳ������ǲ���ͬʱʹ�õģ������������������ѡ��һ��ʹ�á�
������ʹ���߼�������Ļ�������Ҫȷ�����`Render Target`�ĸ�ʽ֧��(֧�ֵĸ�ʽӦ����`UINT`�ı���)��
## 10.3 BLEND FACTORS

ͨ���ı�`Factors(����)`�����ǿ������ø���Ĳ�ͬ�Ļ����ϣ��Ӷ���ʵ�ָ���Ĳ�ͬ��Ч����
���ǽ������������һЩ�����ϣ������㻹��Ҫȥ����һ�����ǵ�Ч�����Ӷ��ܹ���һ�����
���潫�����һЩ������`Factors`�������ȥ��SDK�ĵ������`D3D12_BLEND`ö���˽⵽����߼���`Factors`��
������**C<sub>src</sub> = (r<sub>src</sub>, g<sub>src</sub>, b<sub>src</sub>)**��**A<sub>src</sub> = a<sub>src</sub>**(���RGBAֵ����������ɫ�������)��
**C<sub>dst</sub> = (r<sub>dst</sub>, g<sub>dst</sub>, b<sub>dst</sub>)**��**A<sub>dst</sub> = a<sub>dst</sub>**(���RGBAֵ�Ǵ洢�ں�̨�����е�)��

- `D3D12_BLEND_ZERO`: **F = (0, 0, 0, 0)**
- `D3D12_BLEND_ONE`: **F = (1, 1, 1, 1)**
- `D3D12_BLEND_SRC_COLOR`: **F = (r<sub>src</sub>, g<sub>src</sub>, b<sub>src</sub>)**
- `D3D12_BLEND_INV_SRC_COLOR`: **F<sub>src</sub> = (1 - r<sub>src</sub>, 1 - g<sub>src</sub>, 1 - b<sub>src</sub>)**
- `D3D12_BLEND_SRC_ALPHA`: **F = (a<sub>src</sub>, a<sub>src</sub>, a<sub>src</sub>, a<sub>src</sub>)**
- `D3D12_BLEND_INV_SRC_ALPHA`: **F = (1 - a<sub>src</sub>, 1 - a<sub>src</sub>, 1 - a<sub>src</sub>, 1 - a<sub>src</sub>)**
- `D3D12_BLEND_DEST_ALPHA`: **F = (a<sub>dst</sub>, a<sub>dst</sub>, a<sub>dst</sub>, a<sub>dst</sub>)**
- `D3D12_BLEND_INV_DEST_ALPHA`: **F = (1 - a<sub>dst</sub>,1 - a<sub>dst</sub>, 1 - a<sub>dst</sub>, 1 - a<sub>dst</sub>)**
- `D3D12_BLEND_DEST_COLOR`: **F = (a<sub>dst</sub>, a<sub>dst</sub>, a<sub>dst</sub>)**
- `D3D12_BLEND_INV_DEST_COLOR`: **F = (1 - a<sub>dst</sub>, 1 - a<sub>dst</sub>, 1 - a<sub>dst</sub>)**
- `D3D12_BLEND_SRC_ALPHA_SAT`: **F = (a'<sub>src</sub>, a'<sub>src</sub>, a'<sub>src</sub>, a'<sub>src</sub>), a'<sub>src</sub> = clamp(a<sub>src</sub>, 0, 1)**
- `D3D12_BLEND_BLEND_FACTOR`: **F = (r, g, b, a)**

���һ��ö�������еĲ��� **(r ,g ,b ,a)** ͨ����������������á�

`ID3D12GraphicsCommandList::OMSetBlendFactor`

������һ��`Float[4]`����ʾ4��������ֵ���������Ϊ`nullptr`��ô��Ĭ��ȫ��1��

## 10.4 BLEND STATE

�����Ѿ����۹��˻�ϲ������ͻ�����أ��������������`Direct3D`����������Щ�����أ�
��������`Direct3D`��״̬һ�������״̬Ҳ��`PSO(��Ⱦ�ܵ�)`��һ�����֡�
֮ǰ����ʹ�õĶ���Ĭ�ϵĻ��״̬(����״̬)��

�������Ҫʹ�÷�Ĭ�ϵĻ��״̬�����Ǳ������`D3D12_BLEND_DESC`�ṹ��

```C++
    struct D3D12_BLEND_DESC{
        bool AlphaToCoverageEnable; // false
        bool IndependentBlendEnable; // false
        D3D11_RENDER_TARGET_BLEND_DESC RenderTarget[8];
    };
```

- `AlphaToCoverageEnable`:���ó�`true`����`alpha-to-coverage`��������������Ƕ��ز�����������ȾĳЩ���� **(����ķ����޷���֤��ȷ�ԣ���˾�ʹ��ĳЩ�������)** ��ʱ��ʹ�õġ����ó�`false`���ر����������`alpha-to-coverage`������Ҫ���ز�����������ʹ��(����֮���Ǳ����ڴ�����̨�������Ȼ����ʱ�������ز���)��
- `IndependentBlendEnable`:`Direct3D`���֧��ͬʱ��Ⱦ8��`Render Target`���������������ó�`true`����ô�Ϳ�������Ⱦ��ͬ��'Render Target'��ʱ��ʹ�ò�ͬ�Ļ�ϲ���(���������أ���ϲ�����������Ƿ�����)��������ó�`false`����ô���е�`Render Target`�ͻ�ʹ��ͬ���Ļ�Ϸ���(������˵����`D3D12_BLEND_DESC::RenderTarget`�еĵ�һ��Ԫ����Ϊ���е�`Render Target`ʹ�õĻ�Ϸ���)������������˵������һ��ֻʹ��һ��`Render Target`��
- `RenderTarget`:��i��Ԫ��������i��`Render Target`ʹ�õĻ�Ϸ��������`IndependentBlendEnable`���ó�`false`����ô���е�`Render Target`��ȫ��ʹ��`RenderTarget[0]`�����Ϸ���ȥ���л�ϡ�


`D3D12_RENDER_TARGET_BLEND_DESC`�ṹ������ʾ:

```C++
    struct D3D12_RENDER_TARGET_BLEND_DESC{
        bool BlendEnable; // false
        bool LogicOpEnable; // false
        D3D12_BLEND SrcBlend; // D3D12_BLEND_ONE
        D3D12_BLEND DestBlend; // D3D12_BLEND_ZERO
        D3D12_BLEND_OP BlendOp; // D3D12_BLEND_OP_ADD
        D3D12_BLEND SrcBlendAlpha; // D3D12_BLEND_ONE
        D3D12_BLEND DestBlendAlpha; // D3D12_BLEND_ZERO
        D3D12_BLEND_OP BlendOpAlpha // D3D12_BLEND_OP_ADD
        D3D12_LOGIC_OP LogicOp; // D3D12_LOGIC_OP_NOOP
        UINT8 RenderTargetWriteMask; // D3D12_COLOR_WRITE_ENABLE_ALL
    };
```

- `BlendEnable`: ���ó�`true`�Ϳ�����Ϸ�����ǹرգ�ע�����`LogicOpEnable`��`BlendEnable`����ͬʱ�����������ʹ�����е�һ�������л�ϡ�
- `LogicOpEnable`: ���ó�`true`�Ϳ���ʹ���߼�������Ļ�ϣ�Ȼ���`BlendEnable`����ͬʱʹ�á�
- `SrcBlend`: **RGB**����е�**F<sub>src</sub>**��
- `DestBlend`:  **RGB**����е�**F<sub>dst</sub>**��
- `BlendOp`: **RGB**�����ʹ�õĲ�������
- `SrcBlendAlpha`: **Alpha**����е�**F<sub>src</sub>**��
- `DestBlendAlpha`: **Alpha**����е�**F<sub>dst</sub>**��
- `BlendOpAlpha`: **Alpha**�����ʹ�õĲ�������
- `LogicOp`: �����ʹ�õ��߼��������
- `RenderTargetWriteMask`: ���ڿ��ƻ�Ͻ�������Щ��ɫ(**R,G,B,A**)����д�뵽��̨������ȥ���ٸ�������˵��������������ֹ��**RGB**д�뵽��̨������ȥ�Ļ������ǾͿ������ó�`D3D12_COLOR_WRITE_ENABLE_ALPHA`���������ǹرյģ���ô��û���κ������������̨������ȥ��

**Notice:** ���ڻ����Ҫ����ÿһ�����أ��������Ŀ����ܴ������ֻ����Ҫ��ʱ��Ŵ�����

## 10.5 EXAMPLES

��������֣����ǿ�һЩ��ϲ�������Ч���ӡ���Ȼ����ֻ������**RGB**��ϵ����ӡ�

### 10.5.1 No Color Write

�������ֻ���뵥��������Ŀ�����أ���Դ���ز����Ŀ�����ؽ��л���Լ����ǣ���ô�Ϳ���ʹ�����������
�ٸ�������˵���ǣ���Ŀ�����������`Depth/Stencil Buffer`��ȥ�����������棺

**<center>C = C<sub>src</sub> �� (0, 0, 0) + C<sub>dst</sub> �� (1, 1, 1)</center>**

**<center>C = C<sub>dst</sub></center>**

����һ���������ǽ�`RenderTargetWriteMask`���ó�0��
�����Ļ��ͽ�ֹ�����е���ɫ�����`Back Buffer`�С�


### 10.5.2 Adding/Subtracting

![10.2](Images/10.2.png)


��������뽫Դ���غ�Ŀ�����ؼ�����(�μ�[10.2](Images/10.2.png))���������£�

**<center>C = C<sub>src</sub> �� (1, 1, 1) + C<sub>dst</sub> �� (1, 1, 1)</center>**

**<center>C = C<sub>src</sub> + C<sub>dst</sub></center>**

![10.3](Images/10.3.png)

���ǵ�ȻҲ���������(�μ�[10.3](Images/10.3.png))���������£�

**<center>C = C<sub>src</sub> �� (1, 1, 1) - C<sub>dst</sub> �� (1, 1, 1)</center>**

**<center>C = C<sub>src</sub> + C<sub>dst</sub></center>**

### 10.5.3 Multiplying

![10.4](Images/10.4.png)

��������뽫Դ���غ�Ŀ���������(�μ�[10.4](Images/10.4.png))���������£�

**<center>C = C<sub>src</sub> �� (0, 0, 0) - C<sub>dst</sub> �� C<sub>src</sub></center>**

**<center>C = C<sub>src</sub> + C<sub>dst</sub></center>**

### 10.5.3 Transparency

�������Ǽ���**Alpha**������������Դ���صĲ�͸����(0����**0%**,0.4����**40%**)��
�����費͸����Ϊ**A**��͸����Ϊ**T**����ô͸���ȺͲ�͸���ȵĹ�ϵ����**T = 1 - A**��
���粻͸������0.4,��ô͸���Ⱦ���0.6�����������뽫Դ���غ�Ŀ�������ڱ���Դ���صĲ�͸���ȵ�����£���Ŀ������͸�����������£�

**<center>C = C<sub>src</sub> �� (a<sub>src</sub>, a<sub>src</sub>, a<sub>src</sub>) - C<sub>dst</sub> �� (1 - a<sub>src</sub>, 1 - a<sub>src</sub>, 1 - a<sub>src</sub>)</center>**

**<center>C = a<sub>src</sub> �� C<sub>src</sub> + (1 - a<sub>src</sub>) �� C<sub>dst</sub></center>**

�ٸ����Ӿ��ǣ����Ǽ���**a<sub>src</sub> = 0.25**�����ǲ�͸����Ϊ**25%**��
��Դ���غ�Ŀ�����ػ�ϵ�ʱ������ϣ������ **25%** ��Դ���أ�**75%** ��Ŀ������(����Ŀ��������Դ���ص�ǰ�棬��ʵ����˵����Ŀ�����ص������ھ���Դ���ص�����ǰ��)���������£�

**<center>C = C<sub>src</sub> �� (a<sub>src</sub>, a<sub>src</sub>, a<sub>src</sub>) - C<sub>dst</sub> �� (1 - a<sub>src</sub>, 1 - a<sub>src</sub>, 1 - a<sub>src</sub>)</center>**

**<center>C = 0.25 �� C<sub>src</sub> + 0.75 �� C<sub>dst</sub></center>**

ͨ��ʹ�û�ϵķ��������ǾͿ��Ի�����[10.1](Images/10.1.png)�������ˡ����������Ҫ�ڻ�ϵ�ʱ��ע��һЩ������������ͻ���Ƶ�ʱ��������⡣
���Ǳ����������¹���

���Ȼ�����Щ����Ҫ��ϵ����塣
Ȼ����Ҫ��ϵ����尴�����Ǻ�������ľ�������
Ȼ���մӺ���ǰ��˳��������塣

�Ӻ���ǰ���������ԭ���Ҳ�������������������ˣ�����Ҿ��ú���Ȼ���������������ҵ�**BB**��

����һ��������˵������Ҫ�������л�ϵĻ������Ǳ������Ǻ�������أ����������Ҫ�Ȼ��Ʊ��������أ�Ȼ���ڻ������������ء�

����**10.5.1**�����Ƶ�˳���Ѿ�û�������ˣ������������������**10.5.2**��**10.5.3**������Ȼ�Ȼ��Ʋ���Ҫ��ϵ����壬Ȼ�������Ҫ��ϵ����塣
������Ϊ������Ҫ�ڿ�ʼ���֮ǰ�����еĲ����л�ϵ�����ȷ�����������������ǲ�����Ҫ������Ҫ��ϵ����塣��Ϊ�������������㽻���ɵġ�������Դ����Ϊ**B**��

**<center>B' = B + C<sub>0</sub> + ... + C<sub>n-1</sub></center>**

**<center>B' = B - C<sub>0</sub> + ... - C<sub>n-1</sub></center>**

**<center>B' = B ^ C<sub>0</sub> + ... ^ C<sub>n-1</sub></center>**

### 10.5.5 Blending and the Depth Buffer

�����ǽ���ǰ��˵�ļ��ֻ��(��������һ��)��ʱ������Ȳ��Ե�ʱ����ܻ���һ�����⡣
���������Լӷ������Ϊ���ӣ������Ļ���ǲ���˼·��

�չ��ˣ�ԭ��˵����ô����ʵ���������������㿪������Ȳ��ԵĻ����㲻���մӺ���ǰ��˳����ƵĻ�����ô�Ϳ�����������Ϊ��Ȼ�����������Ӷ�û���ۼӵ���ϵķ�����ȥ������������ɫ�е�С���⡣
����ҾͲ�ֱ�ӷ���ԭ����(ԭ��������һ���ֵ����﷨û��⣬��Ӣ���Ǹ�����������˼�������˼û��)��



