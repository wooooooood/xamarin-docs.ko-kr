---
title: Xamarin.iOS에서 ARKit 소개
description: 이 문서에는 확대 된 현실을 ARKit 사용 하 여 iOS 11에서에서 설명합니다. 앱에 3D 모델을 추가, 보기를 구성, 세션 대리자를 구현, 전 세계에서 3D 모델의 위치 및 증강된 현실 세션을 일시 중지 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 70291430-BCC1-445F-9D41-6FBABE87078E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2017
ms.openlocfilehash: 14bbb35477c098738e9cd7e2cb92154422d394ee
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350617"
---
# <a name="introduction-to-arkit-in-xamarinios"></a>Xamarin.iOS에서 ARKit 소개

_Ios 11의 확대 된 현실_

ARKit 확대 된 현실 응용 프로그램 및 게임의 광범위 한이 있습니다. 이 단원에서는 다음 항목에 대해 설명합니다.

- [ARKit 시작](#gettingstarted)
- [ARKit UrhoSharp 사용](urhosharp.md)

<a name="gettingstarted" />

## <a name="getting-started-with-arkit"></a>ARKit 시작

증강된 현실 시작 하려면 다음 지침을 단계별로 간단한 응용 프로그램: 3D 모델의 위치를 지정 하 고 해당 추적 기능을 사용 하 여 모델을 장소에 보관 하는 ARKit 수 있도록 합니다.

![카메라에서 이미지를 부동 jet 3D 모델](images/jet-sml.png)

### <a name="1-add-a-3d-model"></a>1. 3D 모델 추가

자산 사용 하 여 프로젝트에 추가 해야 합니다 **SceneKitAsset** 빌드 작업입니다.

![프로젝트에서 SceneKit 자산이](images/scene-assets.png)


### <a name="2-configure-the-view"></a>2. 보기 구성

뷰 컨트롤러에서 `ViewDidLoad` 메서드를 장면 자산을 로드 하 고 설정 된 `Scene` 보기에서 속성:

```csharp
ARSCNView SceneView = (View as ARSCNView);

// Create a new scene
var scene = SCNScene.FromFile("art.scnassets/ship");

// Set the scene to the view
SceneView.Scene = scene;
```

### <a name="3-optionally-implement-a-session-delegate"></a>3. 필요에 따라 세션 대리자 구현

간단한 경우에 필요 하지는 않지만는 세션 대리자 구현 ARKit 세션 (및 사용자에 게 피드백을 제공 하는 실제 응용 프로그램) 상태를 디버깅을 위해 유용할 수 있습니다. 아래 코드를 사용 하 여 간단한 대리자를 만듭니다.

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

대리자에 할당 된에서 `ViewDidLoad` 메서드:

```csharp
// Track changes to the session
SceneView.Session.Delegate = new SessionDelegate();
```

### <a name="4-position-the-3d-model-in-the-world"></a>4. 전 세계에서 3D 모델의 위치

`ViewWillAppear`, 다음 코드는 ARKit 세션을 설정 하 고 장치의 카메라를 기준으로 공간에서 3D 모델의 위치를 설정 합니다.

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

응용 프로그램을 실행 하거나 다시 시작 될 때마다 3D 모델 카메라 앞에 배치 됩니다. 모델이 배치 되 면 카메라를 이동 하 고 ARKit 배치 모델을 계속 지켜봅니다.

### <a name="5-pause-the-augmented-reality-session"></a>5. 증강된 현실 세션을 일시 중지

뷰 컨트롤러는 표시 되지 않습니다 ARKit 세션을 일시 중지 하는 것이 좋습니다 (에 `ViewWillDisappear` 메서드:

```csharp
SceneView.Session.Pause();
```

## <a name="summary"></a>요약

위의 코드는 간단한 ARKit 응용 프로그램에서 발생합니다. 더 복잡 한 예제를 구현 하는 확대 된 현실 세션을 호스트 하는 보기 컨트롤러 기대 `IARSCNViewDelegate`, 추가 메서드를 구현 하 고 있습니다.

ARKit 노출 추적 및 사용자 상호 작용 하는 등 보다 복잡 한 기능을 많이 제공합니다. 참조 된 [UrhoSharp 데모](urhosharp.md) ARKit UrhoSharp 사용 하 여 추적을 결합 하는 예입니다.


## <a name="related-links"></a>관련 링크

- [증강된 현실 (Apple)](https://developer.apple.com/arkit/)
- [ARKit UrhoSharp 사용](urhosharp.md)
- [간단한 ARKit (Jet) 샘플](https://developer.xamarin.com/samples/monotouch/ios11/ARKitSample/)
- [ARKit 배치 개체 (샘플)](https://developer.xamarin.com/samples/monotouch/ios11/ARKitPlacingObjects/)
- [ARKit-증강된 현실 iOS (WWDC) (비디오)에 대 한 소개](https://developer.apple.com/videos/play/wwdc2017/602/)
