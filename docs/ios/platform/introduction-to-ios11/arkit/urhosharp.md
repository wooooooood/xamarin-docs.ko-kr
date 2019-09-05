---
title: Xamarin.ios에서 ARKit를 사용 하 여 UrhoSharp 사용
description: 이 문서에서는 Xamarin.ios에서 ARKit 앱을 설정 하는 방법에 대해 설명 하 고, 프레임 렌더링 방법, 카메라를 조정 하는 방법, 비행기를 검색 하는 방법, 조명으로 작업 하는 방법 등을 살펴봅니다. 또한 HoloLens에 대 한 코드 작성에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 877AF974-CC2E-48A2-8E1A-0EF9ABF2C92D
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 08/01/2017
ms.openlocfilehash: 4c23caade91a1a46d6b2b9bb2425a5bdead40030
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70289242"
---
# <a name="using-arkit-with-urhosharp-in-xamarinios"></a>Xamarin.ios에서 ARKit를 사용 하 여 UrhoSharp 사용

[Arkit](https://developer.apple.com/arkit/)가 도입 되면서 Apple은 개발자가 확장 된 현실 응용 프로그램을 간편 하 게 만들 수 있게 되었습니다. ARKit는 장치의 정확한 위치를 추적 하 고 전 세계 다양 한 화면을 검색할 수 있으며, 개발자는 ARKit에서 제공 되는 데이터를 코드에 혼합할 수 있습니다.

[Urhosharp](~/graphics-games/urhosharp/index.md) 는 3d 응용 프로그램을 만드는 데 사용할 수 있는 포괄적이 고 사용 하기 쉬운 3d API를 제공 합니다.   둘 다 함께 혼합할 수 있습니다. ARKit는 전 세계에 대 한 물리적 정보를 제공 하 고, 결과를 렌더링 하는 Urho를 제공 합니다.

이 페이지에서는 이러한 두 가지를 함께 연결 하 여 뛰어난 확대 현실 응용 프로그램을 만드는 방법을 설명 합니다.

## <a name="the-basics"></a>기본 사항

IPhone/iPad에서 볼 수 있듯이 전 세계에 3D 콘텐츠가 제공 됩니다.   여기서는 장치 카메라에서 제공 되는 콘텐츠를 3D 콘텐츠와 혼합 하 고, 장치 사용자가 공간을 중심으로 이동 하 여 3D 개체가 해당 대화방의 일부인 것 처럼 동작 하도록 하는 것이 좋습니다 .이 작업은 개체를이 세계에 고정 하 여 수행 됩니다.

![ARKit의 애니메이션 그림](urhosharp-images/image1.gif)

여기서는 Urho 라이브러리를 사용 하 여 3D 자산을 로드 하 고 전 세계에 배치 하며, ARKit를 사용 하 여 카메라에서 들어오는 비디오 스트림과 전 세계의 휴대폰 위치를 가져옵니다.   사용자가 휴대폰으로 이동 하면 위치에서 변경 사항을 사용 하 여 Urho 엔진이 표시 하는 좌표계를 업데이트 합니다.

이러한 방식으로 3D 공간에 개체를 배치 하 고 사용자가 이동할 때 3D 개체의 위치는 위치와 위치가 배치 된 위치를 반영 합니다.

## <a name="setting-up-your-application"></a>응용 프로그램 설정

### <a name="ios-application-launch"></a>iOS 응용 프로그램 시작

IOS 응용 프로그램에서 3d 콘텐츠를 만들고 시작 해야 합니다 `Urho.Application` .이 작업을 수행 하려면의 서브 클래스를 구현 하 고 메서드를 `Start` 재정의 하 여 설치 코드를 제공 합니다.  여기서는 장면을 데이터로 채우고 이벤트 처리기를 설정 하는 등의 방법을 사용 합니다.

서브 `Urho.ArkitApp` `Start` 클래스와 해당 메서드의 메서드가 많은 리프트를 수행 하는 클래스를 도입 했습니다. `Urho.Application`   기존 urho 응용 프로그램에 대 한 모든 작업을 수행 하려면 기본 클래스를 형식 `Urho.ArkitApp` 으로 변경 하 고 전 세계에서 urho 장면을 실행 하는 응용 프로그램을 만들어야 합니다.

### <a name="the-arkitapp-class"></a>ArkitApp 클래스

이 클래스는 운영 체제에서 제공 하는 ARKit 이벤트의 처리 뿐만 아니라 몇 가지 주요 개체를 포함 하는 장면의 편리한 기본값 집합을 제공 합니다.

설치 프로그램이 `Start` 가상 메서드에서 수행 됩니다.   서브 클래스에서이 메서드를 재정의 하는 경우 고유한 구현에서를 사용 `base.Start()` 하 여 부모에 연결 해야 합니다.

메서드 `Start` 는 장면, 뷰포트, 카메라 및 방향성 광원을 설정 하 고 공용 속성으로 표시 합니다.

- 개체를 보유할입니다. `Scene`
- 그림자가 `Light` 있는 방향성 이며, 속성을 `LightNode` 통해 해당 위치를 사용할 수 있습니다.
- arkit에서 응용 프로그램에 대 한 업데이트를 전달할 때 구성 요소가 업데이트 되는 `Camera` 입니다.
- 결과 `ViewPort` 를 표시 하는입니다.

### <a name="your-code"></a>코드

그런 다음 클래스의 `ArkitApp` 서브 클래스를 만들고 메서드를 `Start` 재정의 해야 합니다.   메서드가 수행 해야 하는 첫 번째 작업은를 호출 `ArkitApp.Start` `base.Start()`하 여에 연결 하는 것입니다.  그런 다음 ArkitApp를 사용 하 여 개체를 장면에 추가 하 고 처리할 광원, 그림자 또는 이벤트를 사용자 지정할 수 있습니다.

ARKit/UrhoSharp 샘플은 질감이 있는 애니메이션 문자를 로드 하 고 다음 구현을 사용 하 여 애니메이션을 재생 합니다.

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

그리고이 시점에서 3D 콘텐츠를 확대 된 현실에 표시 하기 위해 수행 해야 하는 것은 사실입니다.

Urho는 3D 모델 및 애니메이션에 사용자 지정 형식을 사용 하므로 자산을이 형식으로 내보내야 합니다.   [Urho3D Blender 추가 기능](https://github.com/reattiva/Urho3D-Blender) 및 [UrhoAssetImporter](https://github.com/EgorBo/UrhoAssetImporter) 와 같은 도구를 사용 하 여 이러한 자산을 .DBX, DAE, OBJ, Blend, 3d-Max와 같은 인기 있는 형식에서 urho에 필요한 형식으로 변환할 수 있습니다.

Urho를 사용 하 여 3D 응용 프로그램을 만드는 방법에 대해 자세히 알아보려면 [UrhoSharp 소개](~/graphics-games/urhosharp/introduction.md) 가이드를 참조 하세요.

## <a name="arkitapp-in-depth"></a>ArkitApp

> [!NOTE]
> 이 섹션은 UrhoSharp 및 ARKit의 기본 환경을 사용자 지정 하려는 개발자를 위한 것입니다. 또는 통합이 작동 하는 방식을 심층적으로 파악 하고자 합니다.   이 섹션을 읽을 필요는 없습니다.

ARKit API는 매우 간단 하며, [ARSession](https://developer.apple.com/documentation/arkit/arsession) 개체를 만들고 구성한 다음 [ARFrame](https://developer.apple.com/documentation/arkit/arframe) 개체를 전달 하기 시작 합니다.   여기에는 카메라에 의해 캡처된 이미지와 장치의 예상 실제 위치가 모두 포함 됩니다.

카메라에서 3D 콘텐츠를 사용 하 여 전달 되는 이미지를 작성 하 고, 장치 위치 및 위치에 대 한 기회와 일치 하도록 UrhoSharp의 카메라를 조정 합니다.

다음 다이어그램에서는 `ArkitApp` 클래스에서 수행 되는 작업을 보여 줍니다.

[![ArkitApp의 클래스 및 화면 다이어그램](urhosharp-images/image2.png)](urhosharp-images/image2.png#lightbox)

### <a name="rendering-the-frames"></a>프레임 렌더링

아이디어는 간단 하며 카메라에서 제공 되는 비디오를 3D 그래픽으로 결합 하 여 결합 된 이미지를 생성 합니다.     이러한 캡처된 이미지를 순서 대로 가져오는 것 이며,이 입력을 Urho 장면으로 혼합 합니다.

이 작업을 수행 하는 가장 간단한 방법은를 `RenderPathCommand` main `RenderPath`에 삽입 하는 것입니다.  단일 프레임을 그리기 위해 수행 하는 일련의 명령입니다.  이 명령은 뷰포트를 전달 하는 모든 질감으로 채웁니다.    이는 처리 되는 첫 번째 프레임에서 설정 하 고 실제 정의는이 시점에서 로드 되는 th **Arrenderpath .xml** 파일에서 수행 됩니다.

그러나 두 가지 문제를 함께 결합 하는 두 가지 문제에 직면 하 고 있습니다.


1. IOS에서 GPU 질감의 해상도는 2의 거듭제곱 이어야 하지만 카메라에서 가져올 프레임의 해상도는 2의 거듭제곱 이어야 합니다. 예를 들면 다음과 같습니다. 1280x720.
2. 프레임은 [YUV](https://en.wikipedia.org/wiki/YUV) 형식으로 인코딩되고, 두 이미지 (루마 및 크로마)로 표시 됩니다.

YUV 프레임은 서로 다른 두 가지 해상도로 제공 됩니다.  광도 구성 요소에 대 한 광도 (기본적으로 회색 눈금 이미지)와 훨씬 더 작은 640x360을 나타내는 1280x720 이미지는 다음과 같습니다.

![Y 및 UV 구성 요소 결합을 보여 주는 이미지](urhosharp-images/image3.png)


OpenGL ES를 사용 하 여 색이 지정 된 전체 이미지를 그리려면 질감 슬롯에서 광도 (Y 구성 요소) 및 색차 (UV 평면)를 사용 하는 작은 셰이더를 작성 해야 합니다.  UrhoSharp에는 이름 ("sDiffMap" 및 "Sdiffmap")이 있으며이를 RGB 형식으로 변환 합니다.

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

두 해상도의 거듭제곱이 없는 질감을 렌더링 하려면 다음 매개 변수를 사용 하 여 Texture2D를 정의 해야 합니다.

```chsarp
// texture for UV-plane;
cameraUVtexture = new Texture2D();
cameraUVtexture.SetNumLevels(1);
cameraUVtexture.SetSize(640, 360, Graphics.LuminanceAlphaFormat, TextureUsage.Dynamic);
cameraUVtexture.FilterMode = TextureFilterMode.Bilinear;
cameraUVtexture.SetAddressMode(TextureCoordinate.U, TextureAddressMode.Clamp);
cameraUVtexture.SetAddressMode(TextureCoordinate.V, TextureAddressMode.Clamp);
```

따라서 캡처된 이미지를 배경으로 렌더링 하 고이를 통해 해당 하는 모든 장면을 mutant 수 있습니다.

### <a name="adjusting-the-camera"></a>카메라 조정

또한 `ARFrame` 개체에는 예상 장치 위치도 포함 됩니다.  이제 게임 카메라 ARFrame을 이동 해야 합니다. Arframe는 장치 방향 (roll, 피치 및 요)을 추적 하 고 비디오 위에 고정 된 홀로그램을 렌더링 하는 것은 아닙니다. 하지만 장치를 이동 하는 경우에는 비트 holograms가 드리프트 됩니다.

이는 자이로스코프가 같은 기본 제공 센서가 이동을 추적할 수 없고 가속만 가능 하기 때문에 발생 합니다.  ARKit는 각 프레임을 분석 하 여 추적할 기능 요소를 추출 하므로 이동 및 회전 데이터를 포함 하는 정확한 변환 매트릭스를 제공할 수 있습니다.

예를 들어 현재 위치를 가져오는 방법은 다음과 같습니다.

```csharp
var row = arCamera.Transform.Row3;
CameraNode.Position = new Vector3(row.X, row.Y, -row.Z);
```

Arkit `-row.Z` 에서 오른손 좌표계를 사용 하기 때문에를 사용 합니다.


### <a name="plane-detection"></a>평면 검색

ARKit는 가로 평면을 검색할 수 있으며이 기능을 사용 하면 실제 테이블 또는 층에 mutant를 추가할 수 있습니다. 이 작업을 수행 하는 가장 간단한 방법은 System.windows.media.visualtreehelper.hittest 메서드 (raycasting)를 사용 하는 것입니다. 화면 좌표 (0.5; 0.5는 가운데)를 실제 좌표 (0; 0; 0)로 변환 합니다. 는 첫 번째 프레임의 위치)입니다.

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

이제 장치 화면에서 탭 하는 위치에 따라 가로 화면에 mutant를 넣을 수 있습니다.

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

![평면을 보기 이동으로 변경 하는 애니메이션 그림](urhosharp-images/image4.gif)

### <a name="realistic-lighting"></a>현실적인 조명

실제 조명 조건에 따라 가상 장면을 주변 환경에 맞게 더 밝게 하거나 더 밝게 해야 합니다. ARFrame에는 Urho 앰비언트 조명을 조정 하는 데 사용할 수 있는 LightEstimate 속성이 포함 되어 있습니다 .이 속성은 다음과 같이 수행 됩니다.

```csharp
var ambientIntensity = (float) frame.LightEstimate.AmbientIntensity / 1000f;
var zone = Scene.GetComponent<Zone>();
zone.AmbientColor = Color.White * ambientIntensity;
```

### <a name="beyond-ios---hololens"></a>IOS 이상-HoloLens

UrhoSharp는 [모든 주요 운영 체제에서 실행](~/graphics-games/urhosharp/platform/index.md)되므로 기존 코드를 다른 곳에서 재사용할 수 있습니다.

HoloLens는 실행 중인 가장 흥미로운 플랫폼 중 하나입니다.   즉, iOS와 HoloLens 간을 쉽게 전환 하 여 UrhoSharp를 사용 하는 뛰어난 확대 현실 응용 프로그램을 빌드할 수 있습니다.

MutantDemo 소스는 [github.com/EgorBo/ARKitXamarinDemo](https://github.com/EgorBo/ARKitXamarinDemo)에서 찾을 수 있습니다.


## <a name="related-links"></a>관련 링크

- [UrhoSharp](~/graphics-games/urhosharp/index.md)
- [ARKitXamarinDemo (UrhoSharp) (샘플)](https://github.com/EgorBo/ARKitXamarinDemo)
