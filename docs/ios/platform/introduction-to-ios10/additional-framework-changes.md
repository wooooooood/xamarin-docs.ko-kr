---
title: "10 추가 iOS 프레임 워크 변경 내용"
description: "이 문서에서는 추가, 부 버전 변경 또는 10 iOS에 대 한 기존 프레임 워크의 향상 된 기능에 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0E2217F1-FC96-4D0A-ABAB-D40AD8F96502
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: fef543291f0743f8ebfa799b67fca2c8243be9bc
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="additional-ios-10-frameworks-changes"></a>10 추가 iOS 프레임 워크 변경 내용

_이 문서에서는 추가, 부 버전 변경 또는 10 iOS에 대 한 기존 프레임 워크의 향상 된 기능에 설명 합니다._

## <a name="av-foundation-framework-additions"></a>AV Foundation 프레임 워크 추가

AVFoundation 프레임 워크에는 다음과 같은 향상 기능이 포함 됩니다.

- 10 iOS 개발자 더 이상에 다른 구현 하려면 [AVPlayerItem](https://developer.xamarin.com/api/type/AVFoundation.AVPlayerItem/) 콘텐츠 형식을 기반으로 동작 합니다. 설정 하기만 `Rate` 속성과 AVFoundation 정지 하지 않고 재생을 위해 사용할 수 있는 충분 한 콘텐츠를 때 결정 됩니다.
- 새 [AVCapturePhotoOutput](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureFileOutput/) 클래스 대신 사용 되지 않는 `AVCaptureStillImageOutput` 클래스 및 복잡 한 제어를 제공 하 여 캡처 프로세스 모니터링을 모든 사진 워크플로 처리 하는 통합 된 방법을 제공 하 고 라이브 사진 및 원시 캡처 형식이 같은 새로운 기능에 대 한 지원.
- 새 `AVPlayerLooper` 클래스를 사용 하면 보다 쉽게 재생 하는 동안 지정된 된 부분 미디어에 루프 수 있습니다.
- `AVAssetDownloadURLSession` 다운로드에 대 한 클래스를 사용 하 고 FairPlay의 나중에 재생 HLS 스트림을 암호화 합니다.
- 기본적으로는 [AVCaptureSession](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureSession/) 장치 하드웨어 지원할 때 자동으로 클래스 와이드 색, 광범위 캡처 지원 합니다. Apple의를 참조 하십시오. [iOS 장치 호환성 참조](https://developer.apple.com/library/prerelease/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013599) 내용을 확인 합니다.

## <a name="avkit-additions"></a>AVKit 추가

AVKit 프레임 워크는 이제 새 포함 `UpdatesNowPlayingInfoCenter` 지금 재생 정보 센터를 업데이트 해야 하는 경우를 나타내는 속성입니다.

## <a name="core-data-enhancements"></a>핵심 데이터 향상 된 기능

iOS 10 핵심 데이터 프레임 워크는 다음과 같은 향상 기능이 포함 되어 있습니다.

- [NSManagedObjectContext](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectContext/) 이후를 가져오기 위해 특정 데이터베이스 버전을 관리 하는 개체 컨텍스트 (모드)에 고정 수 새 쿼리 생성 WAL 저널 모드 지원에서 SQLite 데이터 저장소를 사용 하 여 개체 기능 및 오류가 있는 트랜잭션.
- 루트 [NSManagedObjectContext](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectContext/) 동시 오류가 있는 및 serialization 없이 인출 개체 지원 합니다.
- [NSPersistentStoreCoordinator](https://developer.xamarin.com/api/type/CoreData.NSPersistentStoreCoordinator/) 클래스 SQLite 데이터 저장소의 풀을 관리 합니다.
- 에 추가 된 몇 가지 새로운 편의 메서드 `NSManagedObject` 쉽게 인출을 수행 하 고 하위 클래스를 만들 수 있습니다.
- 상위 수준를 사용 하 여 `NSPersistenceContainer` 참조에는 `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectModel/) 및 기타 핵심 데이터 구성 리소스입니다.

자세한 내용은 Apple의를 참조 하십시오 [핵심 데이터 프레임 워크 참조](https://developer.apple.com/reference/coredata)합니다.

## <a name="core-image-enhancements"></a>코어 이미지 향상

iOS 10 Core 이미지 프레임 워크의 다음과 같은 향상 된 기능을 사용 하면 됩니다.

- 이제 개발자 처리 전후 색 공간 축소 변환 하 여 작업 색 공간 Core 이미지 컨텍스트 외부에서 색 공간에서 이미지를 처리할 수 있습니다.
- A8 또는 A9 Cpu를 사용 하는 iOS 장치에 대 한 원시 이미지 형식이 이제 지원 됩니다. 원시 이미지 중 하나에서 기본 제공 iSight 카메라 디코딩 또는 제 3 자 카메라에서 core 이미지는 이제 지원을 합니다. 사용 하 여는 `FilterWithImageData` 또는 `FilterWithImageURL` 의 메서드는 [CIFilter](https://developer.xamarin.com/api/type/CoreImage.CIFilter/) 원시 이미지를 처리 하는 클래스입니다.
- 여러 렌더링 성능 개선 된 기능에 적용 된 `UIImage` (Core 이미지 이미지 저장소에서 지 원하는) 하는 경우의 렌더링 `UIImageView` 개체입니다. 
- `UIImage` 개체 태그가 지정 된 차원의 영역으로 광범위 한 색상의 렌더링 `UIImageView` 광범위 한 색을 지 원하는 iOS 장치에 있는 개체입니다.
- 코어 이미지 커널 코드가 특정 픽셀 출력 형식을 요청할 이제 수 있습니다.
- `ImageWithExtent` 의 메서드는 [CIFilter](https://developer.xamarin.com/api/type/CoreImage.CIFilter/) 필터 작업에 사용자 지정 처리를 삽입 하려면 클래스를 사용할 수 있습니다. Core 이미지 출력에 대 한 이미지를 처리할 때 필터 간에 지정 된 콜백을 호출 되거나 표시 됩니다.

또한 다음 새 Core 이미지 필터 추가 되었습니다.

- `CINinePartTiled`
- `CINinePartStretched`
- `CIHueSaturationValueGradient`
- `CIEdgePreserveUpsampleFilter`
- `CIClamp`

## <a name="core-motion-additions"></a>코어 동작 추가

새로운 iOS 10에 코어 동작 프레임 워크를 일시 중지 및 계속 실행 하는 동안 추적 사용자에 대 한 실시간 알림을 받도록 신속 하 고 앱을 사용 하면 pedometer 이벤트를 포함 합니다. 사용 하 여는 [CMPedometer](https://developer.xamarin.com/api/type/CoreMotion.CMPedometer/) 포그라운드 또는 백그라운드 pedometer 이벤트에 대 한 등록 합니다.

## <a name="foundation-enhancements"></a>Foundation 향상 된 기능

IOS 10의 Foundation 프레임 워크 같이 향상 되었습니다.:

- 새로운 [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) 최종 사용자에 게 표시 하는 것에 대 한 지역화 된 측정값의 서식을 지정 하려면 클래스입니다.
- 새로운 [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) 을 간격을 비교 하 고 간격 교차점에 대 한 테스트 기간, 같은 날짜 및 시간 간격 계산을 수행 하는 클래스입니다.
- 새로운 [NSMeasurement](https://developer.apple.com/reference/foundation/nsmeasurement) 간에 서로 다른 단위의 측정값 (UOM)으로 변환 하거나 다른 UOMs의 값에 대해 계산을 수행 하는 클래스입니다.

- 새로운 [NSUnit](https://developer.apple.com/reference/foundation/nsunit) 및 [NSDimension](https://developer.apple.com/reference/foundation/nsdimension) 특정 UOMs 나타내기 위한 클래스입니다.
- 몇 가지 새 속성에 추가 된는 [NSLocal](https://developer.apple.com/reference/foundation/nslocale) 로컬 정보 및 사용 가능한 표시 형식을 얻으려고 하는 클래스입니다.

## <a name="gamekit-enhancements"></a>GameKit 향상 된 기능

IOS 10에서에서 GameKit 프레임 워크 같이 향상 되었습니다.:

- **게임 센터 앱** 사용 되지 않으며 iOS에서 제거 되었습니다. 응용 프로그램 GameKit를 사용 하는 경우 그 _해야_ 순위표 등과 같은 GameKit 기능을 표시 하는 자체 인터페이스를 제공 합니다. 
- 새 iCloud 전용 계정 유형을 구현한 다음에 여는 [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) 클래스입니다.
- 새 [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) 클래스는 게임 센터에서 영구 데이터 저장소 관리를 위한 일반화 된 솔루션을 제공 합니다. `GKGameSession` 플레이어의 목록을 유지 관리 하 고 응용 프로그램을 구현 하기 위한 방법 및 시기 참가자 날짜 저장, 검색 또는 플레이어 사이 교환 합니다. 대부분의 경우에서 게임 세션 기존 턴 기반 일치 항목, 실시간 일치 또는 영구 게임 메서드 저장 바꿀 수 있습니다.

## <a name="gameplaykit-enhancements"></a>GameplayKit 향상 된 기능

IOS 10에서에서 GameplayKit 프레임 워크 같이 향상 되었습니다.:

- 새로운 [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) 고성능, 자연 스러운 경로 제공 하는 클래스입니다.
- 절차적 노이즈 세대 개가 추가 되 고 자연 스러운 한 질감에서 현실감을 높일 카메라 이동을 현실감을 추가 하 고 풍부한 게임 세계를 생성 하는 데 도움이 데 사용할 수 있습니다.
- 데이터 분할 하는 게임 세계 효율적인 검색을 위해 공간 분할을 사용 합니다.
- 새 몬테카를로 전략가 ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) 완전 한 목록이 가능한 이동 계산을 위해 추가 되었습니다.
- 기존 에이전트 및 new를 사용 하는 경로 찾기 동작에 3D 지원이 추가 되었습니다 [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) 및 [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) 클래스입니다.
- 새 [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) 및 [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) 클래스 만들기 GameplayKit 및 그 어느 때 보다 쉽게 SpriteKit 결합 합니다.
- 새로운 의사 결정 트리 API가 추가 되었습니다 ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) 및 [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) 게임 작성 AI 향상을 위해.

## <a name="healthkit-enhancements"></a>HealthKit 향상 된 기능

IOS 10에서에서 HealthKit 프레임 워크 같이 향상 되었습니다.:

- 날씨 형식에 대 한 새 메타 데이터 키가 추가 되었기 (같은 `HKWeatherConditionClear` 및 `HKWeatherConditionCloudy`) 및 워크 아웃 형식 (같은 `HKWorkoutActivityTypeFlexibility` 및 `HKWorkoutActivityTypeWheelchairRunPace`) 추가 되었습니다.
- 새 `HKCDADocument` 임상 문서 아키텍처 (CDA)를 나타내기 위해 문서 서식이 지정 된 클래스가 추가 되었습니다.
- 새로운 [HKWorkoutConfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration) 지정 하는 클래스는 `ActivityType` 및 `LocationType` 는 워크 아웃의 합니다.
- 새 [HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject) 및 `WheelchairUse` 의 메서드는 [HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore) 클래스 장애 자용 사용 하기 위한 추가 된 관련 상태 데이터입니다.

## <a name="homekit-enhancements"></a>HomeKit 향상 된 기능

IOS 10에서에서 HomeKit 프레임 워크 같이 향상 되었습니다.:

- 새 서비스와 특성이 추가 되었습니다.
- IPad, 액세서리 원격 액세스를 제공 하 고, 자동화 트리거를 실행 하 고, 사용 하도록 설정 하려면 HomeKit 허브 공유 사용자 권한을 작동 하도록 구성할 수 있습니다.
- 카메라 및 doorbell accessories에 대 한 지원이 추가 되었습니다.
- Accessories에 대 한 자세한 컨텍스트 및 구성을 제공 되었습니다.

참조 하십시오 우리의 [HomeKit 소개](~/ios/platform/homekit.md) 자세한 정보에 대 한 설명서입니다.

## <a name="metal-enhancements"></a>금속 향상 된 기능

IOS 10에에서는 금속 프레임 워크 같이 향상 되었습니다.:

- 3D 앱과 게임 이제 사용할 수 _공간 분할_ 복잡 한 장면와 GPU 통해 기 하 도형을 효율적으로 렌더링 합니다.
- 힙 리소스를 사용 하 여 앱 및 Memoryless 렌더링 대상을 기반으로 하는 금속의 성능을 최적화 하기 위해 리소스 할당의 세분화 된 제어를 제공 합니다.
- 함수 특수화를 사용 하 여 자재 및 밝은 조합 기능은 장면에 대 한 최적화 된 컬렉션을 만듭니다.

자세한 내용은 Apple의를 참조 하십시오 [금속 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221)합니다.

## <a name="modelio-enhancements"></a>ModelIO 향상 된 기능

IOS 10에서에서 ModelIO 프레임 워크 같이 향상 되었습니다.:

 - 이제 USD 파일 형식이 지원 됩니다.
 - 서명 지원이 추가 되었습니다 거리 필드는 [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) 클래스입니다.
 - 새로운 `MDLLightProbeIrradianceDataSource` Light 프로브 배치에서 지 원하는 클래스를 합니다.
 - 새로운 `MDLMaterialPropertyGraph` 클래스를 쉽게 런타임에서 지 원하는 모델을 변경 합니다.

## <a name="photos-enhancements"></a>사진 향상 된 기능

IOS 10에서에서 사진 프레임 워크 같이 향상 되었습니다.:

- 사용 하 여는 [CIImageProcessorInput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput) 및 [CIImageProcessorOutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput) 편집 작업을 수행 하는 새로운 Core 이미지 프로세서 기능을 활용 하는 클래스입니다.
- 라이브 사진 편집은 이제 사진 프레임 워크를 지 원하는 앱 하며 사진 확장 편집 (사진 및 카메라 내부에서 사용 하기 위해 응용 프로그램).
- 새로운 [PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext) 비디오와 여전히 사진 라이브 콘텐츠를 편집 내용을 적용 하는 클래스입니다.

## <a name="replaykit-enhancements"></a>ReplayKit 향상 된 기능

IOS 10에서에서 ReplayKit 프레임 워크 같이 향상 되었습니다.:

- 사용 하 여는 [RPScreenRecorder](https://developer.apple.com/reference/replaykit/rpscreenrecorder), [RPBroadcastActivityViewController](https://developer.apple.com/reference/replaykit/rpbroadcastactivityviewcontroller) 및 [RPBroadcastController](https://developer.apple.com/reference/replaykit/rpbroadcastcontroller) 의 브로드캐스트를 지 원하는 클래스를 통해 타사 미디어 기록 사이트입니다.
- 응용 프로그램에서 ReplayKit 3rd 파티 브로드캐스트 서비스를 지 원하는 데 필요한 브로드캐스트 UI 및 브로드캐스트 업로드 확장 합니다.

## <a name="scenekit-enhancements"></a>SceneKit 향상 된 기능

IOS 10에서에서 SceneKit 프레임 워크 같이 향상 되었습니다.:

- [SCNCamera](https://developer.xamarin.com/api/type/SceneKit.SCNCamera/) 클래스 HDR 기능과 효과 사용 하 여 큰 현실감을 제공할 수 있습니다. 자동 효과 사용 비네팅 또는 색 번짐과 및 게임에 fillmatic 효과 추가 하려면 그레이 딩 색 만들려면 적응 노출을 사용 합니다.
- 이제 SceneKit 간단 자산 작성 좀 더 현실적인 결과 대 한 새 물리적으로 기반 렌더링 (PBR) 시스템을 포함합니다.
- 새로운 [SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) 세 개의 기본 속성을 사용 하면서 다양 한 범위의 실제 음영 효과 제품 모델 음영 (`Diffuse`, `Metalness` 및 `Roughness`).
- 이후 PBR 조명 환경을 기반으로 가장 잘 작동을 음영을 사용 하 여는 `LightingEnvironment` 이미지 기반 조명 전체 장면에 할당할 속성입니다.
- 사용 하 여는 `IESProfileURL` 강도 (루멘)에서 색 온도 (켈빈) 등의 실제 값을 기반으로 하는 조명을 정의 하는 실제 밝은 설비를 가져올 속성을 합니다.
- PBR와 HDR 카메라 기능이 기존의 렌더링 기술 보다 나은 결과 제공 하 고, 결과적으로, SceneKit 지금 수행 모든 색 계산 선형 색 공간 (와이드 색 장치 디스플레이에 P3 색 영역 사용).
- SceneKit 지금 색 색 프로필 정보를 참조 하 여 모든 색 일치 합니다.
- SceneKit 모든 셰이더 유형에 대 한 선형 RGB 색 공간에서 색상 구성 요소 값을 해석합니다.
- 선형 둘 다 색 공간 렌더링 및 와이드 색을 지정 하 여 해제할 수 있습니다는 `SCNDisableLinearSpaceRendering` 및 `SCNDisableWideGamut` 의 응용 프로그램의 키 `Info.plist`합니다.
- 빌드 (또는 파일에서 로드 한 프로그래밍 방식으로 생성) 임의의 다각형 볼 새 geometry를 지정 하려면 [SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon) 클래스입니다.
- SceneKit 읽고 질감 이미지의 색상 프로필 정보에 대 한 조정를 하므로이 정보는 제공 하려면 모든 이미지에 대 한 자산 카탈로그를 사용 합니다.

## <a name="spritekit-enhancements"></a>SpriteKit 향상 된 기능

IOS 10에서에서 SpriteKit 프레임 워크 같이 향상 되었습니다.:

- 사용자 지정 셰이더 특성을 제공할 수 있습니다 (`SKAttribute`) 특성 값을 제공 하 여 셰이더를 사용 하는 각 노드에 별도로 구성할 수 있는 (`SKAttributeValue`).
- Tilemaps 2D, 2.5 D 및 사용 하 여 측 스크롤 게임에 대 한 사각형, 6 각형 및 아이소메트릭 타일 셰이프는 이제 지원는 `SKTileMapMode`, `SKTileGroup`, `SKTileGroupRule` 및 `SKTileSet` 클래스입니다.
- 새로운 `SKWarpGeometry` 을 늘이거나 왜곡 클래스 [SKSpriteNode](https://developer.xamarin.com/api/type/SpriteKit.SKSpriteNode/) 또는 [SKEffectNode](https://developer.xamarin.com/api/type/SpriteKit.SKEffectNode/) 렌더링 합니다. 새 [SKAction](https://developer.xamarin.com/api/type/SpriteKit.SKAction/) 클래스를 사용 하 여 warp 효과 간의 전환 애니메이션 효과를 줄 수 있습니다.
- [SKView](https://developer.xamarin.com/api/type/SpriteKit.SKView/) 클래스 장면의 렌더링 되는 시기와 방법을 세부적으로 제어할 수 있도록 몇 가지 새로운 메서드를 제공 합니다.

## <a name="scrollview-enhancements"></a>ScrollView 향상 된 기능

10.3 iOS에서 ScrollView 컨트롤 같이 향상 되었습니다.:

- `UIScrollView` 이제 포함는 `IndexDisplayMode` 속성으로는 사용자가 스크롤 하는 동안 인덱스 어떻게 표시를 제어 하는 `UIScrollViewIndexDisplayMode` 의:
    - `Automatic` -인덱스 디스플레이 운영 체제에서 제어 됩니다.
    - `AlwaysHidden` -인덱스 디스플레이 항상 숨겨집니다.

참조는 [iOSTenThree 샘플](https://developer.xamarin.com/samples/monotouch/iOS10/iOSTenThree) 사용에 대 한 합니다.

## <a name="uikit-enhancements"></a>UIKit 향상 된 기능

IOS 10에서에서 UIKit 프레임 워크 같이 향상 되었습니다.:

- 새 [UIPasteboard](https://developer.xamarin.com/api/type/UIKit.UIPasteboard/) API (예: 수명 제한) 새 옵션을 제공 하며 일반적인 클래스 형식에 대해 호환 되는 콘텐츠 유형을 자동으로 선언 됩니다.
- 새 완전 한 대화형, 개체 기반, 인터럽트 가능한 애니메이션 추가 된 지원과 제스처에 연결할 수 있습니다. Pleas 참조 Apple의 [UIViewAnimating 프로토콜 참조](https://developer.apple.com/reference/uikit/uiviewanimating), [UIViewPropertyAnimator 클래스 참조](https://developer.apple.com/reference/uikit/uiviewpropertyanimator), [UITimingCurveProvider 프로토콜 참조](https://developer.apple.com/reference/uikit/uitimingcurveprovider), [UICubicTimingParameters 클래스 참조](https://developer.apple.com/reference/uikit/uicubictimingparameters) 및 [UISpringTimingParameter 클래스 참조](https://developer.apple.com/reference/uikit/uispringtimingparameters) 자세한 정보에 대 한 합니다.
- 새 `UIPreviewInteraction` 및 `UIPreviewInteractionDelegate` 개발자 앱이 미리 보기 및 pop 작업에 대 한 사용자 지정 인터페이스를 제공할 수 있게 합니다.
- 새 `UIAccessibilityCustomRotor` 클래스를 사용 하면 응용 프로그램에서 사용자 지정, 상황에 맞는 기능을 통해 음성 같은 보조 기술의을 제공 합니다.
- 사용 하 여는 `UIAccessibilityIsAssistiveTouchRunning` 및 `UIAccessibilityAssistiveTouchStatusDidChangeNotification` AssistiveTouch을 사용 하는지 확인 하는 기호입니다.
- 사용 하 여는 `UIAccessibilityHearingDevicePairedEar` 및 `UIAccessibilityHearingDevicePairedEarDidChangeNotification` 의 상태를 가져오기 위해 기호 쌍이 MFi 청각 유용 합니다.
- 를 지원 하려면 동적 형식 레이블에 텍스트 필드 및 입력란 사용 새 `PreferredFontForTextStyle` 의 메서드는 `UIFont` 클래스입니다.
- 요소 해당 글꼴을 업데이트 해야 하는 경우를 결정 하 때 장치의 `UIContentSizeCategory` 변경 내용을 사용 하 여는 `AdjustsFontForContentSizeCategory` 의 속성은 `UIContentSizeCategoryAdjusting` 위임 합니다.
- `OpenURL` 의 메서드는 `UIApplication` 클래스 비동기적으로 호출 되 고 이제 열기 작업이 완료 된 후에 호출 되는 완료 처리기를 지원 합니다.
- CloudKit 공유를 시작 하 고 새를 사용 하 여 해당 속성을 수정할 `UICloudSharingController` 및 `UICloudSharingControllerDelegate` 클래스입니다.
- 프리페치된 셀의 스크롤 환경을 개선 하기 위해 이용 `UICollectionViews` 새 `UICollectionViewDataSourcePrefetching` 위임 합니다.
- 개발자 탭 모음 항목 (예: 텍스트 및 배경 색)에 대 한 배지의 모양을 이제 제어할 수 있습니다.
- 새로 고침 이제 모든 스크롤 보기 및 스크롤 뷰 하위 클래스에서 지원 됩니다 (예: `UICollectionView`).

## <a name="webkit-enhancements"></a>WebKit 향상 된 기능

IOS 10에서에서 WebKit 프레임 워크 같이 향상 되었습니다.:

- 피크 (peek) 및 pop 지원에 추가 된는 `WKWebView` 클래스입니다. 사용 하 여는 `ShouldPreviewElement` 메서드를 지정 된 웹 보기 미리 보기를 표시 해야 하는 경우를 결정 합니다.


## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://developer.xamarin.com/samples/ios/iOS10/)
- [IOS 10의에서 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewIniOS/Articles/iOS10.html#//apple_ref/doc/uid/TP40017084-SW1)
