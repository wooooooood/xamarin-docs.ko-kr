---
title: 추가 iOS 10 프레임 워크 변경
description: 이 문서는 사소한 변경 및 iOS 10에서에서 기존 프레임 워크에 대 한 향상 된 기능에 설명 하 고 확인 하는 방법에 설명 Xamarin.iOS에서 이러한 업데이트를 사용 합니다.
ms.prod: xamarin
ms.assetid: 0E2217F1-FC96-4D0A-ABAB-D40AD8F96502
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/29/2017
ms.openlocfilehash: 6ae08264218c8f959b351f059d73fc0aebfea39e
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50118086"
---
# <a name="additional-ios-10-frameworks-changes"></a>추가 iOS 10 프레임 워크 변경

_이 문서에서는 추가, 사소한 변경 하거나 iOS 10에 대 한 기존 프레임 워크의 향상 된 기능을 설명 합니다._

## <a name="av-foundation-framework-additions"></a>AV Foundation 프레임 워크 추가

AVFoundation 프레임 워크에는 다음과 같은 향상 기능이 포함 됩니다.

- Ios 10에서 개발자에 더 이상 다른 구현 [AVPlayerItem](https://developer.xamarin.com/api/type/AVFoundation.AVPlayerItem/) 콘텐츠 형식을 기반으로 동작 합니다. 설정 된 `Rate` 속성과 AVFoundation 상태일 없이 재생에 사용할 수 있는 충분 한 콘텐츠는 경우 결정 됩니다.
- 새 [AVCapturePhotoOutput](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureFileOutput/) 클래스 대신 사용 되지 않는 `AVCaptureStillImageOutput` 클래스 및 복잡 한 제어를 제공 하 고 캡처 프로세스 모니터링 하 여 모든 사진 워크플로 처리 하기 위한 통합 된 메서드를 제공 하 고 원시 캡처 형식과 사진 Live와 같은 새로운 기능을 지원 합니다.
- 새 `AVPlayerLooper` 클래스 쉽게 재생 하는 동안 지정 된 부분 미디어를 반복 합니다.
- `AVAssetDownloadURLSession` 다운로드에 대 한 클래스를 사용 하 고 나중에 재생 FairPlay의 HLS 스트림도 암호화 합니다.
- 기본적으로 [AVCaptureSession](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureSession/) 장치 하드웨어에서 지 원하는 경우 클래스 wide 색, 광범위 캡처를 자동으로 지원 합니다. Apple의를 참조 하세요 [iOS 장치 호환성 참조](https://developer.apple.com/library/prerelease/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013599) 대 한 자세한 내용은 합니다.

## <a name="avkit-additions"></a>AVKit 추가

AVKit 프레임 워크는 이제 새 포함 `UpdatesNowPlayingInfoCenter` 속성을 지금 재생 정보 센터를 업데이트 해야 하는 경우를 나타냅니다.

## <a name="core-data-enhancements"></a>핵심 데이터 개선 사항

iOS 10에는 핵심 데이터 프레임 워크에 다음과 같은 향상 기능이 포함 됩니다.

- 합니다 [NSManagedObjectContext](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectContext/) WAL 저널 모드 지원 새 쿼리 생성에에서 SQLite 데이터 저장소를 사용 하 여 개체 관리 되는 개체 컨텍스트 (MOC) 이후 페치 하기 위해 특정 데이터베이스 버전에 고정할 수 있습니다 위치 기능 및 오류가 있는 트랜잭션.
- 루트 [NSManagedObjectContext](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectContext/) 동시 오류 및 직렬화 없이 가져오는 개체를 지원 합니다.
- 합니다 [NSPersistentStoreCoordinator](https://developer.xamarin.com/api/type/CoreData.NSPersistentStoreCoordinator/) 클래스 SQLite 데이터 저장소의 풀을 관리 합니다.
- 에 추가 된 편리한 몇 가지 새로운 메서드 `NSManagedObject` 쉽게 페치를 수행 하 고 서브 클래스를 만들 수 있습니다.
- 사용 하 여 대략적 `NSPersistenceContainer` 참조를 `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectModel/) 및 다른 핵심 데이터 구성 리소스.

자세한 내용은 Apple의를 참조 하세요 [핵심 데이터 프레임 워크 참조](https://developer.apple.com/reference/coredata)합니다.

## <a name="core-image-enhancements"></a>이미지 향상 된 핵심 기능

iOS 10 Core 이미지 프레임 워크의 다음과 같은 향상 된 기능을 사용 하면 됩니다.

- 이제 개발자 처리 전후 색 공간에서 변환 하 여 색 공간을 Core 이미지 컨텍스트의 작업 색 공간 외부에서 이미지를 처리할 수 있습니다.
- A8 또는 A9 Cpu를 사용 하는 iOS 장치에 대 한 원시 이미지 형식이 이제 지원 됩니다. 기본 제공 iSight 카메라에서 원시 이미지를 디코딩 또는 타사 파티 카메라에서 이제 core 이미지는 지원 합니다. 사용 합니다 `FilterWithImageData` 또는 `FilterWithImageURL` 의 메서드는 [CIFilter](https://developer.xamarin.com/api/type/CoreImage.CIFilter/) 원시 이미지를 처리 하는 클래스입니다.
- 여러 렌더링 성능 향상 기능에 적용 된 `UIImage` (Core 이미지 이미지 저장소에서 지 원하는) 경우 렌더링 `UIImageView` 개체입니다. 
- `UIImage` 개체 태그가 지정 된 와이드 영역으로 광범위 한 색상에서 렌더링 됩니다 `UIImageView` 광범위 한 색을 지 원하는 iOS 장치에 있는 개체입니다.
- Core 이미지 커널 코드 출력 형식 특정 픽셀을 요청할 수 있습니다.
- 합니다 `ImageWithExtent` 메서드를 [CIFilter](https://developer.xamarin.com/api/type/CoreImage.CIFilter/) 필터 작업에 사용자 지정 처리를 삽입 하는 클래스를 사용할 수 있습니다. Core 이미지 출력에 대 한 이미지를 처리 하는 경우 필터 간에 지정 된 콜백을 호출 되거나 표시 됩니다.

또한 다음 새 Core 이미지 필터를 추가 되었습니다.

- `CINinePartTiled`
- `CINinePartStretched`
- `CIHueSaturationValueGradient`
- `CIEdgePreserveUpsampleFilter`
- `CIClamp`

## <a name="core-motion-additions"></a>핵심 동작 추가

새 ios 10, Core 동작 프레임 워크는 사용자가 일시 중지 하 고 실행 하는 동안 추적을 다시 시작에 대 한 실시간 알림을 받도록 신속 하 고 앱을 사용 하도록 설정 하는 pedometer 이벤트를 포함 합니다. 사용 된 [CMPedometer](https://developer.xamarin.com/api/type/CoreMotion.CMPedometer/) 포그라운드 또는 백그라운드 pedometer 이벤트에 대 한 등록 합니다.

## <a name="foundation-enhancements"></a>Foundation 향상 된 기능

IOS 10에 대 한 Foundation 프레임 워크 같이 향상 되었습니다.:

- 새 [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) 최종 사용자에 게 표시 하는 것에 대 한 지역화 된 측정값의 서식을 지정 하는 클래스입니다.
- 새 [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) 기간, 간격을 비교 하 고 간격 교차점에 대 한 테스트와 같은 날짜 및 시간 간격 계산을 수행 하는 클래스입니다.
- 새 [NSMeasurement](https://developer.apple.com/reference/foundation/nsmeasurement) 클래스 간에 서로 다른 단위의 측정값 (UOM)을 변환 하거나 다른 UOMs의 값에 대해 계산을 수행 합니다.

- 새 [NSUnit](https://developer.apple.com/reference/foundation/nsunit) 하 고 [NSDimension](https://developer.apple.com/reference/foundation/nsdimension) 특정 UOMs 나타내기 위한 클래스입니다.
- 에 추가 된 몇 가지 새 속성을 [NSLocal](https://developer.apple.com/reference/foundation/nslocale) 로컬 정보 및 사용 가능한 표시 형식을 가져오려고 클래스.

## <a name="gamekit-enhancements"></a>GameKit 향상 된 기능

IOS 10에서에서 GameKit 프레임 워크 같이 향상 되었습니다.:

- 합니다 **게임 센터 앱** 사용 되지 않으며 iOS에서 제거 되었습니다. 앱 GameKit를 사용 하는 경우 해당 _해야_ 순위표 등 GameKit 기능을 표시 하는 자체 인터페이스를 제공 합니다. 
- 새 iCloud 전용 계정 유형을 구현에 [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) 클래스입니다.
- 새 [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) 클래스 Game Center에서 영구 데이터 저장소 관리를 위한 일반화 된 솔루션을 제공 합니다. `GKGameSession` 플레이어의 목록을 유지 관리 및 구현을 담당 앱 참가자 날짜는 저장, 검색 또는 플레이어 간에 교환 되는 방법 및 시기입니다. 대부분의 기존 턴 기반 일치, 실시간 일치 또는 메서드 저장 영구 게임 게임 세션 바꾸면 됩니다.

## <a name="gameplaykit-enhancements"></a>GameplayKit 향상 된 기능

IOS 10에서에서 GameplayKit 프레임 워크 같이 향상 되었습니다.:

- 새 [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) 성능이 우수 하 고 자연 스러운 경로 제공 하는 클래스입니다.
- 절차적 노이즈 추가한 생성과 자연 스러운 질감에 현실성 향상 카메라 이동을에 현실성을 추가 하 고 풍부한 게임 세계를 생성 하는 데 도움이 데 사용할 수 있습니다.
- 공간 분할을 사용 하 여 효율적인 검색을 위한 게임 데이터를 분할 합니다.
- 새 Monte Carlo 전략가 ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) 철저 한 가능한 이동 계산을 위해 추가 되었습니다.
- 기존 에이전트를 사용 하 여 새 경로 찾기 동작 3D 지원이 추가 되었습니다 [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) 하 고 [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) 클래스입니다.
- 새 [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) 하 고 [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) GameplayKit 및 SpriteKit 그 어느 때 보다 쉽게 결합 하는 클래스 확인 합니다.
- 새 의사 결정 트리 API가 추가 되었습니다 ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) 하 고 [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) 게임 작성 인공 지능을 향상 시키기 위해.

## <a name="healthkit-enhancements"></a>HealthKit 향상 된 기능

IOS 10에서에서 HealthKit 프레임 워크 같이 향상 되었습니다.:

- 날씨 형식에 대 한 새 메타 데이터 키를 추가한 (같은 `HKWeatherConditionClear` 하 고 `HKWeatherConditionCloudy`) 및 운동 형식 (같은 `HKWorkoutActivityTypeFlexibility` 및 `HKWorkoutActivityTypeWheelchairRunPace`) 추가 되었습니다.
- 새 `HKCDADocument` 임상 문서 아키텍처 (CDA)를 나타내는 문서 서식이 지정 된 클래스가 추가 되었습니다.
- 새 [HKWorkoutConfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration) 클래스를 지정 합니다 `ActivityType` 및 `LocationType` 만납니다의 합니다.
- 새 [HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject) 및 `WheelchairUse` 메서드를 [HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore) 휠체어 사용 하기 위한 클래스를 추가한 관련 상태 데이터입니다.

## <a name="homekit-enhancements"></a>HomeKit 향상 된 기능

IOS 10에서에서 HomeKit 프레임 워크 같이 향상 되었습니다.:

- 새로운 서비스 및 특성이 추가 되었습니다.
- IPad, 액세서리 원격 액세스를 제공 하 고, automation 트리거를 실행 하 고, 사용 하도록 설정 하려면 HomeKit 허브 사용자 권한을 공유 역할을 구성할 수 있습니다.
- 카메라 및 doorbell accessories에 대 한 지원이 추가 되었습니다.
- Accessories에 대 한 자세한 컨텍스트 및 구성을 제공 되었습니다.

참조 하십시오 우리의 [HomeKit 소개](~/ios/platform/homekit.md) 자세한 정보에 대 한 설명서입니다.

## <a name="metal-enhancements"></a>Metal 향상 된 기능

IOS 10에서에서 금속 프레임 워크 같이 향상 되었습니다.:

- 3D 앱과 게임 이제 사용할 수 있습니다 _공간 분할_ 효율적으로 복잡 한 장면을 만들 및 GPU 통해 기 하 도형에 렌더링할 수 있습니다.
- 힙 리소스를 사용 하 여 앱 및 Memoryless 렌더링 대상을 기반으로 하는 체제 미 설치 컴퓨터의 성능을 최적화 하기 위해 리소스 할당의 세분화 된 제어를 제공 합니다.
- 함수 특수화를 사용 하 여 장면에 대 한 간단한 조합 함수와 자료의 최적화 된 컬렉션을 만듭니다.

자세한 내용은 Apple의를 참조 하세요 [Metal 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221)합니다.

## <a name="modelio-enhancements"></a>ModelIO 향상 된 기능

IOS 10에서에서 ModelIO 프레임 워크 같이 향상 되었습니다.:

 - USD 파일 형식은 이제 지원 됩니다.
 - 서명 지원이 추가 되었습니다 거리 필드를 [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) 클래스입니다.
 - 새 `MDLLightProbeIrradianceDataSource` Light 프로브 배치 하는 데 도움이 되는 클래스입니다.
 - 새 `MDLMaterialPropertyGraph` 클래스를 쉽게 런타임에서 지 원하는 모델을 변경 합니다.

## <a name="photos-enhancements"></a>사진 향상 된 기능

IOS 10에서에서 사진 프레임 워크 같이 향상 되었습니다.:

- 사용 합니다 [CIImageProcessorInput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput) 하 고 [CIImageProcessorOutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput) 편집을 수행 하는 새로운 Core 이미지 프로세서 기능을 활용 하는 클래스입니다.
- 사진 편집 확장 하는 사진 프레임 워크를 지 원하는 앱에 대 한 출시 되었습니다 라이브 사진 편집 (사진 및 카메라 내부 사용에 대 한 앱).
- 새 [PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext) 비디오와 여전히 사진 라이브 콘텐츠를 편집을 적용 하는 클래스입니다.

## <a name="replaykit-enhancements"></a>ReplayKit 향상 된 기능

IOS 10에서에서 ReplayKit 프레임 워크 같이 향상 되었습니다.:

- 사용 합니다 [RPScreenRecorder](https://developer.apple.com/reference/replaykit/rpscreenrecorder), [RPBroadcastActivityViewController](https://developer.apple.com/reference/replaykit/rpbroadcastactivityviewcontroller) 하 고 [RPBroadcastController](https://developer.apple.com/reference/replaykit/rpbroadcastcontroller) 중 브로드캐스팅을 지 원하는 클래스를 통해 타사 미디어 기록 사이트입니다.
- 브로드캐스트 업로드 및 브로드캐스트 UI 확장을 앱에서 ReplayKit 3rd 파티 브로드캐스트 서비스를 지원 해야 합니다.

## <a name="scenekit-enhancements"></a>SceneKit 향상 된 기능

IOS 10에서에서 SceneKit 프레임 워크 같이 향상 되었습니다.:

- 합니다 [SCNCamera](https://developer.xamarin.com/api/type/SceneKit.SCNCamera/) 클래스 HDR 기능 및 효과 사용 하 여 큰 현실성을 제공할 수 있습니다. 적응 노출을 사용 하 여 자동 효과 또는 사용 하 여 비네팅, 색 번짐과 및 게임 fillmatic 효과 추가할 그레이 딩 색을 만듭니다.
- SceneKit 간단 자산 작성 보다 현실적인 결과 대 한 새 물리적 기반 렌더링 (PBR) 시스템을 현재 포함 되어 있습니다.
- 새 [SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) 만 세 가지 기본 속성을 필요로 하는 동안 다양 한 실제 음영 효과 제품 모델 음영 (`Diffuse`를 `Metalness` 고 `Roughness`).
- 이후 PBR 조명 환경을 기반으로 가장 잘 작동을 음영을 사용 하 여는 `LightingEnvironment` 이미지 기반 조명은 전체 장면에 할당할 속성입니다.
- 사용 된 `IESProfileURL` (루멘)의 강도 등 색 온도 (켈빈) 실제 값을 기반으로 하는 조명을 정의 하는 실제 조명 기구를 가져올 속성을 합니다.
- PBR와 HDR 카메라 기능을 모두 기존 렌더링 기술 보다 나은 결과 제공 하 고 결과적으로, SceneKit을 이제 모든 색 계산 수행 (와이드 컬러 장치 디스플레이 P3 색 영역 사용) 하는 선형 색 공간에서.
- SceneKit 이제 색과 색 프로필 정보를 참조 하 여 모든 색을 일치 합니다.
- SceneKit 모든 셰이더 형식에 대 한 선형 RGB 색 공간에서 색 구성 요소 값을 해석합니다.
- 선형 모두 색 공간 렌더링 및 와이드 컬러를 지정 하 여 사용할 수는 `SCNDisableLinearSpaceRendering` 하 고 `SCNDisableWideGamut` 앱의 키 `Info.plist`합니다.
- 빌드 (또는 파일에서 로드 프로그래밍 방식으로 생성) 임의의 다각형 볼 새 기 하 도형 지정할 [SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon) 클래스.
- SceneKit 읽고 텍스처 이미지에서 색 프로필 정보에 대 한 조정, 되므로 모든 이미지에 대 한 자산 카탈로그 사용 하 여이 정보 제공 되는지 확인 합니다.

## <a name="spritekit-enhancements"></a>SpriteKit 향상 된 기능

IOS 10에서에서 SpriteKit 프레임 워크 같이 향상 되었습니다.:

- 사용자 지정 셰이더 특성을 제공할 수 있습니다 (`SKAttribute`) 특성 값을 제공 하 여 셰이더를 사용 하는 각 노드에 별도로 구성할 수 있는 (`SKAttributeValue`).
- 이제 Tilemaps 2D, 2.5 D 및 사용 하 여 쪽 스크롤 게임에 대 한 사각형, 육각 및 등각 타일 셰이프를 지원 합니다 `SKTileMapMode`, `SKTileGroup`를 `SKTileGroupRule` 및 `SKTileSet` 클래스입니다.
- 새 `SKWarpGeometry` 늘이거나 왜곡 하는 클래스 [SKSpriteNode](https://developer.xamarin.com/api/type/SpriteKit.SKSpriteNode/) 하거나 [SKEffectNode](https://developer.xamarin.com/api/type/SpriteKit.SKEffectNode/) 렌더링 합니다. 새 [SKAction](https://developer.xamarin.com/api/type/SpriteKit.SKAction/) 클래스 warp 효과 간의 전환에 애니메이션 효과를 사용할 수 있습니다.
- 합니다 [SKView](https://developer.xamarin.com/api/type/SpriteKit.SKView/) 클래스 장면 렌더링 되는 방법과 시기를 세부적으로 제어할 수 있도록 몇 가지 새로운 메서드를 제공 합니다.

## <a name="scrollview-enhancements"></a>ScrollView 향상 된 기능

Ios 10.3 ScrollView 컨트롤에 다음과 같은 기능이 향상 되었습니다.

- `UIScrollView` 이제 포함 된 `IndexDisplayMode` 사용자도 스크롤 하는 동안 인덱스는 표시 하는 방법을 제어 하는 속성을 `UIScrollViewIndexDisplayMode` 의:
    - `Automatic` -인덱스 표시는 OS에 의해 제어 됩니다.
    - `AlwaysHidden` -인덱스 표시 항상 숨겨집니다.

참조 된 [iOSTenThree 샘플](https://developer.xamarin.com/samples/monotouch/iOS10/iOSTenThree) 사용량에 대 한 합니다.

## <a name="uikit-enhancements"></a>UIKit 향상 된 기능

IOS 10에서에서의 UIKit 프레임 워크 같이 향상 되었습니다.:

- 새 [UIPasteboard](https://developer.xamarin.com/api/type/UIKit.UIPasteboard/) API (예: 수명 제한 사항) 새 옵션을 제공 하 고 일반적인 클래스 형식에 대해 호환 되는 콘텐츠 형식에 자동으로 선언 됩니다.
- 새로운 완전 한 대화형 하 고, 개체 기반 이며, 인터럽트 가능한 애니메이션 지원 추가 되었습니다 및 제스처에 연결할 수 있습니다. Pleas 참조 Apple의 [UIViewAnimating 프로토콜 참조](https://developer.apple.com/reference/uikit/uiviewanimating)를 [UIViewPropertyAnimator 클래스 참조](https://developer.apple.com/reference/uikit/uiviewpropertyanimator)하십시오 [UITimingCurveProvider 프로토콜 참조](https://developer.apple.com/reference/uikit/uitimingcurveprovider)를 [UICubicTimingParameters 클래스 참조](https://developer.apple.com/reference/uikit/uicubictimingparameters) 하 고 [UISpringTimingParameter 클래스 참조](https://developer.apple.com/reference/uikit/uispringtimingparameters) 자세한 내용은 합니다.
- 새 `UIPreviewInteraction` 고 `UIPreviewInteractionDelegate` 개발자 앱 미리 보기 및 pop 작업에 대 한 사용자 지정 인터페이스를 제공 하도록 허용 합니다.
- 새 `UIAccessibilityCustomRotor` 클래스를 사용 하면 응용 프로그램에서 사용자 지정 상황에 맞는 기능 Voice Over와 같은 보조 기술을 제공 합니다.
- 사용 된 `UIAccessibilityIsAssistiveTouchRunning` 및 `UIAccessibilityAssistiveTouchStatusDidChangeNotification` AssistiveTouch 사용 되는지 여부를 결정 하는 기호입니다.
- 사용 된 `UIAccessibilityHearingDevicePairedEar` 및 `UIAccessibilityHearingDevicePairedEarDidChangeNotification` 의 상태를 가져오려면 기호 쌍을 이루는 MFi 청각 지원.
- 를 지원 하기 위해 동적 형식 레이블에 텍스트 필드 및 입력란 사용 하 여 새 `PreferredFontForTextStyle` 메서드는 `UIFont` 클래스입니다.
- 요소가 해당 글꼴을 업데이트 해야 하는 경우를 결정 때 장치의 `UIContentSizeCategory` 변경 내용을 사용 하 여는 `AdjustsFontForContentSizeCategory` 의 속성을 `UIContentSizeCategoryAdjusting` 대리자.
- `OpenURL` 메서드는 `UIApplication` 클래스 비동기적으로 호출 되 고 이제 열기 작업이 완료 된 후 호출 되는 완료 처리기를 지원 합니다.
- CloudKit 공유를 시작 하 고 새 속성을 수정할 `UICloudSharingController` 고 `UICloudSharingControllerDelegate` 클래스입니다.
- 프리페치된 셀의 스크롤 환경을 개선 하기 위해 활용 `UICollectionViews` 새 `UICollectionViewDataSourcePrefetching` 위임 합니다.
- 개발자 탭 표시줄 항목 (예: 텍스트와 배경 색)에 대 한 배지의 모양을 제어할 수 있습니다.
- 새로 고침 컨트롤은 이제 모든 스크롤 보기와 스크롤 보기 하위 클래스에서 지원 됩니다 (같은 `UICollectionView`).

## <a name="webkit-enhancements"></a>WebKit 향상 된 기능

IOS 10에서에서 WebKit 프레임 워크 같이 향상 되었습니다.:

- 미리 보기 및 pop 지원에 추가 된를 `WKWebView` 클래스입니다. 사용 된 `ShouldPreviewElement` 지정 된 웹 보기를 미리 보기를 표시 해야 하는 경우를 결정 하는 방법입니다.


## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://developer.xamarin.com/samples/ios/iOS10/)
- [IOS 10의에서 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewIniOS/Articles/iOS10.html#//apple_ref/doc/uid/TP40017084-SW1)
