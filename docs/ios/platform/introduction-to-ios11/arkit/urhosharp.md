---
title: "UrhoSharp ARKit 사용"
ms.topic: article
ms.prod: xamarin
ms.assetid: 877AF974-CC2E-48A2-8E1A-0EF9ABF2C92D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/01/2016
ms.openlocfilehash: 94963e92f8372316a982bbe38f1fb653d38b2a3b
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="using-arkit-with-urhosharp"></a>UrhoSharp ARKit 사용

도입으로 [ARKit](https://developer.apple.com/arkit/), 사과 아무런 상관이 보강 된 현실 응용 프로그램을 만드는 개발자를 위한 간단한 합니다. ARKit 장치의 정확한 위치를 추적 하 고 전 세계에 다양 한 화면을 검색할 수 및은 다음 코드에 ARKit에서 들어오는 데이터를 혼합 하려면 개발자의 책임입니다.

[UrhoSharp](~/graphics-games/urhosharp/index.md) 3D 응용 프로그램을 만드는 데 사용할 수 있는 한 포괄적이 고 사용 하기 쉬운 3D API를 제공 합니다.   둘 다에 해당 될 수 있습니다 ARKit 실제 세계에 대 한 정보를 제공 하 고 결과를 렌더링할 Urho 서로 혼합 된 합니다.

이 페이지에서는이 두 환경을 함께 뛰어난 보강 된 현실 응용 프로그램을 만드는 연결 하는 방법을 설명 합니다.


## <a name="the-basics"></a>기본 사항

IPhone에서 인식 되는 전 세계 맨 위에 있는 3D 콘텐츠는 수행 하려는 작업 합니다.   3D 내용 사용 하 여 휴대폰의 카메라에서 오는 내용을 혼합 하는 개념은 및 전화의 사용자 되도록 대화방 주위를 이동할 때 3D 개체에는 해당 대화방의 일부인 것 처럼 동작-이 업계에 개체를 고정 하 여이 작업 수행 됩니다.

![ARKit에 애니메이션 효과 준된 그림](urhosharp-images/image1.gif)


사용할 것임 Urho 라이브러리 로드 3D 자산을 전 세계에 배치 하 고 사용할 것임 ARKit으로 카메라 전 세계의 전화의 위치에서 들어오는 비디오 스트림을 가져오기.   그의 휴대폰으로 사용자를 이동할 때 사용 합니다 변경 내용을 위치에 Urho 엔진을 표시 하는 좌표 시스템을 업데이트 합니다.

이러한 방식으로 3D 공간에서 개체를 배치 하 고 사용자가 이동 3D 개체의 위치를 반영 위치와 배치 된 위치 합니다.

## <a name="setting-up-your-application"></a>응용 프로그램을 설정합니다.

### <a name="ios-application-launch"></a>iOS 응용 프로그램 시작

IOS 응용 프로그램을 만들고 3D 콘텐츠를 시작, 구현 하는의 서브 클래스를 만들어이 작업을 수행 된 [ `Urho.Application` ](https://developer.xamarin.com/api/type/Urho.Application/) 재정의 하 여 설치 코드를 제공 하 고는 `Start` 메서드.  이 데이터를 이벤트 처리기를 등 설정 장면이 가져옵니다 채워집니다.

도입 되었습니다는 `Urho.ArkitApp` 서브클래싱하는 클래스 `Urho.Application` 및 해당 `Start` 메서드는 중요 한 역할을 수행 합니다.   형식이 되도록 기본 클래스를 변경 하기만 하면 작업을 수행 하 여 기존 Urho 응용 프로그램은 `Urho.ArkitApp` 프로그램 urho 장면 세계에서 실행 되는 응용 프로그램 및입니다.

### <a name="the-arkitapp-class"></a>ArkitApp 클래스

이 클래스는 운영 체제에서 배달 될 때는 편리한 기본값, 일부 주요 개체에 장면이 집합 뿐 아니라 ARKit 이벤트 처리를 제공 합니다.

설치 프로그램에서 수행 됩니다는 `Start` 가상 메서드.   사용 하 여 부모로 체인에 있는지 확인 해야 하는 서브 클래스에서이 메서드를 재정의 하는 경우 `base.Start()` 고유한 구현에 있습니다.

`Start` 메서드 장면, 뷰포트, 카메라 및 방향성 광원을 설정 하 고 공용 속성으로 하는 것을 표시 합니다.

- [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Scene/) 개체를 포함 하려면
- 방향 [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light/) 그림자 및 해당 위치를 통해 사용할 수 있는 `LightNode` 속성
- [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera/) ARKit 응용 프로그램에 대 한 업데이트를 배달할 때 구성 요소 업데이트 하 고
- [ `ViewPort` ](https://developer.xamarin.com/api/type/Urho.Viewport/) 결과 표시 합니다.


### <a name="your-code"></a>코드

그런 다음 하위 클래스 해야는 `ArkitApp` 클래스를 재정의 `Start` 메서드.   최대 가장 먼저 방법을 수행 해야 하는 체인에서 `ArkitApp.Start` 호출 하 여 `base.Start()`합니다.  그 후에 추가 하 여 개체 장면을 lights, 그림자 등에 또는 이벤트를 처리 하려면 사용자 지정 ArkitApp 하 여 속성 설정을 사용할 수 있습니다.

애니메이션된 캐릭터 질감으로 로드 하 고 다음 구현으로 애니메이션을 재생 하는 ARKit/UrhoSharp 샘플.

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

있고 3D 콘텐츠 보강 된 현실에 표시 하려면이 시점에서 수행 해야 하는 모든 작업을 실제로입니다.

Urho 자산이이 형식으로 내보낼 필요 하므로 3D 모델 및 애니메이션에 대 한 사용자 지정 형식을 사용 합니다.   와 같은 도구를 사용할 수는 [add-in Urho3D 블렌더](https://github.com/reattiva/Urho3D-Blender) 및 [UrhoAssetImporter](https://github.com/EgorBo/UrhoAssetImporter) Urho에 필요한 형식으로 3D 최대 DBX, 디지털 오디오, OBJ, Blend와 같은 인기 있는 형식에서 이러한 자산을 변환할 수 있는 합니다.

Urho를 사용 하 여 3D 응용 프로그램을 만드는 방법에 대 한 자세한 내용은 참조는 [UrhoSharp 소개](~/graphics-games/urhosharp/introduction.md) 가이드입니다.

## <a name="arkitapp-in-depth"></a>심층 ArkitApp

> [!NOTE]
> 이 섹션은 개발자 UrhoSharp 및 ARKit 기본 환경을 사용자 지정 하거나 가져올 통합 작동 하는 방법에 대해 깊이 이해할을 위한 것입니다.   이 섹션을 읽을 필요는 없습니다.

ARKit API는 상당히 간단, 만들기 및 구성 된 [ARSession](https://developer.apple.com/documentation/arkit/arsession) 개체는 다음 배달 시작 [ARFrame](https://developer.apple.com/documentation/arkit/arframe) 개체입니다.   여기에 카메라 뿐만 아니라 장치의 예상된 실제 위치에 의해 캡처된 이미지 모두 포함 합니다.

म에 배달 하는 카메라에서 우리 우리의 3D 콘텐츠를 포함 하는 이미지를 작성 되며 카메라 UrhoSharp 장치 위치와 위치 가능성에 맞게 조정 합니다.

다음 그림 수행 되 어떤는 `ArkitApp` 클래스:

[![클래스 및 화면에는 ArkitApp의 다이어그램](urhosharp-images/image2.png)](urhosharp-images/image2.png#lightbox)

### <a name="rendering-the-frames"></a>프레임을 렌더링

개념은 쉽게, 결합 된 이미지를 생성 하는 3D 그래픽 카메라에서 들어오는 비디오에 결합 합니다.     म 가져오는 것은 일련의 캡처된 이미지에 이러한 순서로 그리고 우리는이 입력 Urho 장면와 합니다.

그렇게 하는 가장 간단한 방법은 삽입 하는 한 [ `RenderPathCommand` ](https://developer.xamarin.com/api/type/Urho.RenderPathCommand/) 는 main [ `RenderPath` ](https://developer.xamarin.com/api/type/Urho.RenderPath/)합니다.  단일 프레임을 그리는 데 수행 되는 명령의 집합입니다.  이 명령은 뷰포트를 전달 하는 모든 질감으로 채워집니다.    프로세스를 첫 번째 프레임을 설정이, 실제 정의에 번째 **ARRenderPath.xml** 이 시점에서 로드 된 파일입니다.

그러나 우리는 문제에 직면이 두 환경을 혼합 두 가지 문제:


1. Ios, GPU 질감은 2의 거듭제곱이 해결 하지만, 카메라에서 구했습니다 프레임 예 2의 거듭제곱 하는 확인 하지 않은: 1280 x 720입니다.
2. 인코딩 되는 프레임 [YUV](https://en.wikipedia.org/wiki/YUV) 두 가지 이미지-광도가 됩니다 및 명도 나타내는 형식입니다.

두 개의 서로 다른 해상도 YUV 프레임으로 제공 됩니다.  훨씬 더 작은 640 x 360 색차 구성 요소에 대 한 광도 (기본적으로 회색조 이미지)을 나타내는 1280 x 720 이미지:

![Y 결합 및 UV 구성 요소를 보여 주는 이미지](urhosharp-images/image3.png)


OpenGL ES를 사용 하 여 전체 색이 지정 된 이미지를 그릴 광도 (Y 구성 요소) 및 (UV 평면) 색차 질감 슬롯에서 작업을 수행 하는 작은 셰이더를 작성 해야 합니다.  UrhoSharp에 이름-"sDiffMap" 및 "sNormalMap" 및는 RGB 형식으로 변환 하 합니다.

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

두 개의 해상도의 거듭제곱 맺지 않은 텍스처를 렌더링 하는 다음 매개 변수 Texture2D 정의 해야 합니다.

```chsarp
// texture for UV-plane;
cameraUVtexture = new Texture2D();
cameraUVtexture.SetNumLevels(1);
cameraUVtexture.SetSize(640, 360, Graphics.LuminanceAlphaFormat, TextureUsage.Dynamic);
cameraUVtexture.FilterMode = TextureFilterMode.Bilinear;
cameraUVtexture.SetAddressMode(TextureCoordinate.U, TextureAddressMode.Clamp);
cameraUVtexture.SetAddressMode(TextureCoordinate.V, TextureAddressMode.Clamp);
```

따라서 배경으로 캡처된 이미지를 렌더링 하 고 그 위에 모든 장면 그렇게 무시 무시 뮤턴트 렌더링할 수 있습니다.

### <a name="adjusting-the-camera"></a>카메라를 조정합니다.

`ARFrame` 개체도 예상된 장치 위치를 포함 합니다.  게임 카메라 ARFrame 이동 해야-ARKit 전에 없었습니다 추적 장치 방향 (롤포워드, 피치 및 요) 및 렌더링 비디오 위에 고정 된 홀로그램-하지만 이동 하는 경우 장치 약간-큰 문제가 제공 이제 우리 드리프트 됩니다.

자이로스코프가 같은 기본 제공 센서 움직임을 추적할 수 없기 때문에 발생 하는, 가속만 할 수 있습니다.  각 프레임 및 추출 기능 지점은 추적 되어 있으므로 수 정확한 남겨둡니다 ARKit 분석 이동 및 회전 데이터를 포함 하는 매트릭스를 변형 합니다.

예를 들어,이 어떻게 현재 위치를 가져올 수 있습니다.

```csharp
var row = arCamera.Transform.Row3;
CameraNode.Position = new Vector3(row.X, row.Y, -row.Z);
```

사용 하 여 `-row.Z` ARKit 오른손 좌표계를 사용 하기 때문에 있습니다.


### <a name="plane-detection"></a>평면 검색

ARKit는 가로 평면을 검색할 수 하는이 기능을 사용 하면 실제 세계와 상호 작용할 수 있습니다, 그리고는 뮤턴트는 실제 테이블 또는 바닥에 지정할 수는 예를 들어 있습니다. 작업을 수행 하는 가장 간단한 방법은 HitTest 메서드 (raycasting)를 사용 하는 합니다. 화면 좌표를 변환 (0.5, 0.5는 중심)에 실제 세계 좌표 (0, 0; 0은 첫 번째 프레임의 위치).

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

이제 म 탭은 장치 화면에서 위치에 따라 가로 화면에는 뮤턴트를 배치할 수 있습니다.

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

![보기 이동한 것으로 변경 하는 애니메이션된 그림 평면](urhosharp-images/image4.gif)

### <a name="realistic-lighting"></a>실제 광원

현실 세계 조명 조건에 따라 가상 장면 밝은 또는 주변 항목을 더 잘 맞게 짙을 수록 여야 합니다. ARFrame Urho 앰비언트 조명을 조정에 사용할 수 있는 LightEstimate 속성이 포함 되어이 작업은 수행 다음과 같이 합니다.


    var ambientIntensity = (float) frame.LightEstimate.AmbientIntensity / 1000f;
    var zone = Scene.GetComponent<Zone>();
    zone.AmbientColor = Color.White * ambientIntensity;


### <a name="beyond-ios---hololens"></a>IOS-HoloLens 초과

UrhoSharp [모든 주요 운영 체제에서 실행](~/graphics-games/urhosharp/platform/index.md)이므로 다른 곳에서 기존 코드를 다시 사용할 수 있습니다.

HoloLens 가장 흥미로운 플랫폼에서 실행 중 하나입니다.   즉, 있습니다 iOS 및 HoloLens UrhoSharp를 사용 하 여 놀라운 보강 된 현실 응용 프로그램을 빌드할 간을 쉽게 전환할 수 있습니다.

MutantDemo 소스를 찾을 수 [github.com/EgorBo/ARKitXamarinDemo](https://github.com/EgorBo/ARKitXamarinDemo)합니다.


## <a name="related-links"></a>관련 링크

- [UrhoSharp](~/graphics-games/urhosharp/index.md)
- [(UrhoSharp)와 ARKitXamarinDemo (샘플)](https://github.com/EgorBo/ARKitXamarinDemo)
