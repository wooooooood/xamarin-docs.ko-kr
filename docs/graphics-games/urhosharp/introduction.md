---
title: UrhoSharp 소개
description: 이 문서에서는 UrhoSharp 응용 프로그램 및 다양 한 가이드 및 UrhoSharp 사용 하는 방법을 보여 주는 샘플 응용 프로그램에 대 한 링크의 기본 구조를 설명 합니다.
ms.prod: xamarin
ms.assetid: 18041443-5093-4AF7-8B20-03E00478EF35
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 441a3cc19b4246fb2bdea54508142a894af5c051
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832539"
---
# <a name="introduction-to-urhosharp"></a>UrhoSharp 소개

![UrhoSharp 로고](introduction-images/urhosharp-icon.png)

UrhoSharp는 Xamarin 및.NET 개발자를 위한 강력한 3D 게임 엔진.  비슷하며, Apple의 SceneKit 및 SpriteKit 되 고 물리학 포함, 탐색, 네트워킹 및 대부분의 플랫폼 간 중인 동안 더 합니다.

.NET 바인딩하는 것은 [Urho3D](http://urho3d.github.io/) 엔진 및 개발자가 Android, iOS를 대상 지정할 수 있는 플랫폼 간 코드를 작성할 수 있도록 하 고 Windows 및 Mac과 동일한 코드 베이스 OpenGL 및 Direct3D 시스템에 렌더링할 수 있습니다.

UrhoSharp는 많은 기본 기능을 사용 하 여 게임 엔진:

- 강력한 3D 그래픽 렌더링
- 물리학 시뮬레이션 (글머리 기호 라이브러리 사용)
- 장면 처리
- Async/await 지원
- 친숙 한 작업 API
- 2D 통합 3D 장면
- FreeType 사용 하 여 글꼴 렌더링
- 클라이언트 및 서버 네트워킹 기능
- 다양 한 범위의 (사용 하 여 자산 라이브러리 열기) 자산 가져오기
- 탐색 메시 하 고 (다시 캐스팅 우회 사용) 하는 경로 찾기
- 충돌 감지 (StanHull 사용)에 대 한 볼록 집합 생성
- 오디오 재생 (사용 하 여 **libvorbis**)

## <a name="get-started"></a>시작

UrhoSharp로 편리 하 게 배포 되는 [NuGet 패키지](https://www.nuget.org/) 에 추가할 수 있습니다에 C# 또는 F# Windows, Mac, Android 또는 iOS를 대상으로 하는 프로젝트입니다.  NuGet으로 프로그램을 실행 하는 데 필요한 라이브러리를 엔진에 의해 사용 되는 기본 자산 (CoreData) 포함 되어 있습니다.

### <a name="urho-as-a-portable-class-library"></a>이식 가능한 클래스 라이브러리로 Urho

플랫폼 특정 프로젝트 또는 이식 가능한 클래스 라이브러리 프로젝트에서 모든 플랫폼에서 모든 코드를 재사용할 수 있습니다 Urho 패키지를 사용할 수 있습니다.  즉, 플랫폼 특정 진입점을 작성 한 다음 공유 게임 코드를 제어를 전송 하는 각 플랫폼에서 수행 해야 합니다.

### <a name="samples"></a>샘플

Mac 용 Visual Studio 또는 Visual Studio에서 샘플 솔루션을 열어 Urho의 기능에 대 한 사용법을 확인할 수를 가져올 수 있습니다.

[https://github.com/xamarin/urho-samples](https://github.com/xamarin/urho-samples)

기본 솔루션에 대 한 Android, iOS, 프로젝트에 포함 된 Windows 및 Mac.  에서는 된 구조적 해당 솔루션을 작은 플랫폼 특정 표시 아이콘을 있고 이식 가능한 클래스 라이브러리에 거주 하는 모든 게임 코드 및 샘플 코드를 모든 플랫폼에서 코드 재사용을 최대화 하는 방법을 보여 주는 합니다.

참조를 [Your 플랫폼과 Urho](~/graphics-games/urhosharp/platform/index.md) 사용자 고유의 솔루션을 만드는 방법에 대 한 자세한 내용은 페이지입니다.

샘플 모두 공통 집합이 사용자 인터페이스 요소를 공유 하므로 샘플이이 파일의 기본 설치를 추상화 합니다.

[https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs](https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs)

이 일부 기본 키 입력을 처리 하는 샘플 기본 클래스를 제공 하 고 터치 이벤트, 설치 된 카메라를 기본 사용자 인터페이스 요소 및이 각 샘플은 전시 하는 특정 기능에 초점을 맞출 수를 제공 합니다.

다음 샘플에서는 새로운 엔진은 수행할 수 있는 보여 줍니다.

- [게임 samply](https://github.com/xamarin/urho-samples/tree/master/SamplyGame) ShootySkies의 단순 복제 합니다.

다른 샘플 각 샘플의 개별 속성을 표시 합니다.

## <a name="basic-structure"></a>기본 구조

게임의 서브 클래스 여야 합니다 `Application` 클래스 게임에서는 설정입니다 (에 `Setup` 메서드) 게임을 시작 하 고 (에서 `Start` 메서드).  다음 기본 사용자 인터페이스를 작성 합니다.  3 차원 장면, 일부 UI 요소 및 간단한 동작을 연결할 설정 하는 Api를 보여 주는 작은 샘플을 안내 하겠습니다.

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

이 응용 프로그램을 실행 하는 경우 신속 하 게 발견 런타임이 불만족 자산 사항이 없습니다.  수행 해야 하는 새로운 특수 디렉터리 이름을 "Data"로 시작 하는 프로젝트의 계층 구조를 만들 이며 따라서 내 프로그램에서 참조 하는 자산을 삽입할 수 있습니다.  다음을 설정 해야 합니다 "출력 디렉터리로 복사" 내용만 복사 "를 각 자산에 대 한 항목 속성에서 이렇게 하면는 데이터가 그대로 유지 됩니다.

여기에서 일어나 설명 알려 주세요.

응용 프로그램을 시작 하려면 다음과 같은 응용 프로그램 클래스의 새 인스턴스를 만드는 뒤에 엔진이 초기화 함수를 호출할 수 있습니다.

```csharp
new MySample().Run();
```

런타임 호출을 `Setup` 및 `Start` 메서드.  재정의 하는 경우 `Setup` 엔진 매개 변수 (이 샘플에 표시 하지 않으려면)를 구성할 수 있습니다.

재정의 해야 `Start` 으로 게임 시작 됩니다.  이 메서드에서는 자산 로드, 이벤트 처리기를 연결, 장면 설정 고 원하는 모든 작업을 시작 합니다.  이 샘플에서는 두 필자 모두 약간의 3D 장면을 설정 뿐만 아니라 사용자 표시 하는 UI 만듭니다.

다음 부분 코드 UI 프레임 워크를 사용 하 여 텍스트 요소를 만들고 응용 프로그램에 추가 합니다.

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

UI 프레임 워크는 매우 간단한 게임 사용자 인터페이스를 제공 하 고 새 노드를 추가 하 여 작동 합니다 `UI.Root` 노드.

이 샘플 설정 중 두 번째 부분은 기본 장면입니다.  여기에 3D 장면의 3D 상자 화면에서 만들기 광원, 카메라 및 뷰포트 추가 만들기 단계 수가 포함 됩니다.  섹션에서 자세한 세부 정보에 대해서는 이러한 [장면, 노드, 구성 요소 및 카메라](~/graphics-games/urhosharp/using.md#scenenodescomponentsandcameras)합니다.

샘플의 세 번째 부분은 몇을 가지 작업을 트리거합니다.  작업을 호출 하 여 주문형 노드가 특정 효과 설명 하는 만든 레시피를 실행할 수 있습니다 합니다 `RunActionAsync` 메서드는 `Node`합니다.

첫 번째 작업 바운스 효과 사용 하 여 상자를 확장 하 고 두 번째 상자를 영구적으로 회전 합니다.

```csharp
await boxNode.RunActionsAsync(
    new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1)));
```

만든 첫 번째 작업은 어떻게 위의 표시는 `ScaleTo` 작업을 확장 하기 위해 1 값으로 두 번째 노드의 확장 속성을 원하는 지 나타냅니다는 레시피를 단순히이 됩니다.  이 작업에는 래핑하는 감속/가속 작업은 `EaseBounceOut` 작업 합니다.  액션의 선형 실행 왜곡 하 고 효과 적용 하는 감속/가속 작업, 반송 스케일 아웃 효과 제공 하는 경우.
따라서 비법 같이 작성할 수 있습니다.

```csharp
var recipe = new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1));
```

레시피를 만든 후 레시피를 실행 합니다.

```csharp
await boxNode.RunActionsAsync (recipe)
```

Await 나타냅니다는 작업이 완료 되 면이 줄 다음 실행을 다시 시작 하려고 합니다.  작업이 완료 되 면 두 번째 애니메이션을 트리거 했습니다.

합니다 [UrhoSharp 사용 하 여](~/graphics-games/urhosharp/using.md) 문서 Urho 및 게임을 제작 하 여 코드를 구성 하는 방법의 개념을 자세히 살펴봅니다.

## <a name="copyrights"></a>저작권

이 설명서 Xamarin Inc에서 원래 콘텐츠를 포함 하 고 있지만 Urho3D 프로젝트에 대 한 오픈 소스 문서에서 광범위 하 게 그립니다, Cocos2D 프로젝트 스크린 샷을 포함 합니다.
