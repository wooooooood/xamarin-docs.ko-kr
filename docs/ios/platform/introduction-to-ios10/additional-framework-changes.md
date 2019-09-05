---
title: 추가 iOS 10 프레임 워크 변경 내용
description: 이 문서에서는 iOS 10의 기존 프레임 워크에 대 한 사소한 변경 내용 및 향상 된 기능을 설명 하 고 Xamarin.ios에서 이러한 업데이트를 활용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 0E2217F1-FC96-4D0A-ABAB-D40AD8F96502
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 620b89ba4682d65552fa5555c978b7eb5f437714
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290764"
---
# <a name="additional-ios-10-frameworks-changes"></a>추가 iOS 10 프레임 워크 변경 내용

_이 문서에서는 iOS 10의 기존 프레임 워크에 대 한 추가, 사소한 변경 또는 향상 된 기능을 설명 합니다._

## <a name="av-foundation-framework-additions"></a>AV 기반 프레임 워크 추가

AVFoundation 프레임 워크에는 다음과 같은 향상 된 기능이 포함 되어 있습니다.

- IOS 10에서는 개발자가 콘텐츠 형식에 따라 더 이상 다른 [AVPlayerItem](xref:AVFoundation.AVPlayerItem) 동작을 구현할 필요가 없습니다. `Rate` 속성을 설정 하기만 하면 상태일 없이 재생할 수 있는 콘텐츠가 충분 한 경우 avfoundation에서 결정 합니다.
- 새 [AVCapturePhotoOutput](xref:AVFoundation.AVCaptureFileOutput) 클래스는 더 이상 사용 `AVCaptureStillImageOutput` 되지 않는 클래스를 대체 하며, 캡처 프로세스 및 새로운 지원에 대 한 정교한 제어 및 모니터링을 제공 하 여 모든 사진 워크플로를 처리 하는 통합 방법을 제공 합니다. 라이브 사진 및 원시 캡처 형식과 같은 기능
- 새 `AVPlayerLooper` 클래스를 사용 하면 재생 하는 동안 지정 된 미디어 부분을 보다 쉽게 반복할 수 있습니다.
- 클래스 `AVAssetDownloadURLSession` 를 사용 하면 FairPlay 암호화 된 HLS 스트림을 다운로드 하 고 나중에 재생할 수 있습니다.
- 기본적으로 [AVCaptureSession](xref:AVFoundation.AVCaptureSession) 클래스는 장치 하드웨어에서 지원할 때 와이드 색의 넓은 색 영역 캡처를 자동으로 지원 합니다. 자세한 내용은 Apple의 [IOS 장치 호환성 참조](https://developer.apple.com/library/prerelease/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013599) 를 참조 하세요.

## <a name="avkit-additions"></a>AVKit 추가

이제 avkit 프레임 워크에는 현재 `UpdatesNowPlayingInfoCenter` 재생 중인 정보 센터를 업데이트 해야 하는 경우를 나타내는 새 속성이 포함 됩니다.

## <a name="core-data-enhancements"></a>핵심 데이터 향상

iOS 10에는 핵심 데이터 프레임 워크에 대 한 다음과 같은 향상 된 기능이 포함 되어 있습니다.

- 모드 (관리 개체 컨텍스트)를 사용 하는 새 쿼리 생성 기능을 사용 하 여 [NSManagedObjectContext](xref:CoreData.NSManagedObjectContext) 개체를 WAL 저널 모드에 저장 하면 나중에 인출 하 고 오류를 발생 시킬 수 있습니다.
- Root [NSManagedObjectContext](xref:CoreData.NSManagedObjectContext) 개체는 serialization 없이 동시에 오류 및 가져오기를 지원 합니다.
- [NSPersistentStoreCoordinator](xref:CoreData.NSPersistentStoreCoordinator) 클래스는 SQLite 데이터 저장소의 풀을 유지 관리 합니다.
- 더 쉽게 페치를 수행 하 고 하위 `NSManagedObject` 클래스를 만들 수 있도록 몇 가지 새로운 편의 방법이 추가 되었습니다.
- 상위 수준 `NSPersistenceContainer` 을 사용 하 여 `NSPersistentStoreCoordinator`, [NSManagedObjectModel](xref:CoreData.NSManagedObjectModel) 및 기타 핵심 데이터 구성 리소스를 참조 합니다.

자세한 내용은 Apple의 [핵심 데이터 프레임 워크 참조](https://developer.apple.com/reference/coredata)를 참조 하세요.

## <a name="core-image-enhancements"></a>핵심 이미지 향상

iOS 10은 핵심 이미지 프레임 워크에 대해 다음과 같은 향상 된 기능을 만듭니다.

- 이제 개발자는 처리 전후에 색 공간에서를 변환 하 여 핵심 이미지 컨텍스트의 작업 색 공간 외부에 있는 색 공간에서 이미지를 처리할 수 있습니다.
- A8 또는 A9 Cpu를 사용 하는 iOS 장치의 경우 이제 원시 이미지 형식이 지원 됩니다. 핵심 이미지는 이제 기본 제공 iSight 카메라 또는 타사 카메라에서 원시 이미지를 디코딩하는 기능을 지원 합니다. [Cifilter](xref:CoreImage.CIFilter) 클래스 `FilterWithImageURL` 의 또는메서드를사용하여원시이미지를처리합니다.`FilterWithImageData`
- 개체에서 `UIImage` `UIImageView` 렌더링 (핵심 이미지 이미지 저장소에서 지원 되는 경우)에 대 한 몇 가지 렌더링 성능이 향상 되었습니다. 
- `UIImage`넓은 색으로 태그가 지정 된 개체는 넓은 색을 지 원하는 `UIImageView` iOS 장치에서 개체의 넓은 색 영역 색으로 렌더링 됩니다.
- 핵심 이미지 커널 코드는 이제 특정 픽셀 출력 형식을 요청할 수 있습니다.
- [Cifilter](xref:CoreImage.CIFilter) 클래스의 메서드를사용하여사용자지정처리를필터작업에삽입할수있습니다.`ImageWithExtent` 핵심 이미지는 출력 또는 표시를 위해 이미지를 처리할 때 필터 사이에 지정 된 콜백을 호출 합니다.

또한 다음과 같은 새로운 핵심 이미지 필터가 추가 되었습니다.

- `CINinePartTiled`
- `CINinePartStretched`
- `CIHueSaturationValueGradient`
- `CIEdgePreserveUpsampleFilter`
- `CIClamp`

## <a name="core-motion-additions"></a>핵심 동작 추가

IOS 10의 새로운 기능으로, 핵심 동작 프레임 워크에는 앱이 실행 되는 동안 사용자가 일시 중지 하 고 추적을 다시 시작 하는 빠르게 실시간 알림을 받을 수 있도록 하는 pedometer 이벤트가 포함 되어 있습니다. [CMPedometer](xref:CoreMotion.CMPedometer) 를 사용 하 여 포그라운드 또는 background pedometer 이벤트에 등록 합니다.

## <a name="foundation-enhancements"></a>Foundation 향상 된 기능

IOS 10 용 Foundation framework에 대 한 다음과 같은 기능이 향상 되었습니다.

- 새 [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) 클래스를 사용 하 여 최종 사용자에 게 표시 하기 위해 지역화 된 측정값의 서식을 지정 합니다.
- 새 [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) 클래스를 사용 하 여 간격을 비교 하 고 간격 교차로 테스트 하는 기간 등의 날짜 및 시간 간격 계산을 수행할 수 있습니다.
- 새 [Nsmeasurement](https://developer.apple.com/reference/foundation/nsmeasurement) 클래스를 사용하여 다른 Uom (측정 단위) 간에 변환하거나 다른 uom의 값에 대한 계산을 수행합니다.

- 새 [Nsunit](https://developer.apple.com/reference/foundation/nsunit) 및 [nsunit](https://developer.apple.com/reference/foundation/nsdimension) 클래스를 사용 하 여 특정 uoms를 나타냅니다.
- 로컬 정보 및 사용 가능한 표시 형식을 얻기 위해 [Nslocal](https://developer.apple.com/reference/foundation/nslocale) 클래스에 몇 가지 새 속성이 추가 되었습니다.

## <a name="gamekit-enhancements"></a>GameKit 향상

IOS 10에서 GameKit 프레임 워크에 대 한 다음과 같은 기능이 향상 되었습니다.

- **Game Center 앱** 은 더 이상 사용 되지 않으며 iOS에서 제거 되었습니다. 앱에서 GameKit를 사용 하는 경우 순위표 등과 같은 GameKit 기능을 표시 하는 고유한 인터페이스를 제공 _해야 합니다_ . 
- 새 iCloud 전용 계정 유형은 [Gkcloudplayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) 클래스에 의해 구현 되었습니다.
- 새 [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) 클래스는 Game Center에서 영구적 데이터 저장소를 관리 하기 위한 일반화 된 솔루션을 제공 합니다. `GKGameSession`플레이어의 목록을 유지 관리 하 고, 앱이 참가자 날짜를 저장, 검색 또는 교환 하는 방법 및 시기를 구현 해야 합니다. 대부분의 경우 게임 세션은 기존 턴 기반 일치, 실시간 일치 또는 지속적인 게임 저장 방법을 바꿀 수 있습니다.

## <a name="gameplaykit-enhancements"></a>GameplayKit 향상

IOS 10에서 GameplayKit 프레임 워크에 대 한 다음과 같은 기능이 향상 되었습니다.

- 새 [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) 클래스를 사용 하 여 고성능의 자연 스러운 경로를 제공 합니다.
- 절차적 노이즈 생성이 추가 되었고 현실감를 자연스럽 게 볼 수 있는 질감으로 개선 하 고, 카메라 움직임에 현실감을 추가 하 고, 다양 한 게임 환경을 생성 하는 데 사용할 수 있습니다.
- 효율적인 검색을 위해 공간 분할을 사용 하 여 게임 세계 데이터를 분할 합니다.
- 새[GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)(Monte 몬테카를로 전략가)가 가능한 이동 계산을 위해 추가 되었습니다.
- 새 [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) 및 [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) 클래스를 사용 하 여 기존 에이전트 및 경로 찾기 동작에 3d 지원이 추가 되었습니다.
- 새로운 [Gkscene](https://developer.apple.com/reference/gameplaykit/gkscene) 및 [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) 클래스를 사용 하면 GameplayKit와 SpriteKit를 보다 쉽게 결합할 수 있습니다.
- 게임 빌딩 AI를 향상 시키기 위해 새로운 의사 결정 트리 API ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) 및 [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode))가 추가 되었습니다.

## <a name="healthkit-enhancements"></a>HealthKit 향상

IOS 10에서 HealthKit 프레임 워크에 대 한 다음과 같은 기능이 향상 되었습니다.

- 날씨 유형 (예 `HKWeatherConditionClear` : 및 `HKWeatherConditionCloudy`)에 대 한 새 메타 데이터 키가 추가 되 고 `HKWorkoutActivityTypeFlexibility` , 및 등 `HKWorkoutActivityTypeWheelchairRunPace`의 체력 유형 (예: 및)이 추가 되었습니다.
- CDA ( `HKCDADocument` 임상 문서 아키텍처) 형식의 문서를 나타내기 위해 새 클래스가 추가 되었습니다.
- 새 [HKWorkoutConfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration) 클래스를 사용 하 여 체력 `ActivityType` 의 `LocationType` 및를 지정 합니다.
- 휠체어 관련 상태 데이터를 `WheelchairUse` 사용 하기 위해 새 [HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject) 및 [HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore) 클래스의 메서드가 추가 되었습니다.

## <a name="homekit-enhancements"></a>HomeKit 향상

IOS 10에서 HomeKit 프레임 워크에 대 한 다음과 같은 기능이 향상 되었습니다.

- 새 서비스 및 특징이 추가 되었습니다.
- IPad는 HomeKit Hub 역할을 하도록 구성 하 여 원격 액세서리 액세스를 제공 하 고, 자동화 트리거를 실행 하 고, 공유 사용자 권한을 사용할 수 있습니다.
- 카메라 및 doorbell 액세서리에 대 한 지원이 추가 되었습니다.
- 액세서리를 위한 추가 컨텍스트 및 구성이 제공 되었습니다.

자세한 내용은 [HomeKit 소개 설명서를](~/ios/platform/homekit.md) 참조 하세요.

## <a name="metal-enhancements"></a>금속 기능 향상

IOS 10의 금속 프레임 워크에 대해 다음과 같은 기능이 향상 되었습니다.

- 3D 앱 및 게임에서는 이제 _공간 분할_ 을 사용 하 여 GPU를 통해 복잡 한 장면 및 기 하 도형을 효율적으로 렌더링할 수 있습니다.
- 리소스 힙을 사용 하 고 메모리 부족 렌더링 대상을 사용 하 여 금속 기반 앱의 성능을 최적화 하기 위해 리소스 할당에 대 한 세분화 된 제어를 제공 합니다.
- 함수 특수화를 사용 하 여 장면에 대해 매우 최적화 된 재질 및 조명 조합 함수 컬렉션을 만듭니다.

자세히 알아보려면 Apple의 [금속 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221)를 참조 하세요.

## <a name="modelio-enhancements"></a>ModelIO 향상

IOS 10에서 ModelIO 프레임 워크에 대 한 다음과 같은 기능이 향상 되었습니다.

- 이제 USD 파일 형식이 지원 됩니다.
- 서명 된 거리 필드 지원은 [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) 클래스에 추가 되었습니다.
- 새 `MDLLightProbeIrradianceDataSource` 클래스를 사용 하 여 밝은 프로브 배치를 지원 합니다.
- 새 `MDLMaterialPropertyGraph` 클래스를 사용 하 여 모델에 대 한 런타임 변경을 쉽게 지원할 수 있습니다.

## <a name="photos-enhancements"></a>향상 된 사진

IOS 10의 사진 프레임 워크에 대 한 다음과 같은 기능이 향상 되었습니다.

- [CIImageProcessorInput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput) 및 [CIImageProcessorOutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput) 클래스를 사용 하 여 새 핵심 이미지 프로세서 기능을 활용 하 여 편집을 수행 합니다.
- 이제 사진 프레임 워크를 지 원하는 앱과 사진 및 카메라 앱 내부에서 사용할 수 있도록 사진 편집 확장 (라이브 사진 편집)을 사용할 수 있습니다.
- 새 [PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext) 클래스를 사용 하 여 라이브 사진의 비디오 및 콘텐츠 모두에 편집 내용을 적용 합니다.

## <a name="replaykit-enhancements"></a>ReplayKit의 향상 된 기능

IOS 10의 ReplayKit 프레임 워크에 대해 다음과 같은 기능이 향상 되었습니다.

- [RPScreenRecorder](https://developer.apple.com/reference/replaykit/rpscreenrecorder), [RPBroadcastActivityViewController](https://developer.apple.com/reference/replaykit/rpbroadcastactivityviewcontroller) 및 [RPBroadcastController](https://developer.apple.com/reference/replaykit/rpbroadcastcontroller) 클래스를 사용 하 여 타사 사이트를 통해 기록 된 미디어 브로드캐스팅을 지원할 수 있습니다.
- 브로드캐스트 UI 및 Broadcast Upload 확장은 앱에서 ReplayKit 타사 브로드캐스트 서비스를 지 원하는 데 필요 합니다.

## <a name="scenekit-enhancements"></a>SceneKit 향상

IOS 10에서 SceneKit 프레임 워크에 대 한 다음과 같은 기능이 향상 되었습니다.

- [Scncamera](xref:SceneKit.SCNCamera) 클래스는 HDR 기능 및 효과를 사용 하 여 더 큰 현실감를 제공할 수 있습니다. 자동 효과를 만들거나 vignetting, 색 fringing 및 색을 사용 하 여 게임에 fillmatic 효과를 추가 하려면 적응 노출을 사용 합니다.
- 이제 SceneKit에는 더 간단한 자산 제작을 통해 보다 현실적인 결과를 제공 하는 새로운 .PBR (물리적 기반 렌더링) 시스템이 포함 되어 있습니다.
- 새 [SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) 음영 모델을 사용 하 여 세 가지 기본 속성 (`Diffuse` `Metalness` 및 `Roughness`)만 필요 하면서 광범위 한 사실적인 음영 효과를 제품 합니다.
- .Pbr 음영은 환경 기반 조명에서 가장 잘 작동 하므로 `LightingEnvironment` 속성을 사용 하 여 이미지 기반 조명을 전체 장면에 할당 합니다.
- 속성을 `IESProfileURL` 사용 하 여 강도 (lumens) 및 색 온도 (켈빈)와 같은 실제 값을 기준으로 조명을 정의 하는 실제 조명 설비를 가져옵니다.
- .PBR 및 HDR 카메라 기능 모두 기존 렌더링 기술 보다 더 나은 결과를 제공 하므로 이제 SceneKit는 선형 색 공간에서 모든 색 계산을 수행 합니다 (넓은 색 장치 디스플레이에서 P3 색 영역 사용).
- 색 프로필 정보를 읽어 SceneKit 색이 모든 색과 일치 합니다.
- SceneKit는 모든 셰이더 형식에 대해 선형 RGB 색 공간의 색 구성 요소 값을 해석 합니다.
- 응용 프로그램의 `SCNDisableLinearSpaceRendering` `SCNDisableWideGamut`에서및 키를 지정 하 여 선형 색 공간 렌더링과 와이드 색을 모두 사용 하지 않도록 설정할 수 있습니다. `Info.plist`
- 새 [SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon) 클래스를 사용 하 여 geometry를 지정 하기 위해 파일에서 로드 되거나 프로그래밍 방식으로 생성 되는 임의의 다각형 primates을 빌드합니다.
- SceneKit는 질감 이미지에서 색 프로필 정보를 읽고 조정 하므로 모든 이미지에 대 한 자산 카탈로그를 사용 하 여이 정보가 제공 되는지 확인 합니다.

## <a name="spritekit-enhancements"></a>SpriteKit 향상

IOS 10에서 SpriteKit 프레임 워크에 대 한 다음과 같은 기능이 향상 되었습니다.

- 사용자 지정 셰이더는 특성 값`SKAttribute`(`SKAttributeValue`)을 제공 하 여 셰이더를 사용 하는 각 노드에서 별도로 구성할 수 있는 특성 ()을 제공할 수 있습니다.
- 이제 `SKTileMapMode`Tilemaps는 `SKTileGroup` ,및`SKTileSet`클래스 를 사용 하 여 2d, 2.5 d 및 사이드 스크롤 게임에 대해 정사각형, 육각형 및 등각 타일 셰이프를 지원 합니다. `SKTileGroupRule`
- 새 `SKWarpGeometry` 클래스를 사용 하 여 [SKSpriteNode](xref:SpriteKit.SKSpriteNode) 또는 [SKEffectNode](xref:SpriteKit.SKEffectNode) 렌더링을 늘이거나 왜곡할 수 있습니다. 새 고 [기능](xref:SpriteKit.SKAction) 클래스를 사용 하 여 비틀기 효과 간의 전환에 애니메이션 효과를 적용할 수 있습니다.
- 지 수 [뷰](xref:SpriteKit.SKView) 클래스는 장면을 렌더링 하는 시기와 방법을 세밀 하 게 제어할 수 있는 여러 가지 새로운 메서드를 제공 합니다.

## <a name="scrollview-enhancements"></a>ScrollView 향상

IOS 10.3에서 ScrollView 컨트롤에 대 한 다음과 같은 기능이 향상 되었습니다.

- `UIScrollView`이제 속성을 `IndexDisplayMode` 포함 하 여 사용자가 `UIScrollViewIndexDisplayMode` 의으로 스크롤 하는 동안 인덱스가 표시 되는 방식을 제어 합니다.
  - `Automatic`-인덱스 표시는 OS에 의해 제어 됩니다.
  - `AlwaysHidden`-인덱스 표시는 항상 숨겨집니다.

사용에 대 한 [Iostenthree 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-iostenthree) 을 참조 하세요.

## <a name="uikit-enhancements"></a>UIKit 향상 된 기능

IOS 10의 UIKit 프레임 워크에 대 한 다음과 같은 기능이 향상 되었습니다.

- 새 [uipasteboard](xref:UIKit.UIPasteboard) 본 API는 수명 제한과 같은 새 옵션을 제공 하며 공용 클래스 형식에 대해 호환 되는 콘텐츠 형식을 자동으로 선언 합니다.
- 완전히 대화형으로 제공 되는 새로운 개체 기반 애니메이션 지원이 추가 되었으며 제스처에 연결할 수 있습니다. Apple의 [Uiviewanimating 프로토콜 참조](https://developer.apple.com/reference/uikit/uiviewanimating), [uiviewproperty애니메이터 클래스 참조](https://developer.apple.com/reference/uikit/uiviewpropertyanimator), [UITimingCurveProvider Protocol Reference](https://developer.apple.com/reference/uikit/uitimingcurveprovider), [Uicubic ingparameters 클래스 참조](https://developer.apple.com/reference/uikit/uicubictimingparameters) [를 참조 하세요. 자세한 내용은 UISpringTimingParameter 클래스 참조를 참조](https://developer.apple.com/reference/uikit/uispringtimingparameters) 하세요.
- 개발자 앱 `UIPreviewInteraction` 이 `UIPreviewInteractionDelegate` 새로운 기능을 사용 하 여 peek 및 pop 작업용 사용자 지정 인터페이스를 제공할 수 있습니다.
- 새 `UIAccessibilityCustomRotor` 클래스를 사용 하면 앱에서 음성 전달 등의 보조 기술에 대 한 사용자 지정 컨텍스트별 기능을 제공할 수 있습니다.
- `UIAccessibilityIsAssistiveTouchRunning` 및`UIAccessibilityAssistiveTouchStatusDidChangeNotification` 기호를 사용 하 여 AssistiveTouch를 사용할 수 있는지 확인 합니다.
- `UIAccessibilityHearingDevicePairedEar` 및`UIAccessibilityHearingDevicePairedEarDidChangeNotification` 기호를 사용 하 여 쌍을 이루는 MFi 청각 지원의 상태를 가져옵니다.
- 레이블에서 동적 형식을 지원 하기 위해 텍스트 필드와 텍스트 상자는 `PreferredFontForTextStyle` `UIFont` 클래스의 새 메서드를 사용 합니다.
- 장치가 `UIContentSizeCategory` 변경 될 때 요소가 글꼴을 업데이트 해야 하는지 결정 하려면 `UIContentSizeCategoryAdjusting` 대리자의 `AdjustsFontForContentSizeCategory` 속성을 사용 합니다.
- 클래스의 메서드는 비동기적으로 호출 되며 이제는 열기 작업이 완료 된 후 호출 되는 완료 처리기를 지원 합니다. `OpenURL` `UIApplication`
- 새 `UICloudSharingController` 및`UICloudSharingControllerDelegate` 클래스를 사용 하 여 cloudkit 공유를 시작 하 고 해당 속성을 수정 합니다.
- 프리페치된 셀을 활용 하 여 새 `UICollectionViews` `UICollectionViewDataSourcePrefetching` 대리자로의 스크롤 환경을 향상 시킵니다.
- 이제 개발자는 텍스트, 배경색 등의 탭 모음 항목에 대 한 배지의 모양을 제어할 수 있습니다.
- 이제 새로 고침 컨트롤이 모든 스크롤 뷰와 스크롤 뷰 서브 클래스 (예: `UICollectionView`)에서 지원 됩니다.

## <a name="webkit-enhancements"></a>WebKit 향상

IOS 10에서 WebKit 프레임 워크에 대 한 다음과 같은 기능이 향상 되었습니다.

- 피킹 (peeking) 및 pop 지원이 `WKWebView` 클래스에 추가 되었습니다. 지정 된 웹 보기에 미리 보기가 표시 되어야 하는지 여부를 확인 하려면 메서드를사용합니다.`ShouldPreviewElement`


## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
- [IOS 10의 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewIniOS/Articles/iOS10.html#//apple_ref/doc/uid/TP40017084-SW1)
