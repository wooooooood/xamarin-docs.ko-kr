---
title: 추가 tvOS 10 프레임 워크 변경
description: 이 문서는 사소한 변경 및 iOS 10에서에서 기존 프레임 워크에 대 한 향상 된 기능을 설명 합니다. AVFoundation, AVKit, 핵심 데이터, Core Graphics, Foundation, GameKit, GameplayKit, 등에 업데이트에 설명 합니다.
ms.prod: xamarin
ms.assetid: F771640A-F92E-4954-82D5-2D720434971E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: ab6236198d0a5826fc613d1f3839bafdb980d235
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2019
ms.locfileid: "67865640"
---
# <a name="additional-tvos-10-frameworks-changes"></a>추가 tvOS 10 프레임 워크 변경

TvOS에 주요 변경 내용 외에도 Apple가 수정 및 여러 기존 프레임 워크의 향상 된 기능 tvOS 10에서에서 수행 했습니다.

<a name="AV-Foundation-Framework" />

## <a name="avfoundation-framework-additions"></a>AVFoundation Framework Additions

AVFoundation 프레임 워크에는 다음과 같은 향상 기능이 포함 됩니다.

- TvOS 10에서에서 앱을 더 이상 구현 다른 [AVPlayerItem](https://developer.apple.com/reference/avfoundation/avplayeritem) 콘텐츠 형식을 기반으로 동작 합니다. 설정 된 `Rate` 속성과 AVFoundation 상태일 없이 재생에 사용할 수 있는 충분 한 콘텐츠는 경우 결정 됩니다.
- 새 `AVPlayerLooper` 클래스 쉽게 재생 하는 동안 지정 된 부분 미디어를 반복 합니다.

<a name="AVKit-Framework-Enhancements" />

## <a name="avkit-framework-enhancements"></a>AVKit 프레임 워크 향상 된 기능

AVKit 프레임 워크에는 다음과 같은 향상 기능이 포함 됩니다.

- 앱은 이제 건너뜁니다 동작에 대 한 제어에는 [AVPlayerViewController](https://developer.apple.com/reference/avkit/avplayerviewcontroller) 건너뜁니다 제스처를 재생 목록 또는 사전 현재 항목 내에서 다음 항목으로 이동할 수 있도록 합니다.

<a name="Core-Data-Enhancements" />

## <a name="core-data-enhancements"></a>핵심 데이터 개선 사항

tvOS 10에는 핵심 데이터 프레임 워크에 다음과 같은 향상 기능이 포함 됩니다.

- 루트 [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) 동시 오류 및 직렬화 없이 가져오는 개체를 지원 합니다.
- 합니다 [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) 클래스 SQLite 데이터 저장소의 풀을 관리 합니다.
- 합니다 [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) WAL 저널 모드 지원 새 쿼리 생성에에서 SQLite 데이터 저장소를 사용 하 여 개체 관리 되는 개체 컨텍스트 (MOC) 이후 페치 하기 위해 특정 데이터베이스 버전에 고정할 수 있습니다 위치 기능 및 오류가 있는 트랜잭션.
- 사용 하 여 대략적 `NSPersistenceContainer` 참조를 `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) 및 다른 핵심 데이터 구성 리소스.
- 에 추가 된 편리한 몇 가지 새로운 메서드 `NSManagedObject` 쉽게 페치를 수행 하 고 서브 클래스를 만들 수 있습니다.

자세한 내용은 Apple의를 참조 하세요 [핵심 데이터 프레임 워크 참조](https://developer.apple.com/reference/coredata)합니다.

<a name="Core-Graphics-Enhancements" />

## <a name="core-graphics-enhancements"></a>향상 된 핵심 그래픽 기능

tvOS 10 핵심 그래픽 프레임 워크에 다음과 같은 향상 기능이 포함 됩니다.

- 새 [CGColorConverterRef](https://developer.apple.com/reference/coregraphics/cgcolorconverterref) 클래스는 다양 한 색 변환 하는 데 사용할 수 있습니다.

<a name="Core-Image-Enhancements" />

## <a name="core-image-enhancements"></a>이미지 향상 된 핵심 기능

tvOS 10 Core 이미지 프레임 워크의 다음과 같은 향상 된 기능을 사용 하면 됩니다.

- 합니다 `ImageWithExtent` 메서드를 [CIFilter](https://developer.apple.com/reference/coreimage/cifilter) 필터 작업에 사용자 지정 처리를 삽입 하는 클래스를 사용할 수 있습니다. Core 이미지 출력에 대 한 이미지를 처리 하는 경우 필터 간에 지정 된 콜백을 호출 되거나 표시 됩니다.
- 이제 앱 처리 전후 색 공간에서 변환 하 여 색 공간을 Core 이미지 컨텍스트의 작업 색 공간 외부에서 이미지를 처리할 수 있습니다.
- 여러 렌더링 성능 향상 기능에 적용 된 `UIImage` (Core 이미지 이미지 저장소에서 지 원하는) 경우 렌더링 `UIImageView` 개체입니다. 
- `UIImage` 개체 태그가 지정 된 와이드 영역으로 광범위 한 색상에서 렌더링 됩니다 `UIImageView` 광범위 한 색을 지 원하는 iOS 장치에 있는 개체입니다.
- Core 이미지 커널 코드 출력 형식 특정 픽셀을 요청할 수 있습니다.

또한 다음 새 Core 이미지 필터를 추가 되었습니다.

- `CINinePartTiled`
- `CINinePartStretched`
- `CIHueSaturationValueGradient`
- `CIEdgePreserveUpsampleFilter`
- `CIClamp`

<a name="Foundation-Enhancements" />

## <a name="foundation-enhancements"></a>Foundation 향상 된 기능

TvOS 10에 대 한 Foundation 프레임 워크 같이 향상 되었습니다.:

- 새 [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) 기간, 간격을 비교 하 고 간격 교차점에 대 한 테스트와 같은 날짜 및 시간 간격 계산을 수행 하는 클래스입니다.
- 에 추가 된 몇 가지 새 속성을 [NSLocal](https://developer.apple.com/reference/foundation/nslocale) 로컬 정보 및 사용 가능한 표시 형식을 가져오려고 클래스.
- 새 [NSMeasurement](https://developer.apple.com/reference/foundation/nsmeasurement) 클래스 간에 서로 다른 단위의 측정값 (UOM)을 변환 하거나 다른 UOMs의 값에 대해 계산을 수행 합니다.
- 새 [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) 최종 사용자에 게 표시 하는 것에 대 한 지역화 된 측정값의 서식을 지정 하는 클래스입니다.
- 새 [NSUnit](https://developer.apple.com/reference/foundation/nsunit) 하 고 [NSDimension](https://developer.apple.com/reference/foundation/nsdimension) 특정 UOMs 나타내기 위한 클래스입니다.

<a name="GameKit-Enhancements" />

## <a name="gamekit-enhancements"></a>GameKit 향상 된 기능

TvOS 10에서에서 GameKit 프레임 워크 같이 향상 되었습니다.:

- 새 iCloud 전용 계정 유형을 구현에 [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) 클래스입니다.
- 새 [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) 클래스 Game Center에서 영구 데이터 저장소 관리를 위한 일반화 된 솔루션을 제공 합니다. `GKGameSession` 플레이어의 목록을 유지 관리 및 앱은 책임 폼 검색 또는 플레이어 간에 교환 참가자 날짜 저장 되는 방법 및 시기를 구현 합니다. 대부분의 기존 턴 기반 일치, 실시간 일치 또는 메서드 저장 영구 게임 게임 세션 바꾸면 됩니다.

<a name="GameplayKit-Enhancements" />

## <a name="gameplaykit-enhancements"></a>GameplayKit 향상 된 기능

TvOS 10에서에서 GameplayKit 프레임 워크 같이 향상 되었습니다.:

- 절차적 노이즈 추가한 생성과 자연 스러운 질감에 현실성 향상 카메라 이동을에 현실성을 추가 하 고 풍부한 게임 세계를 생성 하는 데 도움이 데 사용할 수 있습니다.
- 공간 분할을 사용 하 여 효율적인 검색을 위한 게임 데이터를 분할 합니다.
- 새 Monte Carlo 전략가 ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) 철저 한 가능한 이동 계산을 위해 추가 되었습니다.
- 새 의사 결정 트리 API가 추가 되었습니다 ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) 하 고 [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) 게임 작성 인공 지능을 향상 시키기 위해.
- 기존 에이전트를 사용 하 여 새 경로 찾기 동작 3D 지원이 추가 되었습니다 [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) 하 고 [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) 클래스입니다.
- 새 [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) 성능이 우수 하 고 자연 스러운 경로 제공 하는 클래스입니다.
- 새 [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) 하 고 [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) GameplayKit 및 SpriteKit 그 어느 때 보다 쉽게 결합 하는 클래스 확인 합니다.

<a name="Metal-Enhancements" />

## <a name="metal-enhancements"></a>Metal 향상 된 기능

TvOS 10에서에서 금속 프레임 워크 같이 향상 되었습니다.:

- 3D 앱과 게임 이제 사용할 수 있습니다 _공간 분할_ 효율적으로 복잡 한 장면을 만들 및 GPU 통해 기 하 도형에 렌더링할 수 있습니다.
- 함수 특수화를 사용 하 여 장면에 대 한 간단한 조합 함수와 자료의 최적화 된 컬렉션을 만듭니다.
- 힙 리소스를 사용 하 여 앱 및 Memoryless 렌더링 대상을 기반으로 하는 체제 미 설치 컴퓨터의 성능을 최적화 하기 위해 리소스 할당의 세분화 된 제어를 제공 합니다.

자세한 내용은 Apple의를 참조 하세요 [Metal 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221)합니다.

<a name="Metal-Performance-Shaders-Enhancements" />

## <a name="metal-performance-shaders-enhancements"></a>성능 metal 셰이더 향상

TvOS 10에서에서 성능 셰이더 금속 프레임 워크 같이 향상 되었습니다.:

- 색 공간 변환 및 신경망 작업 등의 높은 최적화, 데이터 병렬 계산을 활용 하도록 앱을 허용 하기 위해 체제 미 설치 컴퓨터 성능 셰이더 프레임 워크에 많은 새 커널은 추가 되었습니다.

<a name="ModelIO-Enhancements" />

## <a name="modelio-enhancements"></a>ModelIO 향상 된 기능

TvOS 10에서에서 ModelIO 프레임 워크 같이 향상 되었습니다.:

- USD 파일 형식은 이제 지원 됩니다.
- 새 `MDLMaterialPropertyGraph` 클래스를 쉽게 런타임에서 지 원하는 모델을 변경 합니다.
- 서명 지원이 추가 되었습니다 거리 필드를 [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) 클래스입니다.
- 새 `MDLLightProbeIrradianceDataSource` Light 프로브 배치 하는 데 도움이 되는 클래스입니다.

<a name="SceneKit-Enhancements" />

## <a name="scenekit-enhancements"></a>SceneKit 향상 된 기능

TvOS 10에서에서 SceneKit 프레임 워크 같이 향상 되었습니다.:

- SceneKit 간단 자산 작성 보다 현실적인 결과 대 한 새 물리적 기반 렌더링 (PBR) 시스템을 현재 포함 되어 있습니다.
- 새 [SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) 만 세 가지 기본 속성을 필요로 하는 동안 다양 한 실제 음영 효과 제품 모델 음영 (`Diffuse`를 `Metalness` 고 `Roughness`).
- 이후 PBR 조명 환경을 기반으로 가장 잘 작동을 음영을 사용 하 여는 `LightingEnvironment` 이미지 기반 조명 tan 장면 전체를 할당 하는 속성입니다.
- 사용 된 `IESProfileURL` 온도 켈빈) (에서 색 및 강도 (에서 루멘) 등의 실제 값에 기본 조명을 정의 하는 실제 조명 기구를 가져올 속성을 합니다.
- 합니다 [SCNCamera](https://developer.apple.com/reference/scenekit/scncamera) 클래스 HDR 기능 및 효과 사용 하 여 큰 현실성을 제공할 수 있습니다. 적응 노출을 사용 하 여 자동 효과 또는 사용 하 여 비네팅, 색 번짐과 및 게임 filmatic 효과 추가할 그레이 딩 색을 만듭니다.
- PBR와 HDR 카메라 기능을 모두 기존 렌더링 기술 보다 나은 결과 제공 하 고 결과적으로, SceneKit을 이제 모든 색 계산 수행 (와이드 컬러 장치 디스플레이 P3 색 영역 사용) 하는 선형 색 공간에서.
- SceneKit 이제 색과 색 프로필 정보를 참조 하 여 모든 색을 일치 합니다.
- SceneKit 모든 셰이더 형식에 대 한 선형 RGB 색 공간에서 색 구성 요소 값을 해석합니다.
- SceneKit 읽고 텍스처 이미지에서 색 프로필 정보에 대 한 조정, 되므로 모든 이미지에 대 한 자산 카탈로그 사용 하 여이 정보 제공 되는지 확인 합니다.
- 선형 모두 색 공간 렌더링 및 와이드 컬러를 지정 하 여 사용할 수는 `SCNDisableLinearSpaceRendering` 하 고 `SCNDisableWideGamut` 앱의 키 `Info.plist`합니다.
- 빌드 (또는 파일에서 로드 프로그래밍 방식으로 생성) 임의의 다각형 볼 새 기 하 도형 지정할 [SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon) 클래스.

<a name="SpriteKit-Enhancements" />

## <a name="spritekit-enhancements"></a>SpriteKit 향상 된 기능

TvOS 10에서에서 SpriteKit 프레임 워크 같이 향상 되었습니다.:

- 이제 Tilemaps 2D, 2.5 D 및 사용 하 여 쪽 스크롤 게임에 대 한 사각형, 육각 및 등각 타일 셰이프를 지원 합니다 `SKTileMapMode`, `SKTileGroup`를 `SKTileGroupRule` 및 `SKTileSet` 클래스입니다.
- 새 `SKWarpGeometry` 늘이거나 왜곡 하는 클래스 [SKSpriteNode](https://developer.apple.com/reference/spritekit/skspritenode) 하거나 [SKEffectNode](https://developer.apple.com/reference/spritekit/skeffectnode) 렌더링 합니다. 새 [SKAction](https://developer.apple.com/reference/spritekit/skaction) 클래스 warp 효과 간의 전환에 애니메이션 효과를 사용할 수 있습니다.
- 사용자 지정 셰이더 특성을 제공할 수 있습니다 (`SKAttribute`) 특성 값을 제공 하 여 셰이더를 사용 하는 각 노드에 별도로 구성할 수 있는 (`SKAttributeValue`).
- 합니다 [SKView](https://developer.apple.com/reference/spritekit/skview) 클래스 장면 렌더링 되는 방법과 시기를 세부적으로 제어할 수 있도록 몇 가지 새로운 메서드를 제공 합니다.

<a name="UIKit-Enhancements" />

## <a name="uikit-enhancements"></a>UIKit 향상 된 기능

10 tvOS의 UIKit 프레임 워크 같이 향상 되었습니다.:

- 포커스를 API에 또한 view가 아닌 항목의 포커스를 지원 하도록 향상 되었습니다 `UIViews`합니다. 포커스를 지 원하는 항목 _해야 합니다_ 구현 된 `IUIFocusItem` 인터페이스입니다.
- 새 `UIGraphicsRender` 클래스는 UIKit 렌더링 또는 핵심 그래픽 비트맵 또는 Pdf 만드는 개체 지향 메서드를 제공 하 고 사용 되지 않는 대체 `UIGraphicsBeginImageContext` 메서드.
- `UIUserInterfaceStyle` 현재 활성 상태인 사용자 인터페이스 테마 (어둡게 또는 밝게) 인지 클래스가 추가 되었습니다.
- 새 완전 한 대화형 하 고, 개체 기반 이며, 인터럽트 가능한 애니메이션 지원이 추가 되었습니다 van 제스처를 연결할 수 있습니다. Pleas 참조 Apple의 [UIViewAnimating 프로토콜 참조](https://developer.apple.com/reference/uikit/uiviewanimating)를 [UIViewPropertyAnimator 클래스 참조](https://developer.apple.com/reference/uikit/uiviewpropertyanimator)하십시오 [UITimingCurveProvider 프로토콜 참조](https://developer.apple.com/reference/uikit/uitimingcurveprovider)를 [UICubicTimingParameters 클래스 참조](https://developer.apple.com/reference/uikit/uicubictimingparameters) 하 고 [UISpringTimingParameter 클래스 참조](https://developer.apple.com/reference/uikit/uispringtimingparameters) 자세한 내용은 합니다.
- 새 `UIPreviewInteraction` 고 `UIPreviewInteractionDelegate` 앱 미리 보기 및 pop 작업에 대 한 사용자 지정 인터페이스를 제공 하도록 허용 합니다.
- 새 `UIAccessibilityCustomRotor` 클래스를 사용 하면 응용 프로그램에서 사용자 지정 상황에 맞는 기능 Voice Over와 같은 보조 기술을 제공 합니다.
- 사용 된 `UIAccessibilityIsAssistiveTouchRunning` 및 `UIAccessibilityAssistiveTouchStatusDidChangeNotification` AssistiveTouch 사용 되는지 여부를 결정 하는 기호입니다.
- 사용 된 `UIAccessibilityHearingDevicePairedEar` 및 `UIAccessibilityHearingDevicePairedEarDidChangeNotification` 의 상태를 가져오려면 기호 쌍을 이루는 MFi 청각 지원.
- 새 [UIPasteboard](https://developer.apple.com/reference/uikit/uipasteboard) API (예: 수명 제한 사항) 새 옵션을 제공 하 고 일반적인 클래스 형식에 대해 호환 되는 콘텐츠 형식에 자동으로 선언 됩니다.
- 를 지원 하기 위해 동적 형식 레이블에 텍스트 필드 및 입력란 사용 하 여 새 `PreferredFontForTextStyle` 메서드는 `UIFont` 클래스입니다.
- 경우 요소에 업데이트 해야 해당 글꼴을 결정 때 장치 `UIContentSizeCategory` 변경 내용을 사용 하 여는 `AdjustsFontForContentSizeCategory` 의 속성을 `UIContentSizeCategoryAdjusting` 대리자입니다.
- 이제 앱 모양의 예: 텍스트 및 배경 색 탭 표시줄 항목에 대 한 배지를 제어할 수 있습니다.
- 지금에서 새로 고침 지원 되는 모든 스크롤 보기와 스크롤 보기의 서브 클래스 (같은 `UICollectionView`).
- 합니다 `OpenURL` 메서드는 `UIApplication` 클래스 라고 비동기적으로 이제 열기 완료 된 후 호출 되는 완료 처리기를 지원 합니다.
- CloudKit 공유를 시작 하 고 새 속성을 수정할 `UICloudSharingController` 고 `UICloudSharingControllerDelegate` 클래스입니다.
- 프리페치된 셀의 스크롤 환경을 개선 하기 위해 활용 `UICollectionViews` 새 `UICollectionViewDataSourcePrefetching` 위임 합니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [TvOS 10의에서 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
