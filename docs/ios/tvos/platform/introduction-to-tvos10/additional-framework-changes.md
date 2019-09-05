---
title: 추가 tvOS 10 프레임 워크 변경 내용
description: 이 문서에서는 iOS 10의 기존 프레임 워크에 대 한 사소한 변경 내용 및 향상 된 기능에 대해 설명 합니다. AVFoundation, Avfoundation, Core 데이터, Core Graphics, Foundation, GameKit, GameplayKit 등에 대 한 업데이트를 설명 합니다.
ms.prod: xamarin
ms.assetid: F771640A-F92E-4954-82D5-2D720434971E
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/16/2017
ms.openlocfilehash: 88039da5faf911386232d2b189b27a2921f8144c
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70289570"
---
# <a name="additional-tvos-10-frameworks-changes"></a>추가 tvOS 10 프레임 워크 변경 내용

Apple에서는 tvOS에 대 한 주요 변경 내용 외에도 tvOS 10의 여러 기존 프레임 워크에 대 한 수정 및 개선을 수행 했습니다.

<a name="AV-Foundation-Framework" />

## <a name="avfoundation-framework-additions"></a>AVFoundation 프레임 워크 추가

AVFoundation 프레임 워크에는 다음과 같은 향상 된 기능이 포함 되어 있습니다.

- TvOS 10에서 앱은 콘텐츠 형식에 따라 더 이상 다른 [AVPlayerItem](https://developer.apple.com/reference/avfoundation/avplayeritem) 동작을 구현 하지 않습니다. `Rate` 속성을 설정 하기만 하면 상태일 없이 재생할 수 있는 콘텐츠가 충분 한 경우 avfoundation에서 결정 합니다.
- 새 `AVPlayerLooper` 클래스를 사용 하면 재생 하는 동안 지정 된 미디어 부분을 보다 쉽게 반복할 수 있습니다.

<a name="AVKit-Framework-Enhancements" />

## <a name="avkit-framework-enhancements"></a>AVKit 프레임 워크의 향상 된 기능

AVKit 프레임 워크에는 다음과 같은 향상 된 기능이 포함 되어 있습니다.

- 이제 앱은 [AVPlayerViewController](https://developer.apple.com/reference/avkit/avplayerviewcontroller) 의 건너뛰기 동작을 제어 하므로 건너뛴 제스처가 재생 목록의 다음 항목으로 이동 하거나 현재 항목 내에서 이동할 수 있습니다.

<a name="Core-Data-Enhancements" />

## <a name="core-data-enhancements"></a>핵심 데이터 향상

tvOS 10에는 핵심 데이터 프레임 워크에 대 한 다음과 같은 향상 된 기능이 포함 되어 있습니다.

- Root [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) 개체는 serialization 없이 동시에 오류 및 가져오기를 지원 합니다.
- [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) 클래스는 SQLite 데이터 저장소의 풀을 유지 관리 합니다.
- 모드 (관리 개체 컨텍스트)를 사용 하는 새 쿼리 생성 기능을 사용 하 여 [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) 개체를 WAL 저널 모드에 저장 하면 나중에 인출 하 고 오류를 발생 시킬 수 있습니다.
- 상위 수준 `NSPersistenceContainer` 을 사용 하 여 `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) 및 기타 핵심 데이터 구성 리소스를 참조 합니다.
- 더 쉽게 페치를 수행 하 고 하위 `NSManagedObject` 클래스를 만들 수 있도록 몇 가지 새로운 편의 방법이 추가 되었습니다.

자세한 내용은 Apple의 [핵심 데이터 프레임 워크 참조](https://developer.apple.com/reference/coredata)를 참조 하세요.

<a name="Core-Graphics-Enhancements" />

## <a name="core-graphics-enhancements"></a>핵심 그래픽 향상

tvOS 10에는 핵심 그래픽 프레임 워크에 대 한 다음과 같은 향상 된 기능이 포함 되어 있습니다.

- 새 [CGColorConverterRef](https://developer.apple.com/reference/coregraphics/cgcolorconverterref) 클래스를 사용 하 여 일련의 색 변환을 수행할 수 있습니다.

<a name="Core-Image-Enhancements" />

## <a name="core-image-enhancements"></a>핵심 이미지 향상

tvOS 10은 핵심 이미지 프레임 워크에 대해 다음과 같은 향상 된 기능을 만듭니다.

- [Cifilter](https://developer.apple.com/reference/coreimage/cifilter) 클래스의 메서드를사용하여사용자지정처리를필터작업에삽입할수있습니다.`ImageWithExtent` 핵심 이미지는 출력 또는 표시를 위해 이미지를 처리할 때 필터 사이에 지정 된 콜백을 호출 합니다.
- 이제 앱은 처리 전후에 색 공간을 변환 하 여 핵심 이미지 컨텍스트의 작업 색 공간 외부에 있는 색 공간에서 이미지를 처리할 수 있습니다.
- 개체에서 `UIImage` `UIImageView` 렌더링 (핵심 이미지 이미지 저장소에서 지원 되는 경우)에 대 한 몇 가지 렌더링 성능이 향상 되었습니다. 
- `UIImage`넓은 색으로 태그가 지정 된 개체는 넓은 색을 지 원하는 `UIImageView` iOS 장치에서 개체의 넓은 색 영역 색으로 렌더링 됩니다.
- 핵심 이미지 커널 코드는 이제 특정 픽셀 출력 형식을 요청할 수 있습니다.

또한 다음과 같은 새로운 핵심 이미지 필터가 추가 되었습니다.

- `CINinePartTiled`
- `CINinePartStretched`
- `CIHueSaturationValueGradient`
- `CIEdgePreserveUpsampleFilter`
- `CIClamp`

<a name="Foundation-Enhancements" />

## <a name="foundation-enhancements"></a>Foundation 향상 된 기능

TvOS 10 용 Foundation framework에 대 한 다음과 같은 기능이 향상 되었습니다.

- 새 [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) 클래스를 사용 하 여 간격을 비교 하 고 간격 교차로 테스트 하는 기간 등의 날짜 및 시간 간격 계산을 수행할 수 있습니다.
- 로컬 정보 및 사용 가능한 표시 형식을 얻기 위해 [Nslocal](https://developer.apple.com/reference/foundation/nslocale) 클래스에 몇 가지 새 속성이 추가 되었습니다.
- 새 [Nsmeasurement](https://developer.apple.com/reference/foundation/nsmeasurement) 클래스를 사용하여 다른 Uom (측정 단위) 간에 변환하거나 다른 uom의 값에 대한 계산을 수행합니다.
- 새 [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) 클래스를 사용 하 여 최종 사용자에 게 표시 하기 위해 지역화 된 측정값의 서식을 지정 합니다.
- 새 [Nsunit](https://developer.apple.com/reference/foundation/nsunit) 및 [nsunit](https://developer.apple.com/reference/foundation/nsdimension) 클래스를 사용 하 여 특정 uoms를 나타냅니다.

<a name="GameKit-Enhancements" />

## <a name="gamekit-enhancements"></a>GameKit 향상

TvOS 10에서 GameKit 프레임 워크에 대해 다음과 같은 기능이 향상 되었습니다.

- 새 iCloud 전용 계정 유형은 [Gkcloudplayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) 클래스에 의해 구현 되었습니다.
- 새 [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) 클래스는 Game Center에서 영구적 데이터 저장소를 관리 하기 위한 일반화 된 솔루션을 제공 합니다. `GKGameSession`플레이어의 목록을 유지 관리 하 고, 앱이 참가자 날짜를 저장, 검색 또는 교환 하는 방법 및 시기를 구현 하는 일을 담당 합니다. 대부분의 경우 게임 세션은 기존 턴 기반 일치, 실시간 일치 또는 지속적인 게임 저장 방법을 바꿀 수 있습니다.

<a name="GameplayKit-Enhancements" />

## <a name="gameplaykit-enhancements"></a>GameplayKit 향상

TvOS 10에서 GameplayKit 프레임 워크에 대해 다음과 같은 기능이 향상 되었습니다.

- 절차적 노이즈 생성이 추가 되었고 현실감를 자연스럽 게 볼 수 있는 질감으로 개선 하 고, 카메라 움직임에 현실감을 추가 하 고, 다양 한 게임 환경을 생성 하는 데 사용할 수 있습니다.
- 효율적인 검색을 위해 공간 분할을 사용 하 여 게임 세계 데이터를 분할 합니다.
- 새[GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)(Monte 몬테카를로 전략가)가 가능한 이동 계산을 위해 추가 되었습니다.
- 게임 빌딩 AI를 향상 시키기 위해 새로운 의사 결정 트리 API ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) 및 [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode))가 추가 되었습니다.
- 새 [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) 및 [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) 클래스를 사용 하 여 기존 에이전트 및 경로 찾기 동작에 3d 지원이 추가 되었습니다.
- 새 [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) 클래스를 사용 하 여 고성능의 자연 스러운 경로를 제공 합니다.
- 새로운 [Gkscene](https://developer.apple.com/reference/gameplaykit/gkscene) 및 [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) 클래스를 사용 하면 GameplayKit와 SpriteKit를 보다 쉽게 결합할 수 있습니다.

<a name="Metal-Enhancements" />

## <a name="metal-enhancements"></a>금속 기능 향상

TvOS 10의 금속 프레임 워크에 대 한 다음과 같은 기능이 향상 되었습니다.

- 3D 앱 및 게임에서는 이제 _공간 분할_ 을 사용 하 여 GPU를 통해 복잡 한 장면 및 기 하 도형을 효율적으로 렌더링할 수 있습니다.
- 함수 특수화를 사용 하 여 장면에 대해 매우 최적화 된 재질 및 조명 조합 함수 컬렉션을 만듭니다.
- 리소스 힙을 사용 하 고 메모리 부족 렌더링 대상을 사용 하 여 금속 기반 앱의 성능을 최적화 하기 위해 리소스 할당에 대 한 세분화 된 제어를 제공 합니다.

자세히 알아보려면 Apple의 [금속 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221)를 참조 하세요.

<a name="Metal-Performance-Shaders-Enhancements" />

## <a name="metal-performance-shaders-enhancements"></a>메탈 성능 셰이더 기능 향상

TvOS 10의 메탈 Performance 셰이더 프레임 워크에 대 한 다음과 같은 기능이 향상 되었습니다.

- 응용 프로그램에서 색 공간 변환 및 신경망 작업과 같은 높은 최적화 된 데이터 병렬 계산을 활용할 수 있도록 하는 새로운 커널 대부분이 금속 성능 셰이더 프레임 워크에 추가 되었습니다.

<a name="ModelIO-Enhancements" />

## <a name="modelio-enhancements"></a>ModelIO 향상

TvOS 10에서 ModelIO 프레임 워크에 대해 다음과 같은 기능이 향상 되었습니다.

- 이제 USD 파일 형식이 지원 됩니다.
- 새 `MDLMaterialPropertyGraph` 클래스를 사용 하 여 모델에 대 한 런타임 변경을 쉽게 지원할 수 있습니다.
- 서명 된 거리 필드 지원은 [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) 클래스에 추가 되었습니다.
- 새 `MDLLightProbeIrradianceDataSource` 클래스를 사용 하 여 밝은 프로브 배치를 지원 합니다.

<a name="SceneKit-Enhancements" />

## <a name="scenekit-enhancements"></a>SceneKit 향상

TvOS 10에서 SceneKit 프레임 워크에 대해 다음과 같은 기능이 향상 되었습니다.

- 이제 SceneKit에는 더 간단한 자산 제작을 통해 보다 현실적인 결과를 제공 하는 새로운 .PBR (물리적 기반 렌더링) 시스템이 포함 되어 있습니다.
- 새 [SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) 음영 모델을 사용 하 여 세 가지 기본 속성 (`Diffuse` `Metalness` 및 `Roughness`)만 필요 하면서 광범위 한 사실적인 음영 효과를 제품 합니다.
- .Pbr 음영은 환경 기반 조명에서 가장 잘 작동 하므로 `LightingEnvironment` 속성을 사용 하 여 이미지 기반 조명을 황갈색 전체 장면에 할당 합니다.
- 속성을 `IESProfileURL` 사용 하 여 강도 (lumens) 및 색 온도 (켈빈)와 같은 실제 값의 조명 기반을 정의 하는 실제 조명 설비를 가져옵니다.
- [Scncamera](https://developer.apple.com/reference/scenekit/scncamera) 클래스는 HDR 기능 및 효과를 사용 하 여 더 큰 현실감를 제공할 수 있습니다. 자동 효과를 만들거나 vignetting, 색 fringing 및 색을 사용 하 여 게임에 filmatic 효과를 추가 하려면 적응 노출을 사용 합니다.
- .PBR 및 HDR 카메라 기능 모두 기존 렌더링 기술 보다 더 나은 결과를 제공 하므로 이제 SceneKit는 선형 색 공간에서 모든 색 계산을 수행 합니다 (넓은 색 장치 디스플레이에서 P3 색 영역 사용).
- 색 프로필 정보를 읽어 SceneKit 색이 모든 색과 일치 합니다.
- SceneKit는 모든 셰이더 형식에 대해 선형 RGB 색 공간의 색 구성 요소 값을 해석 합니다.
- SceneKit는 질감 이미지에서 색 프로필 정보를 읽고 조정 하므로 모든 이미지에 대 한 자산 카탈로그를 사용 하 여이 정보가 제공 되는지 확인 합니다.
- 응용 프로그램의 `SCNDisableLinearSpaceRendering` `SCNDisableWideGamut`에서및 키를 지정 하 여 선형 색 공간 렌더링과 와이드 색을 모두 사용 하지 않도록 설정할 수 있습니다. `Info.plist`
- 새 [SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon) 클래스를 사용 하 여 geometry를 지정 하기 위해 파일에서 로드 되거나 프로그래밍 방식으로 생성 되는 임의의 다각형 primates을 빌드합니다.

<a name="SpriteKit-Enhancements" />

## <a name="spritekit-enhancements"></a>SpriteKit 향상

TvOS 10에서 SpriteKit 프레임 워크에 대해 다음과 같은 기능이 향상 되었습니다.

- 이제 `SKTileMapMode`Tilemaps는 `SKTileGroup` ,및`SKTileSet`클래스 를 사용 하 여 2d, 2.5 d 및 사이드 스크롤 게임에 대해 정사각형, 육각형 및 등각 타일 셰이프를 지원 합니다. `SKTileGroupRule`
- 새 `SKWarpGeometry` 클래스를 사용 하 여 [SKSpriteNode](https://developer.apple.com/reference/spritekit/skspritenode) 또는 [SKEffectNode](https://developer.apple.com/reference/spritekit/skeffectnode) 렌더링을 늘이거나 왜곡할 수 있습니다. 새 고 [기능](https://developer.apple.com/reference/spritekit/skaction) 클래스를 사용 하 여 비틀기 효과 간의 전환에 애니메이션 효과를 적용할 수 있습니다.
- 사용자 지정 셰이더는 특성 값`SKAttribute`(`SKAttributeValue`)을 제공 하 여 셰이더를 사용 하는 각 노드에서 별도로 구성할 수 있는 특성 ()을 제공할 수 있습니다.
- 지 수 [뷰](https://developer.apple.com/reference/spritekit/skview) 클래스는 장면을 렌더링 하는 시기와 방법을 세밀 하 게 제어할 수 있는 여러 가지 새로운 메서드를 제공 합니다.

<a name="UIKit-Enhancements" />

## <a name="uikit-enhancements"></a>UIKit 향상 된 기능

TvOS 10에서 UIKit 프레임 워크에 대 한 다음과 같은 기능이 향상 되었습니다.

- 뿐만 아니라 `UIViews`뷰 이외의 항목에 대 한 포커스를 지원 하도록 포커스 API가 향상 되었습니다. 포커스를 지 원하는 항목은 인터페이스 `IUIFocusItem` 를 구현 해야 합니다.
- 새 `UIGraphicsRender` 클래스는 uikit 렌더링 또는 핵심 그래픽에서 비트맵 또는 pdf를 만드는 개체 지향 메서드를 제공 하 고 사용 되지 않는 `UIGraphicsBeginImageContext` 메서드를 대체 합니다.
- 클래스 `UIUserInterfaceStyle` 는 현재 활성 상태인 사용자 인터페이스 테마 (어둡게 또는 밝게)를 결정 하기 위해 추가 되었습니다.
- 완전히 대화형으로 제공 되는 새로운 개체 기반 애니메이션 지원이 추가 되 고 van이 제스처에 연결 됩니다. Pleas 참조 Apple의 [Uiviewanimating 프로토콜 참조](https://developer.apple.com/reference/uikit/uiviewanimating), [uiviewproperty애니메이터 클래스 참조](https://developer.apple.com/reference/uikit/uiviewpropertyanimator), [UITimingCurveProvider Protocol Reference](https://developer.apple.com/reference/uikit/uitimingcurveprovider), [Uicubic ingparameters 클래스 참조](https://developer.apple.com/reference/uikit/uicubictimingparameters) 및 [ 자세한 내용은 UISpringTimingParameter 클래스 참조를 참조](https://developer.apple.com/reference/uikit/uispringtimingparameters) 하세요.
- `UIPreviewInteraction` 새`UIPreviewInteractionDelegate` 를 사용 하 여 앱에서 peek 및 pop 작업을 위한 사용자 지정 인터페이스를 제공할 수 있습니다.
- 새 `UIAccessibilityCustomRotor` 클래스를 사용 하면 앱에서 음성 전달 등의 보조 기술에 대 한 사용자 지정 컨텍스트별 기능을 제공할 수 있습니다.
- `UIAccessibilityIsAssistiveTouchRunning` 및`UIAccessibilityAssistiveTouchStatusDidChangeNotification` 기호를 사용 하 여 AssistiveTouch를 사용할 수 있는지 확인 합니다.
- `UIAccessibilityHearingDevicePairedEar` 및`UIAccessibilityHearingDevicePairedEarDidChangeNotification` 기호를 사용 하 여 쌍을 이루는 MFi 청각 지원의 상태를 가져옵니다.
- 새 [uipasteboard](https://developer.apple.com/reference/uikit/uipasteboard) 본 API는 수명 제한과 같은 새 옵션을 제공 하며 공용 클래스 형식에 대해 호환 되는 콘텐츠 형식을 자동으로 선언 합니다.
- 레이블에서 동적 형식을 지원 하기 위해 텍스트 필드와 텍스트 상자는 `PreferredFontForTextStyle` `UIFont` 클래스의 새 메서드를 사용 합니다.
- 장치가 `UIContentSizeCategory` 변경 될 때 요소가 글꼴을 업데이트 해야 하는지 결정 하려면 `UIContentSizeCategoryAdjusting` 대리자의 `AdjustsFontForContentSizeCategory` 속성을 사용 합니다.
- 이제 앱에서 텍스트 및 배경색과 같은 탭 모음 항목의 배지 모양을 제어할 수 있습니다.
- 이제의 새로 고침 컨트롤이 모든 스크롤 뷰와 스크롤 뷰 서브 클래스 (예: `UICollectionView`)에서 지원 됩니다.
- 클래스의 메서드는 이제 비동기 방식으로 호출 되어 open이 완료 된 후 호출 되는 완료 처리기를 지원 합니다. `OpenURL` `UIApplication`
- 새 `UICloudSharingController` 및`UICloudSharingControllerDelegate` 클래스를 사용 하 여 cloudkit 공유를 시작 하 고 해당 속성을 수정 합니다.
- 프리페치된 셀을 활용 하 여 새 `UICollectionViews` `UICollectionViewDataSourcePrefetching` 대리자로의 스크롤 환경을 향상 시킵니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [TvOS 10의 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
