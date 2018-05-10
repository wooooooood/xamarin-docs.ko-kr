---
title: UrhoSharp 소개
description: 이렇게 하면 UrhoSharp 개념에 대 한 간략 한 소개
ms.prod: xamarin
ms.assetid: 18041443-5093-4AF7-8B20-03E00478EF35
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 6fb53aa6d4bad8f8fcae8608ab0192af15fe87b1
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
---
# <a name="an-introduction-to-urhosharp"></a>UrhoSharp 소개

_이렇게 하면 UrhoSharp 개념에 대 한 간략 한 소개_

![UrhoSharp 로고](introduction-images/urhosharp-icon.png)

UrhoSharp는 Xamarin 및.NET 개발자를 위한 강력한 3D 게임 엔진입니다.  작업은 Apple의 SceneKit 및 SpriteKit를 하 고 물리학 포함, 탐색, 네트워킹 및 대부분의 플랫폼 크로스 여전히 되 고 있지만 더 합니다.

에 대 한.NET 바인딩을는 [Urho3D](http://urho3d.github.io/) 엔진 날짜와 개발자가 Android, iOS 대상으로 지정할 수 있는 플랫폼 간 코드를 작성할 수 있도록, Windows 및 Mac과 동일한 코드 베이스 OpenGL 및 Direct3D 시스템 모두에 렌더링할 수 있습니다.

UrhoSharp는 기능을 많이 게임 엔진:

- 강력한 3D 그래픽 렌더링
- [시뮬레이션 물리학](https://developer.xamarin.com/api/namespace/Urho.Physics/) (글머리 기호 라이브러리 사용)
- [장면 처리](https://developer.xamarin.com/api/type/Urho.Scene/)
- Await/비동기 지원
- [친숙 한 API 작업](https://developer.xamarin.com/api/namespace/Urho.Actions/)
- [2D 통합은 3D 장면](https://developer.xamarin.com/api/namespace/Urho.Urho2D/)
- [FreeType 사용 하 여 글꼴 렌더링](https://developer.xamarin.com/api/type/Urho.Gui.FontFaceFreeType/)
- [클라이언트와 서버 네트워킹 기능](https://developer.xamarin.com/api/namespace/Urho.Network/)
- [광범위 한 자산 가져오기](https://developer.xamarin.com/api/namespace/Urho.Resources/) 자산 라이브러리 열기) (있음
- [탐색 메시 및 경로 찾기](https://developer.xamarin.com/api/namespace/Urho.Navigation/) (다시 캐스팅/우회 사용)
- [충돌 감지에 대 한 볼록 집합 생성](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape/) (StanHull 사용)
- [오디오 재생](https://developer.xamarin.com/api/namespace/Urho.Audio/) (으로 **libvorbis**)

## <a name="getting-started"></a>시작

UrhoSharp로 편리 하 게 배포는 [NuGet 패키지](https://www.nuget.org/) 수 추가 C# 또는 F # 프로젝트에 대상으로 하는 Windows, Mac, Android 또는 iOS입니다.  NuGet 엔진에서 사용 되는 기본 자산 (대 한 CoreData)와 프로그램을 실행 하는 데 필요한 라이브러리가 모두 포함 되어 있습니다.

### <a name="urho-as-a-portable-class-library"></a>이식 가능한 클래스 라이브러리로 Urho

Urho 패키지 또는 플랫폼 특정 프로젝트에서 모든 플랫폼에서 모든 코드를 다시 사용할 수 있도록 이식 가능한 클래스 라이브러리 프로젝트에서 사용할 수 있습니다.  이 프로그램 플랫폼 특정 진입점을 작성 한 다음 공유 게임 코드에 제어를 전송 하는 모든 각 플랫폼에서 작업을 수행 하는 것을 의미 합니다.

### <a name="samples"></a>샘플

Mac 용 Visual Studio 또는 Visual Studio에서에서 샘플 솔루션을 열어 Urho의 기능에 대 한 경험을 얻을 수 있습니다.

[https://github.com/xamarin/urho-samples](https://github.com/xamarin/urho-samples)

Android, iOS에 대 한 프로젝트를 포함 하는 기본 솔루션 창과 Mac.  म가 해당 솔루션 있도록 구조화 아주 작은 플랫폼 특정 launcher 있고 이식 가능한 클래스 라이브러리에 거주 하는 모든 샘플 코드와 게임 코드 모든 플랫폼에서 코드 재사용을 최대화 하는 방법을 설명 합니다.

참조는 [Urho 및 Your 플랫폼](~/graphics-games/urhosharp/platform/index.md) 사용자 고유의 솔루션을 만드는 방법에 대 한 자세한 내용은 페이지입니다.

공통 집합이 사용자 인터페이스 요소를 공유 하는 모든 샘플 하기 때문에 샘플이이 파일에 있는 기본 설정 추상화가:

[https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs](https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs)

이 몇 가지 기본 키 입력을 처리 하는 샘플 기본 클래스를 제공 하 고 터치 이벤트를 한 설정 카메라 경우 기본 사용자 인터페이스 요소를 지정 하 고이 하면 각 샘플을 보여 줍니다 되는 특정 기능에 초점을 제공 합니다.

다음 예제는 엔진 이란 수행할 수 있는 것을 보여줍니다.

- [게임 samply](https://github.com/xamarin/urho-samples/tree/master/SamplyGame) ShootySkies의 단순 복제 합니다.

다른 샘플 각 샘플의 개별 속성을 표시 합니다.

## <a name="basic-structure"></a>기본 구조

게임의 서브 클래스 여야는 [ `Application` ](https://developer.xamarin.com/api/type/Urho.Application/) 클래스는 게임을 설치 하는 것이 이것이 (에 [ `Setup` ](https://developer.xamarin.com/api/member/Urho.Application.Setup/) 메서드) 하 여 게임을 시작 하 고 (에 [ `Start` ](https://developer.xamarin.com/api/member/Urho.Application.Start) 메서드).  다음 기본 사용자 인터페이스를 작성 합니다.  3D 장면, 일부 UI 요소 및 동작이 단순한 연결을 설정 하는 Api를 보여 주는 일부 예제를 진행 하기 위해 하겠습니다.

```csharp
class MySample : Application {
    protected override void Start ()
    {
        CreateScene ();
        Input.KeyDown += (args) => {
            if (args.Key == Key.Esc) Exit ();
        };
    }

    async void CreateScene()
    {
        // UI text
        var helloText = new Text()
        {
            Value = "Hello World from MySample",
            HorizontalAlignment = HorizontalAlignment.Center,
            VerticalAlignment = VerticalAlignment.Center
        };
        helloText.SetColor(new Color(0f, 1f, 1f));
        helloText.SetFont(
            font: ResourceCache.GetFont("Fonts/Font.ttf"),
            size: 30);
        UI.Root.AddChild(helloText);

        // Create a top-level scene, must add the Octree
        // to visualize any 3D content.
        var scene = new Scene();
        scene.CreateComponent<Octree>();
        // Box
        Node boxNode = scene.CreateChild();
        boxNode.Position = new Vector3(0, 0, 5);
        boxNode.Rotation = new Quaternion(60, 0, 30);
        boxNode.SetScale(0f);
        StaticModel modelObject = boxNode.CreateComponent<StaticModel>();
        modelObject.Model = ResourceCache.GetModel("Models/Box.mdl");
        // Light
        Node lightNode = scene.CreateChild(name: "light");
        lightNode.SetDirection(new Vector3(0.6f, -1.0f, 0.8f));
        lightNode.CreateComponent<Light>();
        // Camera
        Node cameraNode = scene.CreateChild(name: "camera");
        Camera camera = cameraNode.CreateComponent<Camera>();
        // Viewport
        Renderer.SetViewport(0, new Viewport(scene, camera, null));
        // Perform some actions
        await boxNode.RunActionsAsync(
            new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1)));
        await boxNode.RunActionsAsync(
            new RepeatForever(new RotateBy(duration: 1,
                deltaAngleX: 90, deltaAngleY: 0, deltaAngleZ: 0)));
     }
}
```

이 응용 프로그램을 실행 하는 경우 금방 알 수는 런타임에서 불만족 자산 사항이 없습니다.  수행 해야 할 특별 한 디렉터리 이름을 "데이터"로 시작 하 여 프로젝트의 계층 구조를 만들 이며이 내 프로그램에서 참조 하는 자산을 삽입할 수 있습니다.  다음을 설정 해야에서 각 자산에서 "출력 디렉터리로 복사"를 "내용만 복사에 대 한 항목 속성을 하는 확인 데이터 있는지 합니다.

여기의 진행 상황에 대해 설명 하겠습니다.

응용 프로그램을 시작 하려면 다음과 같이 응용 프로그램 클래스의 새 인스턴스를 만들어 다음 엔진 초기화 함수를 호출할 수 있습니다.

```csharp
new MySample().Run();
```

런타임에서 호출 됩니다는 `Setup` 및 `Start` 메서드.  재정의 하는 경우 `Setup` 엔진 매개 변수 (이 샘플에 표시 하지 않으려면)를 구성할 수 있습니다.

재정의 해야 `Start` 으로 게임 시작 됩니다.  이 방법에서는 자산 로드, 이벤트 처리기를 연결, 장면이 설정을 원하는 작업 시작.  이 샘플에서는 둘 다에 약간의 3D 장면 설정 뿐만 아니라 사용자를 표시 하도록 UI 만듭니다.

코드의 다음 부분에서는 UI 프레임 워크를 사용 하 여 텍스트 요소를 만들고 응용 프로그램에 추가 합니다.

```csharp
// UI text
var helloText = new Text()
{
    Value = "Hello World from UrhoSharp",
    HorizontalAlignment = HorizontalAlignment.Center,
    VerticalAlignment = VerticalAlignment.Center
};
helloText.SetColor(new Color(0f, 1f, 1f));
helloText.SetFont(
    font: ResourceCache.GetFont("Fonts/Font.ttf"),
    size: 30);
UI.Root.AddChild(helloText);
```

UI 프레임 워크는 매우 간단한 게임 사용자 인터페이스를 제공 하 고 새 노드를 추가 하 여 작동는 [ `UI.Root` ](https://developer.xamarin.com/api/property/Urho.Gui.UI.Root/) 노드.

이 샘플의 설치의 두 번째 부분 주 장면 합니다.  3D 장면 화면의 3D 상자 만들기 광원, 카메라 및 뷰포트 추가 만들기 단계 수가 포함 됩니다.  섹션에서 더 자세하게에 대해서는 이러한 [장면, 노드, 구성 요소 및 카메라](~/graphics-games/urhosharp/using.md#scenenodescomponentsandcameras)합니다.

세 번째 부분은 샘플 몇을 가지 작업을 트리거합니다.  작업을 호출 하 여 필요에 따라 노드에서 레시피 있습니다를 만든 후 및 특정 효과 설명 하는 실행할 수 있습니다는 [ `RunActionAsync` ](https://developer.xamarin.com/api/member/Urho.Node.RunActionsAsync) 에서 메서드는 `Node`합니다.

첫 번째 동작인 바운스 효과 사용 하 여 상자 크기를 조정 하 고 두 번째 상자를 영원히 회전:

```csharp
await boxNode.RunActionsAsync(
    new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1)));
```

작성 하는 첫 번째 작업은 어떻게 위의 표시는 [ `ScaleTo` ](https://developer.xamarin.com/api/type/Urho.Actions.ScaleTo/) 속성 노드의 경우 1 값 방향으로 두 번째에 대 한 확장 되도록 나타내는 레시피 단순히이 작업을 합니다.  이 작업에 래핑하는 감속/가속 작업은 [ `EaseBounceOut` ](https://developer.xamarin.com/api/type/Urho.Actions.EaseBounceInOut/) 동작 합니다.  감속/가속 작업 동작의 선형 실행 왜곡 및 효과 적용,이 경우 아웃 반송 효과 제공 합니다.
따라서이 조리법으로 작성할 수 있습니다.

```csharp
var recipe = new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1));
```

레시피가 만들어지면 레시피 실행.

```csharp
await boxNode.RunActionsAsync (recipe)
```

Await 나타냅니다는 작업이 완료 될 때이 줄 다음 실행을 다시 시작 하려고 합니다.  작업이 완료 되 면 두 번째 애니메이션을 트리거 했습니다.

[를 사용 하 여 UrhoSharp](~/graphics-games/urhosharp/using.md) 문서 Urho 및 게임 작성 하는 코드를 구성 하는 방법의 개념에 자세히 살펴봅니다.

## <a name="copyrights"></a>저작권

이 설명서 Xamarin Inc에서 원래 콘텐츠가 포함 되어 있고 Urho3D 프로젝트에 대 한 오픈 소스 문서에서 광범위 하 게 그립니다 Cocos2D 프로젝트에서 한 스크린샷을 제공 합니다.

### <a name="related-links"></a>관련 링크

- [지구 표면 통합 문서](https://developer.xamarin.com/workbooks/graphics/urhosharp/planetearth/planetearth.workbook)
- [좌표 통합 문서를 탐색합니다.](https://developer.xamarin.com/workbooks/graphics/urhosharp/coordinates/ExploringUrhoCoordinates.workbook)
