---
title: "추가 macOS 시에라 Framework 변경 내용"
description: "이 문서에서는 추가, 부 버전 변경 또는 macOS 시에라에 대 한 기존 프레임 워크의 향상 된 기능에 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: CA701269-D11E-4DE3-89C1-58EF8993A482
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: d49276d6367a52a05d486cd5cab198c666965bf7
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="additional-macos-sierra-framework-changes"></a>추가 macOS 시에라 Framework 변경 내용

_이 문서에서는 추가, 부 버전 변경 또는 macOS 시에라에 대 한 기존 프레임 워크의 향상 된 기능에 설명 합니다._

<a name="Accelerate-Framework-Enhancements" />

## <a name="accelerate-framework-enhancements"></a>프레임 워크의 향상 된 기능을 가속화

다음과 같은 향상 된 기능 macOS 시에라의 가속화 프레임 워크를 적용 했습니다.

- 추가 된 구적 (정수 계열 미적분 법).
- 신경망을 생성 하기 위한 기본 기능을 추가 합니다.
- 두 개의 기하학적 개체의 교차 부분 등을 위한 테스트 하려면 추가 기하학적 조건자 함수입니다.

<a name="AppKit-Framework-Enhancements" />

## <a name="appkit-framework-enhancements"></a>Appkit 프레임 워크의 향상 된 기능

다음과 같은 향상 된 기능 macOS 시에라의 AppKit 프레임 워크를 적용 했습니다.

- 여러 가지 `NSCollectionView` 같은:
    - **축소 가능한 섹션** -가로 행으로 컬렉션 뷰 섹션을 축소 수 있습니다.
    - **부동 머리글** -머리글 및 바닥글 수 이제 수 (에 이동식 선형 레이아웃)와 동일한 API를 사용 하 여 [UICollectionView](https://developer.apple.com/reference/uikit/uicollectionview) iOS에서 합니다.
    - **스크롤할 수 있는 배경 뷰** -컬렉션 뷰 배경 내용과 함께 스크롤해야 설정할 수 있습니다.
- 지연 된 보기 레이아웃 단계 최적화 되었으며 확장 합니다.
- 끌어서 놓기 API는 이제 새 포함 `NSFilePromiseProvider` 및 `NSFilePromiseReceiver` 떼지 지 원하는 클래스를 끕니다.
- 여러 편의 생성자 기존 컨트롤에 추가 되었습니다.
    -  `NSButton` 누름 단추를 만들기 위한 새 생성자를 포함 확인란 및 라디오 단추입니다.
    -  `NSTextField` 배치 하 고 비 래핑 레이블, 특성 사용된 하는 레이블 및 편집 가능한 텍스트 필드를 만드는 새 생성자를 포함 합니다.
    -  `NSSegmentedControl` 일반적으로 레이블 또는 이미지 그룹에서 조각화 된 컨트롤을 만드는 새 생성자를 포함 합니다.
    -  `NSSlider` 가로 선형 슬라이더를 만들기 위한 새 생성자를 포함 합니다.
    -  `NSImageView` 뷰를 편집할 수 없는 이미지를 만들기 위한 새 생성자를 포함 한 특정 `NSImage`합니다.
-  새 `NSGridView` 자동 레이아웃 변수로 표에 하위 뷰의 컬렉션 크기가 동적으로 숨기 거 나 표시 수는 행과 열에 추가 되었습니다.

<a name="AVFoundation-Framework-Enhancements" />

## <a name="avfoundation-framework-enhancements"></a>AVFoundation 프레임 워크의 향상 된 기능

다음과 같은 향상 된 기능 macOS 시에라의 AVFoundation 프레임 워크를 적용 했습니다.

- MacOS 등의 응용 프로그램 더 이상에 다른 구현 하려면 [AVPlayerItem](https://developer.apple.com/reference/avfoundation/avplayeritem) 콘텐츠 형식을 기반으로 동작 합니다. 설정 하기만 `Rate` 속성과 AVFoundation 정지 하지 않고 재생을 위해 사용할 수 있는 충분 한 콘텐츠를 때 결정 됩니다.
- 새 `AVPlayerLooper` 클래스를 사용 하면 보다 쉽게 재생 하는 동안 지정된 된 부분 미디어에 루프 수 있습니다.
- `AVAssetDownloadURLSession` 다운로드에 대 한 클래스를 사용 하 고 FairPlay의 나중에 재생 HLS 스트림을 암호화 합니다.

<a name="Core-Data-Framework-Enhancements" />

## <a name="core-data-framework-enhancements"></a>핵심 데이터 프레임 워크 향상 된 기능

MacOS 시에라에 대 한 핵심 데이터 프레임 워크에 다음 기능을 적용 했습니다.

- 루트 [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) 동시 오류가 있는 및 serialization 없이 인출 개체 지원 합니다.
- [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) 클래스 SQLite 데이터 저장소의 풀을 관리 합니다.
- [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) 이후를 가져오기 위해 특정 데이터베이스 버전을 관리 하는 개체 컨텍스트 (모드)에 고정 수 새 쿼리 생성 WAL 저널 모드 지원에서 SQLite 데이터 저장소를 사용 하 여 개체 기능 및 오류가 있는 트랜잭션.
- 상위 수준를 사용 하 여 `NSPersistenceContainer` 참조에는 `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) 및 기타 핵심 데이터 구성 리소스입니다.
- 에 추가 된 몇 가지 새로운 편의 메서드 `NSManagedObject` 쉽게 인출을 수행 하 고 하위 클래스를 만들 수 있습니다.

자세한 내용은 Apple의를 참조 하십시오 [핵심 데이터 프레임 워크 참조](https://developer.apple.com/reference/coredata)합니다.

<a name="Core-Image-Framework-Enhancements" />

## <a name="core-image-framework-enhancements"></a>코어 이미지 프레임 워크 향상 된 기능

MacOS 시에라에 대 한 핵심 이미지 프레임 워크에 다음 기능을 적용 했습니다.

- `ImageWithExtent` 의 메서드는 [CIFilter](https://developer.apple.com/reference/coreimage/cifilter) 필터 작업에 사용자 지정 처리를 삽입 하려면 클래스를 사용할 수 있습니다. Core 이미지 출력에 대 한 이미지를 처리할 때 필터 간에 지정 된 콜백을 호출 되거나 표시 됩니다.
- 이제 응용 프로그램 처리 전후 색 공간 축소 변환 하 여 작업 색 공간 Core 이미지 컨텍스트 외부에서 색 공간에서 이미지를 처리할 수 있습니다.
- Core 이미지 커널 특정 픽셀 출력 형식을 요청 수입니다.
- 추가 된 다음 새 이미지 필터: `CINinePartTitled`, `CINinePartStretched`, `CIHueSaturationValueGradient`, `CIEdgePreserveUpsampleFilter` 및 `CIClamp`합니다.

<a name="Foundation-Framework-Enhancements" />

## <a name="foundation-framework-enhancements"></a>Foundation 프레임 워크의 향상 된 기능

다음과 같은 향상 된 기능 macOS 시에라의 Foundation 프레임 워크를 적용 했습니다.

- 사용 하 여는 [NSDimentions](https://developer.apple.com/reference/foundation/nsdimension) 길이, 속도, 기간 및 온도 표현, 변환 및와 같은 다양 한 가장 일반적인 물리적 단위를 표시 하기 위한 API 질량 합니다.
- 사용 하 여는 [NSISO8601DateFormatter](https://developer.apple.com/reference/foundation/nsiso8601dateformatter) 서식 지정 된 날짜로 구문 분석 하 고 ISO 8601을 생성 하기 위한 클래스입니다.
- 새로운 [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) 을 간격을 비교 하 고 간격 교차점에 대 한 테스트 기간, 같은 날짜 및 시간 간격 계산을 수행 하는 클래스입니다.
- 사용 하 여는 [NSPersonNameComponentsFormatter](https://developer.apple.com/reference/foundation/nspersonnamecomponentsformatter) 문자열에서 개인의 이름 요소를 구문 분석 하는 클래스입니다.
- 새로운 [NSURLSessionTaskMetrics](https://developer.apple.com/reference/foundation/nsurlsessiontaskmetrics) 클래스를 세션 네트워킹 URL에 대 한 메트릭을 가져옵니다.

자세한 내용은 Apple의를 참조 하십시오 [10 iOS 및 OS X v10.12 Foundation 릴리스 정보](https://developer.apple.com/library/prerelease/content/releasenotes/Miscellaneous/RN-Foundation-OSX10.12/index.html)합니다.

<a name="GameKit-Framework-Enhancements" />

## <a name="gamekit-framework-enhancements"></a>GameKit 프레임 워크의 향상 된 기능

다음과 같은 향상 된 기능 macOS 시에라의 GameKit 프레임 워크를 적용 했습니다.

- **게임 센터 앱** 사용 되지 않으며 macOS에서 제거 되었습니다. 응용 프로그램 GameKit를 사용 하는 경우 그 _해야_ 순위표 등과 같은 GameKit 기능을 표시 하는 자체 인터페이스를 제공 합니다. 
- 새 iCloud 전용 계정 유형을 구현한 다음에 여는 [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) 클래스입니다.
- 새 [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) 클래스는 게임 센터에서 영구 데이터 저장소 관리를 위한 일반화 된 솔루션을 제공 합니다. `GKGameSession` 플레이어의 목록을 유지 관리 및 응용 프로그램은 책임 폼 찾거나 플레이어 사이 교환 참가자 날짜 저장 방법과 시기를 구현 합니다. 대부분의 경우에서 게임 세션 기존 턴 기반 일치 항목, 실시간 일치 또는 영구 게임 메서드 저장 바꿀 수 있습니다.

<a name="GamePlayKit-Framework-Enhancements" />

## <a name="gameplaykit-framework-enhancements"></a>GamePlayKit 프레임 워크의 향상 된 기능

다음과 같은 향상 된 기능 macOS 시에라의 GamePlayKit 프레임 워크를 적용 했습니다.

- 절차적 노이즈 세대 개가 추가 되 고 자연 스러운 한 질감에서 현실감을 높일 카메라 이동을 현실감을 추가 하 고 풍부한 게임 세계를 생성 하는 데 도움이 데 사용할 수 있습니다.
- 데이터 분할 하는 게임 세계 효율적인 검색을 위해 공간 분할을 사용 합니다.
- 새 몬테카를로 전략가 ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) 완전 한 목록이 가능한 이동 계산을 위해 추가 되었습니다.
- 새로운 의사 결정 트리 API가 추가 되었습니다 ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) 및 [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) 게임 작성 AI 향상을 위해.
- 기존 에이전트 및 new를 사용 하는 경로 찾기 동작에 3D 지원이 추가 되었습니다 [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) 및 [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) 클래스입니다.
- 새로운 [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) 고성능, 자연 스러운 경로 제공 하는 클래스입니다.
- 새 [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) 및 [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) 클래스 만들기 GameplayKit 및 그 어느 때 보다 쉽게 SpriteKit 결합 합니다.

<a name="Metal-Framework-Enhancements" />

## <a name="metal-framework-enhancements"></a>금속 프레임 워크의 향상 된 기능

다음과 같은 향상 된 기능 macOS 시에라의 금속 프레임 워크를 적용 했습니다.

- 3D 앱과 게임 이제 사용할 수 _공간 분할_ 복잡 한 장면와 GPU 통해 기 하 도형을 효율적으로 렌더링 합니다.
- 함수 특수화를 사용 하 여 자재 및 밝은 조합 기능은 장면에 대 한 최적화 된 컬렉션을 만듭니다.
- 힙 리소스를 사용 하 여 앱 및 Memoryless 렌더링 대상을 기반으로 하는 금속의 성능을 최적화 하기 위해 리소스 할당의 세분화 된 제어를 제공 합니다.

자세한 내용은 Apple의를 참조 하십시오 [금속 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221)합니다.

<a name="ModelIO-Framework-Enhancements" />

## <a name="model-io-framework-enhancements"></a>모델 I/O 프레임 워크의 향상 된 기능

다음과 같은 향상 된 기능 macOS 시에라에 대 한 모델 I/O 프레임 워크를 적용 했습니다.

- 이제 USD 파일 형식이 지원 됩니다.
- 새로운 `MDLMaterialPropertyGraph` 클래스를 쉽게 런타임에서 지 원하는 모델을 변경 합니다.
- 서명 지원이 추가 되었습니다 거리 필드는 [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) 클래스입니다.
- 새로운 `MDLLightProbeIrradianceDataSource` Light 프로브 배치에서 지 원하는 클래스를 합니다.

<a name="Photos-Framework-Enhancements" />

## <a name="photos-framework-enhancements"></a>사진 프레임 워크의 향상 된 기능

다음과 같은 향상 된 기능 macOS 시에라의 사진 프레임 워크를 적용 했습니다.

- 라이브 사진 편집은 이제 사진 프레임 워크를 지 원하는 앱 하며 사진 확장 편집 (사진 및 카메라 내부에서 사용 하기 위해 응용 프로그램).
- 새로운 [PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext) 비디오와 여전히 사진 라이브 콘텐츠를 편집 내용을 적용 하는 클래스입니다.
- 사용 하 여는 [CIImageProcessorInput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput) 및 [CIImageProcessorOutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput) 편집 작업을 수행 하는 새로운 Core 이미지 프로세서 기능을 활용 하는 클래스입니다.
- 라이브 사진을 지원 하기 위해는 [PHLivePhoto](https://developer.apple.com/reference/photos/phlivephoto) 및 [PHLivePhotoView](https://developer.apple.com/reference/photosui/phlivephotoview) 클래스 macOS 하기 iOS에서 이식 되었습니다.

<a name="SceneKit-Framework-Enhancements" />

## <a name="scenekit-framework-enhancements"></a>SceneKit 프레임 워크의 향상 된 기능

다음과 같은 향상 된 기능 macOS 시에라의 SceneKit 프레임 워크를 적용 했습니다.

- 이제 더 간단한 자산 작성 좀 더 현실적인 결과 대 한 새 물리적으로 기반 렌더링 (PBR) 시스템을 포함합니다.
- 새로운 [SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) 세 개의 기본 속성을 사용 하면서 다양 한 범위의 실제 음영 효과 제품 모델 음영 (`Diffuse`, `Metalness` 및 `Roughness`).
- 이후 PBR 조명 환경을 기반으로 가장 잘 작동을 음영을 사용 하 여는 `LightingEnvironment` 속성을 전체 장면 황갈색 조명 이미지 기반 할당을 합니다.
- 사용 하 여 `IESProfileURL` 온도 켈빈) (에서 색 및 루멘) (의 강도 등의 실제 값에 기본 조명을 정의 하는 실제 밝은 설비를 가져올 속성을 합니다.
- [SCNCamera](https://developer.apple.com/reference/scenekit/scncamera) 클래스 HDR 기능과 효과 사용 하 여 큰 현실감을 제공할 수 있습니다. 자동 효과 사용 비네팅 또는 색 번짐과 및 게임에 filmatic 효과 추가 하려면 그레이 딩 색 만들려면 적응 노출을 사용 합니다.
- PBR와 HDR 카메라 기능이 기존의 렌더링 기술 보다 나은 결과 제공 하 고, 결과적으로, SceneKit 지금 수행 모든 색 계산 선형 색 공간 (와이드 색 장치 디스플레이에 P3 색 영역 사용).
- SceneKit 지금 색 색 프로필 정보를 참조 하 여 모든 색 일치 합니다.
- SceneKit 모든 셰이더 유형에 대 한 선형 RGB 색 공간에서 색상 구성 요소 값을 해석합니다.
- SceneKit 읽고 질감 이미지의 색상 프로필 정보에 대 한 조정를 하므로이 정보는 제공 하려면 모든 이미지에 대 한 자산 카탈로그를 사용 합니다.
- 선형 둘 다 색 공간 렌더링 및 와이드 색을 지정 하 여 해제할 수 있습니다는 `SCNDisableLinearSpaceRendering` 및 `SCNDisableWideGamut` 의 응용 프로그램의 키 `Info.plist`합니다.
- 빌드 (또는 파일에서 로드 한 프로그래밍 방식으로 생성) 임의의 다각형 볼 새 geometry를 지정 하려면 [SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon) 클래스입니다.

<a name="Security-Framework-Enhancements" />

## <a name="security-framework-enhancements"></a>보안 프레임 워크의 향상 된 기능

다음과 같은 향상 된 기능 macOS 시에라에 대 한 보안 프레임 워크를 적용 했습니다.

- `SecKey` 인터페이스 현대화 된 되 고 모든 플랫폼 (iOS, tvOS, watchOS 및 macOS)에서 통합 합니다.

<a name="SpriteKit-Framework-Enhancements" />

## <a name="spritekit-framework-enhancements"></a>SpriteKit 프레임 워크의 향상 된 기능

다음과 같은 향상 된 기능 macOS 시에라의 SpriteKit 프레임 워크를 적용 했습니다.

- Tilemaps 2D, 2.5 D 및 사용 하 여 측 스크롤 게임에 대 한 사각형, 6 각형 및 아이소메트릭 타일 셰이프는 이제 지원는 `SKTileMapMode`, `SKTileGroup`, `SKTileGroupRule` 및 `SKTileSet` 클래스입니다.
- 새로운 `SKWarpGeometry` 을 늘이거나 왜곡 클래스 [SKSpriteNode](https://developer.apple.com/reference/spritekit/skspritenode) 또는 [SKEffectNode](https://developer.apple.com/reference/spritekit/skeffectnode) 렌더링 합니다. 새 [SKAction](https://developer.apple.com/reference/spritekit/skaction) 클래스를 사용 하 여 warp 효과 간의 전환 애니메이션 효과를 줄 수 있습니다.
- 사용자 지정 셰이더 특성을 제공할 수 있습니다 (`SKAttribute`) 특성 값을 제공 하 여 셰이더를 사용 하는 각 노드에 별도로 구성할 수 있는 (`SKAttributeValue`).
- [SKView](https://developer.apple.com/reference/spritekit/skview) 클래스 장면의 렌더링 되는 시기와 방법을 세부적으로 제어할 수 있도록 몇 가지 새로운 메서드를 제공 합니다.

<a name="New-Frameworks" />

## <a name="new-frameworks"></a>새 프레임 워크

다음 프레임 워크 macOS 시에라에 추가 되었습니다.

- **의도 프레임 워크** -이 프레임 워크에는 앱이 상호 작용 (예: 위치 또는 사용자 작업)을 점검 하 여 해당 정보에 따라 조치를 취할 수 있게 합니다.
- **SafariServices 프레임 워크** -이 프레임 워크 모두 macOS 및 iOS 용 Safari (예: 콘텐츠 블로 커가)에 대 한 확장을 응용 프로그램을 개발 하는 앱을 허용 합니다.

## <a name="related-links"></a>관련 링크

- [Mac 샘플](https://developer.xamarin.com/samples/mac/)
- [OS X 10.12의 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
