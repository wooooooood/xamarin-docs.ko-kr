---
title: UrhoSharp 소개
description: 이 문서에서는 UrhoSharp 응용 프로그램의 기본 구조에 대해 설명 하 고 UrhoSharp를 사용 하는 방법을 보여 주는 다양 한 가이드 및 샘플 응용 프로그램에 대 한 링크를 제공 합니다.
ms.prod: xamarin
ms.assetid: 18041443-5093-4AF7-8B20-03E00478EF35
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 441a3cc19b4246fb2bdea54508142a894af5c051
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "67832539"
---
# <a name="introduction-to-urhosharp"></a>UrhoSharp 소개

![UrhoSharp 로고](introduction-images/urhosharp-icon.png)

UrhoSharp는 Xamarin 및 .NET 개발자를 위한 강력한 3D 게임 엔진입니다.  이는 Apple의 SceneKit 및 SpriteKit의 경우와 유사 하며, 플랫폼 간 인 경우에도 물리학, 탐색, 네트워킹 등을 포함 합니다.

[Urho3D](http://urho3d.github.io/) 엔진에 대 한 .net 바인딩 이며 개발자는 동일한 코드 베이스를 사용 하 여 Android, IOS, Windows 및 Mac을 대상으로 하는 플랫폼 간 코드를 작성 하 고 OpenGL 및 Direct3D 시스템 모두에 렌더링할 수 있습니다.

UrhoSharp는 다양 한 기능을 사용 하는 게임 엔진입니다.

- 강력한 3D 그래픽 렌더링
- 물리 시뮬레이션 (글머리 기호 라이브러리 사용)
- 장면 처리
- Wait/Async 지원
- 친숙 한 작업 API
- 3D 장면에 2D 통합
- FreeType을 사용한 글꼴 렌더링
- 클라이언트 및 서버 네트워킹 기능
- 다양 한 자산 가져오기 (개방형 자산 라이브러리 사용)
- 탐색 메시 및 pathfinding (다시 캐스팅/우회 사용)
- 충돌 검색을 위한 볼록 선체 생성 (StanHull 사용)
- 오디오 재생 ( **libvorbis**사용)

## <a name="get-started"></a>시작

UrhoSharp는 [NuGet 패키지로](https://www.nuget.org/) 편리 하 게 배포 되며, Windows, Mac, Android C# 또는 F# iOS를 대상으로 하는 또는 프로젝트에 추가할 수 있습니다.  NuGet에는 프로그램을 실행 하는 데 필요한 라이브러리와 엔진에서 사용 하는 기본 자산 (CoreData)이 모두 함께 제공 됩니다.

### <a name="urho-as-a-portable-class-library"></a>이식 가능한 클래스 라이브러리로 urho

Urho 패키지는 플랫폼별 프로젝트 또는 이식 가능한 클래스 라이브러리 프로젝트에서 사용할 수 있으므로 모든 플랫폼에서 모든 코드를 다시 사용할 수 있습니다.  즉, 각 플랫폼에서 수행 해야 하는 작업은 플랫폼별 진입점을 작성 한 다음 제어권을 공유 게임 코드로 전송 하는 것입니다.

### <a name="samples"></a>샘플

Mac용 Visual Studio 또는 Visual Studio의 샘플 솔루션을 열어 Urho 기능에 대 한 다양 한 기능을 얻을 수 있습니다.

[https://github.com/xamarin/urho-samples](https://github.com/xamarin/urho-samples)

기본 솔루션에는 Android, iOS, Windows 및 Mac 용 프로젝트가 포함 되어 있습니다.  이 솔루션을 구조화 하 여 플랫폼 특정 시작 관리자를 사용 하 고 모든 샘플 코드와 게임 코드를 이식 가능한 클래스 라이브러리에 포함 하 여 모든 플랫폼에서 코드 재사용을 최대화 하는 방법을 보여 줍니다.

사용자 고유의 솔루션을 만드는 방법에 대 한 자세한 내용은 [Urho 및 플랫폼](~/graphics-games/urhosharp/platform/index.md) 페이지를 참조 하세요.

모든 샘플은 사용자 인터페이스 요소의 공통 집합을 공유 하므로 샘플은이 파일의 기본 설정을 추상화 했습니다.

[https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs](https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs)

여기에서는 몇 가지 기본 키 입력 및 터치 이벤트를 처리 하 고, 카메라를 설정 하 고, 기본 사용자 인터페이스 요소를 제공 하며, 각 샘플이 전시 되는 특정 기능에 집중할 수 있도록 하는 샘플 기본 클래스를 제공 합니다.

다음 샘플에서는 엔진에서 수행할 수 있는 작업을 보여 줍니다.

- ShootySkies의 간단한 [클론을 만듭니다](https://github.com/xamarin/urho-samples/tree/master/SamplyGame) .

다른 샘플에서는 각 샘플의 개별 속성을 보여 줍니다.

## <a name="basic-structure"></a>기본 구조

게임에서 `Application` 클래스의 서브 클래스를 만들어야 합니다. 여기에서 게임을 설정 (`Setup` 방법) 하 고 게임을 시작 합니다 (`Start` 메서드에서).  그런 다음 주 사용자 인터페이스를 생성 합니다.  3D 장면을 설정 하 고, 일부 UI 요소를 설정 하 고, 간단한 동작을 연결 하는 Api를 보여 주는 작은 샘플을 살펴보겠습니다.

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

이 응용 프로그램을 실행 하는 경우에는 해당 사용자의 자산에 대 한 불만이 런타임이 빠르게 검색 됩니다.  프로젝트에서 특수 디렉터리 이름 "Data"로 시작 하는 계층 구조를 만들고이 내에서 사용자의 프로그램에서 참조 하는 자산을 저장 하는 것이 좋습니다.  그런 다음 각 자산의 항목 속성에서 "출력 디렉터리로 복사"를 "새 항목으로 복사"로 설정 하 여 데이터가 있는지 확인 해야 합니다.

여기에서 진행 되는 사항을 설명 해 주세요.

응용 프로그램을 시작 하려면 다음과 같이 엔진 초기화 함수를 호출한 다음 응용 프로그램 클래스의 새 인스턴스를 만듭니다.

```csharp
new MySample().Run();
```

런타임에서 `Setup` 및 `Start` 메서드를 호출 합니다.  @No__t_0를 재정의 하는 경우 엔진 매개 변수를 구성할 수 있습니다 (이 샘플에서는 표시 되지 않음).

게임을 시작 하기 때문에 `Start`를 재정의 해야 합니다.  이 방법에서는 자산을 로드 하 고, 이벤트 처리기를 연결 하 고, 장면을 설정 하 고, 원하는 작업을 시작 합니다.  이 샘플에서는 사용자에 게 표시 되는 약간의 UI를 만들고 3D 장면을 설정 합니다.

다음 코드 조각에서는 UI 프레임 워크를 사용 하 여 텍스트 요소를 만들고 응용 프로그램에 추가 합니다.

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

UI 프레임 워크는 매우 간단한 게임 사용자 인터페이스를 제공 하며 `UI.Root` 노드에 새 노드를 추가 하는 방식으로 작동 합니다.

샘플의 두 번째 부분에서는 주 장면을 구성 합니다.  여기에는 3D 장면을 만들고, 화면에서 3D 상자를 만들고, 광원, 카메라 및 뷰포트를 추가 하는 여러 단계가 포함 됩니다.  이에 대해서는 [장면, 노드, 구성 요소 및 카메라](~/graphics-games/urhosharp/using.md#scenenodescomponentsandcameras)섹션에서 자세히 설명 합니다.

샘플의 세 번째 부분에서는 몇 가지 작업을 트리거합니다.  작업은 특정 효과를 설명 하는 요리법 이며 생성 된 후에는 `Node`에서 `RunActionAsync` 메서드를 호출 하 여 요청 시 노드에 의해 실행 될 수 있습니다.

첫 번째 작업은 바운스 효과를 사용 하 여 상자의 크기를 조정 하 고, 두 번째 작업은 상자를 계속 회전 합니다.

```csharp
await boxNode.RunActionsAsync(
    new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1)));
```

위의 예에서는 생성 되는 첫 번째 작업이 `ScaleTo` 작업 인지를 보여 줍니다 .이는 단순히 노드의 scale 속성 값을 기준으로 두 번째로 크기를 조정 하고자 함을 나타내는 조리법입니다.  이 작업은 감속/가속 작업 (`EaseBounceOut` 동작) 주위에 래핑됩니다.  감속/가속 동작은 동작의 선형 실행을 왜곡 하 고 효과를 적용 합니다 .이 경우에는 바운스 효과를 제공 합니다.
따라서 조리법은 다음과 같이 작성할 수 있습니다.

```csharp
var recipe = new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1));
```

조리법을 만들었으면 조리법을 실행 합니다.

```csharp
await boxNode.RunActionsAsync (recipe)
```

Wait는이 작업이 완료 될 때이 줄 다음에 실행을 다시 시작 하는 것을 나타냅니다.  작업이 완료 되 면 두 번째 애니메이션을 트리거합니다.

[UrhoSharp 문서 사용](~/graphics-games/urhosharp/using.md) 문서에서는 ur호의 개념과 게임을 빌드하는 코드를 구성 하는 방법을 자세히 살펴봅니다.

## <a name="copyrights"></a>저작권

이 설명서는 Xamarin Inc.의 원본 콘텐츠를 포함 하지만 Urho3D 프로젝트에 대 한 오픈 소스 설명서를 광범위 하 게 그리며 Cocos2D 프로젝트의 스크린샷을 포함 합니다.
