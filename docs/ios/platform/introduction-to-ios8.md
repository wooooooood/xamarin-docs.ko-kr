---
title: "IOS 8 소개"
description: "Ios 8, 사과 새로운 프레임 워크 및 Api를 개발자가 자극 방식과 다양 한을 제공 했습니다. 이 가이드에서 이러한 새 Api를 소개 하 고 개발자와 사용자가 iOS 8 개념과 장점 참조 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 19049ED5-B68E-4A0E-9D57-B7FAE3BB8987
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: 3a77d1a3b597667d8944156b040c2819a5c79ca2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-ios-8"></a>IOS 8 소개

_Ios 8, 사과 새로운 프레임 워크 및 Api를 개발자가 자극 방식과 다양 한을 제공 했습니다. 이 가이드에서 이러한 새 Api를 소개 하 고 개발자와 사용자가 iOS 8 개념과 장점 참조 합니다._

iOS 7에서 첫 번째 iPhone 운영 체제에서 바로 기대 하는 어떤 사용자와 개발자가 온 것일 전체 iOS 사용자 인터페이스를 시각적으로 변경 합니다. IOS 8을 제공 하 여 많은 프레임 워크는 개발자를 위한 사용자가 자신의 iPhone에서 직접 해당 수명의 거의 모든 측면을 제어할 수 있는이 계속 됩니다. 예를 들어 상태 및 적합성에 대 한 분석할 수 있는 *HealthKit*, 암호 코드는 생체 인식 인증에 사용 하 여 더 *LocalAuthentication*, *앱 확장이*타사 앱, 간의 통신 채널을 열기 및 *HomeKit* 집 미래의 홈으로 바꿀 수 있습니다. 

사용자가 delighting 하는 방법에 대 한 iOS 7 되었으면 iOS 8 delighting 개발자 모든 범위의 맛있게 이러한 새로운 도구와에 중점을 둡니다. 

이 가이드에서는 Xamarin.iOS 개발자를 위한 새 Api를 소개합니다.  

이 문서의 끝에 설명 하는 8, iOS에서 사용 되지 않는 몇 가지 Api도 있습니다.

## <a name="requirements"></a>요구 사항

다음 Mac 용 Visual Studio에서 iOS 8 앱을 만들기 위해 필요한입니다.

- **Xcode 7 및 iOS 8 이상이** – Apple의 최신 Xcode 및 iOS Api 설치 하 고 개발자의 컴퓨터에 구성 해야 합니다.
- **Mac 용 visual Studio** -최신 버전의 Mac 용 Visual Studio를 설치 하 고 사용자 장치에 구성 해야 합니다.
- **iOS 8 장치 또는 시뮬레이터** – iOS 장치에서 최신 버전의 테스트를 위해 iOS 8 실행 합니다.

## <a name="home-and-leisure"></a>홈 및 Leisure

iOS 8 모뎀과 HomeKit HealthKit 사용 하면서 집의 핵심 곧바로, 사과 및 iOS 장치를 공장에 있었습니다. 이 섹션에서는 모두 이러한 새로운 프레임 워크 작업과 Xamarin.iOS 응용 프로그램에 통합할 수 있습니다 어떻게에서 살펴보겠습니다.

## <a name="homekit"></a>HomeKit

IPhone에서 기기가 제어 기술의; 새 응용 프로그램이 아닙니다. iOS 앱을 통해 연결 된 홈으로 많은 제품을 제어할 수 있습니다. 그러나 HomeKit 더 길어진이 한 단계 더 나아가 가정 자동화 장치에 대 한 일반 프로토콜을 승격 하 고 특정 제조업체 iHome, 같은 십자 및 Honeywell 공용 API를 사용할 수 있도록 설정 합니다. 자신의 홈에서 원활 하 게의 거의 모든 측면을 제어할 수 즉 사용자에 대 한 응용 프로그램 내 합니다. 것은 십자 색상 전구 또는 중첩 경보 사용 하는 것을 알아야와 관련이 없습니다. 사용자가 다양 한 스마트 홈 프로세스 함께 "상태"으로 연결할 수도 있습니다.

HomeKit와 타사 앱과 Siri 보조 프로그램을 검색 하 고 자신의 개인 홈 구성 데이터베이스에 추가할 하 고이 데이터에 사용 될 편집한 accessories 및 작업을 수행 하려면 서비스와 통신 합니다.

### <a name="configuration"></a>구성

아래 다이어그램에서는 HomeKit 액세서리 구성의 기본 계층 구조를 보여 줍니다.

![](introduction-to-ios8-images/image1.png "이 다이어그램 HomeKit 액세서리 구성의 기본 계층 구조를 보여 줍니다.")
 
HomeKit을 시작 하려면 개발자가 선택 되어 있는 HomeKit 서비스 프로 비전 프로필에 있는지 확인 해야 합니다. Apple 개발자 HomeKit 시뮬레이터 추가 기능에서 Xcode에 제공 했습니다. 찾을 수 있습니다는 [Apple 개발자 센터](https://developer.apple.com/downloads/index.action)아래 `Hardware IO Tools for Xcode`합니다. 

자세한 내용은 참조 하십시오 우리의 [HomeKit](~/ios/platform/homekit.md) 가이드입니다.

## <a name="healthkit"></a>HealthKit

HealthKit 상태 관련 정보에 대 한 중앙 집중화 하 고, 통합, 안전한 데이터 저장소를 제공 하는 iOS 8에에서 도입 된 프레임 워크입니다. 운영 체제는 개인 정보 및 상태 정보 및 보안, 상태 앱을 사용자에 대 한 대시보드를 확인합니다. 사용자의 권한이 있는 응용 프로그램 읽고 다양 한 상태 정보를 쓸 수 있습니다.

Xamarin.iOS 앱에서이 사용 하 여에 대 한 자세한 내용은 참조는 [HealthKit 소개](~/ios/platform/healthkit.md) 가이드입니다.

## <a name="extending-iphone-functionality"></a>IPhone 기능 확장
IOS8와 개발자가 훨씬 더 많은 제어할 타사 앱 들 간의 보다 개방적 통신에 대 한 해당 응용 프로그램 및 향상 된 기능을 사용할 수 있는 사용자 지정 되 고 됩니다. 응용 프로그램 확장 및 문서 선택 등의 기능 Apple의 에코 시스템에서 응용 프로그램을 사용할 수 있는 방법을 대 한 새로운 가능성을 엽니다.

### <a name="app-extensions"></a>응용 프로그램 확장
응용 프로그램 확장을를 지나치게 단순화할는 서로 통신할 수 있는 타사 앱에 대 한 방법입니다. 높은 보안 표준을 유지 관리 하 고 샌드 박싱된 응용 프로그램의 무결성을 하기 필요한이 통신은 응용 프로그램 간에 직접 발생 하지 않습니다. 대신이 수행 하는 여는 *확장* 중간에 있습니다.

앱 확장을 만드는 첫 번째 단계는 올바른 확장 지점을 정의 하는 것 —이 동작 및 올바른 Api의 가용성을 확인 하는 데 중요 합니다. Mac 용 Visual Studio에서 앱 확장을 만드는, 기존 응용 프로그램 여 추가 솔루션에 새 프로젝트를 추가 합니다.

에 **새 프로젝트** 대화 이동 **C#** > **iOS** > **통합 API**  >   **확장**아래 스크린샷에 표시 된 것 처럼,:

![](introduction-to-ios8-images/image2.png "새 확장명 만들기")
 
새 프로젝트 대화 상자 앱 확장을 만들기 위한 새로운 7 개의 프로젝트 템플릿이 제공 하 고 아래에 설명 되어 있습니다. 확장의 대부분 iOS 문서 선택 등의 다른 새 api와 관련 된 주의 사항:

- **작업** -개발자는 특정 작업을 수행 하는 사용자가을 허용 하는 고유한 사용자 지정 작업 단추를 만들 수 있습니다
- **사용자 지정 키보드** – Apple 키보드에서 자신의 사용자 지정 필터를 추가 하 여 빌드한의 범위에 추가 하려면 개발자가이 있습니다. 인기 있는 키보드 Swype를 사용 하 여이 키보드 iOS를 가져와야 합니다.
- **선택기 문서** –이 사용자가 응용 프로그램의 샌드박스 외부 파일에 액세스할 수 있는 문서 선택 보기 컨트롤러를 포함 합니다.
- **선택 파일 공급자를 문서화** -이 문서 선택기를 사용 하 여 파일에 대 한 안전한 저장소를 제공 합니다.
- **사진 편집** –이 필터를 확장 하 고 이미 사용자에 게 더 많은 제어 및 기타 옵션의 사진 편집할 때 사진 응용 프로그램에 Apple에서 제공한 도구를 편집 합니다.
- **오늘** -이렇게 하면 응용 프로그램 위젯 알림 센터의 오늘 섹션에 표시 하는 기능입니다.

Xamarin에서 응용 프로그램 확장 사용에 대 한 자세한 내용은 참조는 [응용 프로그램 확장 소개](~/ios/platform/extensions.md) 가이드입니다.

### <a name="touch-id"></a>Touch ID

Touch ID 사용자를 인증 하는 방법으로 iOS 7에에서 도입 된-암호와 비슷합니다. 그러나 장치를 잠금 해제, 앱 스토어를 사용 하 여 iTunes를 사용 하 여 및 iCloud 키 집합에만 인증으로 제한 되었습니다. 

로컬 인증 API를 사용 하 여 iOS 8 응용 프로그램의 인증 메커니즘으로 Touch ID를 사용 하는 두 가지 됩니다. 현재 수 없으면 원격으로 인증할 수 로컬 인증을 사용 하도록 합니다.

첫째, 새로운 키 집합 액세스 제어 목록 (Acl)를 사용 하 여 기존 키 집합 서비스를 지원 합니다. 키 집합 데이터는 사용자가 지문의 성공적인 인증을 사용 하 여 잠금 수 있습니다.

둘째, LocalAuthentication 응용 프로그램을 로컬로 인증 하는 두 가지 방법을 제공 합니다. 사용 해야 `CanEvaluatePolicy` Touch ID를 받아들일 수 있는 장치 인지 확인 하려면 다음 `EvaluatePolicy` 인증 작업을 시작 합니다.

Touch ID 및 Xamarin.iOS 응용 프로그램에 통합 하는 방법은 자세한 내용은 참조는 [TouchID를 소개](~/ios/platform/touchid.md) 가이드입니다.

### <a name="document-picker"></a>문서 선택

사용자는 다른 앱에서 생성 된, 및 조작할 가져오고 내보낼 파일을 열 수 있도록 사용자가 iCloud 드라이브로 문서 선택 works 다시 철회 합니다. 직관적인 워크플로 및 따라서 훨씬 더 나은 환경을, 사용자가 만듭니다. 이 한 단계를 추가로 사용 icloud와 동기화 하는 중-한 응용 프로그램에서 변경한 내용은 수 반영 됩니다 일관 되 게 모든 장치 간에 합니다.

[문서] 선택기 좀 더 깊이에 대 한 자세한 내용은 및를 Xamarin.iOS 응용 프로그램에 통합 하는 방법에 알아보려면 참조는 [The 문서 선택 소개](~/ios/platform/document-picker.md) 가이드입니다.

### <a name="handoff"></a>전달

핸드 오프의 일부인 큰 연속성 기능, iOS 및 OS X 통합 하기 위한 추가 단계를 사용 합니다. 여기에 플랫폼 간 AirDrop, 호출 iPhone, iPad 및 Mac 및 iPhone에서 테더 링의 향상 된 기능에는 SMS를 수행할 수 있게 합니다.

핸드 오프 Yosemite, 및 iOS 8와 협력 하 고 사용 하려는 다른 모든 장치에 로그인 할 iCloud 계정이 필요 합니다. Safari, iWork, 지도, 일정 및 연락처를 포함 하 여 사전 설치 된 Apple 앱과 함께 작동 해야 합니다.

자세한 내용은 참조 하십시오 우리의 [핸드 오프](~/ios/platform/handoff.md) 가이드입니다.

## <a name="unified-storyboards"></a>통합 된 스토리 보드
새 포함 iOS 8 사용자 인터페이스를 만들기 위한 메커니즘을 사용 하기-통합 된 스토리 보드 합니다. 모든 다른 하드웨어 화면 크기 처리 하려면 단일 스토리 보드와 true "한 번 설계, 많은 사용 하 여" 스타일에서 신속 하 고 응답 뷰를 만들 수 있습니다.

IOS8, 이전 버전 개발자는 다음과 같이 사용 됩니다. `UIInterfaceOrientation` 가로 세로 모드 간을 서로 구별 하 및 `UIInterfaceIdiom` iOS 장치를 구별할 수 있습니다. IOS8에서 더 이상 필요는 iPhone 및 iPad 장치에 대 한 별도 스토리 보드를 만들-방향 및 장치를 사용 하 여 결정 됩니다 *크기 클래스*합니다.

세로 가로 축에서 크기 클래스로 정의 된 모든 장치 및 iOS 8의에서 크기 클래스의 두 종류가 있습니다.

- **일반** -(예: iPad) 큰 화면 크기 또는 크기를 크게 (예: 한 UIScrollView 느낄 수 가젯을입니다
- **Compact** -작은 장치 (iPhone) 등입니다. 이 크기는 장치의 방향을 고려합니다.

두 가지 개념을 함께 사용할 경우 결과 다음 다이어그램에 표시 되는 모두 서로 다른 방향에 사용할 수 있는 다양 한 가능한 크기를 정의 하는 2 x 2 표:

![](introduction-to-ios8-images/image3.png "서로 다른 두 방향에서 사용할 수 있는 다양 한 가능한 크기를 정의 하는 2 x 2 눈금을 나타내는 다이어그램")
 
크기 클래스에 대 한 자세한 내용은 참조는 [통합 스토리 보드를 소개](~/ios/user-interface/storyboards/unified-storyboards.md)합니다.

## <a name="photo-kit"></a>사진 키트
사진 키트는 응용 프로그램 시스템 이미지 라이브러리를 쿼리하고 보고 내용을 수정 하는 사용자 지정 사용자 인터페이스를 만들 수 있도록 하는 새로운 프레임 워크입니다. 다양 한 이미지 및 비디오 자산으로 앨범 및 폴더와 같은 자산 컬렉션을 나타내는 클래스를 포함 합니다.

자세한 내용은 참조 하십시오 우리의 [PhotoKit](~/ios/platform/photokit.md) 가이드입니다.

## <a name="games"></a>게임

<a name="scenekit" />

### <a name="scene-kit"></a>장면 키트

장면 키트 3D 장면 graph API 작업 3D 그래픽을 간소화 하는입니다. OS X 10.8에서 처음 도입 하 고 iOS 8 하기 위해 이제 왔습니다. 장면 키트 몰입 3D 시각화 및 아마추어 3D 게임을 만들기는 OpenGL 관련 지식이 필요 하지 않습니다. 일반적인 장면 그래프 개념에 대해 빌드, 장면 키트에서 벗어난 OpenGL 및 OpenGL ES 매우 간편 하 게 추가 3D 콘텐츠를 응용 프로그램의 복잡성을 합니다. 그러나 OpenGL 전문가 인 경우 장면 키트는도 OpenGL을 사용 하 여 직접 전후해 서 연결에 대 한 뛰어난 지원 합니다. 또한 물리학 같은 3D 그래픽을 보완 하는 다양 한 기능을 포함 하 고 몇 가지 다른 Apple 프레임 워크 Sprite 키트 코어 애니메이션, Core 이미지 등 매우 잘 통합 합니다.

자세한 내용은 참조 하십시오 우리의 [SceneKit](~/ios/platform/gaming/scenekit.md) 설명서입니다.

<a name="spritekit" />

### <a name="sprite-kit"></a>Sprite 키트

Sprite 키트, Apple에서 2D 게임 프레임 워크는 iOS 8 및 OS X Yosemite의 몇 가지 흥미로운 새로운 기능에 있습니다. 여기에 장면 키트, 셰이더 지원, 조명, 그림자, 제약 조건, 기본 맵 생성 및 물리학 향상 된 기능 통합을 포함 합니다. 특히, 새로운 물리 기능 매우 쉽게 게임에 실제 효과를 추가 합니다.

자세한 내용은 참조 하십시오 우리의 [SpriteKit](~/ios/platform/gaming/spritekit.md) 설명서입니다.

## <a name="other-changes"></a>기타 변경 내용
위에서 설명 하는 iOS 8의에서 주요 변경 내용 및 Apple 많은 기존 프레임 워크 또한 업데이트 했습니다. 이 아래에서 자세히:

- **[이미지 핵심](https://developer.apple.com/library/prerelease/ios/documentation/GraphicsImaging/Reference/CoreImagingRef/index.html#//apple_ref/doc/uid/TP40001171)**  – Apple는 사각형 영역을 감지 하기 위한 더 나은 지원을 추가 하 여 해당 이미지 처리 프레임 워크에 확장 하 고 이미지 내에 QR 코드입니다. Mike Bluestein이 권한을 부여 받은 후 그의 블로그에 보며 [iOS 8에서에서 이미지 검색](http://blog.xamarin.com/image-detection-in-ios-8/)

## <a name="deprecated-apis"></a>사용되지 않는 API
IOS 8에서 모든 향상, 다양 한 Api가 사용 되지 않습니다. 이 중 일부는 다음과 같습니다.

- **[UIApplication](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplication_Class/index.html#//apple_ref/occ/cl/UIApplication)**  -메서드 및 속성 원격 알림 등록에 사용 되는 사용 되지 않습니다. 이들은 registerForRemoteNotificationTypes 및 enabledRemoteNotificationTypes입니다.
- **[UIViewController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIViewController_Class/index.html#//apple_ref/occ/cl/UIViewController)**  -메서드 및 인터페이스 방향에 설명 하는 데 사용 되는 속성 특성 및 크기 클래스는 대체 합니다. 참조는 [스토리 보드 통합 소개](~/ios/user-interface/storyboards/unified-storyboards.md) 이 방법을 사용 하는 방법에 대 한 자세한 내용은 합니다.

- **[UISearchDisplayController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UISearchDisplayController_Class/index.html#//apple_ref/occ/cl/UISearchDisplayController)**  –이 iOS8에 UISearchController로 대체 되었습니다.

## <a name="summary"></a>요약
이 문서에서는 Apple에서 iOS 8에에서 도입 된 새로운 기능 중 일부에서 찾았습니다.



## <a name="related-links"></a>관련 링크

- [UIKitEnhancements (샘플)](https://developer.xamarin.com/samples/monotouch/ios8/UIKitEnhancements)
- [응용 프로그램 확장 프로그램 소개](~/ios/platform/extensions.md)
- [CloudKit 소개](~/ios/data-cloud/intro-to-cloudkit.md)
- [문서 선택 소개](~/ios/platform/document-picker.md)
- [HealthKit 소개](~/ios/platform/healthkit.md)
- [수동 카메라 컨트롤 소개](~/ios/user-interface/controls/intro-to-manual-camera-controls.md)
- [TouchID 소개](~/ios/platform/touchid.md)
- [통합 된 스토리 보드에는 소개](~/ios/user-interface/storyboards/unified-storyboards.md)
