# <WEEK 02. Colorspace / Gamma / Color correction>


## Linear Workflow
인간의 눈은 카메라의 방식처럼 빛을 인식하지 않는다. 밝고 어둠을 볼 때 우리의 눈이 반응하는 것이 다르다. (디지털과 아날로그적인 것은 차이가 있음) 
우리가 바라보는 시각과 디지털 데이터 및 장비가 표현하는 방식이 다르기 때문에 선형의 이미지로 만든 다음 작업을 하는 것이다. 
**즉 감마란 컴퓨터 모니터 또는 이미지 전체의 기준 어둡기(밝기)를 말하는 것이다.**
모니터의 성능에 따라 RGB 각각의 감마를 결정할 수 있고, 이를 통합하여 하나의 모드로 감마를 설정할 수 있다. 
그러나 시각적인 판단으로 농도를 맞추는 것은 어려우니 기준 색을 보고 작업해야 한다. 

***Why is important***
CRT에서 LCD로 넘어오는 과정에서 과도기가 있었다. 서로 바는 방식이 다르기 때문에 하나로 맞추기 위함이다. 
1) 디지털 색채를 다루는 전자 장비들 간에 호환성이 없다.
2) 동일 제조회사라도 각 모델에 따라 다른 색체계로 재현된다.
즉 색채정보가 서로 다르다. 이미지 소스들(감마가 적용된 이미지) 등 다양한 커브를 가진 이미지들을 작업할 때 리니어 워크플로우가 작용이 안 되면 밝거나 혹은 더 어두워진다. 
UHD TV, 8K 등 상영할 수 있는 장비들이 너무나도 다양한 컬러스페이스가 나오기 때문에 컬러 매니지먼트가 필요한 것이다.


## Color Management
다양한 종류의 스캐너, 디지털 카메라, 모니터, 프린터 등에서 사용되는 CMY와 RGB 색체계를 CIE XYZ (인간의 시감으로 감지 할 수 있는 모든 색의 영역) 색공간에서 특징지어 주고, 색영역 맵핑 등을 수행함으로써 WYSIWYG (What You See Is What You Get)를 구현하는 기능을 갖는 것이 바로 컬러 매니지먼트이다. 
**+SRGB: 일반적으로 인터넷을 위한 표준 RGB 색공간이다. 상당히 좁은 색공간을 가지고 있어 색과 색을 구분할 수 있는 분해능이 좋은 장치에서 약점이고, 후보정시 표현할 수 있는 색공간이 좁다보니 원하는 색이 표현 안 될 수도 있다.**
ACES는 사람들에게 가장 정확하게 보여줄 수 있는 이미지를 디스플레이한다. 이는 색조의 범위와 밝기를 우리 눈이 인지 가능하도록 가깝게 맞춰준다. 

***용어정리***
IDT: 특정 장치의 이미지/비디오 픽셀 색상을 ACES 색 공간으로 변환하는 데 사용되는 변환이다.
RRT: 참조된 선형 데이터를 하이 다이내믹 레인지 디스플레이 참조 데이터로 준비합니다. 그런 다음 이 데이터는 특정 디스플레이 유형에서 볼 수 있도록 데이터를 변환하기 위해 ODT로 넘겨집니다.
ODT: RRT에 의해 생성된 데이터를 sRGB, P3, Rec. 709와 같은 특정 장치나 색 공간에서 볼 수 있는 데이터로 변환하는 역할을 한다.

### CIE XY
인간이 인식할 수 있는 색상의 범위를 묘사하는 일반적인 도표이다. 이 다이어그램은 색상 및 채도만을 나타내는 값/강도만 나타내는 것이 아니다. 
색역, 원초 및 백점을 설명하기 위해 CIExy 다이어그램을 사용하여 색 공간을 시각화하여 색 공간의 크기와 이러한 색 공간으로 나타낼 수 있는 시각적 광 스펙트럼의 양을 쉽게 볼 수 있다.

### CIE Gamut
촬영하고 표현할 수 있는 색의 총체적인 범위이다. 
가장 넓은 테두리 공간이 우리 두 눈으로 확인할 수 있는 영역이다. 그 안에 삼각형으로 표현하는 것들이 각각의 색공간colour gamut들이 표현할 수 있는 영역을 나타낸 것이고 ITU-709라고 표시한 회색 삼각형이 바로 rec-709이다.

## LUT (Look-Up Table)
색상hue, 채도saturation, 조도brightness를 "수학적으로 정확하게" 조정하여 촬영된 원본 이미지의 RGB값을 새로운 RGB값으로 만들어주는 방법이다.

LUT는 다양한 종류가 있다.

1) Technical LUTs:
테크니컬 LUT는 이미지를 하나의 색공간(color space 또는 gamut)에서 다른 색공간으로 옮기기 위해 만들어졌다.  해당 LUT의 최종 목표는 두 가지 다른 시청 환경에서 똑같은 결과물을 만들어내는 것이다. 예를 들면, (똑같은 소스를) TV에서 보는 것과 잉크젯 프린터로 출력해서 보는 결과를 똑같이 맞춰주는 것이다.   

2) Creative LUTs:
크리에이티브 LUT는 다른 소프트웨어에서도(예를 들면, 프리미어 프로와 다빈치 등에서) 늘 한결같이 LUT가 가진 하나의 룩look을 표현할 수 있게 해준다.

***우리가 creative LUT를 technical LUT라고 생각하고 사용할 때 문제가 발생한다. 실제로 다양한 컬러 보정 LUT들이 이런 오해를 받고 있다.***

3) Camera LUTs:
카메라 LUT의 좋은 예가 ARRI Alexa의 Rec709 LUT다. 이 LUT는 ARRI의 flat한 LOG-C에 적용하기 위해 디자인 되어 있다. 이런 종류의 LUT는 technical LUT와 creative LUT를 합친 결과물이라고 할 수 있다.





