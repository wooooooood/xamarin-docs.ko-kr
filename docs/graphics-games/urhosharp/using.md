---
title: UrhoSharp를 사용 하 여 3D 게임 빌드
description: 이 문서에서는 배경, 구성 요소, 셰이프, 카메라, 작업, 사용자 입력, 소리 등을 설명 하는 UrhoSharp의 개요를 제공 합니다.
ms.prod: xamarin
ms.assetid: D9BEAD83-1D9E-41C3-AD4B-3D87E13674A0
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: cb4e524977b53f1a17552298c509d43ccf460f08
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73011648"
---
# <a name="using-urhosharp-to-build-a-3d-game"></a>UrhoSharp를 사용 하 여 3D 게임 빌드

첫 번째 게임을 작성 하기 전에 장면을 설정 하는 방법, 리소스를 로드 하는 방법, 아트 워크를 포함 하는 방법 및 게임에 대 한 간단한 상호 작용을 만드는 방법 등의 기본 사항을 알아보기 합니다.

<a name="scenenodescomponentsandcameras"/>

## <a name="scenes-nodes-components-and-cameras"></a>장면, 노드, 구성 요소 및 카메라

장면 모델은 구성 요소 기반 장면 그래프로 설명할 수 있습니다. 장면은 전체 장면을 나타내는 루트 노드에서 시작 하는 장면 노드의 계층 구조로 구성 됩니다. 각 `Node`에는 3D 변환 (위치, 회전 및 소수 자릿수), 이름, ID 및 임의 개수의 구성 요소가 있습니다.  구성 요소는 노드를 수명으로 가져오고, 시각적 표시를 추가 (`StaticModel`) 하 고, 소리를 내보내 (`SoundSource`), 충돌 경계를 제공할 수 있습니다.

[Urho 편집기](#urhoeditor)를 사용 하 여 장면 및 설치 노드를 만들거나 C# 코드에서 작업을 수행할 수 있습니다.  이 문서에서는 코드를 사용 하 여 화면에 표시 되는 항목을 확인 하는 데 필요한 요소를 설명 하므로 이러한 설정을 살펴보겠습니다.

장면을 설정 하는 것 외에도 `Camera`를 설정 해야 합니다 .이는 사용자에 게 표시 되는 항목을 결정 하는 것입니다.

### <a name="setting-up-your-scene"></a>장면 설정

일반적으로 시작 메서드로이 폼을 만듭니다.

```csharp
var scene = new Scene ();
// Create the Octree component to the scene. This is required before
// adding any drawable components, or else nothing will show up. The
// default octree volume will be from -1000, -1000, -1000) to
//(1000, 1000, 1000) in world coordinates; it is also legal to place
// objects outside the volume but their visibility can then not be
// checked in a hierarchically optimizing manner
scene.CreateComponent<Octree> ();
// Create a child scene node (at world origin) and a StaticModel
// component into it. Set the StaticModel to show a simple plane mesh
// with a "stone" material. Note that naming the scene nodes is
// optional. Scale the scene node larger (100 x 100 world units)
var planeNode = scene.CreateChild("Plane");
planeNode.Scale = new Vector3 (100, 1, 100);
var planeObject = planeNode.CreateComponent<StaticModel> ();
planeObject.Model = ResourceCache.GetModel ("Models/Plane.mdl");
planeObject.SetMaterial(ResourceCache.GetMaterial("Materials/StoneTiled.xml"));
```

### <a name="components"></a>구성 요소

`CreateComponent<T>()`를 호출 하 여 노드에 다른 구성 요소를 만들어 3D 개체 렌더링, 소리 재생, 물리 및 스크립팅된 논리 업데이트를 모두 사용할 수 있습니다.  예를 들어 다음과 같이 노드 및 라이트 구성 요소를 설정 합니다.

```csharp
// Create a directional light to the world so that we can see something. The
// light scene node's orientation controls the light direction; we will use
// the SetDirection() function which calculates the orientation from a forward
// direction vector.
// The light will use default settings (white light, no shadows)
var lightNode = scene.CreateChild("DirectionalLight");
lightNode.SetDirection (new Vector3(0.6f, -1.0f, 0.8f));
```

"`DirectionalLight`" 라는 이름으로 노드를 만들고이에 대 한 방향을 설정 하지만 그 외에는 그렇지 않습니다.  이제 `CreateComponent`를 사용 하 여 `Light` 구성 요소를 연결 하는 방식으로 위의 노드를 발광 노드로 전환할 수 있습니다.

```csharp
var light = lightNode.CreateComponent<Light>();
```

`Scene` 자체에 생성 되는 구성 요소에는 특별 한 역할이 있습니다. 즉, 장면 차원의 기능을 구현 합니다. 이러한 구성 요소는 다른 모든 구성 요소 보다 먼저 만들어야 하며 다음을 포함 해야 합니다.

 `Octree`: 공간 분할 및 가속화 표시 쿼리를 구현 합니다. 이 3D 개체를 사용 하지 않으면 렌더링할 수 없습니다.
 `PhysicsWorld`: 물리학 시뮬레이션을 구현 합니다. `RigidBody` 또는 `CollisionShape`와 같은 물리학 구성 요소는이를 제외 하 고 제대로 작동할 수 없습니다.
 `DebugRenderer`: 디버그 기 하 도형 렌더링을 구현 합니다.

`Light`, `Camera` 또는 `StaticModel`와 같은 일반 구성 요소
은 (는) `Scene`에 직접 만들지 말고 자식 노드로 만들어야 합니다.

라이브러리에는 사용자에 게 표시 되는 요소 (모델), 소리, 고정 본문, 충돌 셰이프, 카메라, 광원, 파티클 생성기 등을 위해 노드에 연결할 수 있는 다양 한 구성 요소가 제공 됩니다.

### <a name="shapes"></a>도형

편의를 위해 다양 한 셰이프를 Urho. 셰이프 네임 스페이스에서 단순 노드로 사용할 수 있습니다.  여기에는 상자, 구, 원추, 실린더 및 평면이 포함 됩니다.

### <a name="camera-and-viewport"></a>카메라 및 뷰포트

빛과 마찬가지로 카메라는 구성 요소 이므로 다음과 같이 노드에 구성 요소를 연결 해야 합니다.

```csharp
var CameraNode = scene.CreateChild ("camera");
camera = CameraNode.CreateComponent<Camera>();
CameraNode.Position = new Vector3 (0, 5, 0);
```

이를 통해 카메라를 만들고 3D 세계에 카메라를 배치 했습니다. 다음 단계는 사용 하려는 카메라 임을 `Application`에 알리기 위한 것입니다 .이 작업은 다음 코드를 사용 하 여 수행 됩니다.

```csharp
Renderer.SetViewPort (0, new Viewport (Context, scene, camera, null))
```

이제 만든 결과를 볼 수 있습니다.

### <a name="identification-and-scene-hierarchy"></a>식별 및 장면 계층 구조

노드와 달리 구성 요소에는 이름이 없습니다. 동일한 노드 내의 구성 요소는 해당 유형 및 노드 구성 요소 목록의 인덱스 (예: 생성 순서 대로 채워짐)로만 식별 됩니다. 예를 들어 위의 `lightNode` 개체에서 `Light` 구성 요소를 검색할 수 있습니다.

```csharp
var myLight = lightNode.GetComponent<Light>();
```

사용할 수 있는 `IList<Component>` 반환 하는 `Components` 속성을 검색 하 여 모든 구성 요소 목록을 가져올 수도 있습니다.

생성 될 때 노드와 구성 요소는 모두 장면 전역 정수 Id를 가져옵니다. `GetNode(uint id)` 및 `GetComponent(uint id)`함수를 사용 하 여 장면에서 쿼리할 수 있습니다. 예를 들어 재귀 이름 기반 장면 노드 쿼리를 수행 하는 것 보다 훨씬 빠릅니다.

엔터티 또는 게임 개체의 기본 제공 개념은 없습니다. 프로그래머는 노드 계층 구조와 스크립팅된 논리를 저장할 노드를 결정 해야 합니다. 일반적으로 3D 세계의 자유 이동 개체는 루트 노드의 자식으로 생성 됩니다. `CreateChild`를 사용 하 여 이름을 사용 하거나 사용 하지 않고 노드를 만들 수 있습니다. 노드 이름의 고유성은 적용 되지 않습니다.

계층적 컴퍼지션이 있을 때마다 자식 노드를 만들기 위해 구성 요소에 고유한 3D 변환이 없기 때문에이 작업을 수행 하는 것이 좋습니다.

예를 들어 문자가 개체를 보유 하 고 있는 경우 개체에는 문자의 손 모양 (`Node`)의 부모로 지정 된 자체 노드가 있어야 합니다.  예외는 노드와 관련 하 여 개별적으로 offsetted 및 회전할 수 있는 물리학 `CollisionShape`입니다.

`Scene`자체 변환은 자식 노드의 세계 파생 변환을 계산할 때 최적화로 의도적으로 무시 됩니다. 따라서이를 변경 해도 아무런 효과가 없으며 (원본 위치, 회전 안 함, 크기 조정 안 함) 그대로 두어야 합니다.

`Scene` 노드는 자유롭게 재지정 수 있습니다. 이와 대조적으로 구성 요소는 항상 연결 된 노드에 속하며 노드 간에 이동할 수 없습니다. 노드와 구성 요소 모두 부모를 통하지 않고이를 수행 하는 `Remove` 함수를 제공 합니다. 노드가 제거 되 면 해당 함수를 호출한 후 해당 노드나 구성 요소에 대 한 작업은 안전 하지 않습니다.

장면에 속하지 않는 `Node`를 만들 수도 있습니다. 이는 카메라를 로드 하거나 저장할 수 있는 장면으로 이동 하는 경우에 유용 합니다 .이 경우 카메라는 실제 장면과 함께 저장 되지 않으며 장면을 로드 하면 제거 되지 않기 때문입니다. 그러나 연결 되지 않은 노드에 기 하 도형, 물리 또는 스크립트 구성 요소를 만든 후 나중에 장면으로 이동 하면 해당 구성 요소가 제대로 작동 하지 않을 수 있습니다.

### <a name="scene-updates"></a>장면 업데이트

업데이트를 사용할 수 있는 장면 (기본값)은 각 주 루프 반복에서 자동으로 업데이트 됩니다.  응용 프로그램의 `SceneUpdate` 이벤트 처리기가 호출 됩니다.

노드 및 구성 요소를 사용 하지 않도록 설정 하 여 장면 업데이트에서 제외할 수 있습니다 `Enabled`를 참조 하세요.  동작은 특정 구성 요소에 따라 달라 지지만 예를 들어 그릴 수 있는 구성 요소를 사용 하지 않도록 설정 하는 경우 소리 원본 구성 요소를 사용 하지 않도록 설정 하는 동안 표시 되지 않습니다. 노드를 사용 하지 않도록 설정 하면 해당 구성 요소가 모두 사용/사용 안 함 상태에 관계 없이 비활성화 된 것으로 처리 됩니다.

## <a name="adding-behavior-to-your-components"></a>구성 요소에 동작 추가

게임을 구성 하는 가장 좋은 방법은 게임에서 행위자 또는 요소를 캡슐화 하는 고유한 구성 요소를 만드는 것입니다.  이렇게 하면 기능을 표시 하는 데 사용 되는 자산에서 해당 동작으로 기능이 자체 포함 됩니다.

구성 요소에 동작을 추가 하는 가장 간단한 방법은 작업을 사용 하는 것입니다. 작업은 큐에 대기 하 C# 고 비동기 프로그래밍과 결합할 수 있는 지침입니다.  이렇게 하면 구성 요소에 대 한 동작을 매우 높은 수준으로 설정 하 고 발생 하는 상황을 보다 쉽게 이해할 수 있습니다.

또는 각 프레임에서 구성 요소 속성을 업데이트 하 여 구성 요소에서 발생 하는 상황을 정확 하 게 제어할 수 있습니다 (프레임 기반 동작 섹션에서 설명).

### <a name="actions"></a>작업

작업을 사용 하 여 노드 동작을 매우 쉽게 추가할 수 있습니다.  작업은 다양 한 노드 속성을 변경 하 고 일정 기간 동안 실행 하거나 지정 된 애니메이션 곡선을 사용 하 여 여러 번 반복할 수 있습니다.

예를 들어 장면의 "클라우드" 노드를 고려 하 여 다음과 같이 페이드를 수행할 수 있습니다.

```csharp
await cloud.RunActionsAsync (new FadeOut (duration: 3))
```

작업은 다른 개체를 구동 하기 위해 작업을 다시 사용할 수 있도록 하는 변경할 수 없는 개체입니다.

일반적인 방법은 역방향 작업을 수행 하는 작업을 만드는 것입니다.

```csharp
var gotoExit = new MoveTo (duration: 3, position: exitLocation);
var return = gotoExit.Reverse ();
```

다음 예에서는 3 초 동안 개체를 페이드 인 합니다.  한 작업을 다른 작업으로 실행할 수도 있습니다. 예를 들어 먼저 클라우드를 이동한 다음 숨길 수 있습니다.

```csharp
await cloud.RunActionsAsync (
    new MoveBy  (duration: 1.5f, position: new Vector3(0, 0, 15),
    new FadeOut (duration: 3));
```

두 작업을 동시에 수행 하려는 경우 병렬 작업을 사용 하 여 수행 하려는 모든 작업을 병렬로 제공할 수 있습니다.

```csharp
  await cloud.RunActionsAsync (
    new Parallel (
      new MoveBy  (duration: 3, position: new Vector3(0, 0, 15),
      new FadeOut (duration: 3)));
```

위의 예제에서 클라우드는 동시에 이동 하 고 페이드 아웃 됩니다.

`await`사용 하 C# 고 있는 것을 확인할 수 있습니다 .이를 통해 수행할 동작에 대해 선형으로 생각할 수 있습니다.

### <a name="basic-actions"></a>기본 작업

UrhoSharp에서 지원 되는 작업은 다음과 같습니다.

- 노드 이동: `MoveTo`, `MoveBy`, `Place`, `BezierTo`, `BezierBy`, `JumpTo`, `JumpBy`
- 노드 회전: `RotateTo`, `RotateBy`
- 노드 크기 조정: `ScaleTo`, `ScaleBy`
- 페이딩 노드: `FadeIn`, `FadeTo`, `FadeOut`, `Hide`, `Blink`
- 색조: `TintTo`, `TintBy`
- `Hide`, `Show`, `Place`, `RemoveSelf`, `ToggleVisibility`
- 루핑: `Repeat`, `RepeatForever`, `ReverseTime`

기타 고급 기능에는 `Spawn` 및 `Sequence` 작업의 조합이 포함 됩니다.

### <a name="easing---controlling-the-speed-of-your-actions"></a>감속/가속 작업 속도 제어

감속/가속은 애니메이션이 펼침 되는 방식을 지정 하는 방식으로, 애니메이션을 훨씬 더 편리 하 게 만들 수 있습니다.  기본적으로 작업은 선형 동작을 수행 합니다. 예를 들어 `MoveTo` 작업은 매우 많은 로봇 이동이 있습니다.  동작을 감속/가속 작업으로 래핑하여 이동을 천천히 시작 하 고, 속도를 높이고, 종료 (`EasyInOut`) 하는 동작을 변경할 수 있습니다.

이렇게 하려면 기존 작업을 감속/가속 작업에 래핑합니다. 예를 들면 다음과 같습니다.

```csharp
await cloud.RunActionAsync (
   new EaseInOut (
     new MoveTo (duration: 3, position: new Vector (0,0,15)), rate:1))
```

많은 감속/가속 모드를 사용할 수 있으며, 다음 차트에서는 일정 기간 동안 제어 하는 개체의 값에 대 한 다양 한 감속/가속 유형 및 동작을 시작부터 끝까지 보여 줍니다.

![감속/가속 모드](using-images/easing.png "이 차트에서는 일정 기간 동안 제어 하는 개체의 값에 대 한 다양 한 감속/가속 유형 및 동작을 보여 줍니다.")

### <a name="using-actions-and-async-code"></a>작업 및 비동기 코드 사용

`Component` 서브 클래스에서 구성 요소 동작을 준비 하 고 해당 기능을 구동 하는 비동기 메서드를 도입할 수 있습니다.
그런 다음 프로그램의 다른 부분에서`await`C# 키워드를 사용 하 여이 메서드를 호출 합니다 .이 메서드는 `Application.Start`메서드 또는 응용 프로그램의 사용자 또는 스토리 포인트에 대 한 응답으로 실행 됩니다.

예를 들면,

```csharp
class Robot : Component {
    public bool IsAlive;
    async void Launch ()
    {
        // Dress up our robot
        var cache = Application.ResourceCache;
        var model = node.CreateComponent<StaticModel>();
        model.Model = cache.GetModel ("robot.mdl"));
        model.SetMaterial (cache.GetMaterial ("robot.xml"));
        Node.SetScale (1);

        // Bring the robot into our scene
        await Node.RunActionsAsync(
            new MoveBy(duration: 0.6f, position: new Vector3(0, -2, 0)));
        // Make him move around to avoid the user fire
        MoveRandomly(minX: 1, maxX: 2, minY: -3, maxY: 3, duration: 1.5f);
        // And simultaneously have him shoot at the user
        StartShooting();
    }

    protected async void MoveRandomly (float minX, float maxX,
                                       float minY, float maxY,
                       float duration)
    {
        while (IsAlive){
            var moveAction = new MoveBy(duration,
                new Vector3(RandomHelper.NextRandom(minX, maxX),
                            RandomHelper.NextRandom(minY, maxY), 0));
            await Node.RunActionsAsync(moveAction, moveAction.Reverse());
        }
    }
    protected async void StartShooting()
    {
        while (IsAlive && Node.Components.Count > 0){
            foreach (var weapon in Node.Components.OfType<Weapon>()){
                await weapon.FireAsync(false);
                if (!IsAlive)
                    return;
            }
            await Node.RunActionsAsync(new DelayTime(0.1f));
        }
    }
}
```

`Launch` 메서드에서는 세 가지 작업이 시작 됩니다. 즉, 로봇이 장면에 제공 되며,이 작업은 0.6 초 동안 노드의 위치를 변경 합니다.  비동기 옵션 이므로이는 `MoveRandomly`에 대 한 호출인 다음 명령으로 동시에 발생 합니다.  이 메서드는 로봇의 위치를 임의 위치로 병렬로 변경 합니다.  이렇게 하려면 두 개의 복합 작업, 새 위치로 이동, 원래 위치로 다시 이동 하 여 로봇이 활성 상태로 유지 되는 동안이 작업을 반복 합니다.  그리고 더 흥미로운 작업을 수행 하기 위해 로봇이 동시에 계속 유지 됩니다.  해결은 0.1 초 마다 시작 됩니다.

### <a name="frame-based-behavior-programming"></a>프레임 기반 동작 프로그래밍

작업을 사용 하는 대신 프레임별 방식으로 구성 요소의 동작을 제어 하려면 수행 하는 작업은 `Component` 하위 클래스의 `OnUpdate` 메서드를 재정의 하는 것입니다.  이 메서드는 프레임당 한 번 호출 되며 ReceiveSceneUpdates 속성을 true로 설정한 경우에만 호출 됩니다.

다음은 노드에 연결 되는 `Rotator` 구성 요소를 만드는 방법을 보여 줍니다 .이 구성 요소는 노드를 회전 시킵니다.

```csharp
class Rotator : Component {
    public Rotator()
    {
        ReceiveSceneUpdates = true;
    }
    public Vector3 RotationSpeed { get; set; }
    protected override void OnUpdate(float timeStep)
    {
        Node.Rotate(new Quaternion(
            RotationSpeed.X  timeStep,
            RotationSpeed.Y  timeStep,
            RotationSpeed.Z  timeStep),
            TransformSpace.Local);
    }
}
```

이 구성 요소를 노드에 연결 하는 방법은 다음과 같습니다.

```csharp
Node boxNode = new Node();
var rotator = new Rotator() { RotationSpeed = rotationSpeed };
boxNode.AddComponent (rotator);
```

### <a name="combining-styles"></a>스타일 조합

비동기/작업 기반 모델을 사용 하면 프로그래밍의 실행 스타일에 적합 한 대부분의 동작을 프로그래밍할 수 있지만 구성 요소 동작을 미세 조정 하 여 각 프레임에서 일부 업데이트 코드를 실행할 수도 있습니다.

예를 들어 SamplyGame 데모에서이는 기본 동작에서 작업을 사용 하는 `Enemy` 클래스에서 사용 되지만 구성 요소가 `Node.LookAt`를 사용 하 여 노드의 방향을 설정 하 여 사용자를 가리키도록 합니다.

```csharp
    protected override void OnUpdate(SceneUpdateEventArgs args)
    {
        Node.LookAt(
            new Vector3(0, -3, 0),
            new Vector3(0, 1, -1),
            TransformSpace.World);
        base.OnUpdate(args);
    }
```

## <a name="loading-and-saving-scenes"></a>장면 로드 및 저장

XML 형식으로 장면을 로드 하 고 저장할 수 있습니다. `LoadXml` 및 `SaveXML`함수를 참조 하세요. 장면을 로드 하면 모든 기존 콘텐츠 (자식 노드 및 구성 요소)가 먼저 제거 됩니다. `Temporary` 속성을 사용 하 여 임시로 표시 된 노드 및 구성 요소는 저장 되지 않습니다. Serializer는 모든 기본 제공 구성 요소 및 속성을 처리 하지만 구성 요소 서브 클래스에 정의 된 사용자 지정 속성 및 필드를 처리할 수 있을 만큼 지능적이 지 않습니다. 그러나이를 위한 두 가지 가상 메서드를 제공 합니다.

 serialization에 대 한 사용자 지정 상태를 등록할 수 있는 `OnSerialize`

 저장 된 사용자 지정 상태를 가져올 수 있는 `OnDeserialized` 합니다.

일반적으로 사용자 지정 구성 요소는 다음과 같습니다.

```csharp
class MyComponent : Component {
    // Constructor needed for deserialization
    public MyComponent(IntPtr handle) : base(handle) { }
    public MyComponent() { }
    // user defined properties (managed state):
    public Quaternion MyRotation { get; set; }
    public string MyName { get; set; }

    public override void OnSerialize(IComponentSerializer ser)
    {
        // register our properties with their names as keys using nameof()
        ser.Serialize(nameof(MyRotation), MyRotation);
        ser.Serialize(nameof(MyName), MyName);
    }

    public override void OnDeserialize(IComponentDeserializer des)
    {
        MyRotation = des.Deserialize<Quaternion>(nameof(MyRotation));
        MyName = des.Deserialize<string>(nameof(MyName));
    }
    // called when the component is attached to some node
    public override void OnAttachedToNode()
    {
        var node = this.Node;
    }
}
```

### <a name="object-prefabs"></a>개체 prefabs

새 개체를 동적으로 생성 해야 하는 게임에는 전체 장면을 로드 하거나 저장 하는 것 만으로는 유연 하지 않습니다. 반면에 복잡 한 개체를 만들고 코드에서 해당 속성을 설정 하는 것도 지루한 일입니다. 따라서 자식 노드, 구성 요소 및 특성을 포함 하는 장면 노드도 저장할 수 있습니다. 나중에 그룹으로 편리 하 게 로드할 수 있습니다.  이러한 저장 된 개체를 종종 prefab 라고도 합니다. 이 작업을 수행 하는 방법에는 다음 세 가지가 있습니다.

- 코드의 경우 노드에서 `Node.SaveXml`를 호출 합니다.
- 편집기의 계층 창에서 노드를 선택 하 고 "파일" 메뉴에서 "다른 이름으로 노드 저장"을 선택 합니다.
- `AssetImporter`에서 "node" 명령을 사용 하 여 장면 노드 계층 구조와 입력 자산에 포함 된 모든 모델을 저장 합니다 (예: Collada 파일)

저장 된 노드를 장면으로 인스턴스화하려면 `InstantiateXml`를 호출 합니다. 노드가 장면의 자식으로 만들어지지만, 그 후에는 자유롭게 재지정 수 있습니다. 노드를 배치 하는 위치 및 회전을 지정 해야 합니다. 다음 코드에서는 원하는 위치와 회전을 사용 하 여 장면에 prefab `Ninja.xm`를 인스턴스화하는 방법을 보여 줍니다.

```csharp
var prefabPath = Path.Combine (FileSystem.ProgramDir,"Data/Objects/Ninja.xml");
using (var file = new File(Context, prefabPath, FileMode.Read))
{
    scene.InstantiateXml(file, desiredPos, desiredRotation,
        CreateMode.Replicated);
}
```

## <a name="events"></a>이벤트

UrhoObjects는 여러 이벤트를 발생 시키며 이러한 이벤트를 생성 C# 하는 다양 한 클래스에 이벤트로 표시 됩니다.  기반 이벤트 모델 외 C#에도 구독 토큰을 구독 하 고 유지 하는 데 사용할 수 있는`SubscribeToXXX`메서드를 사용할 수 있습니다. 구독 토큰은 나중에 구독 취소 하는 데 사용할 수 있습니다.  차이점은 이전에 많은 호출자가 구독할 수 있는 반면 두 번째 호출자는 하나만 허용 하지만 더 좋은 람다 스타일 방식을 사용할 수 있도록 허용 하는 반면, 구독을 쉽게 제거할 수 있다는 것입니다.  함께 사용할 수 없습니다.

이벤트를 구독할 때 적절 한 이벤트 인수를 사용 하 여 인수를 사용 하는 메서드를 제공 해야 합니다.

예를 들어 다음은 마우스 단추 다운 이벤트를 구독 하는 방법입니다.

```csharp
public void override Start ()
{
    UI.MouseButtonDown += HandleMouseButtonDown;
}

void HandleMouseButtonDown(MouseButtonDownEventArgs args)
{
    Console.WriteLine ("button pressed");
}
```

람다 스타일 사용:

```csharp
public void override Start ()
{
    UI.MouseButtonDown += args => {
        Console.WriteLine ("button pressed");
    };
}
```

이벤트에 대 한 알림 수신을 중지 하려는 경우가 있습니다. 이러한 경우에는 `SubscribeTo` 메서드에 대 한 호출의 반환 값을 저장 하 고이에 대 한 구독 취소 메서드를 호출 합니다.

```csharp
Subscription mouseSub;

public void override Start ()
{
    mouseSub = UI.SubscribeToMouseButtonDown (args => {
    Console.WriteLine ("button pressed");
      mouseSub.Unsubscribe ();
    };
}
```

이벤트 처리기에서 받은 매개 변수는 각 이벤트와 관련 되 고 이벤트 페이로드를 포함 하는 강력한 형식의 이벤트 인수 클래스입니다.

## <a name="responding-to-user-input"></a>사용자 입력에 응답

이벤트를 구독 하 고 전달 되는 입력에 응답 하 여 키 입력과 같은 다양 한 이벤트를 구독할 수 있습니다.

```csharp
Start ()
{
    UI.KeyDown += HandleKeyDown;
}

void HandleKeyDown (KeyDownEventArgs arg)
{
     switch (arg.Key){
     case Key.Esc: Engine.Exit (); return;
}
```

하지만 대부분의 시나리오에서 장면 업데이트 처리기가 키가 업데이트 될 때 키의 현재 상태를 확인 하 고 그에 따라 코드를 업데이트 하려고 합니다.  예를 들어 다음을 사용 하 여 키보드 입력을 기준으로 카메라 위치를 업데이트할 수 있습니다.

```csharp
protected override void OnUpdate(float timeStep)
{
    Input input = Input;
    // Movement speed as world units per second
    const float moveSpeed = 4.0f;
    // Read WASD keys and move the camera scene node to the
    // corresponding direction if they are pressed
    if (input.GetKeyDown(Key.W))
        CameraNode.Translate(Vector3.UnitY  moveSpeed  timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.S))
        CameraNode.Translate(new Vector3(0.0f, -1.0f, 0.0f)  moveSpeed  timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.A))
        CameraNode.Translate(new Vector3(-1.0f, 0.0f, 0.0f)  moveSpeed  timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.D))
        CameraNode.Translate(Vector3.UnitX  moveSpeed  timeStep, TransformSpace.Local);
}
```

## <a name="resources-assets"></a>리소스 (자산)

리소스에는 초기화 또는 런타임 중 대량 저장소에서 로드 되는 UrhoSharp의 대부분 항목이 포함 됩니다.

- `Animation`-골격 애니메이션에 사용 됩니다.
- `Image`-다양 한 그래픽 형식으로 저장 된 이미지를 나타냅니다.
- `Model` 3D 모델
- 모델 렌더링에 사용 되는 `Material` 자료입니다.
- `ParticleEffect`- 파티클 내보내기의 작동 방식에 [대해 설명](https://urho3d.github.io/documentation/1.4/_particles.html) 합니다. 아래의 "[파티클](#particles)"을 참조 하세요.
- `Shader`-사용자 지정 셰이더
- 재생 `Sound` 소리는 아래의 "[소리](#sound)"를 참조 하십시오.
- `Technique` 재질 렌더링 기술
- `Texture2D`-2D 질감
- `Texture3D`-3D 질감
- `TextureCube`-큐브 질감
- `XmlFile`

`ResourceCache` 하위 시스템에 의해 관리 및 로드 됩니다 (`Application.ResourceCache`로 제공 됨).

리소스 자체는 등록 된 리소스 디렉터리 또는 패키지 파일을 기준으로 파일 경로로 식별 됩니다. 기본적으로 엔진은 리소스 디렉터리 `Data` 및 `CoreData`를 등록 하거나, 패키지 `Data.pak` 하 고 있는 경우 `CoreData.pak` 합니다.

리소스를 로드 하지 못하면 오류가 기록 되 고 null 참조가 반환 됩니다.

다음 예제에서는 리소스 캐시에서 리소스를 인출 하는 일반적인 방법을 보여 줍니다.  이 경우 UI 요소에 대 한 질감이 `Application` 클래스의 `ResourceCache` 속성을 사용 합니다.

```csharp
healthBar.SetTexture(ResourceCache.GetTexture2D("Textures/HealthBarBorder.png"));
```

리소스를 수동으로 만들고 디스크에서 로드 된 것 처럼 리소스 캐시에 저장할 수도 있습니다.

리소스 유형별 메모리 예산을 설정할 수 있음: 리소스가 허용 된 것 보다 많은 메모리를 사용 하는 경우 더 이상 사용 하지 않는 경우 캐시에서 가장 오래 된 리소스가 제거 됩니다. 기본적으로 메모리 예산은 무제한으로 설정 됩니다.

### <a name="bringing-3d-models-and-images"></a>3D 모델 및 이미지 가져오기

Urho3D은 가능 하면 기존 파일 형식을 사용 하 고 모델 (mdl) 및 애니메이션 (ani)의 경우와 같이 반드시 필요한 경우에만 사용자 지정 파일 형식을 정의 합니다. 이러한 유형의 자산에 대해 Urho는 fbx, dae, 3ds, obj 등과 같은 인기 있는 여러 3D 형식을 사용할 수 있는 [AssetImporter](https://urho3d.github.io/documentation/1.4/_tools.html) 를 제공 합니다.

Urho3D에 적합 한 형식으로 Blender 자산을 내보낼 수 있는 Blender [https://github.com/reattiva/Urho3D-Blender](https://github.com/reattiva/Urho3D-Blender) 에 대 한 편리한 추가 기능도 있습니다.

### <a name="background-loading-of-resources"></a>리소스의 백그라운드 로드

일반적으로 `ResourceCache`의 `Get` 메서드 중 하나를 사용 하 여 리소스를 요청할 때 주 스레드에 즉시 로드 되며, 필요한 모든 단계 (디스크에서 파일 로드, 데이터 구문 분석, 필요한 경우 GPU로 업로드)에 대해 몇 밀리초 정도 걸릴 수 있습니다. 따라서 프레임 속도가 감소 합니다.

필요한 리소스를 미리 알고 있는 경우 `BackgroundLoadResource`를 호출 하 여 백그라운드 스레드에서 로드 되도록 요청할 수 있습니다. `SubscribeToResourceBackgroundLoaded` 메서드를 사용 하 여 리소스 백그라운드 로드 이벤트를 구독할 수 있습니다. 실제로 로드가 성공 또는 실패 인지를 알려 줍니다. 리소스에 따라 로드 프로세스의 일부만 백그라운드 스레드로 이동할 수 있습니다. 예를 들어, GPU 업로드 단계 완료는 항상 주 스레드에서 수행 해야 합니다. 백그라운드 로드를 위해 큐에 대기 중인 리소스에 대 한 리소스 로드 메서드 중 하나를 호출 하는 경우 주 스레드는 로드가 완료 될 때까지 정지 됩니다.

비동기 장면 로딩 기능 `LoadAsync` 및 `LoadAsyncXML`에는 장면 콘텐츠를 로드 하기 전에 먼저 리소스를 백그라운드에서 로드 하는 옵션이 있습니다. 또한 `LoadMode.ResourcesOnly`를 지정 하 여 장면을 수정 하지 않고 리소스를 로드 하는 데만 사용할 수 있습니다. 이렇게 하면 빠른 인스턴스화에 대 한 장면 또는 개체 prefab 파일을 준비할 수 있습니다.

마지막으로, `ResourceCache`에서 `FinishBackgroundResourcesMs` 속성을 설정 하 여 백그라운드에서 로드 한 리소스를 완료 하는 데 각 프레임에 소요 된 최대 시간 (밀리초)을 구성할 수 있습니다.

<a name="sound"/>

## <a name="sound"></a>사운드

사운드는 게임 플레이의 중요 한 부분이 며, UrhoSharp 프레임 워크는 게임에서 소리를 재생 하는 방법을 제공 합니다.  `SoundSource` 연결 하 여 소리를 재생 합니다.
구성 요소를 `Node` 한 다음 리소스에서 명명 된 파일을 재생 합니다.

이 작업이 수행 되는 방법은 다음과 같습니다.

```csharp
var explosionNode = Scene.CreateChild();
var sound = explosionNode.CreateComponent<SoundSource>();
soundSource.Play(Application.ResourceCache.GetSound("Sounds/ding.wav"));
soundSource.Gain = 0.5f;
soundSource.AutoRemove = true;
```

<a name="particles"/>

## <a name="particles"></a>파티클

파티클은 응용 프로그램에 간단 하 고 저렴 한 효과를 추가 하는 간단한 방법을 제공 합니다.  [http://onebyonedesign.com/flash/particleeditor/](http://onebyonedesign.com/flash/particleeditor/)와 같은 도구를 사용 하 여 PEX 형식으로 저장 된 입자를 사용할 수 있습니다.

파티클은 노드에 추가할 수 있는 구성 요소입니다.  노드의 `CreateComponent<ParticleEmitter2D>` 메서드를 호출 하 여 파티클을 만든 다음 효과 속성을 리소스 캐시에서 로드 되는 2D 효과로 설정 하 여 파티클을 구성 해야 합니다.

예를 들어 구성 요소에 대해이 메서드를 호출 하 여 적중 시 폭발으로 렌더링 되는 일부 파티클을 표시할 수 있습니다.

```csharp
public async void Explode (Component target)
{
    // show a small explosion when the missile reaches an aircraft.
    var cache = Application.ResourceCache;
    var explosionNode = Scene.CreateChild();
    explosionNode.Position = target.Node.WorldPosition;
    explosionNode.SetScale(1f);
    var particle = explosionNode.CreateComponent<ParticleEmitter2D>();
    particle.Effect = cache.GetParticleEffect2D("explosion.pex");
    var scaleAction = new ScaleTo(0.5f, 0f);
    await explosionNode.RunActionsAsync(
        scaleAction, new DelayTime(0.5f));
    explosionNode.Remove();
}
```

위의 코드는 현재 구성 요소에 연결 된 분해 노드를 만듭니다 .이 분해 노드 내에서 2D 파티클 송신기를 만들고 효과 속성을 설정 하 여 구성 합니다.  두 작업을 실행 합니다. 하나는 노드 크기를 더 작게 조정 하 고 다른 하나는 0.5 초 동안이 크기를 유지 합니다.  그런 다음 폭발을 제거 하 여 화면에서 파티클 효과를 제거 합니다.

위의 파티클은 구 질감을 사용할 때 다음과 같이 렌더링 됩니다.

![구 질감이 있는 파티클](using-images/image-1.png "위의 파티클은 구 질감을 사용할 때 다음과 같이 렌더링 됩니다.")

Blocky 텍스처를 사용 하는 경우 다음과 같이 표시 됩니다.

![상자 질감이 있는 파티클](using-images/image-2.png "Blocky 텍스처를 사용 하는 경우 표시 되는 모양")

## <a name="multi-threading-support"></a>다중 스레딩 지원

UrhoSharp은 단일 스레드 라이브러리입니다.  즉, 백그라운드 스레드에서 UrhoSharp의 메서드를 호출 해서는 안 됩니다. 그렇지 않으면 응용 프로그램 상태가 손상 되어 응용 프로그램의 작동이 중단 될 위험이 있습니다.

백그라운드에서 일부 코드를 실행 한 다음 주 UI에서 Urho 구성 요소를 업데이트 하려면 `Application.InvokeOnMain(Action)`를 사용 하면 됩니다.
메서드를 재정의합니다.  또한 wait 및 .NET 작업 C# api를 사용 하 여 코드가 적절 한 스레드에서 실행 되도록 할 수 있습니다.

## <a name="urhoeditor"></a>UrhoEditor

[Urho 웹 사이트](http://urho3d.github.io/)에서 플랫폼용 Urho 편집기를 다운로드 하 고 다운로드로 이동 하 여 최신 버전을 선택할 수 있습니다.

## <a name="copyrights"></a>저작권

이 설명서는 Xamarin Inc.의 원본 콘텐츠를 포함 하지만 Urho3D 프로젝트에 대 한 오픈 소스 설명서를 광범위 하 게 그리며 Cocos2D 프로젝트의 스크린샷을 포함 합니다.
