---
title: Xamarin.iOS에서 ARKit 2
description: 이 문서에서는 ARKit iOS 12에서에서 업데이트를 설명 합니다. 검색에 대 한 참조 개체 및 이미지를 사용 하 여에 중점을 두고, 환경 질감에 대 한 코드를 포함 하 고 ARKit 프로그래밍에서 일반적인 문제에 설명 합니다.
ms.prod: xamarin
ms.assetid: af758092-1523-4ab7-aa53-c37a81fb156a
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/22/2018
ms.openlocfilehash: 559ef9cc9a3e5ace7a4023e81363825c6861f6d4
ms.sourcegitcommit: 495680e74c72e7c570e68cde95d3d3643b1fcc8a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58870315"
---
# <a name="arkit-2-in-xamarinios"></a>Xamarin.iOS에서 ARKit 2

ARKit 작년 iOS 11에서에서 소개 된 이후로 크게 발전 합니다. 무엇 보다도, 검색할 수 있습니다 이제 가로 평면 뿐만 아니라 세로 실용성 실내 증강된 현실 환경을 크게 향상 됩니다. 또한 가지 새로운 기능

* 실제 세계와 디지털 이미지 간의 연결으로 참조 이미지 및 개체 인식
* 실제 조명을 시뮬레이션 하는 새로운 조명 모드
* 공유 하 고 AR 환경을 유지 하는 기능
* 새 파일 형식인 AR 콘텐츠를 저장 하기 위한 기본 설정

## <a name="recognizing-reference-objects"></a>개체 참조를 인식합니다.

한 쇼케이스 ARKit 2에서 기능은 참조 이미지 및 개체 인식 하는 기능입니다. 일반 이미지 파일에서 참조 이미지를 로드할 수 있습니다 ([나중에 설명할](#more-tracking-configurations)), 개체 검색 해야, 개발자 중심을 사용 하 여 참조 하지만 [ `ARObjectScanningConfiguration` ](xref:ARKit.ARObjectScanningConfiguration)합니다.

### <a name="sample-app-scanning-and-detecting-3d-objects"></a>샘플 앱: 검색 및 3D 개체를 검색 합니다.

[스캔 및 3D 개체를 감지](https://developer.xamarin.com/samples/monotouch/ios12/ScanningAndDetecting3DObjects/) 샘플은 포트의는 [Apple 프로젝트](https://developer.apple.com/documentation/arkit/scanning_and_detecting_3d_objects?language=objc) 보여 주는:

* 사용 하 여 응용 프로그램 상태 관리 [ `NSNotification` ](xref:Foundation.NSNotification) 개체
* 사용자 지정 시각화
* 복잡 한 제스처
* 개체 검색
* 저장 된 [`ARReferenceObject`](xref:ARKit.ARReferenceObject)

참조 개체를 검색 하면 배터리 및 프로세스 집약적 이며 이전 장치는 안정적인 추적을 달성 하는 데 문제가 경우가 많습니다.

### <a name="state-management-using-nsnotification-objects"></a>NSNotification 개체를 사용 하 여 상태 관리

이 응용 프로그램에서는 다음 상태 간에 전환 되는 상태 시스템을 사용 합니다.

* `AppState.StartARSession`
* `AppState.NotReady`
* `AppState.Scanning`
* `AppState.Testing`

또한 포함 된 상태 집합을 사용 하 고에서 전환 `AppState.Scanning`:

* `Scan.ScanState.Ready`
* `Scan.ScanState.DefineBoundingBox`
* `Scan.ScanState.Scanning`
* `Scan.ScanState.AdjustingOrigin`

상태 전환 알림을 게시 되는 반응 형 아키텍처를 사용 하는 앱 [ `NSNotificationCenter` ](xref:Foundation.NSNotificationCenter) 하 고 이러한 알림을 구독 합니다. 설치 프로그램에서이 코드 조각은 다음과 같습니다 `ViewController.cs`:

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

일반적인 알림 처리기를 UI를 업데이트 하 고 응용 프로그램 상태 개체를 검색할 때 업데이트 하는이 처리기와 같은 수정 됩니다.

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

마지막으로, `Enter{State}` 새 상태를 적절 하 게 모델 및 UX를 수정 하는 메서드:

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

검색 된 가로 평면으로 프로젝션 된 하위 수준 지점을"클라우드" 개체의 경계 상자 내에 포함 된 앱 표시 합니다.

이 지점 클라우드는 개발자에 게 제공 합니다 [ `ARFrame.RawFeaturePoints` ](xref:ARKit.ARFrame.RawFeaturePoints) 속성입니다. 지점 클라우드를 효율적으로 시각화는 까다로운 문제가 될 수 있습니다. 요소를 반복을 만들고 각 지점에 대 한 새 SceneKit 노드 배치는 종료할 프레임 속도입니다. 또는 비동기적으로 수행 하는 경우 지연 될 것입니다. 샘플은 세 부분으로 구성 전략을 사용 하 여 성능을 유지합니다.

* Pin의 데이터를 배치 하 고 원시 버퍼 바이트으로 데이터를 해석 하는 안전 하지 않은 코드를 사용 합니다.
* 변환에 원시 버퍼를 [ `SCNGeometrySource` ](xref:SceneKit.SCNGeometrySource) "템플릿"를 만들고 [ `SCNGeometryElement` ](xref:SceneKit.SCNGeometryElement) 개체.
* 신속 하 게 "만드므로" 원시 데이터 및 사용 하 여 템플릿을 [`SCNGeometry.Create(SCNGeometrySource[], SCNGeometryElement[])`](xref:SceneKit.SCNGeometry.Create(SceneKit.SCNGeometrySource[],SceneKit.SCNGeometryElement[]))

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

사용자 수 크기 조정, 회전 하 고 대상 개체를 둘러싸고 있는 경계 상자를 끌어 옵니다. 연결 된 제스처 인식기에서 흥미로운 두 가지입니다.

임계값; 전달 된 후에 활성화 된 제스처 인식기의 모든는 먼저 예를 들어, 손가락 많은 픽셀을 끌어가 또는 회전 일부 각도 초과 합니다. 기술은 다음과 같습니다. 임계값을 초과한 후 증분 방식으로 적용 될 때까지 이동 누적

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

제스처 관계에서 수행 되 고 두 번째 흥미로운 항목에 상대적인 실제 평면을 검색 하는 경계 상자를 이동 하는 방법이 있습니다. 이 항목에서 설명한 [Xamarin 블로그 포스트](https://blog.xamarin.com/exploring-new-ios-12-arkit-capabilities-with-xamarin/)합니다.

## <a name="other-new-features-in-arkit-2"></a>ARKit 2의 다른 새로운 기능

### <a name="more-tracking-configurations"></a>자세한 추적 구성

이제 사용할 수 있습니다 다음 중 하나를 기준으로 혼합 현실 환경.

* 장치가 속도계만 ([`AROrientationTrackingConfiguration`](xref:ARKit.AROrientationTrackingConfiguration), iOS 11)
* 얼굴 ([`ARFaceTrackingConfiguration`](xref:ARKit.ARFaceTrackingConfiguration), iOS 11)
* 이미지를 참조 ([`ARImageTrackingConfiguration`](xref:ARKit.ARImageTrackingConfiguration), iOS 12)
* 3D 개체를 검사 ([`ARObjectScanningConfiguration`](xref:ARKit.ARObjectScanningConfiguration), iOS 12)
* Visual 관성 odometry ([`ARWorldTrackingConfiguration`](xref:ARKit.ARWorldTrackingConfiguration)iOS 12에서에서 향상 된)

`AROrientationTrackingConfiguration`에 나오는 [이 블로그 게시물 및 F# 샘플](https://github.com/lobrien/FSharp_Face_AR), 가장 제한 되며만에 화면 및 장치를 연결 하지 않고 장치의 동작을 기준으로 디지털 개체를 배치 하는 대로 잘못 된 혼합 현실 환경을 제공 합니다. 실제 환경입니다.

`ARImageTrackingConfiguration` 실제 2D 이미지 (회화, 로고 등)를 인식 하 고 앵커 디지털 이미지에 사용할 수 있습니다.

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

이 구성에 흥미로운 측면을 두 가지 있습니다.

* 효율적인 되 고 잠재적으로 많은 수의 참조 이미지를 사용 하 여 사용할 수 있습니다.
* 실제 환경에서 해당 이미지를 이동 하는 경우에 이미지를 고정 되는 디지털 이미지 (예를 들어, 책의 표지 인식 되 면이 추적 책 쉽게 얻을 수 있는, 레이아웃 등 축소 끌어온 구성 인 것 처럼.).

`ARObjectScanningConfiguration` 에서 설명한 [이전에](#recognizing-reference-objects) 3D 개체를 검색 하는 것에 대 한 개발자 중심 구성이 있습니다. 항상는 프로세서 및 배터리 집약적인 최종 사용자 응용 프로그램에서 사용할 수 없습니다. 샘플 [3D 개체를 검색 하 고 검색](https://developer.xamarin.com/samples/monotouch/ios12/ScanningAndDetecting3DObjects/) 이 구성의 사용을 보여 줍니다.

마지막 추적 구성을 `ARWorldTrackingConfiguration` , 대부분의 혼합 현실 환경 자주 사용 되는 합니다. 이 구성은 실제 "기능 지점" 디지털 이미지와 관련 된 것 "visual 관성 odometry"를 사용 합니다. 디지털 기 하 도형 또는 스프라이트 실제 가로 및 세로 평면을 기준으로 고정 또는 기준으로 검색 `ARReferenceObject` 인스턴스. 이 구성에서는 세계 원점은 중력에 맞게 z 축 중심을 사용 하 여 공간에서 카메라의 원래 위치 및 디지털 개체 "그대로 유지" 실제 환경에서 개체를 기준으로 합니다.

### <a name="environmental-texturing"></a>환경 질감

캡처된 이미지를 사용 하 여 조명을 예측 하 고도 shiny 개체에 반사 하이라이트를 적용 하는 "환경 질감" ARKit 2를 지원 합니다. 환경 큐브 맵 동적으로 작성 됩니다 및 카메라 모든 방향으로 살펴보고 면 매우 현실적인 환경을 생성할 수 있습니다.

![환경 질감 기능 데모 이미지](images/arkit_env_texturing.png)

하려면 환경 질감을 사용 합니다.

* 프로그램 [ `SCNMaterial` ](xref:SceneKit.SCNMaterial) 개체를 사용 해야 합니다 [ `SCNLightingModel.PhysicallyBased` ](xref:SceneKit.SCNLightingModel.PhysicallyBased) 에 대 한 0 ~ 1 범위에 값을 할당 하 고 [ `Metalness.Contents` ](xref:SceneKit.SCNMaterial.Metalness) 고 [ `Roughness.Contents` ](xref:SceneKit.SCNMaterialProperty.Contents) 및
* 추적 구성을 설정 해야 합니다 [ `EnvironmentTexturing` ](xref:ARKit.ARWorldTrackingConfiguration.EnvironmentTexturing)  =  [AREnvironmentTexturing.Automatic'](xref:ARKit.AREnvironmentTexturing.Automatic) :

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

환경 질감 하는 것이 좋습니다 앞의 코드 조각에 표시 된 완벽 하 게 반사 질감 샘플의 재미 있는 경우에 해당 트리거 (질감은 어떤 카메라를 기준으로 추정만 "묘한 valley" 응답을 받지 않도록 지지대 사용 기록).


### <a name="shared-and-persistent-ar-experiences"></a>공유 및 영구 AR 환경

ARKit 2로 주 또한 합니다 [ `ARWorldMap` ](xref:ARKit.ARWorldMap) 공유 또는 전 세계 추적 데이터를 저장할 수 있는 클래스입니다. 현재 전 세계 맵에 사용 하 여 얻게 [ `ARSession.GetCurrentWorldMapAsync` ](xref:ARKit.ARSession.GetCurrentWorldMapAsync) 하거나 [ `GetCurrentWorldMap(Action<ARWorldMap,NSError>` ](xref:ARKit.ARSession.GetCurrentWorldMap(System.Action{ARKit.ARWorldMap,Foundation.NSError})) :

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

공유 또는 세계 지도 복원 합니다.

1. 파일에서 데이터를 로드 합니다.
2. 에 보관 해제는 `ARWorldMap` 개체
3. 에 대 한 값으로 사용 합니다 [ `ARWorldTrackingConfiguration.InitialWorldMap` ](xref:ARKit.ARWorldTrackingConfiguration.InitialWorldMap) 속성:

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

`ARWorldMap` 만 표시 되지 않는 세계 추적 데이터를 포함 하며 [ `ARAnchor` ](xref:ARKit.ARAnchor) 개체를 수행 _하지_ 디지털 자산을 포함 합니다. 기 하 도형 또는 이미지를 공유 하려면 사용 사례에 맞게 사용자 고유의 전략을 수립 해야 (저장/전송 위치 및 기 하 도형에 방향을 정적으로 적용 하 여 아마도 `SCNGeometry` 또는 저장/전송 하 여 serialize 된 개체)입니다. 이점은 합니다 `ARWorldMap` 자산을 한 번 공유에 상대적으로 배치 하는 `ARAnchor`, 장치 또는 세션 간에 일관 되 게 표시 됩니다.

### <a name="universal-scene-description-file-format"></a>범용 장면 설명 파일 형식

ARKit 2의 최종 헤드라인 기능은 Apple Pixar의 채택 [Universal 장면 Description](https://graphics.pixar.com/usd/docs/Introduction-to-USD.html) 파일 형식입니다. 이 형식은 공유 및 ARKit 자산을 저장 하기 위한 기본 형식으로의 Collada DAE 형식으로 대체 합니다. IOS 12 및 Mojave 자산을 시각화에 대 한 지원 기능이 있습니다. USDZ 파일 확장명은 USD 파일이 포함 된 압축 및 암호화 되지 않은 zip 보관 합니다. Pixar [USD 파일 작업에 대 한 도구를 제공](https://graphics.pixar.com/usd/docs/USD-Toolset.html#USDToolset-usdedit) 있지만 않습니다 아직 훨씬 타사 지원 합니다.

## <a name="arkit-programming-tips"></a>ARKit 프로그래밍 팁

### <a name="manual-resource-management"></a>수동 리소스 관리

ARKit 것 수동으로 리소스를 관리 하는 것이 중요 합니다. 이는 높은 프레임 속도 실제로 뿐만 아니라 _필요한_ "화면 고정 합니다."을 혼동 하지 않도록 ARKit 프레임 워크는 새 카메라 프레임을 제공 하는 방법에 대 한 지연 ([`ARSession.CurrentFrame`](xref:ARKit.ARSession.CurrentFrame)합니다. 현재까지 [ `ARFrame` ](xref:ARKit.ARFrame) 했습니다 `Dispose()` 것에서 호출 되 면 ARKit는 제공 하지 새 프레임! 이렇게 하면 앱의 나머지 부분 응답 이더라도 "고정" 비디오. 해결책은 언제 든 지 액세스할 `ARSession.CurrentFrame` 사용 하 여는 `using` 차단 하거나 수동으로 호출 `Dispose()` 에 있습니다.

파생 된 모든 개체 `NSObject` 됩니다 `IDisposable` 및 `NSObject` 구현 합니다 [Dispose 패턴](https://docs.microsoft.com/dotnet/standard/design-guidelines/dispose-pattern)이므로 일반적으로 따라야 [이 패턴을 구현 하기 위한 `Dispose` 에서 파생 된 클래스](https://docs.microsoft.com/dotnet/standard/garbage-collection/implementing-dispose)합니다.

### <a name="manipulating-transform-matrices"></a>변환 행렬을 조작

모든 3D 응용 프로그램에서 조밀 하 게 이동, 회전 하 고 3D 공간 개체를 기울일 방법에 설명 하는 4 x 4 변환 매트릭스를 사용 하 여 조사 하려는 합니다. 이들은, SceneKit [ `SCNMatrix4` ](xref:SceneKit.SCNMatrix4) 개체입니다.  

[ `SCNNode.Transform` ](xref:SceneKit.SCNNode.Transform) 속성에서 반환 합니다 `SCNMatrix4` 에 대 한 변형 매트릭스를 [ `SCNNode` ](xref:SceneKit.SCNNode) _에서 지 원하는 대로_ 행 중심 `simdfloat4x4` 형식. 따라서 예를 들어:

```csharp
var node = new SCNNode { Position = new SCNVector3(2, 3, 4) };  
var xform = node.Transform;
Console.WriteLine(xform);
// Output is: "(1, 0, 0, 0)\n(0, 1, 0, 0)\n(0, 0, 1, 0)\n(2, 3, 4, 1)"
```  

알 수 있듯이 위치는 맨 아래 행의 처음 세 개의 요소도 인코딩됩니다.

변환 행렬을 조작 하기 위한 일반적인 종류의 Xamarin `NVector4`, 규칙에 따라 이며 열 중심 방식으로 해석 됩니다. 즉, 변환/위치 구성 요소 M14 M24, M34, 없습니다 M41, M42, M43에 필요 합니다.

![행 중심 vs 열 중심](images/arkit_row_vs_column.png)

행렬 해석 선택 하 여 일치 하는 것이 적절 한 동작에 중요 합니다. 일관성 실수는 어떤 유형의 컴파일 타임 또는 런타임도 예외를 생성 하지 않습니다 3D 변환 행렬 4x4 되므로-작업이 예기치 않게 할 뿐입니다. 경우에 SceneKit / 멈출 수, 귀여운, 또는 지터 처럼 보이는 ARKit 개체는 잘못 된 변환 행렬을 가능성이 높습니다. 해결책은 간단 합니다. [ `NMatrix4.Transpose` ](xref:OpenTK.NMatrix4.Transpose*) 요소는 전체 변환 수행 됩니다.

## <a name="related-links"></a>관련 링크

- [샘플 앱-검색 및 3D 개체를 검색 합니다.](https://developer.xamarin.com/samples/monotouch/iOS12/ScanningAndDetecting3DObjects/)
- [ARKit 2 (WWDC 2018)의 새로운 기능](https://developer.apple.com/videos/play/wwdc2018/602/)
- [추적 이해 ARKit 및 검색 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/610/)
- [Xamarin.iOS에서 ARKit 소개](https://docs.microsoft.com/xamarin/ios/platform/introduction-to-ios11/arkit/)
