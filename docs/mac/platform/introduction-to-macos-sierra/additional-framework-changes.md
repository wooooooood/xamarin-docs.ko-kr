---
title: 추가 macOS Sierra 프레임워크 변경 내용
description: 이 문서에서는 macOS Sierra에 도입 된 기존 프레임 워크의 일부 변경 내용 및 향상 된 기능에 대해 설명 합니다. 가속화 된 프레임 워크, AppKit, AVFoundation, 핵심 데이터, 핵심 이미지, 기반 등의 변경 내용을 검사 합니다.
ms.prod: xamarin
ms.assetid: CA701269-D11E-4DE3-89C1-58EF8993A482
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 03840e127ba53ee63623252585e51e51e6890eb5
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571599"
---
# <a name="additional-macos-sierra-framework-changes"></a>추가 macOS Sierra 프레임워크 변경 내용

<a name="Accelerate-Framework-Enhancements"></a>

## <a name="accelerate-framework-enhancements"></a>향상 된 프레임 워크 향상

MacOS Sierra의 가속화 된 프레임 워크에는 다음과 같은 기능이 향상 되었습니다.

- Quadrature (정수 미적분학)가 추가 되었습니다.
- 신경망을 구성 하기 위한 기본 기능이 추가 되었습니다.
- 두 기하학적 개체가 교차 하는 것과 같은 항목을 테스트 하는 기 하 도형 조건자 함수를 추가 했습니다.

<a name="AppKit-Framework-Enhancements"></a>

## <a name="appkit-framework-enhancements"></a>AppKit 프레임 워크의 향상 된 기능

MacOS Sierra AppKit 프레임 워크에 대 한 다음과 같은 기능이 향상 되었습니다.

- 다음과 같은 몇 가지 향상 된 기능이 `NSCollectionView` 있습니다.
  - **축소 가능한 섹션** -사용자가 컬렉션 뷰 섹션을 단일 가로 행으로 축소할 수 있습니다.
  - **부동 헤더** -이제 IOS에서 [UICOLLECTIONVIEW](https://developer.apple.com/reference/uikit/uicollectionview) 와 동일한 API를 사용 하 여 헤더 및 바닥글을 선형 레이아웃으로 이동할 수 있습니다.
  - **스크롤 가능한 배경 보기** -이제 콘텐츠와 함께 스크롤하도록 컬렉션 뷰 배경을 설정할 수 있습니다.
- 지연 된 뷰 레이아웃 패스가 최적화 되 고 확장 되었습니다.
- 이제 끌어서 놓기 API에는 `NSFilePromiseProvider` `NSFilePromiseReceiver` 끌기 flocking 지원 하기 위한 새로운 및 클래스가 포함 되어 있습니다.
- 기존 컨트롤에 몇 가지 편리한 생성자를 추가 했습니다.
  - `NSButton`에는 누름 단추, 확인란 및 라디오 단추를 만들기 위한 새로운 생성자가 포함 되어 있습니다.
  - `NSTextField`줄 바꿈 및 줄 바꿈하지 않는 레이블, 특성 사용 레이블 및 편집 가능한 텍스트 필드를 만들기 위한 새로운 생성자를 포함 합니다.
  - `NSSegmentedControl`에는 레이블이나 이미지 그룹에서 분할 된 컨트롤을 만들기 위한 새로운 생성자가 포함 되어 있습니다.
  - `NSSlider`에는 가로 선형 슬라이더를 만들기 위한 새로운 생성자가 포함 되어 있습니다.
  - `NSImageView`지정 된에서 편집할 수 없는 이미지 뷰를 만들기 위한 새 생성자를 포함 `NSImage` 합니다.
- 동적으로 `NSGridView` 숨기 거 나 표시할 수 있는 가변 크기의 행과 열이 있는 그리드에 하위 뷰의 컬렉션을 자동으로 레이아웃 하기 위해 새가 추가 되었습니다.

<a name="AVFoundation-Framework-Enhancements"></a>

## <a name="avfoundation-framework-enhancements"></a>AVFoundation 프레임 워크의 향상 된 기능

MacOS Sierra에 대 한 AVFoundation 프레임 워크에 대 한 다음과 같은 기능이 향상 되었습니다.

- MacOS에서 앱은 더 이상 콘텐츠 형식에 따라 서로 다른 [AVPlayerItem](https://developer.apple.com/reference/avfoundation/avplayeritem) 동작을 구현할 필요가 없습니다. 속성을 설정 하기만 하면 `Rate` 상태일 없이 재생할 수 있는 콘텐츠가 충분 한 경우 AVFoundation에서 결정 합니다.
- 새 `AVPlayerLooper` 클래스를 사용 하면 재생 하는 동안 지정 된 미디어 부분을 보다 쉽게 반복할 수 있습니다.
- `AVAssetDownloadURLSession`클래스를 사용 하면 FairPlay 암호화 된 HLS 스트림을 다운로드 하 고 나중에 재생할 수 있습니다.

<a name="Core-Data-Framework-Enhancements"></a>

## <a name="core-data-framework-enhancements"></a>핵심 데이터 프레임 워크 향상

MacOS Sierra에 대 한 핵심 데이터 프레임 워크에 대 한 다음과 같은 기능이 향상 되었습니다.

- Root [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) 개체는 serialization 없이 동시에 오류 및 가져오기를 지원 합니다.
- [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) 클래스는 SQLite 데이터 저장소의 풀을 유지 관리 합니다.
- 모드 (관리 개체 컨텍스트)를 사용 하는 새 쿼리 생성 기능을 사용 하 여 [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) 개체를 WAL 저널 모드에 저장 하면 나중에 인출 하 고 오류를 발생 시킬 수 있습니다.
- 상위 수준을 사용 하 여 `NSPersistenceContainer` `NSPersistentStoreCoordinator` , [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) 및 기타 핵심 데이터 구성 리소스를 참조 합니다.
- 더 쉽게 페치를 수행 하 고 하위 클래스를 만들 수 있도록 몇 가지 새로운 편의 방법이 추가 되었습니다 `NSManagedObject` .

자세한 내용은 Apple의 [핵심 데이터 프레임 워크 참조](https://developer.apple.com/reference/coredata)를 참조 하세요.

<a name="Core-Image-Framework-Enhancements"></a>

## <a name="core-image-framework-enhancements"></a>핵심 이미지 프레임 워크 향상

MacOS Sierra에 대 한 핵심 이미지 프레임 워크에 대 한 다음과 같은 기능이 향상 되었습니다.

- `ImageWithExtent` [Cifilter](https://developer.apple.com/reference/coreimage/cifilter) 클래스의 메서드를 사용 하 여 사용자 지정 처리를 필터 작업에 삽입할 수 있습니다. 핵심 이미지는 출력 또는 표시를 위해 이미지를 처리할 때 필터 사이에 지정 된 콜백을 호출 합니다.
- 이제 앱은 처리 전후에 색 공간을 변환 하 여 핵심 이미지 컨텍스트의 작업 색 공간 외부에 있는 색 공간에서 이미지를 처리할 수 있습니다.
- 이제 코어 이미지 커널이 특정 픽셀 출력 형식을 요청할 수 있습니다.
- `CINinePartTitled`,, `CINinePartStretched` `CIHueSaturationValueGradient` `CIEdgePreserveUpsampleFilter` 및와 `CIClamp` 같은 새 이미지 필터가 추가 되었습니다.

<a name="Foundation-Framework-Enhancements"></a>

## <a name="foundation-framework-enhancements"></a>향상 된 Foundation Framework

MacOS Sierra에 대 한 Foundation Framework의 향상 된 기능은 다음과 같습니다.

- [NSDimentions](https://developer.apple.com/reference/foundation/nsdimension) API를 사용 하 여 대량, 길이, 속도, 기간 및 온도와 같은 가장 일반적인 물리적 단위를 표현 하 고, 변환 하 고, 표시 합니다.
- ISO 8601 형식의 날짜를 구문 분석 하 고 생성 하려면 [NSISO8601DateFormatter](https://developer.apple.com/reference/foundation/nsiso8601dateformatter) 클래스를 사용 합니다.
- 새 [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) 클래스를 사용 하 여 간격을 비교 하 고 간격 교차로 테스트 하는 기간 등의 날짜 및 시간 간격 계산을 수행할 수 있습니다.
- [NSPersonNameComponentsFormatter](https://developer.apple.com/reference/foundation/nspersonnamecomponentsformatter) 클래스를 사용 하 여 문자열에서 사람의 이름 요소를 구문 분석 합니다.
- 새 [NSURLSessionTaskMetrics](https://developer.apple.com/reference/foundation/nsurlsessiontaskmetrics) 클래스를 사용 하 여 URL 네트워킹 세션의 메트릭을 가져옵니다.

자세한 내용은 [OS X v 10.12 및 iOS 10에 대 한 Apple의 기본 릴리스 정보](https://developer.apple.com/library/prerelease/content/releasenotes/Miscellaneous/RN-Foundation-OSX10.12/index.html)를 참조 하세요.

<a name="GameKit-Framework-Enhancements"></a>

## <a name="gamekit-framework-enhancements"></a>GameKit Framework의 향상 된 기능

MacOS Sierra에 대 한 GameKit 프레임 워크에 대 한 다음과 같은 기능이 향상 되었습니다.

- **Game Center 앱** 은 macos에서 더 이상 사용 되지 않고 제거 되었습니다. 앱에서 GameKit를 사용 하는 경우 순위표 등과 같은 GameKit 기능을 표시 하는 고유한 인터페이스를 제공 _해야 합니다_ .
- 새 iCloud 전용 계정 유형은 [Gkcloudplayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) 클래스에 의해 구현 되었습니다.
- 새 [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) 클래스는 Game Center에서 영구적 데이터 저장소를 관리 하기 위한 일반화 된 솔루션을 제공 합니다. `GKGameSession`플레이어의 목록을 유지 관리 하 고, 앱이 참가자 날짜를 저장, 검색 또는 교환 하는 방법 및 시기를 구현 하는 일을 담당 합니다. 대부분의 경우 게임 세션은 기존 턴 기반 일치, 실시간 일치 또는 지속적인 게임 저장 방법을 바꿀 수 있습니다.

<a name="GamePlayKit-Framework-Enhancements"></a>

## <a name="gameplaykit-framework-enhancements"></a>GamePlayKit Framework의 향상 된 기능

MacOS Sierra에 대 한 GamePlayKit 프레임 워크에 대 한 다음과 같은 기능이 향상 되었습니다.

- 절차적 노이즈 생성이 추가 되었고 현실감를 자연스럽 게 볼 수 있는 질감으로 개선 하 고, 카메라 움직임에 현실감을 추가 하 고, 다양 한 게임 환경을 생성 하는 데 사용할 수 있습니다.
- 효율적인 검색을 위해 공간 분할을 사용 하 여 게임 세계 데이터를 분할 합니다.
- 새[GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)(Monte 몬테카를로 전략가)가 가능한 이동 계산을 위해 추가 되었습니다.
- 게임 빌딩 AI를 향상 시키기 위해 새로운 의사 결정 트리 API ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) 및 [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode))가 추가 되었습니다.
- 새 [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) 및 [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) 클래스를 사용 하 여 기존 에이전트 및 경로 찾기 동작에 3d 지원이 추가 되었습니다.
- 새 [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) 클래스를 사용 하 여 고성능의 자연 스러운 경로를 제공 합니다.
- 새로운 [Gkscene](https://developer.apple.com/reference/gameplaykit/gkscene) 및 [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) 클래스를 사용 하면 GameplayKit와 SpriteKit를 보다 쉽게 결합할 수 있습니다.

<a name="Metal-Framework-Enhancements"></a>

## <a name="metal-framework-enhancements"></a>금속 프레임 워크 향상

MacOS Sierra 용 금속 프레임 워크에 대 한 다음과 같은 기능이 향상 되었습니다.

- 3D 앱 및 게임에서는 이제 _공간 분할_ 을 사용 하 여 GPU를 통해 복잡 한 장면 및 기 하 도형을 효율적으로 렌더링할 수 있습니다.
- 함수 특수화를 사용 하 여 장면에 대해 매우 최적화 된 재질 및 조명 조합 함수 컬렉션을 만듭니다.
- 리소스 힙을 사용 하 고 메모리 부족 렌더링 대상을 사용 하 여 금속 기반 앱의 성능을 최적화 하기 위해 리소스 할당에 대 한 세분화 된 제어를 제공 합니다.

자세히 알아보려면 Apple의 [금속 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221)를 참조 하세요.

<a name="ModelIO-Framework-Enhancements"></a>

## <a name="model-io-framework-enhancements"></a>모델 i/o 프레임 워크 향상

MacOS Sierra에 대 한 모델 i/o 프레임 워크에서 다음과 같은 기능이 향상 되었습니다.

- 이제 USD 파일 형식이 지원 됩니다.
- 새 클래스를 사용 `MDLMaterialPropertyGraph` 하 여 모델에 대 한 런타임 변경을 쉽게 지원할 수 있습니다.
- 서명 된 거리 필드 지원은 [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) 클래스에 추가 되었습니다.
- 새 클래스를 사용 `MDLLightProbeIrradianceDataSource` 하 여 밝은 프로브 배치를 지원 합니다.

<a name="Photos-Framework-Enhancements"></a>

## <a name="photos-framework-enhancements"></a>사진 프레임 워크 향상

MacOS Sierra에 대 한 사진 프레임 워크가 다음과 같이 향상 되었습니다.

- 이제 사진 프레임 워크를 지 원하는 앱과 사진 및 카메라 앱 내부에서 사용할 수 있도록 사진 편집 확장 (라이브 사진 편집)을 사용할 수 있습니다.
- 새 [PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext) 클래스를 사용 하 여 라이브 사진의 비디오 및 콘텐츠 모두에 편집 내용을 적용 합니다.
- [CIImageProcessorInput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput) 및 [CIImageProcessorOutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput) 클래스를 사용 하 여 새 핵심 이미지 프로세서 기능을 활용 하 여 편집을 수행 합니다.
- 라이브 사진을 지원 하기 위해 [PHLivePhoto](https://developer.apple.com/reference/photos/phlivephoto) 및 [Phlivephotoview](https://developer.apple.com/reference/photosui/phlivephotoview) 클래스는 iOS에서 macos로 이식 되었습니다.

<a name="SceneKit-Framework-Enhancements"></a>

## <a name="scenekit-framework-enhancements"></a>SceneKit Framework의 향상 된 기능

MacOS Sierra에 대 한 SceneKit 프레임 워크에 대 한 다음과 같은 기능이 향상 되었습니다.

- 이제에는 더 간단한 자산 제작을 통해 보다 현실적인 결과를 제공 하는 새로운 .PBR (물리적 기반 렌더링) 시스템이 포함 되어 있습니다.
- 새 [SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) 음영 모델을 사용 하 여 세 가지 기본 속성 (및)만 필요 하면서 광범위 한 사실적인 음영 효과를 제품 `Diffuse` `Metalness` `Roughness` 합니다.
- .PBR 음영은 환경 기반 조명에서 가장 잘 작동 하므로 속성을 사용 `LightingEnvironment` 하 여 이미지 기반 조명을 황갈색 전체 장면에 할당 합니다.
- 속성을 사용 `IESProfileURL` 하 여 강도 (lumens) 및 색 온도 (켈빈)와 같은 실제 값의 조명 기반을 정의 하는 실제 조명 설비를 가져옵니다.
- [Scncamera](https://developer.apple.com/reference/scenekit/scncamera) 클래스는 HDR 기능 및 효과를 사용 하 여 더 큰 현실감를 제공할 수 있습니다. 자동 효과를 만들거나 vignetting, 색 fringing 및 색을 사용 하 여 게임에 filmatic 효과를 추가 하려면 적응 노출을 사용 합니다.
- .PBR 및 HDR 카메라 기능 모두 기존 렌더링 기술 보다 더 나은 결과를 제공 하므로 이제 SceneKit는 선형 색 공간에서 모든 색 계산을 수행 합니다 (넓은 색 장치 디스플레이에서 P3 색 영역 사용).
- 색 프로필 정보를 읽어 SceneKit 색이 모든 색과 일치 합니다.
- SceneKit는 모든 셰이더 형식에 대해 선형 RGB 색 공간의 색 구성 요소 값을 해석 합니다.
- SceneKit는 질감 이미지에서 색 프로필 정보를 읽고 조정 하므로 모든 이미지에 대 한 자산 카탈로그를 사용 하 여이 정보가 제공 되는지 확인 합니다.
- `SCNDisableLinearSpaceRendering` `SCNDisableWideGamut` 응용 프로그램의에서 및 키를 지정 하 여 선형 색 공간 렌더링과 와이드 색을 모두 사용 하지 않도록 설정할 수 있습니다 `Info.plist` .
- 새 [SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/documentation/scenekit/scngeometryprimitivetype/scngeometryprimitivetypepolygon?language=objc) 클래스를 사용 하 여 geometry를 지정 하기 위해 파일에서 로드 되거나 프로그래밍 방식으로 생성 되는 임의의 다각형 primates을 빌드합니다.

<a name="Security-Framework-Enhancements"></a>

## <a name="security-framework-enhancements"></a>향상 된 보안 프레임 워크

MacOS Sierra에 대 한 보안 프레임 워크의 향상 된 기능은 다음과 같습니다.

- `SecKey`인터페이스는 모든 플랫폼 (iOS, tvOS, watchOS 및 macOS)에서 현대화 되 고 통합 되었습니다.

<a name="SpriteKit-Framework-Enhancements"></a>

## <a name="spritekit-framework-enhancements"></a>SpriteKit Framework의 향상 된 기능

MacOS Sierra에 대 한 SpriteKit 프레임 워크에 대 한 다음과 같은 기능이 향상 되었습니다.

- 이제 Tilemaps는, 및 클래스를 사용 하 여 2d, 2.5 d 및 사이드 스크롤 게임에 대해 정사각형, 육각형 및 등각 타일 셰이프를 지원 `SKTileMapMode` `SKTileGroup` `SKTileGroupRule` `SKTileSet` 합니다.
- 새 클래스를 사용 `SKWarpGeometry` 하 여 [SKSpriteNode](https://developer.apple.com/reference/spritekit/skspritenode) 또는 [SKEffectNode](https://developer.apple.com/reference/spritekit/skeffectnode) 렌더링을 늘이거나 왜곡할 수 있습니다. 새 고 [기능](https://developer.apple.com/reference/spritekit/skaction) 클래스를 사용 하 여 비틀기 효과 간의 전환에 애니메이션 효과를 적용할 수 있습니다.
- 사용자 지정 셰이더는 `SKAttribute` 특성 값 ()을 제공 하 여 셰이더를 사용 하는 각 노드에서 별도로 구성할 수 있는 특성 ()을 제공할 수 있습니다 `SKAttributeValue` .
- 지 수 [뷰](https://developer.apple.com/reference/spritekit/skview) 클래스는 장면을 렌더링 하는 시기와 방법을 세밀 하 게 제어할 수 있는 여러 가지 새로운 메서드를 제공 합니다.

<a name="New-Frameworks"></a>

## <a name="new-frameworks"></a>새 프레임 워크

MacOS Sierra에 다음 프레임 워크가 추가 되었습니다.

- 인 텐트 **프레임 워크** -이 프레임 워크를 사용 하면 앱에서 위치 또는 사용자 동작과 같은 상호 작용을 검사 하 고 해당 정보에 따라 동작을 수행할 수 있습니다.
- **SafariServices framework** -이 프레임 워크를 사용 하면 앱에서 Macos와 iOS 모두에 대해 Safari 용 앱 확장 (예: 콘텐츠 차단)을 개발할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [Mac 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Mac)
- [OS X 10.12의 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
