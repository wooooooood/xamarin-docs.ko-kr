---
title: ARKit Xamarin.iOS에서 UrhoSharp 사용
description: 이 문서는 ARKit 앱에서 Xamarin.iOS 설정 하는 방법에 설명 다음 프레임이 렌더링 되는 방법, 카메라를 조정 하는 방법을 살펴봅니다 검색 방법을 평면, 조명 등을 사용 하는 방법입니다. 또한 UrhoSharp 및 HoloLens 용 코드 작성에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 877AF974-CC2E-48A2-8E1A-0EF9ABF2C92D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/01/2017
ms.openlocfilehash: 728082eb27684c2176feb2038b7948986ce6a694
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351693"
---
# <a name="using-arkit-with-urhosharp-in-xamarinios"></a>ARKit Xamarin.iOS에서 UrhoSharp 사용

도입 [ARKit](https://developer.apple.com/arkit/), Apple 만들었다 증강된 현실 응용 프로그램을 만드는 개발자를 위한 간단 합니다. ARKit 장치의 정확한 위치를 추적 하 고 검색 전 세계의 다양 한 표면 및 코드로 ARKit 전송 되는 데이터를 혼합 하려면 개발자에 게 달려 됩니다.

[UrhoSharp](~/graphics-games/urhosharp/index.md) 제공 하는 포괄적이 고 사용 하기 쉬운 3D API를 3D 응용 프로그램을 만드는 데 사용할 수 있습니다.   둘 다 수 전 세계에 대 한 실제 정보를 제공 하는 ARKit 및 결과를 렌더링할 Urho 함께 혼합 합니다.

이 페이지를 증강된 현실 훌륭한 응용 프로그램을 만드는 데이 두 환경을 연결 하는 방법에 설명 합니다.


## <a name="the-basics"></a>기본 사항

원하는 작업을 수행 하는 iPhone에서 볼 수 있듯이 전 세계 위에 있는 3D 콘텐츠입니다.   3D 콘텐츠를 사용 하 여 휴대폰의 카메라에서 가져온 내용을 blend는 및의 휴대폰으로 이동할 때 전시실 되도록 해당 대화방의 일부인 이러한 이전과 3D 개체-이 환경에 개체를 고정 하면 됩니다.

![ARKit에 애니메이션된 그림](urhosharp-images/image1.gif)


Urho 라이브러리 3D 자산을 로드 하 고 전 세계에서 배치를 사용 하 고 ARKit 카메라 뿐만 아니라 전 세계에서 전화의 위치에서 들어오는 비디오 스트림을 가져오려면 사용 합니다.   사용자가 자신의 휴대폰을 사용 하 여,는를 사용 하 여 변경 내용을 위치에 Urho 엔진 표시 하는 좌표계를 업데이트 합니다.

이러한 방식으로 3D 공간에서 개체를 배치 하 고 사용자가 이동할 때 3D 개체의 위치 위치 및 배치 된 위치를 반영 합니다.

## <a name="setting-up-your-application"></a>응용 프로그램 설정

### <a name="ios-application-launch"></a>iOS 응용 프로그램 시작

IOS 응용 프로그램 만들기 및 3D 콘텐츠를 시작, 구현 하는의 서브 클래스를 만들어이 작업을 수행 합니다 [ `Urho.Application` ](https://developer.xamarin.com/api/type/Urho.Application/) 재정의 하 여 설치 코드를 입력 하 고는 `Start` 메서드.  이 데이터를 이벤트 처리기에 설정 된 장면 가져옵니다 채워집니다.

도입 했습니다를 `Urho.ArkitApp` 는 클래스 `Urho.Application` 및 해당 `Start` 메서드는 주요 작업을 수행 합니다.   형식의 기본 클래스를 변경 하기만 하면 작업을 수행 하는 응용 프로그램에 기존 Urho `Urho.ArkitApp` urho 장면에 전 세계에서 실행 하는 응용 프로그램에 있습니다.

### <a name="the-arkitapp-class"></a>ArkitApp 클래스

편리한 기본값, 일부 키 개체를 사용 하 여 두 장면 집합 뿐만 아니라 ARKit 이벤트를 처리 하는 운영 체제에서 배달 하는 대로이 클래스를 제공 합니다.

설치 프로그램 수행 된 `Start` 가상 메서드.   사용 하 여 부모 체인에 있는지 확인 해야 하는 서브 클래스에서이 메서드를 재정의 하는 경우 `base.Start()` 구현에 따라 고유 합니다.

`Start` 메서드 장면, 뷰포트, 카메라 및 방향성을 설정 하 고 공용 속성으로 표시 합니다.

- [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Scene/) 개체를 저장 하려면
- 방향 [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light/) 그림자 및 해당 위치를 통해 사용할 수는 `LightNode` 속성
- [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera/) ARKit 응용 프로그램에 대 한 업데이트를 제공 하는 경우 해당 구성 요소 업데이트 되 고
- [ `ViewPort` ](https://developer.xamarin.com/api/type/Urho.Viewport/) 결과 표시 합니다.


### <a name="your-code"></a>코드

서브 클래스 해야 합니다 `ArkitApp` 클래스를 재정의 합니다 `Start` 메서드.   먼저 방법을 수행 해야 하는 체인이 최대 합니다 `ArkitApp.Start` 호출 하 여 `base.Start()`입니다.  그 후 장면에 개체를 추가, 광원, 어두운 영역 또는 이벤트를 처리 하려면 사용자 지정 하 여 ArkitApp 속성 설정을 사용할 수 있습니다.

질감을 사용 하 여 애니메이션된 된 문자를 로드 하 고 다음 구현으로 애니메이션을 재생 하는 ARKit/UrhoSharp 샘플:

    ```csharp
    public class MutantDemo : ArkitApp
    {
        [Preserve]
        public MutantDemo(ApplicationOptions opts) : base(opts) { }

        Node mutantNode;

        protected override void Start()
        {
            base.Start ();

            // Mutant
            mutantNode = Scene.CreateChild();
            mutantNode.Rotation = new Quaternion(x: 0, y:15, z:0);
            mutantNode.Position = new Vector3(0, -1f, 2f); /*two meters away*/
            mutantNode.SetScale(0.5f);

            var mutant = mutantNode.CreateComponent<AnimatedModel>();
            mutant.Model = ResourceCache.GetModel("Models/Mutant.mdl");
            mutant.Material = ResourceCache.GetMaterial("Materials/mutant_M.xml");

            var animation = mutantNode.CreateComponent<AnimationController>();
            animation.Play("Animations/Mutant_HipHop1.ani", 0, true, 0.2f);
        }
    }
    ```

실제로 3D 콘텐츠 확대 된 현실에 표시 하도록이 시점에서 수행 해야 하는 모든 하며

Urho 자산이이 형식으로 내보낼 필요 하므로 애니메이션 및 3D 모델에 대 한 사용자 지정 형식을 사용 합니다.   와 같은 도구를 사용할 수는 [Urho3D Blender add-in](https://github.com/reattiva/Urho3D-Blender) 및 [UrhoAssetImporter](https://github.com/EgorBo/UrhoAssetImporter) Urho에 필요한 형식으로 3D 최대 DAE, DBX, OBJ, Blend와 같은 인기 있는 형식에서 이러한 자산을 변환할 수 있는 합니다.

Urho를 사용 하 여 3D 응용 프로그램 만들기에 대 한 자세한 내용은 참조는 [UrhoSharp 소개](~/graphics-games/urhosharp/introduction.md) 가이드입니다.

## <a name="arkitapp-in-depth"></a>ArkitApp 방어

> [!NOTE]
> 이 섹션에서는 UrhoSharp 및 ARKit 기본 환경을 사용자 지정 하거나 통합의 작동 방식에 대해 깊이 이해 하는 개발자를 위한 것입니다.   이 섹션을 참조 하는 데 필요한 것입니다.

ARKit API는 상당히 간단, 만들기 및 구성 하는 [ARSession](https://developer.apple.com/documentation/arkit/arsession) 하는 개체를 제공 하기 시작 [ARFrame](https://developer.apple.com/documentation/arkit/arframe) 개체입니다.   여기에 예상 되는 실제 위치가 장치 뿐만 아니라 카메라에서 캡처된 이미지 모두 포함 되어 있습니다.

수 카메라에서 3D 콘텐츠를 사용 하 여 우리에 게 제공 되는 이미지를 작성 하 고 장치 위치와 위치 가능성에 맞게 UrhoSharp 카메라를 조정 합니다.

다음 다이어그램에서는 일어나는 무엇을 `ArkitApp` 클래스:

[![클래스 및 화면을 ArkitApp에서의 다이어그램](urhosharp-images/image2.png)](urhosharp-images/image2.png#lightbox)

### <a name="rendering-the-frames"></a>프레임을 렌더링합니다.

아이디어는 간단 하, 결합 된 이미지를 생성 하는 3D 그래픽을 사용 하 여 카메라에서 들어오는 비디오를 결합 합니다.     에서는 됩니다 수 일련의 이러한 캡처된 이미지 순서 대로 가져오고 Urho 장면으로이 입력을 혼합 됩니다 것입니다.

작업을 수행 하는 가장 간단한 방법은 삽입 하는 것을 [ `RenderPathCommand` ](https://developer.xamarin.com/api/type/Urho.RenderPathCommand/) 를 주 [ `RenderPath` ](https://developer.xamarin.com/api/type/Urho.RenderPath/)합니다.  단일 프레임을 그리려면 수행 되는 명령의 집합입니다.  이 명령에 전달 하는 모든 질감으로 뷰포트를 채워집니다.    에서는이 프로세스는 첫 번째 프레임에 설정과 실제 정의에서 수행 됩니다 **ARRenderPath.xml** 이 시점에서 로드 되는 파일입니다.

그러나이 두 환경을 함께 혼합 하려면 두 가지 문제를 사용 하 여 직면 했습니다.


1. IOS, GPU 질감은 2의 제곱이 해상도 있어야 없는 카메라에서 얻게 되는 프레임 예를 들어 2의 거듭제곱 된 해상도: 1280 x 720입니다.
2. 프레임은 인코딩된 [YUV](https://en.wikipedia.org/wiki/YUV) 두 이미지-하 고 색도 광도가 됩니다 나타내는 형식입니다.

YUV 프레임을 두 개의 다른 해결 방법으로 제공 됩니다.  광도 (기본적으로 회색조 이미지)를 나타내며 훨씬 더 작은 640x360 색차 구성 요소를 1280 x 720 이미지:

![Y 결합 및 UV 구성 요소를 보여 주는 이미지](urhosharp-images/image3.png)


OpenGL ES를 사용 하 여 전체 색이 지정 된 이미지를 그릴 텍스처 슬롯에서 광도 (Y 구성 요소) 및 색차 (UV 평면)를 사용 하는 작은 셰이더를 작성 해야 합니다.  UrhoSharp에서 이름이-"sDiffMap" 및 "sNormalMap" 하며 RGB 형식으로 변환:

```csharp
mat4 ycbcrToRGBTransform = mat4(
    vec4(+1.0000, +1.0000, +1.0000, +0.0000),
    vec4(+0.0000, -0.3441, +1.7720, +0.0000),
    vec4(+1.4020, -0.7141, +0.0000, +0.0000),
    vec4(-0.7010, +0.5291, -0.8860, +1.0000));

vec4 ycbcr = vec4(texture2D(sDiffMap, vTexCoord).r,
                    texture2D(sNormalMap, vTexCoord).ra, 1.0);
gl_FragColor = ycbcrToRGBTransform * ycbcr;
```

두 확인 활용 되지 않은 텍스처를 렌더링 하 Texture2D 다음 매개 변수를 사용 하 여 정의 해야 합니다.

```chsarp
// texture for UV-plane;
cameraUVtexture = new Texture2D();
cameraUVtexture.SetNumLevels(1);
cameraUVtexture.SetSize(640, 360, Graphics.LuminanceAlphaFormat, TextureUsage.Dynamic);
cameraUVtexture.FilterMode = TextureFilterMode.Bilinear;
cameraUVtexture.SetAddressMode(TextureCoordinate.U, TextureAddressMode.Clamp);
cameraUVtexture.SetAddressMode(TextureCoordinate.V, TextureAddressMode.Clamp);
```

따라서 배경으로 캡처된 이미지를 렌더링 하 고 같은 scary 뮤턴트 모든 장면 위에 렌더링할 수 있습니다.

### <a name="adjusting-the-camera"></a>카메라를 조정합니다.

`ARFrame` 개체도 예상된 장치 위치를 포함 합니다.  게임 카메라 ARFrame 이동 해야-ARKit 전에 없었습니다 하 추적 장치 방향 (롤포워드, 피치 및 요) 비디오 위에 고정된 홀로그램 렌더링-하며 장치를 조금 이동 하는 경우-그리 홀로그램 이제 드리프트 됩니다.

자이로스코프가 같은 내장 센서 이동을 추적할 수 없기 때문에 이런, 가속만 할 수 있습니다.  ARKit 분석 각 프레임 및 추출 기능 지점은 추적 되며 따라서 수를 정확 하 게 제공 하는 데이터 이동 및 회전 포함 된 행렬을 변환 합니다.

이 예를 들어 방법에서는 현재 위치를 가져올 수 있습니다.

```csharp
var row = arCamera.Transform.Row3;
CameraNode.Position = new Vector3(row.X, row.Y, -row.Z);
```

사용 하 여 `-row.Z` ARKit 오른손 좌표계를 사용 하기 때문에 있습니다.


### <a name="plane-detection"></a>평면 검색

ARKit는 가로 평면을 검색할 수 있습니다 하 고이 기능을 사용 하면 실제 세계와 상호 작용할 수 있습니다, 그리고 예를 들어, 실제 테이블 또는 층을 뮤턴트를 지정할 수 있습니다. 작업을 수행 하는 가장 간단한 방법은 HitTest 메서드 (플레이어가)를 사용 하는 것입니다. 화면 좌표로 변환 (0.5, 0.5 중앙) 실제 좌표를 (0, 0 0은 첫 번째 프레임의 위치).

```chsarp
protected Vector3? HitTest(float screenX = 0.5f, float screenY = 0.5f)
{
    var result = ARSession.CurrentFrame.HitTest(new CGPoint(screenX, screenY),
        ARHitTestResultType.ExistingPlaneUsingExtent)?.FirstOrDefault();
    if (result != null)
    {
        var row = result.WorldTransform.Row3;
        return new Vector3(row.X, row.Y, -row.Z);
    }
    return null;
}
```

이제 탭에서는 장치 화면에서 위치에 따라 가로 화면에서를 뮤턴트를 지정할 수 있습니다.

```chsarp
void OnTouchEnd(TouchEndEventArgs e)
{
    float x = e.X / (float)Graphics.Width;
    float y = e.Y / (float)Graphics.Height;
    var pos = HitTest(x, y);
    if (pos != null)
    mutantNode.Position = pos.Value;
}
```

![보기 이동으로 변경 하는 애니메이션된 그림 평면](urhosharp-images/image4.gif)

### <a name="realistic-lighting"></a>사실적 조명

실제 조명 조건에 따라 더 밝게 또는 주변에 더 잘 맞게 음영이 짙을 수록 가상 장면 이어야 합니다. Urho 앰비언트 조명 조정에 사용할 수 있는 LightEstimate 속성을 포함 하는 ARFrame 이렇게 같이:


    var ambientIntensity = (float) frame.LightEstimate.AmbientIntensity / 1000f;
    var zone = Scene.GetComponent<Zone>();
    zone.AmbientColor = Color.White * ambientIntensity;


### <a name="beyond-ios---hololens"></a>IOS-HoloLens 초과

UrhoSharp [모든 주요 운영 체제에서 실행](~/graphics-games/urhosharp/platform/index.md)이므로 다른 곳에서 기존 코드를 다시 사용할 수 있습니다.

HoloLens에서 실행 하는 가장 흥미로운 플랫폼 중 하나입니다.   즉, 있습니다 iOS과 UrhoSharp 사용 되는 멋진 증강 현실 응용 프로그램을 빌드하는 HoloLens 간을 쉽게 전환할 수 있습니다.

MutantDemo 원본에서 찾을 수 있습니다 [github.com/EgorBo/ARKitXamarinDemo](https://github.com/EgorBo/ARKitXamarinDemo)합니다.


## <a name="related-links"></a>관련 링크

- [UrhoSharp](~/graphics-games/urhosharp/index.md)
- [(사용 하 여 UrhoSharp) ARKitXamarinDemo (샘플)](https://github.com/EgorBo/ARKitXamarinDemo)
