---
title: Xamarin.ios의 ARKit 소개
description: 이 문서에서는 ARKit를 사용 하는 iOS 11의 보강 된 현실에 대해 설명 합니다. 응용 프로그램에 3D 모델을 추가 하 고, 보기를 구성 하 고, 세션 대리자를 구현 하 고, 전 세계에 3D 모델을 배치 하 고, 확대 된 현실 세션을 일시 중지 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 70291430-BCC1-445F-9D41-6FBABE87078E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/30/2017
ms.openlocfilehash: 0094a496ce99addb08648431d993bd4afddca2f4
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032255"
---
# <a name="introduction-to-arkit-in-xamarinios"></a>Xamarin.ios의 ARKit 소개

_IOS 11에 대 한 보강 현실_

ARKit를 사용 하면 다양 한 확대 현실 응용 프로그램 및 게임을 사용할 수 있습니다. 이 단원에서는 다음 항목에 대해 설명합니다.

- [ARKit 시작](#gettingstarted)
- [UrhoSharp에서 ARKit 사용](urhosharp.md)

<a name="gettingstarted" />

## <a name="getting-started-with-arkit"></a>ARKit 시작

확대 된 현실를 시작 하기 위해 다음 지침에서는 간단한 응용 프로그램을 안내 합니다. 3D 모델의 위치를 지정 하 고 ARKit에서 해당 추적 기능을 사용 하 여 모델을 유지 하도록 허용 합니다.

![카메라 이미지의 Jet 3D 모델 부동](images/jet-sml.png)

### <a name="1-add-a-3d-model"></a>1.3D 모델 추가

**SceneKitAsset** 빌드 작업을 사용 하 여 프로젝트에 자산을 추가 해야 합니다.

![프로젝트에서 자산 SceneKit](images/scene-assets.png)

### <a name="2-configure-the-view"></a>2. 보기 구성

뷰 컨트롤러의 `ViewDidLoad` 메서드에서 장면 자산을 로드 하 고 보기에서 `Scene` 속성을 설정 합니다.

```csharp
ARSCNView SceneView = (View as ARSCNView);

// Create a new scene
var scene = SCNScene.FromFile("art.scnassets/ship");

// Set the scene to the view
SceneView.Scene = scene;
```

### <a name="3-optionally-implement-a-session-delegate"></a>3. 세션 대리자를 선택적으로 구현 합니다.

간단한 경우에는 필요 하지 않지만 세션 대리자를 구현 하면 ARKit 세션 (그리고 실제 응용 프로그램에서는 사용자에 게 피드백 제공)의 상태를 디버깅 하는 데 도움이 될 수 있습니다. 아래 코드를 사용 하 여 간단한 대리자를 만듭니다.

```csharp
public class SessionDelegate : ARSessionDelegate
{
  public SessionDelegate() {}
  public override void CameraDidChangeTrackingState(ARSession session, ARCamera camera)
  {
    Console.WriteLine("{0} {1}", camera.TrackingState, camera.TrackingStateReason);
  }
}
```

`ViewDidLoad` 메서드의에서 대리자를 할당 합니다.

```csharp
// Track changes to the session
SceneView.Session.Delegate = new SessionDelegate();
```

### <a name="4-position-the-3d-model-in-the-world"></a>4. 전 세계에 3D 모델 배치

`ViewWillAppear`에서 다음 코드는 ARKit 세션을 설정 하 고 장치의 카메라를 기준으로 3D 모델의 위치를 공간으로 설정 합니다.

```csharp
// Create a session configuration
var configuration = new ARWorldTrackingConfiguration {
  PlaneDetection = ARPlaneDetection.Horizontal,
  LightEstimationEnabled = true
};

// Run the view's session
SceneView.Session.Run(configuration, ARSessionRunOptions.ResetTracking);

// Find the ship and position it just in front of the camera
var ship = SceneView.Scene.RootNode.FindChildNode("ship", true);

ship.Position = new SCNVector3(2f, -2f, -9f);
```

응용 프로그램을 실행 하거나 다시 시작할 때마다 3D 모델이 카메라 앞에 배치 됩니다. 모델이 배치 되 면 카메라를 이동 하 고 ARKit가 모델을 배치 된 상태로 유지 합니다.

### <a name="5-pause-the-augmented-reality-session"></a>5. 확대 된 현실 세션 일시 중지

뷰 컨트롤러가 표시 되지 않는 경우 (`ViewWillDisappear` 메서드에서 ARKit 세션을 일시 중지 하는 것이 좋습니다.

```csharp
SceneView.Session.Pause();
```

## <a name="summary"></a>요약

위의 코드는 간단한 ARKit 응용 프로그램을 생성 합니다. 더 복잡 한 예제에서는 확대 된 현실 세션을 호스팅하는 뷰 컨트롤러에서 `IARSCNViewDelegate`를 구현 하 고 추가 메서드를 구현할 것으로 간주 합니다.

ARKit는 surface 추적 및 사용자 상호 작용과 같은 다양 한 고급 기능을 제공 합니다. UrhoSharp와 ARKit 추적을 결합 하는 예제는 [urhosharp 데모](urhosharp.md) 를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [확대 현실 (Apple)](https://developer.apple.com/arkit/)
- [UrhoSharp에서 ARKit 사용](urhosharp.md)
- [Jet (Simple ARKit) 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-arkitsample)
- [개체를 배치 하는 ARKit (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-arkitplacingobjects)
- [IOS에 대 한 ARKit-확대 현실 소개 (WWDC) (비디오)](https://developer.apple.com/videos/play/wwdc2017/602/)
