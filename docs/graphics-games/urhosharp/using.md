---
title: 3D 게임을 제작 하 UrhoSharp 사용
description: 이 문서 장면, 구성, 셰이프, 카메라, 작업, 사용자 입력, 사운드 등을 설명 하는 UrhoSharp에 대 한 개요를 제공 합니다.
ms.prod: xamarin
ms.assetid: D9BEAD83-1D9E-41C3-AD4B-3D87E13674A0
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 7307f7dcfcc6b5e2576ce4b425879ae05c5a6056
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832667"
---
# <a name="using-urhosharp-to-build-a-3d-game"></a>3D 게임을 제작 하 UrhoSharp 사용

기본 사항 알아보기 하려는 생애 첫 게임을 작성 하기 전에: 장면을 설정 하는 방법, 리소스 (아트 워크 포함)를 로드 하는 방법 및 게임에 대 한 간단한 상호 작용 하는 방법입니다.

<a name="scenenodescomponentsandcameras"/>

## <a name="scenes-nodes-components-and-cameras"></a>장면, 노드, 구성 요소 및 카메라

구성 요소 기반 장면 그래프로 장면 모델을 설명할 수 있습니다. 장면 전체 장면에도 나타내는 루트 노드에서 시작 장면 노드 계층으로 구성 됩니다. 각 `Node` 개의 3D 변환 (위치, 회전 및 배율), 이름, ID와 임의 개수의 구성 요소입니다.  구성 요소에 활기를 노드, 시각적 표시를 추가 할 수 있습니다 (`StaticModel`), 소리를 내보낼 수 있습니다 (`SoundSource`), 충돌 경계에 제공할 수 있습니다.

설정 노드를 사용 하 고 자동으로 만들 수 있습니다 합니다 [Urho 편집기](#urhoeditor), 또는 C# 코드에서 작업을 수행할 수 있습니다.  이 문서의 살펴봅니다 코드를 사용 하 여 설정 작업을으로 진행 화면에 표시 되도록 하는 데 필요한 요소를 보여 줍니다.

장면에 설정 하는 것 외에도 설치 해야는 `Camera`,이 사용자에 게 표시 가져오기는 무엇을 결정 합니다.

### <a name="setting-up-your-scene"></a>장면 설정

일반적으로이 양식을 시작 방법을 만들 수 있습니다.

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

3D 개체 렌더링 소리 재생, 물리 및 스크립팅된 논리 업데이트는 모두 사용 하도록 설정 호출 하 여 노드를 다른 구성 요소를 만들어 `CreateComponent<T>()`합니다.  예를 들어, 다음과 같은 간단한 구성 요소 확인 하 고 노드 설치:

```csharp
// Create a directional light to the world so that we can see something. The
// light scene node's orientation controls the light direction; we will use
// the SetDirection() function which calculates the orientation from a forward
// direction vector.
// The light will use default settings (white light, no shadows)
var lightNode = scene.CreateChild("DirectionalLight");
lightNode.SetDirection (new Vector3(0.6f, -1.0f, 0.8f));
```

노드 이름 사용 하 여 위에서 만든 "`DirectionalLight`" 있지만 아무에 대 한 방향을 설정 합니다.  이제 설정 하 여 위의 노드 light 내보내기 노드로 연결 하 여는 `Light` 구성 요소를 사용 하 여 `CreateComponent`:

```csharp
var light = lightNode.CreateComponent<Light>();
```

구성 요소를 생성 합니다 `Scene` 자체는 특별 한 역할이: 장면 전체 기능을 구현 하 합니다. 다른 모든 구성 요소 전에 생성 해야 하며 다음과 같습니다.

 `Octree`: 가시성 쿼리를 가속화 및 공간 분할을 구현 합니다. 이 3D 하지 않고 개체를 렌더링할 수 있습니다.
 `PhysicsWorld`: 물리학 시뮬레이션을 구현 합니다. 구성 요소를 같은 물리학 `RigidBody` 또는 `CollisionShape` 이렇게 하지 않으면 제대로 작동 하지 않을 수 있습니다.
 `DebugRenderer`: 구현 디버그 기 하 도형 렌더링 합니다.

일반 구성 요소와 같은 `Light`, `Camera` 또는 `StaticModel`
직접 만들 수 없습니다는 `Scene`, 있지만 자식 노드를 대신 합니다.

라이브러리는 다양 한에 활기를 해당 노드에 연결할 수 있는 구성 요소가 함께: 사용자가 볼 수 요소 (모델), 소리, 엄격한 본문, 충돌 셰이프, 카메라, 광원, 파티클 송신기 및 등입니다.

### <a name="shapes"></a>셰이프

편의 다양 한 모양을 Urho.Shapes 네임 스페이스의 간단한 노드로 제공 됩니다.  여기에 상자, 구, 콘, 실린더 및 평면이 포함 됩니다.

### <a name="camera-and-viewport"></a>카메라 및 뷰포트

광원 마찬가지로 카메라 되므로 구성 요소 구성 요소는 아래와 같이 연결 해야 합니다.

```csharp
var CameraNode = scene.CreateChild ("camera");
camera = CameraNode.CreateComponent<Camera>();
CameraNode.Position = new Vector3 (0, 5, 0);
```

카메라,이 사용 하 여 만든 3D 세계에서 카메라를 배치 했으면를 알리기 위해 다음 단계는는 `Application` 이것이 사용 하려는 카메라, 이렇게 다음 코드를 사용 하 여:

```csharp
Renderer.SetViewPort (0, new Viewport (Context, scene, camera, null))
```

이제 프로그램 만들기의 결과 확인할 수 해야 하며

### <a name="identification-and-scene-hierarchy"></a>식별 및 장면 계층 구조

노드와 달리 구성 요소 이름을 갖지 않습니다. 동일한 노드 내에서 구성 요소는 해당 유형 및 인덱스 만들기 순서 대로 채워집니다 노드 구성 요소 목록에서로 식별 되, 예를 들어, 검색할 수 있습니다 합니다 `Light` 의 구성 요소는 `lightNode` 같이 위의 개체:

```csharp
var myLight = lightNode.GetComponent<Light>();
```

검색 하 여 모든 구성 요소 목록을 가져올 수도 있습니다는 `Components` 반환 하는 속성을 `IList<Component>` 사용할 수 있는 합니다.

를 만들면 노드 및 구성 요소를 모두 장면 전역 정수 Id를 가져옵니다. 함수를 사용 하 여 장면에서 쿼리할 수 있습니다 `GetNode(uint id)` 고 `GetComponent(uint id)`입니다. 예를 들어 재귀적 이름 기반 장면 노드 쿼리를 수행할 때 보다 훨씬 빠릅니다.

엔터티 또는 게임 개체;의 기본 개념은 대신 스크립팅된 논리를 배치 하는 노드 및 노드 계층 구조를 결정 하는 프로그래머의 몫 됩니다. 일반적으로 3D 세계에서 사용 가능한 이동 개체는 루트 노드의 자식으로 만들어졌습니다. 중 유무에 관계 없이 사용 하 여 이름 노드를 만들 수 있습니다 `CreateChild`합니다. 노드 이름의 고유성이 적용 되지 않습니다.

권장 되는 (및 구성 요소에 고유한 3D 변환 하지 않으므로 필요한 사실)는 일부 계층적 컴퍼지션 있을 때마다 자식 노드를 만듭니다.

예를 들어 개체 문자 직접 뼈 부모가 될 수 있는 자체 노드에 있어야 문자 자신의 손에서 개체를 보유 된, (또한을 `Node`).  예외는 물리 `CollisionShape`, offsetted 및 노드를 기준으로 개별적으로 회전할 수 있습니다.

`Scene`의 파생 된 세계 변환 자식 노드를 계산할 때 최적화로 변환 의도적으로 무시 됩니다, 따라서 변경 해도 효과가 없습니다 및 (원점에 없는 회전, 크기 조정 없음 위치입니다.)를 그대로 두어야 소유

`Scene` 노드 자유롭게 부모가 재지정 될 수 있습니다. 반면 구성 요소가 항상 속한 노드를 연결 하며 노드 간에 이동할 수 있습니다. 노드 및 구성 요소를 모두 제공을 `Remove` 부모를 통해 이동 하지 않고도이 작업을 수행 하는 함수입니다. 노드 제거 되 면 해당 함수를 호출한 후 작업이 없는 노드 또는 해당 구성 요소는 안전 합니다.

만들 수 이기도 한 `Node` 장면에 속하지 않습니다. 이 유용한 예를 들어 때문에 저장 하거나 로드할 수 있는 장면에서 이동 카메라를 사용 하 여 다음 카메라 실제 장면 함께 저장 되지 것입니다 하 고 장면 로드 될 때 소멸 되지 것입니다. 그러나는 기 하 도형, 물리학 또는 스크립트 구성 요소에서 연결 되지 않은 노드를 만들고 다음 나중에 장면으로 이동 하면 이러한 구성 요소를 제대로 작동 하지 note 합니다.

### <a name="scene-updates"></a>장면 업데이트

해당 업데이트를 사용 하는 장면 (기본값) 주 루프가 반복 될 때마다 자동으로 업데이트 됩니다.  응용 프로그램의 `SceneUpdate` 에 이벤트 처리기가 호출 됩니다.

노드 및 구성 요소 비활성화 하 여 장면 업데이트에서 제외할 수 있습니다, 참조 `Enabled`합니다.  동작은 특정 구성 요소에 따라 다르지만 예를 들어 그릴 수 있는 구성 요소를 사용 하지 않도록 설정 하기가, 보이지 않는 음을 소거 사운드 원본 구성 요소를 사용 하지 않도록 설정 하는 동안. 노드 비활성화 된 경우에 자체 활성화/비활성화 상태에 관계 없이 사용 안 함으로 처리 됩니다 모든 구성 요소.

## <a name="adding-behavior-to-your-components"></a>구성 요소에 동작 추가

게임을 구성 하는 가장 좋은 방법은 행위자 또는 게임 요소를 캡슐화 하는 사용자 고유의 구성 요소를 만드는 방법은입니다.  이 기능을 사용 하면 해당 동작을 표시 하는 데 사용 된 자산에서 포함 된 자체입니다.

동작 구성 요소를 추가 하는 가장 간단한 방법은 큐 수 하는 C#의 비동기 프로그래밍에 결합할 수 있는 지침이 되는 작업을 사용 하 여 것입니다.  이 매우 높은 수준 되도록 구성 요소에 대 한 동작을 통해 간편 하 게 발생 하는 상황을 이해.

또는 정확 하 게 되나요 구성 요소 (프레임 기반 동작 섹션에서 설명) 각 프레임에 구성 요소 속성을 업데이트 하 여 제어할 수 있습니다.

### <a name="actions"></a>동작

작업을 사용 하 여 쉽게 매우 노드로 동작을 추가할 수 있습니다.  작업 다양 한 노드 속성을 변경 하 고 시간 동안 실행 하거나 지정된 애니메이션 곡선을 사용 하 여 다시 시도 횟수를 반복 수 있습니다.

예를 들어 장면에서 "클라우드" 노드, 같이 페이드 수 있습니다.

```csharp
await cloud.RunActionsAsync (new FadeOut (duration: 3))
```

작업은 서로 다른 개체를 촉진에 대 한 작업을 다시 사용할 수 있는 변경할 수 없는 개체입니다.

일반적인 방법은 다음과 같습니다. 반대 작업을 수행 하는 동작을 만들려면

```csharp
var gotoExit = new MoveTo (duration: 3, position: exitLocation);
var return = gotoExit.Reverse ();
```

다음 예제에서는 3 초 동안의 개체를 페이드 됩니다.  하나의 작업을 실행할 수도 있습니다 다른 예를 들어, 있습니다 수 먼저 클라우드로 이동 후 다음 숨깁니다.

```csharp
await cloud.RunActionsAsync (
    new MoveBy  (duration: 1.5f, position: new Vector3(0, 0, 15),
    new FadeOut (duration: 3));
```

동시에 수행할 동작을 모두를 하려는 경우 병렬 작업을 사용 하 고 모든 작업을 병렬로 수행를 제공할 수 있습니다.

```csharp
  await cloud.RunActionsAsync (
    new Parallel (
      new MoveBy  (duration: 3, position: new Vector3(0, 0, 15),
      new FadeOut (duration: 3)));
```

위의 예에서 클라우드 이동를 동시에 페이드 아웃을 적용 합니다.

이 사용 하는 것을 알 수 있습니다 C# `await`를 수행 하려는 동작에 대 한 선형으로 생각할 수 있습니다.

### <a name="basic-actions"></a>기본 작업

다음은 UrhoSharp에서 지원 되는 작업입니다.

- 노드 이동: `MoveTo`, `MoveBy`, `Place`, `BezierTo`합니다 `BezierBy`, `JumpTo`, `JumpBy`
- 노드를 회전: `RotateTo`, `RotateBy`
- 노드 크기를 조정: `ScaleTo`, `ScaleBy`
- 노드 페이딩: `FadeIn`, `FadeTo`를 `FadeOut`, `Hide`, `Blink`
- 색조: `TintTo`, `TintBy`
- : 인스턴트 `Hide`, `Show`를 `Place`, `RemoveSelf`, `ToggleVisibility`
- : 반복 `Repeat`, `RepeatForever`, `ReverseTime`

기타 고급 기능 조합을 포함 합니다 `Spawn` 고 `Sequence` 작업 합니다.

### <a name="easing---controlling-the-speed-of-your-actions"></a>감속/가속-작업의 속도 제어 합니다.

감속/가속 애니메이션 펼치려면 됩니다 및 수 있도록 애니메이션 손쉽게 방식을 지시 하는 방법입니다.  기본적으로 작업 해야 선형 동작을 예를 들어를 `MoveTo` 작업 매우 로봇 이동 해야 합니다.  사용자의 작업은 느리게 시작 이동, 가속화 및 느린 끝에 도달 하는 예를 들어 동작을 변경 하려면 감속/가속 작업을 래핑할 수 있습니다 (`EasyInOut`).

예를 들어을 감속/가속 작업으로 기존 작업을 래핑하고이 수행 합니다.

```csharp
await cloud.RunActionAsync (
   new EaseInOut (
     new MoveTo (duration: 3, position: new Vector (0,0,15)), rate:1))
```

여러 감속/가속 모드를 다음 차트는 처음부터 끝까지 기간을 통해 제어 하는 개체의 값에 다양 한 감속/가속 형식 및 해당 동작을 보여줍니다.

![모드를 감속/가속](using-images/easing.png "이 차트는 기간을 통해 제어 하는 개체의 값에 다양 한 감속/가속 형식 및 해당 동작에 표시")

### <a name="using-actions-and-async-code"></a>작업 및 비동기 코드를 사용 하 여

사용자 `Component` 서브 클래스에서 구성 요소 동작 준비 및 기능에 대 한 드라이브는 비동기 메서드를 도입 해야 합니다.
C#을 사용 하 여이 메서드를 호출 하는 경우 다음 `await` 프로그램의 다른 부분에서 키워드 중 하나에 `Application.Start` 메서드 또는 응용 프로그램에서 사용자 또는 스토리 지점에 대 한 응답입니다.

예:

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

에 `Launch` 위의 세 가지 작업 메서드는 시작: 로봇 장면 야,이 작업은 0.6 초의 기간 동안 노드의 위치를 변경 합니다.  비동기 옵션 이므로이 현상이 동시에 호출 되는 다음 명령으로를 `MoveRandomly`입니다.  이 메서드는 임의 위치에 병렬로 로봇의 위치를 변경 합니다.  새 위치로 이동 두 개의 복합된 작업을 수행 하 여 이렇게 하 고 원래로 돌아가면 위치 로봇 유지 있다면이 단계를 반복 합니다.  및 좀 더 흥미로운, 로봇은 유지 해결 동시에 합니다.  해결 된 0.1 초 마다만 시작 됩니다.

### <a name="frame-based-behavior-programming"></a>프레임 기반 동작 프로그래밍

재정의 하는 것이 수행할 작업을 사용 하는 대신 프레임별으로 기준 구성 요소의 동작을 제어 하려는 경우는 `OnUpdate` 메서드의 여 `Component` 하위 클래스입니다.  이 메서드는가 프레임 마다 한 번 호출 하 고 ReceiveSceneUpdates 속성을 true로 설정 하는 경우에 호출 됩니다.

다음 만드는 방법을 보여 줍니다는 `Rotator` 노드를 회전 하는 노드로 연결 되는 구성 요소:

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

및이 구성 요소 노드를 연결 하는 방법입니다.

```csharp
Node boxNode = new Node();
var rotator = new Rotator() { RotationSpeed = rotationSpeed };
boxNode.AddComponent (rotator);
```

### <a name="combining-styles"></a>스타일을 결합합니다.

대부분의 프로그래밍 스타일 실행 후 제거에 매우 적합 동작이 프로그래밍에 대 한 비동기/작업 기반 모델을 사용할 수 있지만 각 프레임에서 일부 업데이트 코드를 실행 하 여 구성 요소의 동작을도 미세 조정할 수 있습니다.

예를 들어 SamplyGame 데모에서이 합니다 `Enemy` 는 구성 요소를 가리키는 사용자 방향을 사용 하 여 노드를 설정 하 여 유지도 이지만 클래스는 기본 동작 사용 하 여 작업을 인코딩하고 `Node.LookAt`:

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

## <a name="loading-and-saving-scenes"></a>로드 하 고 자동으로 저장

백그라운드에서 로드 되 고 XML 형식으로 저장 함수를 살펴봅니다 `LoadXml` 고 `SaveXML`입니다. 장면 로드 되 면 모든 기존 콘텐츠 (자식 노드 및 구성 요소)를 먼저 제거 됩니다. 노드 및 사용 하 여 임시 표시 되는 구성 요소는 `Temporary` 속성 저장 되지 것입니다. 모든 기본 제공 구성 요소 및 속성을 처리 하는 serializer가 있지만 구성 요소 하위 클래스에 정의 된 필드 및 사용자 지정 속성을 처리 하지 못합니다 아닙니다. 그러나이 두 개의 가상 메서드를 제공합니다.

 `OnSerialize` 등록할 수 있습니다 serialization에 대 한 사용자 지정 상태

 `OnDeserialized` 위치에 저장 된 사용자 지정 상태를 가져올 수 있습니다.

일반적으로 사용자 지정 구성 요소는 다음과 같이 표시 됩니다.

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

### <a name="object-prefabs"></a>개체를 prefabs

방금 로드 또는 전체 장면 저장 유연성은 없습니다 게임에 대 한 새 개체를 동적으로 작성 해야 하는 위치입니다. 반면에 복잡 한 개체를 만들고 코드에서 해당 속성을 설정 수도 지루한 작업입니다. 이러한 이유로 구성 요소와 특성, 자식 노드를 포함 하는 장면 노드를 저장할 수 이기도 합니다. 이러한 그룹으로 나중에 편리 하 게 로드할 수 있습니다.  이러한 저장 된 개체를 prefab은 라고도 합니다. 이렇게 하는 데는 다음과 같은 세 가지 방법이 있습니다.

- 호출 하 여 코드에서 `Node.SaveXml` 노드
- 편집기에서 계층 구조 창의 노드를 선택 하 여 "저장" 노드 "파일" 메뉴에서.
- "노드" 명령을 사용 하 여 `AssetImporter`장면 노드 계층 저장 하는, 및 모든 모델 (예: 입력된 자산에 포함 된 Collada 파일)

저장 된 노드는 장면을 인스턴스화할 때 호출 `InstantiateXml`합니다. 노드에 장면의 자식으로 만들어지지만 그 후 자유롭게 부모를 재지정할 수 있습니다. 위치 및 회전 노드를 배치 하기 위해 지정 해야 합니다. 다음 코드를 prefab을 인스턴스화하는 방법을 보여 줍니다 `Ninja.xm` 원하는 위치 및 회전을 사용 하 여 장면에:

```csharp
var prefabPath = Path.Combine (FileSystem.ProgramDir,"Data/Objects/Ninja.xml");
using (var file = new File(Context, prefabPath, FileMode.Read))
{
    scene.InstantiateXml(file, desiredPos, desiredRotation,
        CreateMode.Replicated);
}
```

## <a name="events"></a>이벤트

UrhoObjects 이벤트 수가 발생, 이러한 생성 하는 다양 한 클래스에서 C# 이벤트로 표시 됩니다.  C# 뿐만 아니라-이벤트 기반된 모델, 것도 사용할 수는 `SubscribeToXXX` 구독 하는 구독 토큰을 유지 하는 사용할 수 있는 메서드 나중 하 여 구독을 취소 합니다.  이전을 사용 하면 많은 호출자에 구독할, 멋진 람다 스타일을 사용할 수 있으며 아직 구독 쉽게 제거할 수 있도록 하나를 허용 하지만 허용만 두 번째는 다릅니다.  함께 사용할 수 없습니다.

이벤트를 구독할 때 적절 한 이벤트 인수를 사용 하 여 인수를 사용 하는 메서드를 제공 해야 합니다.

예를 들어 다음과 같습니다. 마우스 버튼 누름 이벤트를 구독 하는 방법

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

람다 스타일:

```csharp
public void override Start ()
{
    UI.MouseButtonDown += args => {
        Console.WriteLine ("button pressed");
    };
}
```

이벤트를 이러한 경우 반환 값에서에서 저장에 대 한 호출에 대 한 알림 수신을 중지할 수도 있습니다 `SubscribeTo` 메서드를 구독 취소 메서드를 호출 합니다.

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

이벤트 처리기에서 받은 각 이벤트에 관련 되 고 이벤트 페이로드를 포함 하는 강력한 형식의 이벤트 인수 클래스입니다.

## <a name="responding-to-user-input"></a>사용자 입력에 응답

이벤트 구독 및 배달 되 고 입력에 응답 하 여 아래로 키와 같은 다양 한 이벤트를 구독할 수 있습니다.

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

하지만 대부분의 시나리오에서 장면 업데이트 처리기 업데이트 되 고 그에 따라 코드를 업데이트할 때 키의 현재 상태를 확인 합니다.  예를 들어, 다음 사용할 수는 키보드 입력에 따라 카메라 위치를 업데이트 하려면:

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

리소스에서 UrhoSharp 초기화 또는 런타임 중에 대용량 저장소에서 로드 되는 대부분의 항목이 포함 됩니다.

- `Animation` -기본 애니메이션에 대 한 사용
- `Image` -다양 한 그래픽 형식에에서 저장 된 이미지를 나타냅니다.
- `Model` -3D 모델
- `Material` -모델을 렌더링 하는 데 사용 합니다.
- `ParticleEffect`- [설명](http://urho3d.github.io/documentation/1.4/_particles.html) 파티클 내보내기 작동 방식 참조 "[입자](#particles)" 아래.
- `Shader` -사용자 지정 셰이더
- `Sound` -소리 재생을를 참조 하세요. "[소리](#sound)" 아래.
- `Technique` -재질 렌더링 기술을
- `Texture2D` -2D 질감
- `Texture3D` -3D 질감
- `TextureCube` -정육면체 텍스처
- `XmlFile`

관리 되 고 로드 합니다 `ResourceCache` 하위 시스템 (으로 사용할 수 있는 `Application.ResourceCache`).

리소스 자체는 등록 된 리소스 디렉터리 또는 패키지 파일을 기준으로 해당 파일 경로 의해 식별 됩니다. 엔진은 기본적으로 리소스 디렉터리를 등록 `Data` 하 고 `CoreData`, 또는 패키지 `Data.pak` 및 `CoreData.pak` 있을 경우.

실패 하면 리소스를 로드 하는 경우 오류로 기록 되 고 null 참조가 반환 됩니다.

다음 예제에서는 리소스 캐시에서 리소스를 가져오는 일반적인 방법을 보여 줍니다.  이 경우 UI 요소에 질감을 사용 합니다 `ResourceCache` 속성을 `Application` 클래스.

```csharp
healthBar.SetTexture(ResourceCache.GetTexture2D("Textures/HealthBarBorder.png"));
```

또한 리소스는 수동으로 만든 고, 해당 디스크에서 로드 된 것 처럼 리소스 캐시에 저장 수 있습니다.

리소스 유형별 메모리 예산을 설정할 수 있습니다: 리소스 보다 더 많은 메모리를 사용 하는 경우 가장 오래 된 리소스를 제거할 캐시에서 사용 되지 않는 경우 더 이상. 기본적으로 메모리 예산은 무제한으로 설정 합니다.

### <a name="bringing-3d-models-and-images"></a>3D 모델 및 이미지

Urho3D 모델 (.mdl) 및 애니메이션 (.ani)와 같이 꼭 필요한 경우에 사용자 지정 파일 형식을 정의 하 고 가능 하면 기존 파일 형식을 사용 하려고 합니다. 이러한 유형의 자산에 대 한 Urho-변환기를 제공 [AssetImporter](http://urho3d.github.io/documentation/1.4/_tools.html) fbx, dae, 3ds, 및 obj 등과 같은 여러 인기 있는 3D 형식을 사용할 수는 있습니다.

이기도 한 유용한 용 추가 기능에서 Blender [ https://github.com/reattiva/Urho3D-Blender ](https://github.com/reattiva/Urho3D-Blender) 는 Blender 자산 Urho3D에 적합 한 형식으로 내보낼 수 있습니다.

### <a name="background-loading-of-resources"></a>리소스의 백그라운드에 로드

일반적으로 중 하나를 사용 하 여 리소스를 요청 하는 경우는 `ResourceCache`의 `Get` 에서 필요한 모든 단계에 대 한 몇 가지 밀리초 소요 될 수 있는 주 스레드를 즉시 로드 된 메서드 (디스크에서 파일을 로드, 데이터를 구문 분석, 필요한 경우 GPU에 업로드 ) 따라서 framerate 삭제 발생할 수 있습니다.

사전에 어떤 리소스가 필요한 경우에 호출 하 여 백그라운드 스레드에서 로드 되도록를 요청할 수 있습니다 `BackgroundLoadResource`합니다. 사용 하 여 리소스 백그라운드 로드 이벤트를 구독할 수 있습니다는 `SubscribeToResourceBackgroundLoaded` 메서드. 성공 또는 실패를 로드 하는 실제로 된 경우이 알려줍니다. 백그라운드 스레드로 이동할 수 있습니다만 로드 프로세스의 일부 리소스에 따라을 예를 들어 완료 GPU 업로드 단계는 항상 주 스레드에서 발생 합니다. 배경 로드에 대 한 대기 중인 리소스에 대 한 메서드를 로드 하는 리소스 중 하나를 호출 하는 경우 주 스레드는 지연 로드가 완료 될 때까지 note 합니다.

기능을 로드 하는 비동기 장면 `LoadAsync` 고 `LoadAsyncXML` 리소스가 백그라운드 로드 하는 옵션을 먼저 전에 장면 콘텐츠를 로드 합니다. 또한 사용할 수만 지정 하 여 장면을 수정 하지 않고 리소스를 로드할 수는 `LoadMode.ResourcesOnly`합니다. 이렇게 하면 빠른 인스턴스화에 대 한 개체 또는 장면을 prefab 파일을 준비 합니다.

최대 시간 (밀리초)에 각 프레임을 소요 하는 마지막으로 완료 배경을 설정 하 여 로드 된 리소스를 구성할 수 있습니다는 `FinishBackgroundResourcesMs` 속성에는 `ResourceCache`합니다.

<a name="sound"/>

## <a name="sound"></a>소리

소리 게임 플레이 중 중요 한 부분 이며 UrhoSharp 프레임 워크는 게임에서 소리를 재생 하는 방법을 제공 합니다.  연결 하 여 소리를 재생 하는 `SoundSource`
구성 요소는 `Node` 하 고 다음 리소스에서 명명된 된 파일을 재생 합니다.

다음은 수행 되는 방법입니다.

```csharp
var explosionNode = Scene.CreateChild();
var sound = explosionNode.CreateComponent<SoundSource>();
soundSource.Play(Application.ResourceCache.GetSound("Sounds/ding.wav"));
soundSource.Gain = 0.5f;
soundSource.AutoRemove = true;
```

<a name="particles"/>

## <a name="particles"></a>파티클

파티클에는 응용 프로그램에 일부 쉽고 저렴 한 효과 추가 하는 간단한 방법을 제공 합니다.  와 같은 도구를 사용 하 여 PEX 형식으로 저장 하는 입자 사용할 수 있습니다 [ http://onebyonedesign.com/flash/particleeditor/ ](http://onebyonedesign.com/flash/particleeditor/)합니다.

파티클은 노드를 추가할 수 있는 구성 요소.  노드를 호출 해야 `CreateComponent<ParticleEmitter2D>` 파티클이 만들고 파티클이 2D 효과를 Effect 속성을 설정 하 여 구성한 방법 리소스 캐시에서 로드 됩니다.

예를 들어 도달 했을 때 급증으로 렌더링 되는 일부 파티클 표시할 구성 요소에서이 메서드를 호출할 수 있습니다.

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

위의 코드는 현재 구성 요소에 연결 된 급증 노드를 만들고,이 급증 노드 내에서 2D 파티클 내보내기 만들고 Effect 속성을 설정 하 여 구성 합니다.  두 가지 작업, 더 작게 할 노드를 확장 하는 하나 및 0.5 초에 대 한 해당 크기로 유지 하는 실행 합니다.  다음 화면에서 파티클 효과 제거 하는 급증을 제거 합니다.

위의 파티클이 구체 질감을 사용 하는 경우 같이 렌더링 됩니다.

![구체 질감으로 파티클](using-images/image-1.png "구체 질감을 사용 하는 경우 위의 파티클이 다음과 같이 렌더링")

및의 모양 딱딱한 질감을 사용 하는 경우입니다.

![상자 질감으로 파티클](using-images/image-2.png "모양 딱딱한 질감을 사용 하는 경우 이것이")

## <a name="multi-threading-support"></a>다중 스레딩 지원

UrhoSharp는 단일 스레드 라이브러리입니다.  즉, 백그라운드 스레드에서 UrhoSharp의 메서드를 호출 하지 않아야 하거나 응용 프로그램 상태를 손상 시키는 위험 및 응용 프로그램에 충돌이 발생 합니다.

백그라운드에서 몇 가지 코드를 실행 한 다음 주 UI에서 Urho 구성 요소를 업데이트 하려는 경우 사용할 수는 `Application.InvokeOnMain(Action)`
메서드를 재정의합니다.  또한, 수행할 수 있습니다 사용 하 여 C# await 및.NET 작업 Api 코드는 적절 한 스레드에서 실행 되도록 합니다.

## <a name="urhoeditor"></a>UrhoEditor

사용 중인 플랫폼에 대 한 Urho 편집기를 다운로드할 수 있습니다 합니다 [Urho 웹 사이트](http://urho3d.github.io/)다운로드로 이동 하 고 최신 버전을 선택 합니다.

## <a name="copyrights"></a>저작권

이 설명서 Xamarin Inc에서 원래 콘텐츠를 포함 하 고 있지만 Urho3D 프로젝트에 대 한 오픈 소스 문서에서 광범위 하 게 그립니다, Cocos2D 프로젝트 스크린 샷을 포함 합니다.
