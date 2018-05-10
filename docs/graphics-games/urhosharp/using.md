---
title: UrhoSharp를 사용 하 여
description: UrhoSharp 엔진 개요
ms.prod: xamarin
ms.assetid: D9BEAD83-1D9E-41C3-AD4B-3D87E13674A0
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 8eed81817620b3f68510ab2e043c3aeaafb6e78a
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
---
# <a name="using-urhosharp"></a>UrhoSharp를 사용 하 여

_UrhoSharp 엔진 개요_

기본 사항을 파악 한 다음 가져올 하려는 첫 번째 게임을 작성 하기 전에: 장면이 설정 하는 방법, 리소스 (아트 워크 포함)를 로드 하는 방법 및 게임에 대 한 간단한 상호 작용 하는 방법입니다.

<a name="scenenodescomponentsandcameras"/>

## <a name="scenes-nodes-components-and-cameras"></a>장면, 노드, 구성 요소 및 카메라

장면 모델 구성 요소 기반 장면 그래프도 설명할 수 있습니다. 장면 전체 장면을 나타내는 루트 노드에서 시작 장면 노드 계층으로 이루어져 있습니다. 각 [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node/) 3D 변환을 (위치, 회전 및 배율), 이름, ID, + 구성 요소는 임의 개수의 했습니다.  구성 요소 수명 상태로 전환 하는 노드, 표시 되는 추가 내릴 수 ([`StaticModel`](https://developer.xamarin.com/api/type/Urho.StaticModel)), 소리를 내보낼 수 있습니다 ([`SoundSource`](https://developer.xamarin.com/api/type/Urho.Audio.SoundSource)), 충돌 경계에 제공할 수 있습니다.

장면 및 사용 하 여 설정 노드를 만들 수 있습니다는 [Urho 편집기](#UrhoEditor), 또는 C# 코드에서 작업을 할 수 있습니다.  이 문서에서 설명 합니다 진행 화면에 표시 되도록 하는 데 필요한 요소를 설명 대로 설정 높이려면 코드를 사용 하 여

장면이 설정 하는 것 외에도 설정 해야는 [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera/),이 사용자에 게 표시 가져오기 내용을 결정 합니다.

### <a name="setting-up-your-scene"></a>장면이 설정

일반적으로이 양식을 Start 메서드 만들 수 있습니다.

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

3D 개체 렌더링, 소리 재생, 물리 및 논리 스크립팅된 업데이트를 모두 사용 호출 하 여 노드를 서로 다른 구성 요소를 만들어 [ `CreateComponent<T>()` ](https://developer.xamarin.com/api/member/Urho.Node.CreateComponent%3CT%3E/p/Urho.CreateMode/System.UInt32/)합니다.  예를 들어, 노드 및 이와 같은 밝은 구성 요소 설치:

```csharp
// Create a directional light to the world so that we can see something. The
// light scene node's orientation controls the light direction; we will use
// the SetDirection() function which calculates the orientation from a forward
// direction vector.
// The light will use default settings (white light, no shadows)
var lightNode = scene.CreateChild("DirectionalLight");
lightNode.SetDirection (new Vector3(0.6f, -1.0f, 0.8f));
```

이름으로 노드 위에 만들어져 "`DirectionalLight`" 하지만 방법 이외에 대 한 방향을 설정 합니다.  이제 설정 하 여 위의 노드 light 내보내기 노드로 연결 하 여 한 [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light/) , 구성 요소와 `CreateComponent`:

```csharp
var light = lightNode.CreateComponent<Light>();
```

에 생성 된 구성 요소는 `Scene` 자체는 특별 한 역할: 장면 전체 기능을 구현 합니다. 다른 구성 요소 보다 먼저 만들어야 하며 다음과 같습니다.

* [`Octree`](https://developer.xamarin.com/api/type/Urho.Octree/): 공간 분할을 구현 하 고 표시 유형 쿼리 가속화 됩니다. 이 3D 없이 개체를 렌더링할 수 있습니다.
* [`PhysicsWorld`](https://developer.xamarin.com/api/type/Urho.Physics.PhysicsWorld/): 물리 시뮬레이션을 구현 합니다. 구성 요소와 같은 물리학 [ `RigidBody` ](https://developer.xamarin.com/api/type/Urho.Physics.RigidBody/) 또는 [ `CollisionShape` ](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape/) 없이이 제대로 작동 하지 않을 수 있습니다.
* [`DebugRenderer`](https://developer.xamarin.com/api/type/Urho.DebugRenderer/): 구현 디버그 기 하 도형을 렌더링 합니다.

일반 구성 요소와 같은 [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light), [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera) 또는 [ `StaticModel` ](https://developer.xamarin.com/api/type/Urho.StaticModel) 에 직접 만들 수 없습니다는 [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Scene), 되지만 자식 노드 대신 합니다.

다양 한 수명에 가져오는 노드에 연결할 수 있는 구성 요소와 함께 라이브러리 제공: 사용자가 볼 수 (있습니다 모델) 요소, 소리, 고정 된 관계로 본문, 충돌 셰이프, 카메라, 광원, 입자가 미터 및 등입니다.

### <a name="shapes"></a>도형

편의 위해 다양 한 셰이프에 Urho.Shapes 네임 스페이스의 간단한 노드도 사용할 수 있습니다.  여기에 상자, 구, 콘, 원통형 및 평면 포함 됩니다.

### <a name="camera-and-viewport"></a>카메라 및 뷰포트

카메라의 구성 요소는 광원에서와 마찬가지로 구성 요소 노드에 연결 해야 합니다 하므로 이렇게를 이렇게:

```csharp
var CameraNode = scene.CreateChild ("camera");
camera = CameraNode.CreateComponent<Camera>();
CameraNode.Position = new Vector3 (0, 5, 0);
```

카메라,이 만들었습니다 및 3D 세계에서 카메라를 놓은, 알리기 위해 다음 단계는는 `Application` 사용 하려는 카메라, 이것이 이렇게를 다음 코드로:

```csharp
Renderer.SetViewPort (0, new Viewport (Context, scene, camera, null))
```

고 이제 사용자 만들기의 결과 볼 수 해야 합니다.

### <a name="identification-and-scene-hierarchy"></a>식별 및 장면 계층 구조

노드와 달리 구성 요소에는 이름이 없습니다. 동일한 노드에 내부의 구성 요소 형식 및 생성 순서 채워진 노드의 구성 요소 목록에서 인덱스 별로 으로만 식별, 예를 들어 검색할 수 있습니다는 [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light) 의 구성 요소는 `lightNode` 개체 위에 다음과 같이 합니다.

```csharp
var myLight = lightNode.GetComponent<Light>();
```

모든 구성 요소 목록을 검색 하 여 가져올 수도 있습니다는 [ `Components` ](https://developer.xamarin.com/api/property/Urho.Node.Components/) 속성을 반환 하는 `IList<Component>` 사용할 수 있는 합니다.

를 만들 때 노드 및 구성 요소를 모두 장면 전역 정수 Id를 가져옵니다. 함수를 사용 하 여 장면에서 쿼리할 수 있는 [ `GetNode(uint id)` ](https://developer.xamarin.com/api/member/Urho.Scene.GetNode/p/System.UInt32/) 및 [ `GetComponent(uint id)` ](https://developer.xamarin.com/api/member/Urho.Scene.GetComponent/p/System.UInt32/)합니다. 예를 들어 재귀 이름 기반 장면 노드가 쿼리를 수행할 때 보다 훨씬 빠릅니다.

엔터티 또는 게임 개체;의 기본 개념은 대신 노드 계층 구조를 결정 하는 프로그래머 백업 하 고 모든 스크립트를 사용한 논리를 노드가 됩니다. 일반적으로 3D 전 세계에서-이동 개체는 루트 노드의 자식으로 만들어졌습니다. 노드 또는 비 사용 하 여 이름을 만들 수 있습니다 [ `CreateChild()` ](https://developer.xamarin.com/api/member/Urho.Node.CreateChild/p/System.String/Urho.CreateMode/System.UInt32/)합니다. 노드 이름의 고유성이 적용 되지 않습니다.

일부 계층적 컴퍼지션 있을 때마다 며 권장 (자신의 3D 변환 구성 요소에 없기 때문에 필요한 실제로) 자식 노드를 만듭니다.

예를 들어 있으면 문자 자신의 손에 개체에 사용한, 개체가 있어야 합니다. 문자 손 본 부모가 될 자체 노드를 (또한는 [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node/)).  예외는, 물리적인 [ `CollisionShape` ](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape), offsetted 및 노드를 기준으로 개별적으로 회전 될 수 있습니다.

[ `Scene` ](https://developer.xamarin.com/api/type/Urho.Node/)의 파생 된 세계 변환의 자식 노드를 계산 하는 데 최적화로 변환 의도적으로 무시 됩니다, 아무 효과가 변경 것와 원점에 회전 없음을 (위치를 그대로 남겨 두어야 하면 소유 크기가.)

[`Scene`](https://developer.xamarin.com/api/type/Urho.Node/) 노드 하위 자유롭게 레코드가 있습니다. 이와 달리 구성 요소 항상를 노드에 속하는에 연결 하 고 노드 간에 이동할 수 없습니다. 노드 및 구성 요소를 모두 제공는 [ `Remove()` ](https://developer.xamarin.com/api/member/Urho.Node.Remove()/) 부모를 통해 이동 하지 않고도이 수행 하는 함수입니다. 노드 제거 되 면 해당 함수를 호출한 후 노드 또는 해당 구성 요소에서 어떤 작업도 안전 합니다.

만들 수 이기도 한 `Node` 장면에 속해 있지 않습니다. 이 유용한 예를 들어 때문에 로드 하거나 저장 장면에 이동 카메라로 그런 다음 카메라 실제 장면에 함께 저장 되지 것입니다 장면의 로드 될 때 소멸 되지 것입니다. 그러나 제대로 작동 하지에 이러한 구성 요소 기 하 도형, 물리학 또는 스크립트 구성 요소에서 연결 되지 않은 노드를 만들고 나중에 장면에 이동 후 인해 됨 note 합니다.

### <a name="scene-updates"></a>장면 업데이트

해당 업데이트를 사용할 수는 장면 (기본값) 주 루프가 반복 될 때마다 자동으로 업데이트 됩니다.  응용 프로그램의 [ `SceneUpdate` ](https://developer.xamarin.com/api/event/Urho.Scene.SceneUpdate/) 에 이벤트 처리기가 호출 됩니다.

노드 및 구성 요소 사용 하지 않도록 하 여 장면 업데이트에서 제외할 수, 참조 [ `Enabled` ](https://developer.xamarin.com/api/member/Urho.Node.Enabled)합니다.  동작은 특정 구성 요소에 따라 하지만 예 그릴 수 있는 구성 요소 사용 안 함을 사용 하면, 보이지 않는 것 음을 소거 사운드 원본 구성 요소를 사용 하지 않도록 설정 하는 동안 합니다. 노드 비활성화 된 경우 자신의 설정/해제 상태에 관계 없이 비활성화 된 것으로 처리 됩니다 모든 구성 요소.

## <a name="adding-behavior-to-your-components"></a>동작 구성 요소에 추가

게임을 구성 하는 가장 좋은 방법은 작업자 또는 게임에 요소를 캡슐화 하는 구성 요소를 직접 확인 하는입니다.  이 기능을 사용 하면 자체의 동작에 표시 하는 데 사용 된 자산 중에서 포함 합니다.

동작 구성 요소에 추가 하는 가장 간단한 방법은 작업을 큐에 대기 하 고 가비지 수집기가 C# 비동기 프로그래밍 수 있는 지침을 사용 하는 것입니다.  매우 높은 수준 수를 구성 요소에 대 한 동작 수 있도록 허용 하 고 간편 하 게 하는 상황을 이해 합니다.

또는 발생 하는 동작은 해당 구성 요소 (프레임 기반 동작 섹션에서 설명) 각 프레임에 프로그램 구성 요소 속성을 업데이트 하 여 제어할 수 있습니다.

### <a name="actions"></a>작업

작업을 사용 하 여 쉽게 매우 노드에 동작을 추가할 수 있습니다.  작업 다양 한 노드 속성을 변경 및 시간, 기간 동안이 실행 하거나 지정한 애니메이션 곡선이 있는 횟수를 반복 합니다.

예를 들어 장면에 "클라우드" 노드, 다음과 같은 페이드 수 있습니다.:

```csharp
await cloud.RunActionsAsync (new FadeOut (duration: 3))
```

작업은 변경할 수 없는 개체를 서로 다른 개체를 구동 하는 것에 대 한 작업을 다시 사용할 수 있습니다.

반대 작업을 수행 하는 작업을 만들 때 일반적입니다.

```csharp
var gotoExit = new MoveTo (duration: 3, position: exitLocation);
var return = gotoExit.Reverse ();
```

다음 예에서는 3 초 기간 동안 사용자에 대 한 개체를 페이드 것입니다.  한 가지 동작을 실행할 수도 있습니다 서로 예를 들어 수 먼저 클라우드 이동한 다음 숨길:

```csharp
await cloud.RunActionsAsync (
    new MoveBy  (duration: 1.5f, position: new Vector3(0, 0, 15),
    new FadeOut (duration: 3));
```

두 작업이 동시에 수행 하려는 경우 병렬 작업을 사용할 수 있고 모든 작업을 병렬로 수행 제공 됩니다.

```csharp
  await cloud.RunActionsAsync (
    new Parallel (
      new MoveBy  (duration: 3, position: new Vector3(0, 0, 15),
      new FadeOut (duration: 3)));
```

위의 예에서 클라우드 옮기고 동시에 페이드 아웃 합니다.

이러한 사용 하는 C# 알게 될 await 달성 하려는 동작에 대 한 선형으로 생각할 수 있습니다.

### <a name="basic-actions"></a>기본 작업

다음은 UrhoSharp에서 지원 되는 작업입니다.

* 노드 이동: [ `MoveTo` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveTo), [ `MoveBy` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveBy), [ `Place` ](https://developer.xamarin.com/api/type/Urho.Actions.Place), [ `BezierTo` ](https://developer.xamarin.com/api/type/Urho.Actions.BezierTo), [ `BezierBy` ](https://developer.xamarin.com/api/type/Urho.Actions.BezierBy) , [`JumpTo`](https://developer.xamarin.com/api/type/Urho.Actions.JumpTo), [`JumpBy`](https://developer.xamarin.com/api/type/Urho.Actions.JumpBy)
* 노드를 회전: [ `RotateTo` ](https://developer.xamarin.com/api/type/Urho.Actions.RotateTo), [`RotateBy`](https://developer.xamarin.com/api/type/Urho.Actions.RotateBy)
* 노드를 배율: [ `ScaleTo` ](https://developer.xamarin.com/api/type/Urho.Actions.ScaleTo), [`ScaleBy`](https://developer.xamarin.com/api/type/Urho.Actions.ScaleBy)
* 노드 페이딩: [ `FadeIn` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeIn), [ `FadeTo` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeTo), [ `FadeOut` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeOut), [ `Hide` ](https://developer.xamarin.com/api/type/Urho.Actions.Hide), [`Blink`](https://developer.xamarin.com/api/type/Urho.Actions.Blink)
* 색조: [ `TintTo` ](https://developer.xamarin.com/api/type/Urho.Actions.TintTo), [`TintBy`](https://developer.xamarin.com/api/type/Urho.Actions.TintBy)
* 기간의 시작 종료: [ `Hide` ](https://developer.xamarin.com/api/type/Urho.Actions.Hide), [ `Show` ](https://developer.xamarin.com/api/type/Urho.Actions.Show), [ `Place` ](https://developer.xamarin.com/api/type/Urho.Actions.Place), [ `RemoveSelf` ](https://developer.xamarin.com/api/type/Urho.Actions.RemoveSelf), [`ToggleVisibility`](https://developer.xamarin.com/api/type/Urho.Actions.ToggleVisibility)
* 반복: [ `Repeat` ](https://developer.xamarin.com/api/type/Urho.Actions.Repeat), [ `RepeatForever` ](https://developer.xamarin.com/api/type/Urho.Actions.RepeatForever), [`ReverseTime`](https://developer.xamarin.com/api/type/Urho.Actions.ReverseTime)

기타 고급 기능의 조합을 포함 된 [ `Spawn` ](https://developer.xamarin.com/api/type/Urho.Actions.Spawn) 및 [ `Sequence` ](https://developer.xamarin.com/api/type/Urho.Actions.Sequence) 동작 합니다.

### <a name="easing---controlling-the-speed-of-your-actions"></a>감속/가속-작업의 속도 제어 합니다.

감속/가속는 애니메이션 펼침은 고 하 게 하려면 애니메이션 훨씬 더 쾌적 한 하는 방식에 지시 하는 방법입니다.  기본적으로 작업을 수행할 때 적용 됩니다 선형 동작 예를 들어 한 [ `MoveTo` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveTo) 동작 매우 로보틱 이동 해야 합니다.  사용자의 작업 예를 들어 동작을 변경 하는 느린 움직임을 시작, 가속화 하 고 있는 느린 끝에 도달 감속/가속 작업을 래핑할 수 있습니다 ([`EasyInOut`](https://developer.xamarin.com/api/type/Urho.Actions.EasyInOut)).

예를 들어 감속/가속 작업에 기존 작업을 래핑하고이 수행 합니다.

```csharp
await cloud.RunActionAsync (
   new EaseInOut (
     new MoveTo (duration: 3, position: new Vector (0,0,15)), rate:1))
```

많은 감속/가속 모드는, 처음부터 끝까지 시간, 기간 동안 제어 하는 개체의 값에 다양 한 감속/가속 형식 및 해당 명령의 동작은 다음 차트에 표시 합니다.

![모드 감속/가속](using-images/easing.png "시간 동안 제어 하는 개체의 값에 다양 한 감속/가속 형식 및 해당 명령의 동작은이 차트에 표시")

### <a name="using-actions-and-async-code"></a>작업 및 비동기 코드를 사용 하 여

사용자 [ `Component` ](https://developer.xamarin.com/api/type/Urho.Component/) 하위 클래스, 구성 동작을 준비 하 고 기능에 대 한 드라이브 하는 비동기 메서드를 도입 해야 합니다.
C#을 사용 하 여이 메서드를 호출 하는 다음 `await` 프로그램의 다른 부분에서 키워드 중 하나에 `Application.Start` 메서드 또는 응용 프로그램에서 사용자 또는 스토리 포인트에 대 한 응답에서입니다.

예를 들어:

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

에 `Launch` 위의 세 가지 동작 메서드에서 시작: 로봇 장면 놓임,이 작업에서 0.6 초의 기간 동안 노드의 위치를 변경 합니다.  이 비동기 옵션 이므로이 수행 됩니다 동시에 호출 되는 다음 명령이를 `MoveRandomly`합니다.  이 메서드를 임의의 위치로 병렬로 로봇의 위치를 변경 됩니다.  새 위치로 이동 두 개의 복합된 작업을 수행 하 여 그렇게 하 고 원래를 다시 살펴보자면 배치 로봇 활성 상태인 동안이 단계를 반복 합니다.  작업 및 작업을 수행 하기 더 흥미로운, 로봇 됩니다 유지 촬영 동시에 합니다.  촬영 0.1 초 마다만 시작 됩니다.

### <a name="frame-based-behavior-programming"></a>동작 프레임 기반 프로그래밍

작업을 사용 하는 대신 프레임별으로 별로 구성 요소의 동작을 제어 하려는 경우 재정의 하는 것은 수행할는 [ `OnUpdate` ](https://developer.xamarin.com/api/member/Urho.Component.OnUpdate) 방식의 프로그램 [ `Component` ](https://developer.xamarin.com/api/type/Urho.Component) 하위 클래스입니다.  이 메서드는 프레임을 마다 한 번씩 호출 되 고 ReceiveSceneUpdates 속성을 true로 설정 하는 경우에 호출 됩니다.

다음 만드는 방법을 보여 줍니다.는 `Rotator` 고정 된 상태로 유지 하 고 회전 노드가 노드에 있는 구성 요소:

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
            RotationSpeed.X * timeStep,
            RotationSpeed.Y * timeStep,
            RotationSpeed.Z * timeStep),
            TransformSpace.Local);
    }
}
```

그리고이 어떻게 노드를이 구성 요소를 연결 합니다.

```csharp
Node boxNode = new Node();
var rotator = new Rotator() { RotationSpeed = rotationSpeed };
boxNode.AddComponent (rotator);
```

### <a name="combining-styles"></a>스타일을 결합합니다.

대부분의 동작 및 잊어 화재 스타일의 프로그래밍에 대 한 훌륭한의 프로그래밍에 대 한 비동기/작업 기반 모델을 사용할 수 있지만 각 프레임에서 일부 업데이트 코드를 실행 하 여 구성 요소의 동작을 또한 미세 조정할 수 있습니다.

예를 들어 SamplyGame 데모에서에 사용 됩니다는 `Enemy` 도 인 노드의 방향을 설정 하 여 구성 요소 사용자 입장 지점 유지 이지만 클래스를 사용 하 여 동작 기본 동작 인코딩합니다 `Node.LookAt`:

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

## <a name="loading-and-saving-scenes"></a>로드 및 장면 저장

백그라운드에서 로드 되 고 XML 형식으로 저장 함수 참조 [ `LoadXml` ](https://developer.xamarin.com/api/member/Urho.Scene.LoadXml) 및 [ `SaveXML()` ](https://developer.xamarin.com/api/member/Urho.Scene.SaveXml)합니다. 장면이 로드 되 면 모든 기존 콘텐츠가 (자식 노드 및 구성 요소)를 먼저 제거 됩니다. 노드와 일시적으로 표시 된 구성 요소는 `Temporary` 속성에 저장 되지 것입니다. 모든 기본 제공 구성 요소 및 속성을 처리 하는 serializer 있지만 사용자 지정 속성 및 하위 클래스에 구성 요소에 정의 된 필드를 처리 하 여 아닙니다. 그러나이 두 개의 가상 메서드를 제공합니다.

* [`OnSerialize`](https://developer.xamarin.com/api/member/Urho.Component.OnSerialize) 여기서 등록할 수 있습니다는 serialization에 대 한 사용자 지정 상태

* [`OnDeserialized`](https://developer.xamarin.com/api/member/Urho.Component.OnDeserialize) 위치에 저장 된 사용자 지정 상태를 가져올 수 있습니다.

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

### <a name="object-prefabs"></a>개체 Prefabs

방금 로드 또는 저장 하는 전체 백그라운드 유연성은 없습니다 게임에 대 한 새 개체를 동적으로 해야 하는 위치입니다. 반면에 복잡 한 개체를 만들고 코드에서 해당 속성을 설정 합니다. 또한 됩니다 번거로운. 이러한 이유로 해당 자식 노드, 구성 요소 및 특성이 포함 된 장면 노드가 저장할 수 이기도 합니다. 이러한 그룹으로 나중에 편리 하 게 로드할 수 있습니다.  이러한 저장 된 개체는 prefab 라고도 합니다. 이렇게 하는 데는 다음과 같은 세 가지 방법이 있습니다.

- 호출 하 여 코드에서 [ `Node.SaveXml` ](https://developer.xamarin.com/api/member/Urho.Node.SaveXml) 노드
- 편집기에서 계층 구조 창에서 노드를 선택 하 고 선택 하 여 "저장" 노드 "File" 메뉴에서 합니다.
- 에 "노드" 명령을 사용 하 여 `AssetImporter`, 장면 노드 계층 구조를 저장 및 모든 모델 (예: 입력된 자산에 포함 된. Collada 파일)

저장 된 노드를 장면에를 인스턴스화하고 호출 [ `InstantiateXml()` ](https://developer.xamarin.com/api/member/Urho.Scene.InstantiateXml)합니다. 노드 장면의 자식으로 만들 수 이지만 그 후 자유롭게 부모가 될 수 있습니다. 위치 및 회전 노드를 배치 하도록 지정 되어 있어야 합니다. 다음 코드는 prefab 인스턴스화하는 방법을 보여 줍니다 `Ninja.xm` 원하는 위치 및 회전 된 장면에:

```csharp
var prefabPath = Path.Combine (FileSystem.ProgramDir,"Data/Objects/Ninja.xml");
using (var file = new File(Context, prefabPath, FileMode.Read))
{
    scene.InstantiateXml(file, desiredPos, desiredRotation,
        CreateMode.Replicated);
}
```

## <a name="events"></a>이벤트

UrhoObjects 다양 한 이벤트를 발생 시킵니다., 이러한을 생성 하는 다양 한 클래스에 C# 이벤트로 표시 됩니다.  C# 외에도-이벤트 기반된 모델을 것도 가능 사용 하는 `SubscribeToXXX` 를 구독 하 고 구독 토큰을 유지 하는 사용할 수 있는 메서드가 사용할 수 있습니다 나중에 구독을 취소 하려면.  전자을 사용 하면 많은 호출자가 구독을, 두 번째 식만 하나를 허용 하지만 고려한 더 좋은 람다 스타일 사용 되는 접근 방식 및이 아직 구독 쉽게 제거할 수 있도록 하는 점이 다릅니다.  함께 사용할 수 없습니다.

이벤트를 구독 하면 적절 한 이벤트 인수를 사용 하는 인수를 사용 하는 메서드를 제공 해야 합니다.

예를 들어 다음은 마우스 단추 누름 이벤트를 구독 하는 방법입니다.

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

이벤트를 이러한 경우 반환 값에서에서 저장에 대 한 호출에 대 한 알림 수신을 중지 하도록 할 경우도 있습니다 `SubscribeTo` 메서드를 하 고 Unsubscribe 메서드를 호출 합니다.

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

이벤트 처리기에서 수신 하는 매개 변수는 각 이벤트와 관련 이벤트 페이로드를 포함 하는 강력한 형식의 이벤트 인수 클래스입니다.

## <a name="responding-to-user-input"></a>사용자 입력에 응답

이벤트를 구독 하 고 배달 되 고 입력에 응답 하 여 아래로 키와 같은 다양 한 이벤트를 구독할 수 있습니다.

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

하지만 대부분의 시나리오에서 업데이트 되 고 그에 따라 코드를 업데이트할 때 키의 현재 상태를 확인 하 여 장면 업데이트 처리기.  예를 들어 다음 데 사용할 수 키보드 입력에 따라 카메라 위치를 업데이트 합니다.

```csharp
protected override void OnUpdate(float timeStep)
{
    Input input = Input;
    // Movement speed as world units per second
    const float moveSpeed = 4.0f;
    // Read WASD keys and move the camera scene node to the
    // corresponding direction if they are pressed
    if (input.GetKeyDown(Key.W))
        CameraNode.Translate(Vector3.UnitY * moveSpeed * timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.S))
        CameraNode.Translate(new Vector3(0.0f, -1.0f, 0.0f) * moveSpeed * timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.A))
        CameraNode.Translate(new Vector3(-1.0f, 0.0f, 0.0f) * moveSpeed * timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.D))
        CameraNode.Translate(Vector3.UnitX * moveSpeed * timeStep, TransformSpace.Local);
}
```

## <a name="resources-assets"></a>리소스 (자산)

리소스에서 UrhoSharp 초기화 또는 런타임 중에 대용량 저장소에서 로드 되는 항목의 대부분 포함 됩니다.

- [`Animation`](https://developer.xamarin.com/api/type/Urho.Animation/) -골격 애니메이션에 사용 되는
- [`Image`](https://developer.xamarin.com/api/type/Urho.Resources.Image) -다양 한 그래픽 형식에에서 저장 된 이미지를 나타냅니다.
- [`Model`](https://developer.xamarin.com/api/type/Urho.Model/) -3D 모델
- [`Material`](https://developer.xamarin.com/api/type/Urho.Material) -모델을 렌더링 하는 데 사용 되는 자료입니다.
- [`ParticleEffect`](https://developer.xamarin.com/api/type/Urho.ParticleEffect)- [설명](http://urho3d.github.io/documentation/1.4/_particles.html) 작동 하는 입자 송신기 참조 "[입자](#particles)" 아래 합니다.
- [`Shader`](https://developer.xamarin.com/api/type/Urho.Shader) -사용자 지정 셰이더
- [`Sound`](https://developer.xamarin.com/api/type/Urho.Audio.Sound) -소리를 재생 참조 "[소리](#sound)" 아래 합니다.
- [`Technique`](https://developer.xamarin.com/api/type/Urho.Technique/) -자재 렌더링 기술을
- [`Texture2D`](https://developer.xamarin.com/api/type/Urho.Urho2D.Texture2D/) -2D 질감
- [`Texture3D`](https://developer.xamarin.com/api/type/Urho.Texture3D/) -3D 질감
- [`TextureCube`](https://developer.xamarin.com/api/type/Urho.TextureCube/) -큐브 질감
- `XmlFile`

관리 하 고에 의해 로드 된 [ `ResourceCache` ](https://developer.xamarin.com/api/type/Urho.Resources.ResourceCache/) 하위 시스템 (로 제공 [ `Application.ResourceCache` ](https://developer.xamarin.com/api/property/Urho.Application.ResourceCache/)).

리소스 자체 등록 된 리소스 디렉터리 또는 패키지 파일을 기준으로 해당 파일 경로 의해 식별 됩니다. 엔진은 기본적으로 리소스 디렉터리에 등록 `Data` 및 `CoreData`, 또는 패키지 `Data.pak` 및 `CoreData.pak` 있을 경우.

오류가 발생 하면 리소스를 로드 하는 경우 오류가 기록 되 고 null 참조가 반환 됩니다.

다음 예제에는 리소스 캐시에서 리소스를 가져오는 중의 일반적인 방법을 보여 줍니다.  이 경우 UI 요소에 대 한 질감이 사용 하 여는 `ResourceCache` 에서 속성의 `Application` 클래스입니다.

```csharp
healthBar.SetTexture(ResourceCache.GetTexture2D("Textures/HealthBarBorder.png"));
```

리소스를도 수동으로 작성 및 디스크에서 로드 되었을 것 처럼 리소스 캐시에 저장 합니다.

리소스 종류 별로 메모리 예산을 설정할 수 있습니다: 리소스는 허용 된 것 보다 더 많은 메모리를 소비 하는 경우 가장 오래 된 리소스를 제거할 캐시에서 사용 되지 않는 경우 더 이상. 기본적으로 메모리 예산은 무제한으로 설정 합니다.

### <a name="bringing-3d-models-and-images"></a>3D 모델 및 이미지

가능한 경우 항상 기존 파일 형식을 사용 하 고 반드시 필요한 경우가 아니면와 같은 모델에 대 한 사용자 지정 파일 형식 정의 하려고 시도 Urho3D (*.mdl) 및 애니메이션에 대 한 (*.ani). 이러한 유형의 자산에 대 한 Urho 제공 변환기- [AssetImporter](http://urho3d.github.io/documentation/1.4/_tools.html) fbx, 디지털 오디오, 3ds, 및 obj 등과 같은 여러 인기 있는 3D 형식에 사용할 수 있는 합니다.

이기도 한 편리한 용 추가 기능에 블렌더 [ https://github.com/reattiva/Urho3D-Blender ](https://github.com/reattiva/Urho3D-Blender) 블렌더 자산 Urho3D에 대 한 적합 한 형식으로 내보낼 수입니다.

### <a name="background-loading-of-resources"></a>리소스의 백그라운드에 로드

일반적으로 중 하나를 사용 하 여 리소스를 요청할 때는 `ResourceCache`의 `Get` 필요한 모든 단계에 대 한 몇 가지 밀리초 소요 될 수 있는 주 스레드에서 즉시 로드 된 메서드를 (디스크에서 파일을 로드, 데이터를 구문 분석, 필요한 경우 GPU에 업로드 ) framerate 삭제 합니다. 따라서 발생할 수 있습니다.

호출 하 여 백그라운드 스레드에서 로드 되도록 들 요청할 수 사전에 어떤 리소스가 필요한 경우, `BackgroundLoadResource()`합니다. 사용 하 여 리소스 백그라운드 로드 이벤트를 구독할 수 있습니다는 `SubscribeToResourceBackgroundLoaded` 메서드. 이 쿼리는 성공 또는 실패 로드 실제로 경우 확인할 수 있습니다. 백그라운드 스레드로 이동할 수는 로드 프로세스의 일부분만 리소스에 따라, 예를 들어 마무리 GPU 업로드 단계 항상 수행 하면 주 스레드에서 합니다. 로드 배경에 대 한 대기 중인 리소스에 대 한 메서드를 로드 하는 리소스 중 하나를 호출 하는 경우 주 스레드 지체 됩니다 로드가 완료 될 때까지 note 합니다.

기능을 로드 하는 비동기 장면 `LoadAsync()` 및 `LoadAsyncXML()` 백그라운드 로드 하는 옵션 먼저 장면 콘텐츠를 로드 하기 전에 진행 하는 리소스를가지고 있습니다. 또한 사용할 수만 지정 하 여 장면에 수정 하지 않고 리소스를 로드할 수는 `LoadMode.ResourcesOnly`합니다. 이렇게 하면 빠른 인스턴스화에 대 한 장면 또는 개체 prefab 파일을 준비 하는 것입니다.

최대 시간 (밀리초)에 각 프레임에 소요 하는 마지막으로 완료 하는 배경색을 설정 하 여 로드 된 리소스를 구성할 수 있습니다는 `FinishBackgroundResourcesMs` 속성에는 `ResourceCache`합니다.

<a name="sound"/>

## <a name="sound"></a>소리

소리 게임 플레이의 중요 한 부분이 이며 UrhoSharp 프레임 워크 게임에서 소리를 재생 하는 방법을 제공 합니다.  연결 하 여 소리를 재생 한 [ `SoundSource` ](https://developer.xamarin.com/api/type/Urho.Audio.SoundSource/) 구성 요소는 [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node) 하 고 다음 리소스에서 명명 된 파일을 재생 합니다.

이 수행 되는 방법입니다.

```csharp
var explosionNode = Scene.CreateChild();
var sound = explosionNode.CreateComponent<SoundSource>();
soundSource.Play(Application.ResourceCache.GetSound("Sounds/ding.wav"));
soundSource.Gain = 0.5f;
soundSource.AutoRemove = true;
```

<a name="particles"/>

## <a name="particles"></a>파티클

파티클 응용 프로그램에 몇 가지 단순 하 고 저렴 한 효과 추가 하는 간단한 방법을 제공 합니다.  와 같은 도구를 사용 하 여 PEX 형식으로 저장 하는 입자 소비할 수 있는 [ http://onebyonedesign.com/flash/particleeditor/ ](http://onebyonedesign.com/flash/particleeditor/)합니다.

파티클은 노드를 추가할 수 있는 구성 요소.  노드를 호출 하면 `CreateComponent<ParticleEmitter2D>` 파티클이 만들고 다음 2 차원 효과로 효과 속성을 설정 하 여 파티클이 구성 하는 메서드는 리소스 캐시에서 로드 합니다.

예를 들어 도달 했을 때 분해도 렌더링 되는 일부 입자 표시 하려면 구성 요소에이 메서드를 호출할 수 있습니다.

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

위의 코드에서는 현재 구성 요소에 연결 된 폭발 노드를 만듭니다, 그리고이 폭발 노드 내 만든 2D 입자 송신기 하 효과 속성을 설정 하 여 구성 합니다.  두 가지 동작, 해당 노드를 더 작게 크기가 조정 되 고 다른 하나는 0.5 초에 대 한 크기에서 그대로 실행 합니다.  그런 다음도 화면에서 입자 효과 제거 하는 분해를 제거 합니다.

위의 입자 구 텍스처를 사용 하는 경우 다음과 같이 렌더링 됩니다.

![구 질감으로 입자](using-images/image-1.png "구 텍스처를 사용 하는 경우 다음과 같이 위의 입자 렌더링")

그리고이 나타납니다 일그러지거나 텍스처를 사용 하는 경우:

![상자 질감으로 입자](using-images/image-2.png "나타납니다 일그러지거나 텍스처를 사용 하는 경우이")

## <a name="multithreading-support"></a>다중 스레딩 지원

UrhoSharp는 단일 스레드 라이브러리입니다.  즉, 백그라운드 스레드에서 UrhoSharp의 메서드를 호출 하지 않아야 하거나 응용 프로그램 상태가 손상의 위험 및 응용 프로그램에 충돌이 발생 합니다.

백그라운드에서 일부 코드를 실행 한 다음 주 UI에 Urho 구성 요소를 업데이트 하려는 경우 사용할 수 있습니다는 [ `Application.InvokeOnMain(Action)` ](https://developer.xamarin.com/api/member/Urho.Application.InvokeOnMain) 메서드.  수 또한,.NET 및 C# await 사용 작업 Api 코드는 적절 한 스레드에서 실행 되도록 합니다.

## <a name="urhoeditor"></a>UrhoEditor

사용 중인 플랫폼에 대 한 Urho 편집기를 다운로드할 수 있습니다는 [Urho 웹 사이트](http://urho3d.github.io/)다운로드로 이동 하 고 최신 버전을 선택 합니다.

## <a name="copyrights"></a>저작권

이 설명서 Xamarin Inc에서 원래 콘텐츠가 포함 되어 있고 Urho3D 프로젝트에 대 한 오픈 소스 문서에서 광범위 하 게 그립니다 Cocos2D 프로젝트에서 한 스크린샷을 제공 합니다.

## <a name="related-links"></a>관련 링크

- [지구 표면 통합 문서](https://developer.xamarin.com/workbooks/graphics/urhosharp/planetearth/planetearth.workbook)
- [좌표 통합 문서를 탐색합니다.](https://developer.xamarin.com/workbooks/graphics/urhosharp/coordinates/ExploringUrhoCoordinates.workbook)
