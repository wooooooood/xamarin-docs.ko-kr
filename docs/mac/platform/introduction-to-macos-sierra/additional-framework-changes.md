---
title: 추가 macOS Sierra 프레임워크 변경 내용
description: 이 문서는 사소한 변경 및 macOS Sierra에에서 도입 된 기존 프레임 워크의 향상 된 기능을 설명 합니다. 획기적인 프레임 워크, AppKit, AVFoundation, 핵심 데이터, Core 이미지, Foundation 및 자세한 변경 내용은 검사합니다.
ms.prod: xamarin
ms.assetid: CA701269-D11E-4DE3-89C1-58EF8993A482
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 6ed827c018931e5b79887dc355f136e2a84623d6
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61031629"
---
# <a name="additional-macos-sierra-framework-changes"></a>추가 macOS Sierra 프레임워크 변경 내용

<a name="Accelerate-Framework-Enhancements" />

## <a name="accelerate-framework-enhancements"></a>Framework의 향상 된 가속화

다음 기능을 가속화 하기 위한 프레임 워크 macOS Sierra 적용 했습니다.

- 추가 구적 (정수 계산법).
- 신경망을 생성 하기 위한 기본 기능을 추가 합니다.
- 추가 기하학적 조건자 함수를 두 기하학적 개체의 교집합 등을 위해 테스트 합니다.

<a name="AppKit-Framework-Enhancements" />

## <a name="appkit-framework-enhancements"></a>AppKit 프레임 워크 향상 된 기능

MacOS Sierra에 대 한 AppKit 프레임 워크에 다음 기능을 적용 했습니다.

- 여러 개선 된 기능 `NSCollectionView` 같은:
    - **축소 가능한 섹션** -단일 가로 행으로 컬렉션 뷰 섹션을 축소할 수 있습니다.
    - **헤더를 부동** -머리글 및 바닥글 수 이제 놓을지 (선형 레이아웃)에서 동일한 API를 사용 하 여 [UICollectionView](https://developer.apple.com/reference/uikit/uicollectionview) iOS에서.
    - **스크롤할 수 있는 배경 뷰** -컬렉션 뷰 배경이 콘텐츠와 함께 스크롤해야 이제 설정할 수 있습니다.
- 지연 된 보기 레이아웃 패스에 최적화 되었으며 확장 합니다.
- 끌어서 놓기 API는 이제 새 포함 `NSFilePromiseProvider` 고 `NSFilePromiseReceiver` 떼지을 지 원하는 클래스를 끌어 옵니다.
- 몇 가지 편의 생성자 기존 컨트롤에 추가 되었습니다.
    -  `NSButton` 누름 단추를 만들기 위한 새 생성자를 포함 확인란 및 라디오 단추입니다.
    -  `NSTextField` 줄 바꿈 및 줄 바꿈 이외의 레이블, 특성이 지정 된 레이블 및 편집 가능한 텍스트 필드를 만들기 위한 새 생성자를 포함 합니다.
    -  `NSSegmentedControl` 레이블 또는 이미지 그룹에서 분할 된 컨트롤을 만들기 위한 새 생성자를 포함 합니다.
    -  `NSSlider` 가로 선형 슬라이더를 만들기 위한 새 생성자를 포함 합니다.
    -  `NSImageView` 편집할 수 없는 이미지 뷰를 만들기 위한 새 생성자를 포함 한 지정 된 `NSImage`합니다.
-  새 `NSGridView` 하위 뷰의 컬렉션 변수를 사용 하 여 표 형태에 동적으로 숨기 거 나 표시 수는 행과 열 크기는 자동 레이아웃에 추가 되었습니다.

<a name="AVFoundation-Framework-Enhancements" />

## <a name="avfoundation-framework-enhancements"></a>AVFoundation 프레임 워크 향상 된 기능

Macos Sierra AVFoundation 프레임 워크에 다음 기능을 적용 했습니다.

- MacOS에서 앱에 더 이상 다른 구현 [AVPlayerItem](https://developer.apple.com/reference/avfoundation/avplayeritem) 콘텐츠 형식을 기반으로 동작 합니다. 설정 된 `Rate` 속성과 AVFoundation 상태일 없이 재생에 사용할 수 있는 충분 한 콘텐츠는 경우 결정 됩니다.
- 새 `AVPlayerLooper` 클래스 쉽게 재생 하는 동안 지정 된 부분 미디어를 반복 합니다.
- `AVAssetDownloadURLSession` 다운로드에 대 한 클래스를 사용 하 고 나중에 재생 FairPlay의 HLS 스트림도 암호화 합니다.

<a name="Core-Data-Framework-Enhancements" />

## <a name="core-data-framework-enhancements"></a>핵심 데이터 프레임 워크 향상 된 기능

MacOS Sierra에 대 한 핵심 데이터 프레임 워크에 다음 기능을 적용 했습니다.

- 루트 [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) 동시 오류 및 직렬화 없이 가져오는 개체를 지원 합니다.
- 합니다 [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) 클래스 SQLite 데이터 저장소의 풀을 관리 합니다.
- 합니다 [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) WAL 저널 모드 지원 새 쿼리 생성에에서 SQLite 데이터 저장소를 사용 하 여 개체 관리 되는 개체 컨텍스트 (MOC) 이후 페치 하기 위해 특정 데이터베이스 버전에 고정할 수 있습니다 위치 기능 및 오류가 있는 트랜잭션.
- 사용 하 여 대략적 `NSPersistenceContainer` 참조를 `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) 및 다른 핵심 데이터 구성 리소스.
- 에 추가 된 편리한 몇 가지 새로운 메서드 `NSManagedObject` 쉽게 페치를 수행 하 고 서브 클래스를 만들 수 있습니다.

자세한 내용은 Apple의를 참조 하세요 [핵심 데이터 프레임 워크 참조](https://developer.apple.com/reference/coredata)합니다.

<a name="Core-Image-Framework-Enhancements" />

## <a name="core-image-framework-enhancements"></a>Core 이미지 프레임 워크 향상 된 기능

MacOS Sierra에 대 한 핵심 이미지 프레임 워크에 다음 기능을 적용 했습니다.

- 합니다 `ImageWithExtent` 메서드를 [CIFilter](https://developer.apple.com/reference/coreimage/cifilter) 필터 작업에 사용자 지정 처리를 삽입 하는 클래스를 사용할 수 있습니다. Core 이미지 출력에 대 한 이미지를 처리 하는 경우 필터 간에 지정 된 콜백을 호출 되거나 표시 됩니다.
- 이제 앱 처리 전후 색 공간에서 변환 하 여 색 공간을 Core 이미지 컨텍스트의 작업 색 공간 외부에서 이미지를 처리할 수 있습니다.
- Core 이미지 커널 특정 픽셀 출력 형식을 요청할 수 있습니다.
- 다음 새 이미지 필터를 추가한: `CINinePartTitled`, `CINinePartStretched`를 `CIHueSaturationValueGradient`, `CIEdgePreserveUpsampleFilter` 고 `CIClamp`입니다.

<a name="Foundation-Framework-Enhancements" />

## <a name="foundation-framework-enhancements"></a>Foundation 프레임 워크 향상 된 기능

Macos Sierra Foundation 프레임 워크에 다음 기능을 적용 했습니다.

- 사용 된 [NSDimentions](https://developer.apple.com/reference/foundation/nsdimension) 표현, 변환 및와 같은 다양 한 가장 일반적인 물리적 단위를 표시 하기 위한 API 질량, 길이, 속도, 기간 및 온도입니다.
- 사용 된 [NSISO8601DateFormatter](https://developer.apple.com/reference/foundation/nsiso8601dateformatter) 서식 지정 된 날짜로 구문 분석 및 ISO 8601을 생성 하기 위한 클래스입니다.
- 새 [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) 기간, 간격을 비교 하 고 간격 교차점에 대 한 테스트와 같은 날짜 및 시간 간격 계산을 수행 하는 클래스입니다.
- 사용 된 [NSPersonNameComponentsFormatter](https://developer.apple.com/reference/foundation/nspersonnamecomponentsformatter) 사람의 이름 문자열에서 요소를 구문 분석 합니다.
- 새 [NSURLSessionTaskMetrics](https://developer.apple.com/reference/foundation/nsurlsessiontaskmetrics) 세션 네트워킹 URL에 대 한 메트릭을 얻으려면 클래스입니다.

자세한 내용은 Apple의를 참조 하세요 [v10.12 OS X 및 iOS 10에 대 한 Foundation 릴리스](https://developer.apple.com/library/prerelease/content/releasenotes/Miscellaneous/RN-Foundation-OSX10.12/index.html)합니다.

<a name="GameKit-Framework-Enhancements" />

## <a name="gamekit-framework-enhancements"></a>GameKit 프레임 워크 향상 된 기능

Macos Sierra GameKit 프레임 워크에 다음 기능을 적용 했습니다.

- 합니다 **게임 센터 앱** 사용 되지 않으며 macOS에서 제거 되었습니다. 앱 GameKit를 사용 하는 경우 해당 _해야_ 순위표 등 GameKit 기능을 표시 하는 자체 인터페이스를 제공 합니다. 
- 새 iCloud 전용 계정 유형을 구현에 [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) 클래스입니다.
- 새 [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) 클래스 Game Center에서 영구 데이터 저장소 관리를 위한 일반화 된 솔루션을 제공 합니다. `GKGameSession` 플레이어의 목록을 유지 관리 및 앱은 책임 폼 검색 또는 플레이어 간에 교환 참가자 날짜 저장 되는 방법 및 시기를 구현 합니다. 대부분의 기존 턴 기반 일치, 실시간 일치 또는 메서드 저장 영구 게임 게임 세션 바꾸면 됩니다.

<a name="GamePlayKit-Framework-Enhancements" />

## <a name="gameplaykit-framework-enhancements"></a>GamePlayKit 프레임 워크 향상 된 기능

Macos Sierra GamePlayKit 프레임 워크에 다음 기능을 적용 했습니다.

- 절차적 노이즈 추가한 생성과 자연 스러운 질감에 현실성 향상 카메라 이동을에 현실성을 추가 하 고 풍부한 게임 세계를 생성 하는 데 도움이 데 사용할 수 있습니다.
- 공간 분할을 사용 하 여 효율적인 검색을 위한 게임 데이터를 분할 합니다.
- 새 Monte Carlo 전략가 ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) 철저 한 가능한 이동 계산을 위해 추가 되었습니다.
- 새 의사 결정 트리 API가 추가 되었습니다 ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) 하 고 [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) 게임 작성 인공 지능을 향상 시키기 위해.
- 기존 에이전트를 사용 하 여 새 경로 찾기 동작 3D 지원이 추가 되었습니다 [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) 하 고 [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) 클래스입니다.
- 새 [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) 성능이 우수 하 고 자연 스러운 경로 제공 하는 클래스입니다.
- 새 [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) 하 고 [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) GameplayKit 및 SpriteKit 그 어느 때 보다 쉽게 결합 하는 클래스 확인 합니다.

<a name="Metal-Framework-Enhancements" />

## <a name="metal-framework-enhancements"></a>금속 프레임 워크 향상 된 기능

Macos Sierra 금속 프레임 워크에 다음 기능을 적용 했습니다.

- 3D 앱과 게임 이제 사용할 수 있습니다 _공간 분할_ 효율적으로 복잡 한 장면을 만들 및 GPU 통해 기 하 도형에 렌더링할 수 있습니다.
- 함수 특수화를 사용 하 여 장면에 대 한 간단한 조합 함수와 자료의 최적화 된 컬렉션을 만듭니다.
- 힙 리소스를 사용 하 여 앱 및 Memoryless 렌더링 대상을 기반으로 하는 체제 미 설치 컴퓨터의 성능을 최적화 하기 위해 리소스 할당의 세분화 된 제어를 제공 합니다.

자세한 내용은 Apple의를 참조 하세요 [Metal 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221)합니다.

<a name="ModelIO-Framework-Enhancements" />

## <a name="model-io-framework-enhancements"></a>모델 I/O 프레임 워크 향상 된 기능

MacOS Sierra에 대 한 모델 I/O 프레임 워크에 다음 기능을 적용 했습니다.

- USD 파일 형식은 이제 지원 됩니다.
- 새 `MDLMaterialPropertyGraph` 클래스를 쉽게 런타임에서 지 원하는 모델을 변경 합니다.
- 서명 지원이 추가 되었습니다 거리 필드를 [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) 클래스입니다.
- 새 `MDLLightProbeIrradianceDataSource` Light 프로브 배치 하는 데 도움이 되는 클래스입니다.

<a name="Photos-Framework-Enhancements" />

## <a name="photos-framework-enhancements"></a>사진 프레임 워크 향상 된 기능

MacOS Sierra에 대 한 사진 프레임 워크에 다음 기능을 적용 했습니다.

- 사진 편집 확장 하는 사진 프레임 워크를 지 원하는 앱에 대 한 출시 되었습니다 라이브 사진 편집 (사진 및 카메라 내부 사용에 대 한 앱).
- 새 [PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext) 비디오와 여전히 사진 라이브 콘텐츠를 편집을 적용 하는 클래스입니다.
- 사용 합니다 [CIImageProcessorInput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput) 하 고 [CIImageProcessorOutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput) 편집을 수행 하는 새로운 Core 이미지 프로세서 기능을 활용 하는 클래스입니다.
- 사진 Live 지원에 [PHLivePhoto](https://developer.apple.com/reference/photos/phlivephoto) 및 [PHLivePhotoView](https://developer.apple.com/reference/photosui/phlivephotoview) 클래스 macOS에 iOS에서 이식 되었습니다.

<a name="SceneKit-Framework-Enhancements" />

## <a name="scenekit-framework-enhancements"></a>SceneKit 프레임 워크 향상 된 기능

MacOS Sierra 용 SceneKit 프레임 워크에 다음 기능을 적용 했습니다.

- 이제 간단한 자산 작성 보다 현실적인 결과 대 한 새 물리적 기반 렌더링 (PBR) 시스템을 포함합니다.
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

<a name="Security-Framework-Enhancements" />

## <a name="security-framework-enhancements"></a>보안 프레임 워크 향상 된 기능

MacOS Sierra에 대 한 보안 프레임 워크에 다음 기능을 적용 했습니다.

- `SecKey` 인터페이스를 현대화 하 고 모든 플랫폼 (iOS, tvOS, watchOS 및 macOS)에서 통합 되었습니다.

<a name="SpriteKit-Framework-Enhancements" />

## <a name="spritekit-framework-enhancements"></a>SpriteKit 프레임 워크 향상 된 기능

MacOS Sierra 용 SpriteKit 프레임 워크에 다음 기능을 적용 했습니다.

- 이제 Tilemaps 2D, 2.5 D 및 사용 하 여 쪽 스크롤 게임에 대 한 사각형, 육각 및 등각 타일 셰이프를 지원 합니다 `SKTileMapMode`, `SKTileGroup`를 `SKTileGroupRule` 및 `SKTileSet` 클래스입니다.
- 새 `SKWarpGeometry` 늘이거나 왜곡 하는 클래스 [SKSpriteNode](https://developer.apple.com/reference/spritekit/skspritenode) 하거나 [SKEffectNode](https://developer.apple.com/reference/spritekit/skeffectnode) 렌더링 합니다. 새 [SKAction](https://developer.apple.com/reference/spritekit/skaction) 클래스 warp 효과 간의 전환에 애니메이션 효과를 사용할 수 있습니다.
- 사용자 지정 셰이더 특성을 제공할 수 있습니다 (`SKAttribute`) 특성 값을 제공 하 여 셰이더를 사용 하는 각 노드에 별도로 구성할 수 있는 (`SKAttributeValue`).
- 합니다 [SKView](https://developer.apple.com/reference/spritekit/skview) 클래스 장면 렌더링 되는 방법과 시기를 세부적으로 제어할 수 있도록 몇 가지 새로운 메서드를 제공 합니다.

<a name="New-Frameworks" />

## <a name="new-frameworks"></a>새 프레임 워크

다음 프레임 워크 macOS Sierra에 추가 되었습니다.

- **인 텐트 Framework** -이 프레임 워크 (예: 위치 또는 사용자 작업), 상호 작용을 검토 하 고 해당 정보를 기반으로 하는 작업을 수행 하도록 앱을 허용 합니다.
- **SafariServices Framework** -이 프레임 워크 모두 macOS 및 iOS 용 safari 콘텐츠 차단) (예: 앱 확장을 개발 하도록 앱을 허용 합니다.

## <a name="related-links"></a>관련 링크

- [Mac 샘플](https://developer.xamarin.com/samples/mac/)
- [OS X 10.12의 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
