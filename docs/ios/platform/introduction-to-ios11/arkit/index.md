---
title: "ARKit 소개"
description: "IOS 11에 대 한 보강 된 현실"
ms.topic: article
ms.prod: xamarin
ms.assetid: 70291430-BCC1-445F-9D41-6FBABE87078E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2016
ms.openlocfilehash: 1655d426a0181c88479eef2bdea909eb5935e642
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-arkit"></a>ARKit 소개

_IOS 11에 대 한 보강 된 현실_

ARKit 다양을 한 보고 확대 된 현실 응용 프로그램 및 게임을 수 있습니다. 이 단원에서는 다음 항목에 대해 설명합니다.

- [ARKit 시작](#gettingstarted)
- [UrhoSharp ARKit 사용](urhosharp.md)

<a name="gettingstarted" />

## <a name="getting-started-with-arkit"></a>ARKit 시작

보강 된 현실 시작 하려면 다음 지침 안내는 간단한 응용 프로그램: 3D 모델을 배치 하 고 있으므로 ARKit 모델 여 추적 기능을 그대로 유지 합니다.

![Jet 3D 모델 부동 카메라 이미지](images/jet-sml.png)

### <a name="1-add-a-3d-model"></a>1. 3D 모델 추가

자산을 사용 하 여 프로젝트에 추가 해야는 **SceneKitAsset** 빌드 작업입니다.

![프로젝트에서 SceneKit 자산](images/scene-assets.png)


### <a name="2-configure-the-view"></a>2. 보기를 구성 합니다.

뷰 컨트롤러에서 `ViewDidLoad` 메서드를 장면 자산을 로드 하 고 설정 된 `Scene` 보기에서 속성:

```csharp
ARSCNView SceneView = (View as ARSCNView);

// Create a new scene
var scene = SCNScene.FromFile("art.scnassets/ship");

// Set the scene to the view
SceneView.Scene = scene;
```

### <a name="3-optionally-implement-a-session-delegate"></a>3. 필요에 따라 세션 대리자 구현

간단한 경우에 필요 하지는 않지만 세션 대리자 구현 상태 ARKit 세션 (및 사용자에 게 피드백을 제공 하는 실제 응용 프로그램에서) 디버깅을 위해 유용할 수 있습니다. 아래 코드를 사용 하 여 간단한 위임을 만듭니다.

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

대리자에 할당 된에 `ViewDidLoad` 메서드:

```csharp
// Track changes to the session
SceneView.Session.Delegate = new SessionDelegate();
```

### <a name="4-position-the-3d-model-in-the-world"></a>4. 세계에서 3D 모델을 배치 합니다.

`ViewWillAppear`, 다음 코드에서는 ARKit 세션을 설정 하 고 장치의 카메라와 연관 된 공간에서 3D 모델의 위치를 설정 합니다.

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

응용 프로그램을 실행 하거나 다시 시작할 때마다 3D 모델 카메라 앞에 배치 됩니다. 모델이 배치 되 면 카메라 이동한 지켜봅니다 ARKit에 배치 하는 모델은 유지 합니다.

### <a name="5-pause-the-augmented-reality-session"></a>5. 확대 된 현실의 세션을 일시 중지

것이 좋습니다 뷰 컨트롤러 표시 된 경우 ARKit 세션을 일시 중지 (에 `ViewWillDisappear` 메서드:

```csharp
SceneView.Session.Pause();
```

## <a name="summary"></a>요약

위의 코드는 간단한 ARKit 응용 프로그램에서 발생합니다. 더 복잡 한 예제를 구현 하는 확대 된 현실의 세션을 호스트 하는 보기 컨트롤러 기대 `IARSCNViewDelegate`, 추가 메서드를 구현 하 고 있습니다.

ARKit 표면 추적 및 사용자 상호 작용 같은 보다 복잡 한 기능을 많이 제공합니다. 참조는 [UrhoSharp 데모](urhosharp.md) ARKit UrhoSharp를 사용 하 여 추적을 결합 하는 예입니다.


## <a name="related-links"></a>관련 링크

- [확대 된 현실의 (Apple)](https://developer.apple.com/arkit/)
- [UrhoSharp ARKit 사용](urhosharp.md)
- [간단한 ARKit (Jet) 샘플](https://developer.xamarin.com/samples/monotouch/ios11/ARKitSample/)
- [ARKit 배치 개체 (샘플)](https://developer.xamarin.com/samples/monotouch/ios11/ARKitPlacingObjects/)
- [IOS (WWDC) (비디오)에 대 한 보강 된 현실 ARKit-소개](https://developer.apple.com/videos/play/wwdc2017/602/)
