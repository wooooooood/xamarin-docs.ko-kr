---
title: Xamarin.ios의 ARKit 2
description: 이 문서에서는 iOS 12의 ARKit 업데이트에 대해 설명 합니다. 검색을 위해 참조 개체 및 이미지를 사용 하는 데 중점을 질감, 환경 코드를 포함 하 고 ARKit 프로그래밍의 일반적인 문제에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: af758092-1523-4ab7-aa53-c37a81fb156a
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/22/2018
ms.openlocfilehash: 4af5e7ea9c1d744cd3b5ea5444312ba68bfcea11
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70120444"
---
# <a name="arkit-2-in-xamarinios"></a>Xamarin.ios의 ARKit 2

IOS 11의 작년 출시 이후 ARKit가 상당히 발전 했습니다. 무엇 보다도 이제 수직 및 수평 비행기를 검색 하 여 실내 확대 현실 환경의 practicality을 크게 향상 시킬 수 있습니다. 또한 다음과 같은 새로운 기능이 있습니다.

- 실제 이미지와 디지털 이미지 간의 접합으로 참조 이미지 및 개체 인식
- 실제 조명을 시뮬레이트하는 새로운 조명 모드
- AR 환경을 공유 하 고 유지 하는 기능
- AR 콘텐츠를 저장 하는 데 선호 되는 새 파일 형식

## <a name="recognizing-reference-objects"></a>참조 개체 인식

ARKit 2의 한 가지 전시 기능은 참조 이미지 및 개체를 인식할 수 있는 기능입니다. 참조 이미지는 일반적인 이미지 파일 ([나중에 설명](#more-tracking-configurations))에서 로드 될 수 있지만 개발자 중심 [`ARObjectScanningConfiguration`](xref:ARKit.ARObjectScanningConfiguration)을 사용 하 여 참조 개체를 검색 해야 합니다.

### <a name="sample-app-scanning-and-detecting-3d-objects"></a>샘플 앱: 3D 개체 검색 및 검색

[3D 개체 검색 및 검색](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-scanninganddetecting3dobjects) 샘플은 다음을 보여 주는 [Apple 프로젝트](https://developer.apple.com/documentation/arkit/scanning_and_detecting_3d_objects?language=objc) 의 포트입니다.

- 개체를 사용 하 [`NSNotification`](xref:Foundation.NSNotification) 여 응용 프로그램 상태 관리
- 사용자 지정 시각화
- 복잡 한 제스처
- 개체 검색
- 저장[`ARReferenceObject`](xref:ARKit.ARReferenceObject)

참조 개체를 검색 하면 배터리를 사용 하 고 프로세서를 많이 사용 하 고 오래 된 장치에서 안정적인 추적을 구현 하는 데 문제가 있는 경우가 많습니다.

### <a name="state-management-using-nsnotification-objects"></a>NSNotification 개체를 사용 하 여 상태 관리

이 응용 프로그램은 다음 상태를 전환 하는 상태 시스템을 사용 합니다.

- `AppState.StartARSession`
- `AppState.NotReady`
- `AppState.Scanning`
- `AppState.Testing`

및에서는 다음과 같은 경우에 `AppState.Scanning`도 포함 된 상태 및 전환 집합을 사용 합니다.

- `Scan.ScanState.Ready`
- `Scan.ScanState.DefineBoundingBox`
- `Scan.ScanState.Scanning`
- `Scan.ScanState.AdjustingOrigin`

앱은에 [`NSNotificationCenter`](xref:Foundation.NSNotificationCenter) 상태 전환 알림을 게시 하 고 이러한 알림을 구독 하는 대응식 아키텍처를 사용 합니다. 설정은 `ViewController.cs`다음과 같습니다.

```csharp
// Configure notifications for application state changes
var notificationCenter = NSNotificationCenter.DefaultCenter;

notificationCenter.AddObserver(Scan.ScanningStateChangedNotificationName, State.ScanningStateChanged);
notificationCenter.AddObserver(ScannedObject.GhostBoundingBoxCreatedNotificationName, State.GhostBoundingBoxWasCreated);
notificationCenter.AddObserver(ScannedObject.GhostBoundingBoxRemovedNotificationName, State.GhostBoundingBoxWasRemoved);
notificationCenter.AddObserver(ScannedObject.BoundingBoxCreatedNotificationName, State.BoundingBoxWasCreated);
notificationCenter.AddObserver(BoundingBox.ScanPercentageChangedNotificationName, ScanPercentageChanged);
notificationCenter.AddObserver(BoundingBox.ExtentChangedNotificationName, BoundingBoxExtentChanged);
notificationCenter.AddObserver(BoundingBox.PositionChangedNotificationName, BoundingBoxPositionChanged);
notificationCenter.AddObserver(ObjectOrigin.PositionChangedNotificationName, ObjectOriginPositionChanged);
notificationCenter.AddObserver(NSProcessInfo.PowerStateDidChangeNotification, DisplayWarningIfInLowPowerMode);

```

일반적인 알림 처리기는 UI를 업데이트 하 고 개체를 검색할 때 업데이트 되는이 처리기와 같은 응용 프로그램 상태를 수정할 수 있습니다.

```csharp
private void ScanPercentageChanged(NSNotification notification)
{
    var pctNum = TryGet<NSNumber>(notification.UserInfo, BoundingBox.ScanPercentageUserKey);
    if (pctNum == null)
    {
        return;
    }
    double percentage = pctNum.DoubleValue;
    // Switch to the next state if scan is complete
    if (percentage >= 100.0)
    {
        State.SwitchToNextState();
    }
    else
    {
        DispatchQueue.MainQueue.DispatchAsync(() => navigationBarController.SetNavigationBarTitle($"Scan ({percentage})"));
    }
}

```

마지막으로 `Enter{State}` 메서드는 새 상태에 맞게 모델 및 UX를 수정 합니다.

```csharp
internal void EnterStateTesting()
{
    navigationBarController.SetNavigationBarTitle("Testing");
    navigationBarController.ShowBackButton(false);
    loadModelButton.Hidden = true;
    flashlightButton.Hidden = false;
    nextButton.Enabled = true;
    nextButton.SetTitle("Share", UIControlState.Normal);

    testRun = new TestRun(sessionInfo, sceneView);
    TestObjectDetection();
    CancelMaxScanTimeTimer();
}
```

### <a name="custom-visualization"></a>사용자 지정 시각화

앱은 검색 된 가로 평면으로 프로젝션 된 경계 상자 내에 포함 된 개체의 하위 수준 "지점 클라우드"를 표시 합니다.

이 지점 클라우드는 개발자가 [`ARFrame.RawFeaturePoints`](xref:ARKit.ARFrame.RawFeaturePoints) 속성에서 사용할 수 있습니다. 지점 클라우드를 효율적으로 시각화 하는 것은 까다로운 문제입니다. 점을 반복 하 고 각 점에 대 한 새 SceneKit 노드를 만들고 배치 하면 프레임 비율이 종료 됩니다. 또는 비동기적으로 수행 되는 경우에는 지연 시간이 발생 합니다. 이 샘플에서는 세 부분으로 구성 된 전략을 통해 성능을 유지 합니다.

- 안전 하지 않은 코드를 사용 하 여 데이터를 제자리에 고정 하 고 데이터를 원시 버퍼 (바이트)로 해석 합니다.
- 해당 원시 버퍼를로 [`SCNGeometrySource`](xref:SceneKit.SCNGeometrySource) 변환 하 고 "template" [`SCNGeometryElement`](xref:SceneKit.SCNGeometryElement) 개체를 만듭니다.
- 를 사용 하 여 원시 데이터와 템플릿을 빠르게 "중철"[`SCNGeometry.Create(SCNGeometrySource[], SCNGeometryElement[])`](xref:SceneKit.SCNGeometry.Create(SceneKit.SCNGeometrySource[],SceneKit.SCNGeometryElement[]))

```csharp
internal static SCNGeometry CreateVisualization(NVector3[] points, UIColor color, float size)
{
  if (points.Length == 0)
  {
    return null;
  }

  unsafe
  {
    var stride = sizeof(float) * 3;

    // Pin the data down so that it doesn't move
    fixed (NVector3* pPoints = &amp;points[0])
    {
      // Important: Don't unpin until after `SCNGeometry.Create`, because geometry creation is lazy

      // Grab a pointer to the data and treat it as a byte buffer of the appropriate length
      var intPtr = new IntPtr(pPoints);
      var pointData = NSData.FromBytes(intPtr, (System.nuint) (stride * points.Length));

      // Create a geometry source (factory) configured properly for the data (3 vertices)
      var source = SCNGeometrySource.FromData(
        pointData,
        SCNGeometrySourceSemantics.Vertex,
        points.Length,
        true,
        3,
        sizeof(float),
        0,
        stride
      );

      // Create geometry element
      // The null and bytesPerElement = 0 look odd, but this is just a template object
      var template = SCNGeometryElement.FromData(null, SCNGeometryPrimitiveType.Point, points.Length, 0);
      template.PointSize = 0.001F;
      template.MinimumPointScreenSpaceRadius = size;
      template.MaximumPointScreenSpaceRadius = size;

      // Stitch the data (source) together with the template to create the new object
      var pointsGeometry = SCNGeometry.Create(new[] { source }, new[] { template });
      pointsGeometry.Materials = new[] { Utilities.Material(color) };
      return pointsGeometry;
    }
  }
}
```

결과 다음과 같습니다.

![point_cloud](images/arkit_point_cloud.jpeg)

### <a name="complex-gestures"></a>복잡 한 제스처

사용자는 대상 개체를 둘러싸는 경계 상자를 확장 하 고 회전 하 고 끌 수 있습니다. 연결 된 제스처 인식기에는 두 가지 흥미로운 항목이 있습니다.

첫째, 모든 제스처 인식기는 임계값이 전달 된 후에만 활성화 됩니다. 예를 들어 손가락으로 많은 픽셀을 끌거나 회전이 일부 각도를 초과 합니다. 이 기술은 임계값이 초과 될 때까지 이동을 누적 한 후 증분 방식으로 적용 하는 것입니다.

```csharp
// A custom rotation gesture recognizer that fires only when a threshold is passed
internal partial class ThresholdRotationGestureRecognizer : UIRotationGestureRecognizer
{
    // The threshold after which this gesture is detected.
    const double threshold = Math.PI / 15; // (12°)

    // Indicates whether the currently active gesture has exceeded the threshold
    private bool thresholdExceeded = false;

    private double previousRotation = 0;
    internal double RotationDelta { get; private set; }

    internal ThresholdRotationGestureRecognizer(IntPtr handle) : base(handle)
    {
    }

    // Observe when the gesture's state changes to reset the threshold
    public override UIGestureRecognizerState State
    {
        get => base.State;
        set
        {
            base.State = value;

            switch(value)
            {
                case UIGestureRecognizerState.Began :
                case UIGestureRecognizerState.Changed :
                    break;
                default :
                    // Reset threshold check
                    thresholdExceeded = false;
                    previousRotation = 0;
                    RotationDelta = 0;
                    break;
            }
        }
    }

    public override void TouchesMoved(NSSet touches, UIEvent evt)
    {
        base.TouchesMoved(touches, evt);

        if (thresholdExceeded)
        {
            RotationDelta = Rotation - previousRotation;
            previousRotation = Rotation;
        }

        if (! thresholdExceeded && Math.Abs(Rotation) > threshold)
        {
            thresholdExceeded = true;
            previousRotation = Rotation;
        }
    }
}
```

제스처와 관련 하 여 수행 되는 두 번째 흥미로운 작업은 검색 된 실제 평면에 대 한 경계 상자를 이동 하는 방법입니다. 이 측면은 [이 Xamarin 블로그 게시물](https://blog.xamarin.com/exploring-new-ios-12-arkit-capabilities-with-xamarin/)에 설명 되어 있습니다.

## <a name="other-new-features-in-arkit-2"></a>ARKit 2의 다른 새로운 기능

### <a name="more-tracking-configurations"></a>추가 추적 구성

이제 혼합 현실 환경의 기반으로 다음 중 하나를 사용할 수 있습니다.

- 장치만가 속도계 ([`AROrientationTrackingConfiguration`](xref:ARKit.AROrientationTrackingConfiguration), iOS 11)
- 얼굴 ([`ARFaceTrackingConfiguration`](xref:ARKit.ARFaceTrackingConfiguration), iOS 11)
- 참조 이미지 ([`ARImageTrackingConfiguration`](xref:ARKit.ARImageTrackingConfiguration), iOS 12)
- 3d 개체 스캔 ([`ARObjectScanningConfiguration`](xref:ARKit.ARObjectScanningConfiguration), iOS 12)
- 시각적 관성 odometry ([`ARWorldTrackingConfiguration`](xref:ARKit.ARWorldTrackingConfiguration), iOS 12에서 향상 됨)

`AROrientationTrackingConfiguration`[이 블로그 게시물 F# 및 샘플](https://github.com/lobrien/FSharp_Face_AR)에서 설명 하는는 장치 및 화면을 실제 환경에 연결 하지 않고도 장치의 움직임을 기준으로 디지털 개체만 배치 하므로 가장 제한적 이며 매우 제한적인 혼합 현실 환경을 제공 합니다.

에서는 `ARImageTrackingConfiguration` 실제 2d 이미지 (paintings, 로고 등)를 인식 하 고 디지털 이미지를 고정 하는 데 사용할 수 있습니다.

```csharp
var imagesAndWidths = new[] {
    ("cover1.jpg", 0.185F),
    ("cover2.jpg", 0.185F),
     //...etc...
    ("cover100.jpg", 0.185F),
};

var referenceImages = new NSSet<ARReferenceImage>(
    imagesAndWidths.Select( imageAndWidth =>
    {
      // Tuples cannot be destructured in lambda arguments
        var (image, width) = imageAndWidth;
        // Read the image
        var img = UIImage.FromFile(image).CGImage;
        return new ARReferenceImage(img, ImageIO.CGImagePropertyOrientation.Up, width);
    }).ToArray());

configuration.TrackingImages = referenceImages;
```

이 구성에는 다음과 같은 두 가지 흥미로운 측면이 있습니다.

- 효율적 이며 잠재적으로 많은 수의 참조 이미지와 함께 사용할 수 있습니다.
- 디지털 이미지는 이미지에 고정 되어 있습니다. 예를 들어 책의 덮개가 인식 되는 경우 책의 덮개가 인식 되 면 책을 추적 하 여 책을 추적 합니다.

는 `ARObjectScanningConfiguration` [이전](#recognizing-reference-objects) 에 설명 되었으며 3d 개체를 검색 하기 위한 개발자 중심 구성입니다. 프로세서 및 배터리 집약적 이며 최종 사용자 응용 프로그램에서 사용 하면 안 됩니다. [3D 개체 검색 및 검색](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-scanninganddetecting3dobjects) 샘플에서는이 구성을 사용 하는 방법을 보여 줍니다.

최종 추적 구성 `ARWorldTrackingConfiguration` ()은 대부분의 혼합 현실 환경에서 근무 하는 것입니다. 이 구성은 "시각적 관성 odometry"을 사용 하 여 실제 "기능 요소"를 디지털 이미지에 연결 합니다. 디지털 geometry 또는 스프라이트는 실제 가로 및 세로 평면 또는 검색 `ARReferenceObject` 된 인스턴스를 기준으로 고정 됩니다. 이 구성에서 세계는 Z 축이 중력에 맞춰진 카메라의 원래 위치 이며 디지털 개체는 실제 세계의 개체를 기준으로 "그대로 유지" 됩니다.

### <a name="environmental-texturing"></a>환경 질감

ARKit 2는 캡처된 이미지를 사용 하 여 조명을 예측 하 고 빛나는 개체에 반사 하이라이트를 적용 하는 "환경 질감"를 지원 합니다. 환경 큐브 맵는 동적으로 구축 되며, 카메라가 모든 방향에서 살펴본 후에는 더욱이 현실적인 환경을 만들 수 있습니다.

![환경 질감 데모 이미지](images/arkit_env_texturing.png)

환경 질감를 사용 하려면 다음을 수행 합니다.

- 개체가를 사용 [`SCNLightingModel.PhysicallyBased`](xref:SceneKit.SCNLightingModel.PhysicallyBased) 하 고 및에 대해 [`Metalness.Contents`](xref:SceneKit.SCNMaterial.Metalness) [`Roughness.Contents`](xref:SceneKit.SCNMaterialProperty.Contents) 0에서 1 사이의 값을 할당 해야 합니다. [`SCNMaterial`](xref:SceneKit.SCNMaterial)
- 추적 구성에서 다음을 [`EnvironmentTexturing`](xref:ARKit.ARWorldTrackingConfiguration.EnvironmentTexturing) 설정  =  [`AREnvironmentTexturing.Automatic`](xref:ARKit.AREnvironmentTexturing.Automatic) 해야 합니다.

```csharp
var sphere = SCNSphere.Create(0.33F);
sphere.FirstMaterial.LightingModelName = SCNLightingModel.PhysicallyBased;
// Shiny metallic sphere
sphere.FirstMaterial.Metalness.Contents = new NSNumber(1.0F);
sphere.FirstMaterial.Roughness.Contents = new NSNumber(0.0F);

// Session configuration:
var configuration = new ARWorldTrackingConfiguration
{
    PlaneDetection = ARPlaneDetection.Horizontal | ARPlaneDetection.Vertical,
    LightEstimationEnabled = true,
    EnvironmentTexturing = AREnvironmentTexturing.Automatic
};
```

위의 코드 조각에 표시 된 완벽 하 게 반사 된 질감이 샘플에서 재미 있지만, 환경 질감은 강화 목록와 함께 사용 하는 것이 좋습니다 .이는 "uncanny 유역" 응답을 트리거합니다 (질감이 카메라를 기준으로 하는 추정치 임). 기록 됨).


### <a name="shared-and-persistent-ar-experiences"></a>공유 및 영구 AR 환경

Arkit 2에 대 한 또 다른 주요 [`ARWorldMap`](xref:ARKit.ARWorldMap) 추가는 세계적인 추적 데이터를 공유 하거나 저장할 수 있도록 하는 클래스입니다. [`ARSession.GetCurrentWorldMapAsync`](xref:ARKit.ARSession.GetCurrentWorldMapAsync) [또는`GetCurrentWorldMap(Action<ARWorldMap,NSError>)`](xref:ARKit.ARSession.GetCurrentWorldMap(System.Action{ARKit.ARWorldMap,Foundation.NSError})) 를 사용 하 여 현재 세계 지도를 가져옵니다.

```csharp
// Local storage
var PersistentWorldPath => Environment.GetFolderPath(Environment.SpecialFolder.Personal) + "/arworldmap";

// Later, after scanning the environment thoroughly...
var worldMap = await Session.GetCurrentWorldMapAsync();
if (worldMap != null)
{
    var data = NSKeyedArchiver.ArchivedDataWithRootObject(worldMap, true, out var err);
    if (err != null)
    {
        Console.WriteLine(err);
    }
    File.WriteAllBytes(PersistentWorldPath, data.ToArray());
}
```

세계 지도를 공유 하거나 복원 하려면:

1. 파일에서 데이터를 로드 합니다.
2. `ARWorldMap` 개체에 보관 하지 않습니다.
3. 이 값을 [`ARWorldTrackingConfiguration.InitialWorldMap`](xref:ARKit.ARWorldTrackingConfiguration.InitialWorldMap) 속성의 값으로 사용 합니다.

```csharp
var data = NSData.FromArray(File.ReadAllBytes(PersistentWorldController.PersistenWorldPath));
var worldMap = (ARWorldMap)NSKeyedUnarchiver.GetUnarchivedObject(typeof(ARWorldMap), data, out var err);

var configuration = new ARWorldTrackingConfiguration
{
    PlaneDetection = ARPlaneDetection.Horizontal | ARPlaneDetection.Vertical,
    LightEstimationEnabled = true,
    EnvironmentTexturing = AREnvironmentTexturing.Automatic,
    InitialWorldMap = worldMap
};
```

에 `ARWorldMap` 는 볼 수 없는 세계 수준의 추적 데이터 [`ARAnchor`](xref:ARKit.ARAnchor) 와 개체만 포함 되며 디지털 자산은 포함 _되지_ 않습니다. 기 하 도형 또는 이미지를 공유 하려면 사용 사례에 적합 한 고유한 전략을 개발 해야 합니다. 즉, 기 하 도형 위치와 방향만 저장/전송 하 여 정적 `SCNGeometry` 으로 적용 하거나 저장/전송 하 여 적용할 수 있습니다. serialize 된 개체). 의 `ARWorldMap` 혜택은 공유 `ARAnchor`에 상대적으로 배치 된 자산이 장치나 세션 간에 일관 되 게 표시 된다는 것입니다.

### <a name="universal-scene-description-file-format"></a>유니버설 장면 설명 파일 형식

ARKit 2의 최종 헤드라인 기능은 Apple의 Pixar 세계 [장면 설명](https://graphics.pixar.com/usd/docs/Introduction-to-USD.html) 파일 형식을 도입 하는 것입니다. 이 형식은 ARKit 자산을 공유 하 고 저장 하기 위한 기본 형식으로 Collada의 DAE 형식을 바꿉니다. 자산 시각화 지원은 iOS 12 및 Mojave에 기본 제공 됩니다. USDZ 파일 확장명은 USD 파일을 포함 하는 압축 되지 않고 암호화 되지 않은 zip 보관 파일입니다. Pixar는 [USD 파일을 사용 하기 위한 도구를 제공](https://graphics.pixar.com/usd/docs/USD-Toolset.html#USDToolset-usdedit) 하지만 아직 타사를 지원 하지 않습니다.

## <a name="arkit-programming-tips"></a>ARKit 프로그래밍 팁

### <a name="manual-resource-management"></a>수동 리소스 관리

ARKit에서 리소스를 수동으로 관리 하는 것이 중요 합니다. 높은 프레임 속도를 허용 하는 것이 아니라 실제로 "화면이 중단" 하는 것을 방지 하는 _데 필요_ 합니다. ARKit 프레임 워크는 새 카메라 프레임 ()[`ARSession.CurrentFrame`](xref:ARKit.ARSession.CurrentFrame)을 제공 하는 데 지연 됩니다. 현재 [`ARFrame`](xref:ARKit.ARFrame) 이 ( `Dispose()` 가) 호출 될 때까지 arkit는 새 프레임을 제공 하지 않습니다. 이렇게 하면 응용 프로그램의 나머지 부분이 응답 하지 않더라도 비디오가 "고정" 됩니다. 솔루션은 항상 `using` 블록을 사용 `ARSession.CurrentFrame` 하 여 액세스 하거나 수동으로 호출 `Dispose()` 하는 것입니다.

에서 `NSObject` 파생 된 모든 개체 `IDisposable` `NSObject` 는 [Dispose 패턴](https://docs.microsoft.com/dotnet/standard/design-guidelines/dispose-pattern)을 구현 하므로 일반적으로 [파생 클래스에서를 구현 `Dispose` 하기 위해이 패턴](https://docs.microsoft.com/dotnet/standard/garbage-collection/implementing-dispose)을 따라야 합니다.

### <a name="manipulating-transform-matrices"></a>Transform 행렬 조작

3D 응용 프로그램에서 3D 공간을 통해 개체를 이동 하 고, 회전 하 고, 조밀 하는 방법을 설명 하는 4x4 변환 매트릭스를 다룰 것입니다. SceneKit에서 이러한 개체는 [`SCNMatrix4`](xref:SceneKit.SCNMatrix4) 개체입니다.  

속성 [`SCNNode.Transform`](xref:SceneKit.SCNNode.Transform) 은 행- `SCNMatrix4` [`SCNNode`](xref:SceneKit.SCNNode) 주`simdfloat4x4` 형식으로 _지원 되_ 는에 대 한 변환 매트릭스를 반환 합니다. 예를 들면 다음과 같습니다.

```csharp
var node = new SCNNode { Position = new SCNVector3(2, 3, 4) };  
var xform = node.Transform;
Console.WriteLine(xform);
// Output is: "(1, 0, 0, 0)\n(0, 1, 0, 0)\n(0, 0, 1, 0)\n(2, 3, 4, 1)"
```  

여기서 볼 수 있듯이 위치는 맨 아래 행의 처음 3 개 요소에서 인코딩됩니다.

Xamarin에서 변환 매트릭스 조작을 위한 공통 형식은 규칙에 따라 `NVector4`열 중심 방식으로 해석 되는입니다. 즉, M14, M24, M34, not M41, M42, M43에는 translation/position 구성 요소가 필요 합니다.

![행-주 및 열-주](images/arkit_row_vs_column.png)

행렬 해석을 선택 하는 것과 일치 하는 것은 적절 한 동작에 매우 중요 합니다. 3D transform 행렬은 4x4 이므로 일관성 실수는 컴파일 시간 또는 런타임 예외를 전혀 생성 하지 않습니다. 즉, 작업이 예기치 않게 동작 하는 것입니다. SceneKit/ARKit 개체가 고정, 날아가기 또는 지터로 보이는 것 처럼 보이는 경우 잘못 된 변환 매트릭스가 좋은 경우입니다. 솔루션은 간단 합니다. [`NMatrix4.Transpose`](xref:OpenTK.NMatrix4.Transpose*) 는 요소의 내부 transposition를 수행 합니다.

## <a name="related-links"></a>관련 링크

- [샘플 앱-3D 개체 검색 및 검색](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-scanninganddetecting3dobjects)
- [ARKit 2의 새로운 기능 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/602/)
- [ARKit 추적 및 검색 이해 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/610/)
- [Xamarin.ios의 ARKit 소개](https://docs.microsoft.com/xamarin/ios/platform/introduction-to-ios11/arkit/)
