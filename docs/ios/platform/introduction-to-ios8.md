---
title: iOS 8 소개
description: IOS 8 사용 하 여 Apple는 다양 한 새로운 프레임 워크 및 자극 개발자를 만족 하는 Api를 제공 합니다. 이 가이드의 이러한 새 Api를 소개 하 고 개발자와 사용자 모두 iOS 8 이점을 얻을 수 있습니다 하는 방법을 참조 하세요.
ms.prod: xamarin
ms.assetid: 33AD66C0-3743-49FE-9DCE-88ED3A16BA63
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/14/2017
ms.openlocfilehash: 9299322eb20561444262c2b2ba87191d2bddcde4
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61317639"
---
# <a name="introduction-to-ios-8"></a>iOS 8 소개

_IOS 8 사용 하 여 Apple는 다양 한 새로운 프레임 워크 및 자극 개발자를 만족 하는 Api를 제공 합니다. 이 가이드의 이러한 새 Api를 소개 하 고 개발자와 사용자 모두 iOS 8 이점을 얻을 수 있습니다 하는 방법을 참조 하세요._

iOS 7에서 어떤 사용자와 개발자가 예상 되는 첫 번째 iPhone OS에서 바로 제공 했습니다 전체 iOS 사용자 인터페이스를 시각적으로 변경 합니다. IOS 8 iPhone에서 직접 해당 수명의 거의 모든 측면을 제어할 수 있는 개발자를 위한 많은 프레임 워크를 제공 하 여이 사용 하 여 계속 합니다. 예를 들어 상태 및 적합성에 대 한 분석할 수 있습니다 *HealthKit*, 암호는 생체 인식 인증을 사용 하 여 사용 되지 않는 *LocalAuthentication*, *앱 확장*타사 앱, 간에 통신 채널을 열고 하 고 *HomeKit* 집 앞으로의 홈으로 전환할 수 있습니다. 

사용자를 만족 하는 방법에 대 한 iOS 7 인 경우 iOS 8 광범위 한 최상의 이러한 새 도구를 사용 하 여 개발자 에너지가에 중점을 둡니다. 

이 가이드에서는 Xamarin.iOS 개발자를 위한 새로운 Api를 소개 합니다.  

이 문서의 끝에 자세히 설명 하는 iOS 8에서에서 사용 되지 않는 몇 가지 Api도 있습니다.

## <a name="requirements"></a>요구 사항

다음은 Mac 용 Visual Studio에서 iOS 8 앱을 만들려면 필요 합니다.

- **Xcode 7 및 iOS 8 이상** – Apple의 최신 Xcode 및 iOS Api 설치 하 고 개발자의 컴퓨터에서 구성 해야 합니다.
- **Mac 용 visual Studio** – 최신 버전의 Mac 용 Visual Studio를 설치 하 고 사용자 장치에 구성 해야 합니다.
- **iOS 8 장치 또는 시뮬레이터** – 최신 버전의 테스트를 위해 iOS 8 실행 하는 iOS 장치.

## <a name="home-and-leisure"></a>홈 및 레저

iOS 8 단단히 HomeKit 및 HealthKit를 사용 하 여 집의 핵심으로 바로 Apple 및 iOS 장치를 공장에 참여 했습니다. 이 섹션에서는 이러한 새 프레임 워크 작동 하 고 Xamarin.iOS 응용 프로그램에 통합할 수 있습니다 하는 방법에 대해 살펴보겠습니다.

## <a name="homekit"></a>HomeKit

새 응용 프로그램이 기술의; 아닙니다 iPhone에서 기기를 제어 합니다. iOS 앱을 통해 여러 개의 연결 된 홈 제품을 제어할 수 있습니다. 그러나 HomeKit 이제는이 단계 더 나아가 특정 제조업체 iHome, 같은 십자 및 Honeywell 공용 API를 사용할 수 있도록 설정 하 고 홈 자동화 장치에 대 한 일반적인 프로토콜을 촉진 하 여 됩니다. 해당 홈에서 원활 하 게의 거의 모든 측면을 제어할 수 있습니다 즉 사용자에 게 하나의 응용 프로그램 내에서. 중첩 경보 또는 십자 Hue 전구를 사용 하는 것을 알아야 하는 관련 된 것입니다. 사용자가 다양 한 스마트 홈 프로세스 "백그라운드 에서"으로 함께 연결할 수도 있습니다.

HomeKit를 사용 하 여 제 3 자 프로그램과 Siri 보조 프로그램을 검색 하 고 해당 개인 홈 구성 데이터베이스에 추가할 및이 데이터를 작업할 편집한 accessories 및 동작을 수행 하는 서비스와 통신 합니다.

### <a name="configuration"></a>구성

아래 다이어그램 HomeKit 액세서리 구성의 기본 계층을 보여 줍니다.

![](introduction-to-ios8-images/image1.png "이 다이어그램 HomeKit 액세서리 구성의 기본 계층을 보여 줍니다.")
 
HomeKit를 사용 하 여 시작 하려면 개발자가 선택한 HomeKit 서비스 프로 비전 프로필에 있는지 확인 해야 합니다. 또한 Apple xcode HomeKit 시뮬레이터 추가 기능에서 개발자를 제공 했습니다. 찾을 수 있습니다 합니다 [Apple Developer Center](https://developer.apple.com/downloads/index.action)아래에 있는 `Hardware IO Tools for Xcode`합니다. 

자세한 내용은 참조 하십시오 우리의 [HomeKit](~/ios/platform/homekit.md) 가이드입니다.

## <a name="healthkit"></a>HealthKit

HealthKit은 건강 관련 정보에 대 한 중앙 집중화 하 고 조정 된 보안 데이터 저장소를 제공 하는 iOS 8에에서 도입 된 프레임 워크입니다. 운영 체제 상태 정보를 상태 앱을 사용자에 대 한 대시보드를 사용 하 여 보안을 확인합니다. 사용자의 권한을 사용 하 여 응용 프로그램 읽고 다양 한 상태 정보를 쓸 수 있습니다.

이 사용 하 여 Xamarin.iOS 앱에 대 한 자세한 내용은 참조는 [HealthKit 소개](~/ios/platform/healthkit.md) 가이드입니다.

## <a name="extending-iphone-functionality"></a>IPhone 기능 확장
IOS8를 사용 하 여 개발자가 많은 제어가 더 개방적 통신 타사 앱에 대 한 자신의 앱 및 향상 된 기능을 사용할 수 있는 사용자 지정 되 고 됩니다. 앱 확장 및 문서 선택기와 같은 기능 Apple의 에코 시스템에서 응용 프로그램을 사용할 수 있습니다 하는 방법에 대 한 새로운 가능성을 엽니다.

### <a name="app-extensions"></a>앱 확장
간단히 앱 확장은 타사 앱이 서로 통신 하는 방법입니다. 높은 보안 표준을 유지 하기 위해 하 고 샌드박스가 적용 된 앱의 무결성을 규정 하이 통신은 응용 프로그램 간에 직접 발생 하지 않습니다. 대신 해당 의해 수행 되는 *확장* 중간에 있습니다.

올바른 확장명 지점을 정의 하는 앱 확장을 만드는 첫 번째 단계-이것이 중요 한 동작 및 올바른 Api의 가용성을 보장 합니다. Mac 용 Visual Studio에서 앱 확장을 만들기를 추가 하면 기존 응용 프로그램 솔루션에 새 프로젝트를 추가 하 여 합니다.

에 **새 프로젝트** 으로 이동 대화 상자 **C#**  >  **iOS** > **통합 API**  >  **확장**와 아래 스크린샷에 표시 된 것과 같이:

![](introduction-to-ios8-images/image2.png "새 확장명 만들기")
 
새 프로젝트 대화 상자 앱 확장을 만들기 위한 7 개의 새 프로젝트 템플릿을 제공 하 고 아래에서 설명 합니다. Ios에서 문서 선택기와 같은 다른 새 Api를 관련 확장의 대부분을 확인 합니다.

- **작업** – 개발자는 특정 작업을 수행 하는 사용자가 허용 하는 고유한 사용자 지정 작업 단추를 만들 수 있습니다
- **사용자 지정 키보드** – Apple 키보드 자체 사용자 지정 항목을 추가 하 여 기본 제공 개발자는 범위에 추가할 수 있습니다. 인기 있는 키보드 Swype를 사용 하 여이 iOS에 키보드를 표시 합니다.
- **문서 선택기** – 사용자가 응용 프로그램의 샌드박스 외부 파일에 액세스할 수 있는 문서 선택기 뷰 컨트롤러를 포함 합니다.
- **문서 선택기 파일 공급자** -이 문서 선택기를 사용 하 여 파일에 대 한 안전한 저장소를 제공 합니다.
- **사진 편집** -이 필터를 확장 하 고 이미 사용자에 게 더 많은 제어 및 더 많은 옵션 해당 사진 편집 하는 경우 사진 응용 프로그램에서 Apple에서 제공한 도구를 편집 합니다.
- **지금** – 응용 프로그램 사용 하면이 위젯을 알림 센터의 오늘 구역에 표시할 수 있습니다.

앱 확장을 사용 하 여 Xamarin에 대 한 자세한 내용은 참조는 [앱 확장 소개](~/ios/platform/extensions.md) 가이드입니다.

### <a name="touch-id"></a>Touch ID

Touch ID 사용자를 인증 하는 수단으로 iOS 7에서에서 도입 되었습니다-암호와 비슷합니다. 그러나 장치를 잠금 해제, 앱 스토어를 사용 하 여, iTunes를 사용 하 여와 iCloud 키 집합에만 인증으로 제한 되었습니다. 

이제 두 가지 로컬 인증 API를 사용 하 여 iOS 8 응용 프로그램에서 인증 메커니즘으로 Touch ID를 사용 합니다. 불가능 현재 로컬 인증 원격으로 인증을 사용 합니다.

첫째, 새 키 집합 액세스 제어 목록 (Acl)를 사용 하 여 기존 키 집합 서비스를 지원 합니다. 키 집합 데이터는 사용자가 지문을 성공적인 인증을 사용 하 여 잠금 수 있습니다.

둘째, LocalAuthentication 로컬로 응용 프로그램을 인증 하는 두 가지 방법 제공 합니다. 개발자 사용할지 `CanEvaluatePolicy` Touch ID를 받아들일 수 있는 장치 인지 확인 하 고 `EvaluatePolicy` 인증 작업을 시작 합니다.

Touch ID 및 Xamarin.iOS 응용 프로그램에 통합 하는 방법에 자세한 내용은 참조는 [소개에 TouchID](~/ios/platform/touchid.md) 가이드입니다.

### <a name="document-picker"></a>문서 선택기

문서 선택기 작동 하는 다른 앱에서 생성 된, 가져오기, 조작 하 고 내보내야 파일을 열 수 있도록 사용자가 iCloud 드라이브를 사용 하 여 다시 합니다. 이 직관적인 워크플로 및 따라서 환경을 더욱 개선, 사용자를 만듭니다. 이 한 단계 더 icloud와 동기화-응용 프로그램에서 변경한 내용은 수 반영 됩니다 일관 되 게 모든 장치에서.

문서 선택기 더 자세히 알아보려면 및 Xamarin.iOS 응용 프로그램에 통합 하는 방법을 알아보려면 참조를 [The 문서 선택기 소개](~/ios/platform/document-picker.md) 가이드입니다.

### <a name="handoff"></a>Handoff

전달의 일부인 큰 연속성 기능, OS X 및 iOS를 통합 하기 위한 단계를 사용 합니다. 여기에 플랫폼 간 AirDrop, 호출 iPhone, iPad 및 Mac iPhone에서 테더 링의 향상 된 기능에는 SMS를 수행할 수 있게 합니다.

핸드 오프 iOS 8 및 Yosemite를 사용 하 여 작동 및 사용 하려는 다른 모든 장치에 로그인 할 iCloud 계정이 필요 합니다. Safari, iWork, Maps, 일정 및 연락처를 포함 하 여 사전 설치 된 Apple 앱을 사용 하 여 작동 해야 합니다.

자세한 내용은 참조 하십시오 우리의 [핸드 오프](~/ios/platform/handoff.md) 가이드입니다.

## <a name="unified-storyboards"></a>통합 Storyboards
iOS 8 포함 새 사용자 인터페이스를 만들기 위한 메커니즘을 사용 하기 더욱 간편해졌으며-통합된 스토리 보드입니다. 모든 다른 하드웨어 화면 크기에 맞게 단일 스토리 보드를 사용 하 여 true "한 번 설계 하면, 대부분 사용 하 여" 스타일의 빠르고 응답성이 뷰를 만들 수 있습니다.

Ios8에 전송 하기 전에 개발자는 다음과 같이 사용 됩니다. `UIInterfaceOrientation` 가로 및 세로 모드를 구별할 수 및 `UIInterfaceIdiom` iOS 장치를 구별할 수 있습니다. Ios8에 전송에서는 더 이상 iPhone 및 iPad 장치에 대 한 별도 스토리 보드를 작성 하는 데 필요한-방향 및 장치를 사용 하 여 결정 됩니다 *Size 클래스*합니다.

세로 및 가로 축에서 크기 클래스에 정의 된 모든 장치 및 iOS 8에서에서 size 클래스의 두 종류가 있습니다.

- **일반** -큰 화면 크기 (예: iPad) 또는 (예:는 UIScrollView 큰 크기의 느낌을 제공 하는 가젯의입니다
- **Compact** -작은 장치 (예: iPhone)입니다. 이 크기는 장치의 방향을 고려 합니다.

두 가지 개념을 함께 사용할 경우 결과 다음 다이어그램에 표시 된 대로 모두 서로 다른 방향에서 사용할 수 있는 가능한 다양 한 크기를 정의 하는 2x2 표:

![](introduction-to-ios8-images/image3.png "서로 다른 두 방향을 모두에서 사용할 수 있는 가능한 다양 한 크기를 정의 하는 2x2 표를 나타내는 다이어그램")
 
Size 클래스에 대 한 자세한 내용은 참조는 [통합 스토리 보드 소개](~/ios/user-interface/storyboards/unified-storyboards.md)합니다.

## <a name="photo-kit"></a>사진 키트
사진 키트는 응용 프로그램을 시스템 이미지 라이브러리를 쿼리 및 확인 하 고 해당 내용을 수정할 사용자 지정 사용자 인터페이스를 만들 수 있는 새로운 프레임 워크입니다. 많은 앨범 및 폴더와 같은 자산 컬렉션 뿐만 아니라 이미지 및 비디오 자산을 나타내는 클래스가 포함 됩니다.

자세한 내용은 참조 하십시오 우리의 [PhotoKit](~/ios/platform/photokit.md) 가이드입니다.

## <a name="games"></a>게임

<a name="scenekit" />

### <a name="scene-kit"></a>Scene Kit

장면 키트는 3D 장면 그래프 3D 그래픽 작업을 간소화 하는 API입니다. OS X 10.8에 처음 도입 하 고 iOS 8 하기 위해 이제 왔습니다. Scene Kit를 사용 하 여 몰입 감이 뛰어난 3D 시각화 및 아마추어 3D 게임을 만들기는 OpenGL에 대 한 전문이 필요 하지 않습니다. 일반적인 장면 그래프 개념을 토대로 Scene Kit 추상화 OpenGL 및 OpenGL ES를 매우 간편 하 게 추가 3D 콘텐츠 응용 프로그램의 복잡성입니다. 그러나 OpenGL 전문가 라면 Scene Kit는도 OpenGL을 사용 하 여 직접 시도 하는 것에 대 한 훌륭한 지원 합니다. 또한 물리학 같은 3D 그래픽을 보완 하는 다양 한 기능을 포함 하 고 몇 가지 Apple 등 다른 프레임 워크, Core Animation, Core 이미지 및 Sprite Kit를 사용 하 여 매끄럽게 통합 되 합니다.

자세한 내용은 참조 하십시오 우리의 [SceneKit](~/ios/platform/gaming/scenekit.md) 설명서.

<a name="spritekit" />

### <a name="sprite-kit"></a>Sprite Kit

Sprite Kit를 Apple에서 2D 게임 프레임 워크에는 iOS 8 및 OS X Yosemite의 몇 가지 흥미로운 새 기능에 있습니다. 여기에 Scene Kit, 셰이더 지원, 조명, 그림자, 제약 조건, 법선 맵 생성 및 물리학 향상 된 기능을 사용 하 여 통합이 포함 됩니다. 특히 물리 기능과 매우 쉽게 게임에 실제 효과를 추가 합니다.

자세한 내용은 참조 하십시오 우리의 [SpriteKit](~/ios/platform/gaming/spritekit.md) 설명서.

## <a name="other-changes"></a>기타 변경 내용
위에서 설명 하는 iOS 8의에서 주요 변경 내용 뿐 아니라 Apple 또한 많은 기존 프레임 워크를 업데이트 했습니다. 이러한 아래에서 자세히 설명 합니다.

- **[Core 이미지가](https://developer.apple.com/library/prerelease/ios/documentation/GraphicsImaging/Reference/CoreImagingRef/index.html#//apple_ref/doc/uid/TP40001171)**  – Apple에 해당 이미지 처리 프레임 워크의 사각형 영역을 검색에 대 한 더 나은 지원을 추가 하 여 확장 하 고 QR 코드 이미지 내에서. Mike Bluestein 탐색이 post 자격이 블로그에서 [iOS 8에서에서 이미지 검색](https://blog.xamarin.com/image-detection-in-ios-8/)

## <a name="deprecated-apis"></a>사용되지 않는 API
모든의 개선 된 ios 8 사용 하 여 다양 한 Api가 사용 되지 않습니다. 그 중 일부는 다음과 같습니다.

- **[UIApplication](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplication_Class/index.html#//apple_ref/occ/cl/UIApplication)**  – 원격 알림 등록에 사용 되는 속성과 메서드는 사용 되지 않습니다. 이들은 registerForRemoteNotificationTypes 및 enabledRemoteNotificationTypes입니다.
- **[UIViewController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIViewController_Class/index.html#//apple_ref/occ/cl/UIViewController)**  – 특성 및 size 클래스 메서드 및 인터페이스 방향에 설명 하는 데 사용 되는 속성을 대체 했습니다. 참조 된 [통합 스토리 보드 소개](~/ios/user-interface/storyboards/unified-storyboards.md) 이 방법을 사용 하는 방법에 대 한 자세한 내용은 합니다.

- **[UISearchDisplayController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UISearchDisplayController_Class/index.html#//apple_ref/occ/cl/UISearchDisplayController)**  –이 iOS8에 UISearchController로 대체 되었습니다.

## <a name="summary"></a>요약
이 문서에서 Apple iOS 8에에서 도입 된 새로운 기능 중 일부에 대해 살펴보았습니다.



## <a name="related-links"></a>관련 링크

- [UIKitEnhancements (샘플)](https://developer.xamarin.com/samples/monotouch/ios8/UIKitEnhancements)
- [앱 확장 소개](~/ios/platform/extensions.md)
- [CloudKit 소개](~/ios/data-cloud/intro-to-cloudkit.md)
- [문서 선택기 소개](~/ios/platform/document-picker.md)
- [HealthKit 소개](~/ios/platform/healthkit.md)
- [수동 카메라 컨트롤 소개](~/ios/user-interface/controls/intro-to-manual-camera-controls.md)
- [Touch Id 소개](~/ios/platform/touchid.md)
- [통합된 Storyboards 소개](~/ios/user-interface/storyboards/unified-storyboards.md)
