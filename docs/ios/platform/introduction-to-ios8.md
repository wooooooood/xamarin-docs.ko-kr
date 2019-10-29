---
title: iOS 8 소개
description: IOS 8을 사용 하 여 Apple은 흥미 및 즐거움 개발자에 게 새로운 프레임 워크 및 Api 다양 한을 제공 했습니다. 이 가이드에서는 이러한 새 Api를 소개 하 고 iOS 8에서 개발자와 사용자 모두에 게 어떤 이점을 누릴 수 있는지 확인 합니다.
ms.prod: xamarin
ms.assetid: 33AD66C0-3743-49FE-9DCE-88ED3A16BA63
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/14/2017
ms.openlocfilehash: 2da018b3595850582331280909fa327cee4ff6e0
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031813"
---
# <a name="introduction-to-ios-8"></a>iOS 8 소개

_IOS 8을 사용 하 여 Apple은 흥미 및 즐거움 개발자에 게 새로운 프레임 워크 및 Api 다양 한을 제공 했습니다. 이 가이드에서는 이러한 새 Api를 소개 하 고 iOS 8에서 개발자와 사용자 모두에 게 어떤 이점을 누릴 수 있는지 확인 합니다._

iOS 7은 사용자 및 개발자가 첫 번째 iPhone OS에서 제공 하는 것과 같은 전체 iOS 사용자 인터페이스를 시각적으로 변경 했습니다. IOS 8은 개발자를 위한 많은 프레임 워크를 제공 하 여이를 계속 합니다 .이를 통해 사용자는 iPhone에서 직접 일상의 거의 모든 측면을 제어할 수 있습니다. 예를 들어 *HealthKit*를 사용 하 여 상태 및 적합성을 분석할 수 있으며, 암호는 *localauthentication*을 사용 하 여 생체 인식 인증을 사용 하 여 sal, *앱 확장* 은 타사 앱 *간의 통신 채널을 엽니다. HomeKit* 를 사용 하면 집을 미래 홈으로 전환할 수 있습니다. 

IOS 7이 만족 사용자에 대 한 것 이라면 iOS 8은 이러한 맛의 전체 범위를 갖춘 만족 개발자에 게 주력 합니다. 

이 가이드에서는 Xamarin.ios 개발자를 위한 새로운 Api를 소개 합니다.  

IOS 8에서 더 이상 사용 되지 않는 몇 가지 Api도 있습니다 .이에 대해서는이 문서의 끝에 자세히 설명 되어 있습니다.

## <a name="requirements"></a>요구 사항

Mac용 Visual Studio에서 iOS 8 앱을 만들려면 다음이 필요 합니다.

- **Xcode 7 및 ios 8 이상** – Apple의 최신 Xcode 및 iOS api를 개발자의 컴퓨터에 설치 하 고 구성 해야 합니다.
- **Mac용 Visual Studio** – 최신 버전의 Mac용 Visual Studio를 설치 하 고 사용자 장치에 구성 해야 합니다.
- **ios 8 장치 또는 시뮬레이터** – 테스트를 위해 최신 버전의 ios 8을 실행 하는 ios 장치입니다.

## <a name="home-and-leisure"></a>Home 및 레저

iOS 8은 HomeKit 및 HealthKit를 사용 하 여 Apple 및 iOS 장치를 홈의 핵심에 수직 화 하는 데 도움을 주었습니다. 이 섹션에서는 이러한 새 프레임 워크의 작동 방식 및 이러한 프레임 워크를 Xamarin.ios 응용 프로그램에 통합 하는 방법을 살펴봅니다.

## <a name="homekit"></a>HomeKit

IPhone에서 어플라이언스를 제어 하는 것은 새로운 기술의 응용 프로그램이 아닙니다. 많은 연결 된 홈 제품은 iOS 앱을 통해 제어할 수 있습니다. 그러나 이제 HomeKit은 홈 자동화 장치에 대 한 공통 프로토콜을 승격 하 고 iHome, Philips 및 Honeywell와 같은 특정 제조업체에서 공용 API를 사용할 수 있도록 하 여이 단계를 더 수행 합니다. 이는 사용자에 게 한 응용 프로그램 내에서 홈의 거의 모든 측면을 원활 하 게 제어할 수 있음을 의미 합니다. Philips 색 전구를 사용 하 고 있거나 중첩 된 경보를 사용 하 고 있다는 것을 아는 것은 의미가 없습니다. 사용자는 다양 한 스마트 홈 프로세스를 "장면"으로 함께 연결할 수도 있습니다.

HomeKit를 사용 하 여 타사 앱 및 Siri는 액세서리를 검색 하 고 개인 홈 구성 데이터베이스에 추가 하 고,이 데이터를 편집 하 고 작업 하며, 액세서리 및 해당 서비스와 통신 하 여 작업을 수행할 수 있습니다.

### <a name="configuration"></a>Configuration

아래 다이어그램은 HomeKit 액세서리 구성의 기본 계층을 보여 줍니다.

![](introduction-to-ios8-images/image1.png "This diagram shows the basic hierarchy of the configuration of HomeKit accessories")

HomeKit을 시작 하려면 개발자가 프로 비전 프로필에 HomeKit 서비스를 선택 했는지 확인 해야 합니다. 또한 Apple은 개발자에 게 Xcode에 대 한 HomeKit 시뮬레이터 추가 기능을 제공 했습니다. 이는 [Apple 개발자 센터](https://developer.apple.com/downloads/index.action)의 `Hardware IO Tools for Xcode`에서 찾을 수 있습니다. 

자세한 내용은 [HomeKit](~/ios/platform/homekit.md) 가이드를 참조 하세요.

## <a name="healthkit"></a>HealthKit

HealthKit는 상태 관련 정보에 대 한 중앙 집중화 되 고 조정 된 보안 데이터 저장소를 제공 하는 iOS 8에 도입 된 프레임 워크입니다. 운영 체제는 상태 앱 및 사용자에 대 한 대시보드를 사용 하 여 개인 정보 보호 및 상태에 대 한 보안을 보장 합니다. 응용 프로그램은 사용자의 사용 권한을 사용 하 여 다양 한 상태 정보를 읽고 쓸 수 있습니다.

Xamarin.ios 앱에서이를 사용 하는 방법에 대 한 자세한 내용은 [HealthKit 소개](~/ios/platform/healthkit.md) 가이드를 참조 하세요.

## <a name="extending-iphone-functionality"></a>IPhone 기능 확장
IOS8를 사용 하 여 개발자는 앱을 사용할 수 있는 사용자에 대해 훨씬 더 많은 제어를 제공 하 고 타사 앱 간의 오픈 통신 기능을 향상 시킵니다. 앱 확장 및 문서 선택기와 같은 기능을 통해 Apple의 에코 시스템에서 응용 프로그램을 사용 하는 방법에 대 한 가능성을 누릴 수 있습니다.

### <a name="app-extensions"></a>앱 확장
앱 확장은 더 간단 하 게 하기 위해 타사 앱에서 서로 통신 하는 방법입니다. 높은 수준의 보안 표준을 유지 하 고 샌드박스가 적용 된 앱의 무결성을 취했습니다이 통신은 응용 프로그램 간에 직접 발생 하지 않습니다. 대신 중간에 *확장* 에 의해 수행 됩니다.

앱 확장을 만드는 첫 번째 단계는 올바른 확장 지점을 정의 하는 것입니다 .이는 올바른 Api의 동작과 가용성을 보장 하는 데 중요 합니다. Mac용 Visual Studio에서 앱 확장을 만들려면 솔루션에 새 프로젝트를 추가 하 여 기존 응용 프로그램에 추가 합니다.

아래 스크린샷에 나와 있는 것 처럼 **새 프로젝트** 대화 상자 **C#** 에서 > **iOS** > **Unified API** > **확장**으로 이동 합니다.

![](introduction-to-ios8-images/image2.png "Creating a new extension")

새 프로젝트 대화 상자는 앱 확장을 만들기 위한 7 가지 새 프로젝트 템플릿을 제공 하며 아래에서 설명 합니다. 많은 확장 프로그램이 iOS의 다른 새 Api (예: 문서 선택기)와 관련이 있습니다.

- **작업** -개발자가 특정 작업을 수행할 수 있도록 하는 고유한 사용자 지정 작업 단추를 만들 수 있습니다.
- **사용자 지정 키보드** -개발자는 고유한 사용자 지정 항목을 추가 하 여 기본 제공 Apple 키보드 범위에 추가할 수 있습니다. 인기 키보드는이를 사용 하 여 키보드를 iOS로 가져옵니다.
- **문서 선택기** – 사용자가 응용 프로그램의 샌드박스 외부에서 파일에 액세스할 수 있도록 하는 문서 선택기 보기 컨트롤러를 포함 합니다.
- **문서 선택기 파일 공급자** – 문서 선택기를 사용 하 여 파일에 대 한 보안 저장소를 제공 합니다.
- **사진 편집** – 사진 응용 프로그램에서 Apple이 이미 제공 하는 필터 및 편집 도구를 확장 하 여 사용자에 게 더 많은 제어를 제공 하 고 사진을 편집할 때 더 많은 옵션을 제공 합니다.
- **오늘** -알림 센터의 오늘 섹션에서 위젯을 표시 하는 기능을 응용 프로그램에 제공 합니다.

Xamarin에서 앱 확장을 사용 하는 방법에 대 한 자세한 내용은 [앱 확장 소개](~/ios/platform/extensions.md) 가이드를 참조 하세요.

### <a name="touch-id"></a>Touch ID

Touch ID는 암호와 유사 하 게 사용자를 인증 하는 수단으로 iOS 7에서 도입 되었습니다. 그러나 장치를 잠금 해제 하 고, 앱 스토어를 사용 하 고, iTunes를 사용 하 고, iCloud 키 집합만 인증 하도록 제한 되었습니다. 

이제 로컬 인증 API를 사용 하 여 iOS 8 응용 프로그램에서 Touch ID를 인증 메커니즘으로 사용 하는 두 가지 방법이 있습니다. 현재 원격으로 인증 하는 데 로컬 인증을 사용할 수 없습니다.

첫째, 새 키 집합 Access Control 목록 (Acl)을 사용 하 여 기존 키 집합 서비스를 지원 합니다. 사용자 지문을 성공적으로 인증 하면 키 집합 데이터의 잠금을 해제할 수 있습니다.

두 번째로 LocalAuthentication은 응용 프로그램을 로컬로 인증 하는 두 가지 방법을 제공 합니다. 개발자는 `CanEvaluatePolicy`를 사용 하 여 장치에서 Touch ID를 수락할 수 있는지 확인 한 다음 `EvaluatePolicy` 하 여 인증 작업을 시작 해야 합니다.

Touch ID에 대 한 자세한 내용과 Xamarin.ios 응용 프로그램에 통합 하는 방법에 대 한 자세한 내용은 [TouchID 가이드 소개](~/ios/platform/touchid.md) 를 참조 하세요.

### <a name="document-picker"></a>문서 선택기

문서 선택은 사용자 iCloud 드라이브와 함께 작동 하 여 사용자가 다른 앱에서 만들어진 파일을 열고, 해당 파일을 가져오고 조작 하 고, 다시 내보낼 수 있습니다. 이를 통해 직관적인 워크플로를 만들 수 있으므로 사용자에 게 더 나은 환경이 제공 됩니다. iCloud 동기화는이 한 단계를 추가로 수행 합니다. 즉, 한 응용 프로그램에서 변경한 내용이 모든 장치에서 일관 되 게 반영 됩니다.

문서 선택기에 대해 자세히 알아보고 문서 선택기를 Xamarin.ios 응용 프로그램에 통합 하는 방법에 대 한 자세한 내용은 [문서 선택기 소개](~/ios/platform/document-picker.md) 가이드를 참조 하세요.

### <a name="handoff"></a>Handoff

더 큰 연속성 기능에 포함 된 핸드 오프는 OS X와 iOS의 통합에 대 한 단계를 추가로 수행 합니다. 여기에는 플랫폼 간 이동, iPhone 호출 기능, iPad 및 Mac에서의 SMS, iPhone에서 테더 링의 향상 기능 등이 포함 됩니다.

핸드 오프는 iOS 8 및 Yosemite와 함께 작동 하며, 사용 하려는 다른 모든 장치에 iCloud 계정이 로그인 해야 합니다. Safari, iWork, 지도, 일정 및 연락처를 비롯 한 대부분의 사전 설치 된 Apple 앱과 함께 작동 합니다.

자세한 내용은 [핸드 오프](~/ios/platform/handoff.md) 가이드를 참조 하세요.

## <a name="unified-storyboards"></a>통합 Storyboards
iOS 8에는 사용자 인터페이스를 만드는 데 사용 하는 보다 간단한 새로운 메커니즘인 통합 storyboard가 포함 되어 있습니다. 단일 storyboard를 사용 하 여 다양 한 하드웨어 화면 크기를 모두 포함 하는 경우 "한 번 디자인 하 고 다 수" 스타일로 신속 하 고 응답성이 뛰어난 보기를 만들 수 있습니다.

IOS8 이전에는 개발자가 `UIInterfaceOrientation`를 사용 하 여 가로 및 세로 모드를 구분 하 고 `UIInterfaceIdiom` iOS 장치를 구분할 수 있습니다. IOS8에서는 iPhone 및 iPad 장치에 대 한 별도의 스토리 보드를 만들 필요가 없습니다. 방향 및 장치는 *크기 클래스*를 사용 하 여 결정 됩니다.

모든 장치는 가로 및 세로 축에서 크기 클래스로 정의 되며 iOS 8에는 두 가지 유형의 크기 클래스가 있습니다.

- **Regular** -큰 화면 크기 (예: iPad) 또는 큰 크기의 인상을 제공 하는 가젯 (예: UIScrollView)에 대 한 것입니다.
- **Compact** -작은 장치 (예: iPhone)를 위한 것입니다. 이 크기는 장치의 방향을 고려 합니다.

두 가지 개념을 함께 사용 하는 경우 결과는 다음 다이어그램에 표시 된 것 처럼 서로 다른 방향으로 사용할 수 있는 다양 한 크기를 정의 하는 2 x 2 그리드입니다.

![](introduction-to-ios8-images/image3.png "A diagram representing the 2 x 2 grid that defines the different possible sizes that can be used in both the differing orientations")

크기 클래스에 대 한 자세한 내용은 [통합 Storyboard 소개](~/ios/user-interface/storyboards/unified-storyboards.md)를 참조 하세요.

## <a name="photo-kit"></a>사진 키트
Photo Kit는 응용 프로그램이 시스템 이미지 라이브러리를 쿼리하고 사용자 지정 사용자 인터페이스를 만들어 콘텐츠를 보고 수정할 수 있도록 하는 새로운 프레임 워크입니다. 여기에는 이미지 및 비디오 자산을 나타내는 여러 클래스 뿐만 아니라 앨범 및 폴더와 같은 자산 컬렉션도 포함 됩니다.

자세한 내용은 [사진 키트](~/ios/platform/photokit.md) 가이드를 참조 하세요.

## <a name="games"></a>게임

<a name="scenekit" />

### <a name="scene-kit"></a>장면 키트

장면 키트는 3D 그래픽 작업을 간소화 하는 3D 장면 그래프 API입니다. OS X 10.8에 처음 도입 되었으며 이제 iOS 8로 제공 되었습니다. 장면 키트를 사용 하면 몰입 형 3D 시각화 및 일반 3D 게임을 만들 때 OpenGL의 전문 지식이 필요 하지 않습니다. 일반적인 장면 그래프 개념을 기반으로 하는 장면 키트는 OpenGL 및 OpenGL ES의 복잡성을 추상화 하 여 응용 프로그램에 3D 콘텐츠를 매우 쉽게 추가할 수 있도록 합니다. 그러나 OpenGL 전문가 인 경우 장면 키트는 OpenGL에 직접 연결할 수 있는 뛰어난 지원을 제공 합니다. 또한 물리와 같은 3D 그래픽을 보완 하 고 핵심 애니메이션, 핵심 이미지 및 스프라이트 키트와 같은 다른 여러 Apple 프레임 워크와 매우 잘 통합 하는 다양 한 기능이 포함 되어 있습니다.

자세한 내용은 [SceneKit](~/ios/platform/gaming/scenekit.md) 설명서를 참조 하세요.

<a name="spritekit" />

### <a name="sprite-kit"></a>스프라이트 키트

Apple의 2D 게임 프레임 워크인 Sprite Kit에는 iOS 8 및 OS X Yosemite의 몇 가지 흥미로운 새로운 기능이 있습니다. 여기에는 장면 키트와의 통합, 셰이더 지원, 조명, 그림자, 제약 조건, 일반적인 맵 생성 및 물리학 향상이 포함 됩니다. 특히 새로운 물리학 기능을 통해 게임에 현실적인 효과를 매우 쉽게 추가할 수 있습니다.

자세한 내용은 [SpriteKit](~/ios/platform/gaming/spritekit.md) 설명서를 참조 하세요.

## <a name="other-changes"></a>기타 변경 내용
위에서 설명한 iOS 8의 주요 변경 내용 외에도 Apple에서 많은 기존 프레임 워크를 추가로 업데이트 했습니다. 이러한 내용은 아래에 자세히 설명 되어 있습니다.

- **[핵심 이미지](https://developer.apple.com/library/prerelease/ios/documentation/GraphicsImaging/Reference/CoreImagingRef/index.html#//apple_ref/doc/uid/TP40001171)** – Apple은 사각형 영역 검색에 대 한 지원 및 이미지 내 QR 코드를 더 잘 추가 하 여 이미지 처리 프레임 워크를 확장 했습니다. Mike Bluestein는 해당 블로그 게시물에서 [iOS 8의 이미지 검색](https://blog.xamarin.com/image-detection-in-ios-8/) 에 대 한이를 탐색 합니다.

## <a name="deprecated-apis"></a>사용되지 않는 API
IOS 8에서 향상 된 기능을 사용 하는 경우 다양 한 Api가 사용 되지 않습니다. 이 중 일부는 아래에 자세히 설명 되어 있습니다.

- **[Uiapplication 프로그램](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplication_Class/index.html#//apple_ref/occ/cl/UIApplication)** – 원격 알림을 등록 하는 데 사용 되는 메서드 및 속성은 더 이상 사용 되지 않습니다. RegisterForRemoteNotificationTypes 및 enabledRemoteNotificationTypes입니다.
- **[Uiviewcontroller](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIViewController_Class/index.html#//apple_ref/occ/cl/UIViewController)** – 특성 및 크기 클래스는 인터페이스 방향을 설명 하는 데 사용 되는 메서드와 속성을 대체 했습니다. 이러한 방법을 사용 하는 방법에 대 한 자세한 내용은 [통합 Storyboard 소개](~/ios/user-interface/storyboards/unified-storyboards.md) 를 참조 하세요.

- **[Uisearchdisplaycontroller](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UISearchDisplayController_Class/index.html#//apple_ref/occ/cl/UISearchDisplayController)** – IOS8에서 uisearchcontroller로 대체 되었습니다.

## <a name="summary"></a>요약
이 문서에서는 iOS 8의 Apple에서 도입 된 새로운 기능 중 일부를 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [UIKitEnhancements (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-uikitenhancements)
- [앱 확장 소개](~/ios/platform/extensions.md)
- [CloudKit 소개](~/ios/data-cloud/intro-to-cloudkit.md)
- [문서 선택기 소개](~/ios/platform/document-picker.md)
- [HealthKit 소개](~/ios/platform/healthkit.md)
- [수동 카메라 컨트롤 소개](~/ios/user-interface/controls/intro-to-manual-camera-controls.md)
- [TouchID 소개](~/ios/platform/touchid.md)
- [통합 Storyboard 소개](~/ios/user-interface/storyboards/unified-storyboards.md)
