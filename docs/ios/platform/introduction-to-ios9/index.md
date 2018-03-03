---
title: "IOS 9 소개"
description: "이 문서 Xamarin.iOS 개발자를 위한 모든 새로운 기능과 수정 된 Api 및 iOS 9에에서 사용할 수 있는 기능을 소개합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4D71BBD9-B948-4B59-9AF5-F199C51CBEB3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 5b26989603695cfb309fba5a5318f7ef4d2460e2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-ios-9"></a>IOS 9 소개

_이 문서 Xamarin.iOS 개발자를 위한 모든 새로운 기능과 수정 된 Api 및 iOS 9에에서 사용할 수 있는 기능을 소개합니다._

![](images/ios9-sml.png "IOS 9 로고")

Apple는 iOS 9 기존 기능에는 많은 향상 된 구성 요소가에서 몇 가지 새로운 Api 및 서비스를 추가 되었습니다.

## <a name="3d-touch"></a>3D 터치

IOS 9 iPhone 6s와 iPhone 6s 새 또한 3D 터치 iOS 앱에가 중 중요 한 제스처를 추가 합니다. 3D 사용 하 여 터치, iPhone 앱 뿐만 아니라 사용자가 장치의 화면을 터치 할 때를 알 수 이제는 것도 사용자 있으므로 넓은 얼마나 압력을 감지 하 고 응답할 수 다른가 중 수준입니다.

3D 터치 앱에 다음과 같은 기능을 제공합니다.

- **압력 감지** -앱이 얼마나 측정할 이제 수 또는 light 사용자 화면을 수행 하십시오 활용 해당 정보에 연결 되어 있습니다. 예를 들어 그리기 응용 프로그램 사용자는 화면을 터치가 얼마나 살 기반으로 또는 두꺼울수록 줄을 가능 합니다.
- **피크 (peek) 및 팝** -응용 프로그램 사용자가 자신의 현재 컨텍스트 외부 이동 하지 않고도 데이터와 상호 작용할 수 이제 수 있습니다. 수 화면에 하드 눌러 *피킹* (예: 메시지를 미리 보기) 관심 있는 항목입니다. 수 쉽다는 점 눌러 *팝* 항목으로 합니다.
- **빠른 작업** -수 수 팝 접속 데스크톱 응용 프로그램에서 해당 항목의 누를 때 있는 상황에 맞는 메뉴와 같은 빠른의 작업의 생각 합니다. 빠른 작업을 사용 하 여 추가할 수 있습니다 빠르고 쉽게 일반 함수에 대 한 액세스 바로 가기에 응용 프로그램에서 iOS 장치에서 홈 화면 아이콘에서.

자세한 내용은 참조 하세요 우리의 [3D 터치 소개](~/ios/platform/3d-touch.md) 가이드입니다.

## <a name="app-transport-security"></a>앱의 전송 보안

새 앱 전송 보안 (AT) iOS 9에는 인터넷 리소스 (예: 응용 프로그램의 백 엔드 서버)와 응용 프로그램 간 보안 연결을 적용 합니다. 응용 프로그램 또는 소비 하는 라이브러리를 통해 직접 중요 한 정보가 노출 되거나 실수로 인 한 것을 방지 모든 인터넷 통신 보안 연결 모범 사례를 준수 AT에서 확인 합니다.

AT 9 iOS 및 OS X 10.11 (El Capitan)에 대 한 모든 연결을 사용 하 여 빌드한 앱에 기본적으로 활성화 되어 있으므로 [NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/), [CFUrl](https://developer.xamarin.com/api/type/CoreFoundation.CFUrl/) 또는 [NSUrlSession](https://developer.xamarin.com/api/type/Foundation.NSUrlSession/) 를 일으키는 됩니다 AT 보안 요구 사항입니다. 연결에는 이러한 요구 사항을 충족 하지 않는 경우 예외와 함께 실패 합니다.

AT에 대 한 자세한 정보를 확인 하려면를 참조 하십시오 우리의 [앱 전송 보안](~/ios/app-fundamentals/ats.md) 가이드입니다.

<a name="multitasking" />

## <a name="multitasking-for-ipad"></a>IPad 용 멀티태스킹

IOS 9와 Apple iPad 특정 하드웨어에 동시에 두 개의 응용 프로그램을 실행 하기 위한 멀티태스킹 지원을 추가 했습니다. 결과적으로, Xamarin.iOS 앱 어느 시점에서 실행 중인 유일한 앱 되는지 또는 전체 화면 또는 장치의 리소스에 액세스할 수 있는 더 이상 가질 수 없습니다.

멀티태스킹 iPad에 대 한 다음과 같은 기능을 통해 사용할 수 있습니다.

- **통해 슬라이드** -일시적으로 현재 실행 중인 기본 응용 프로그램의 25%에 약 적용 (언어 방향에 따라 화면 오른쪽 이나 왼쪽에 하나) 패널 아웃 슬라이드에서 두 번째는 iOS 앱을 실행 하려면 사용자 수 있습니다. 통해 이동은 iPad Pro, 공기 iPad, iPad 공기 2, iPad 미니 2, 미니 3, iPad 또는 iPad를 미니 4 에서만 사용할 수 있습니다.
- **분할 된 뷰** -지원 되는 iPad 하드웨어 (iPad iPad를 미니 4 및 iPad Pro만 공기 2)에서 사용자는 두 번째 응용 프로그램을 선택 하 고 실행할 수 나란히 분할 화면 모드에서 현재 실행 중인 앱과 함께 합니다. 사용자는 각 응용 프로그램에서 차지 하는 기본 화면의 백분율을 제어할 수 있습니다.
- **그림에는 그림** -비디오 콘텐츠 재생, 이제 비디오 수 있는 iOS 장치에서 현재 실행 중인 다른 응용 프로그램을 통해 배치 되는 이동 가능한 및 크기 조정 가능한 창에서 재생할 수 있는 응용 프로그램에 있습니다. 사용자에 게이 창의 위치와 크기에 대 한 모든 권한을 합니다. 그림에는 그림은 iPad Pro, 공기 iPad, iPad 공기 2, iPad 미니 2, 미니 3, iPad 또는 iPad를 미니 4 에서만 사용할 수 있습니다.

IOS 9의 새로운 멀티태스킹 능력에 대 한 자세한 정보를 확인 하려면를 참조 하십시오 우리의 [iPad 용 멀티태스킹](~/ios/platform/multitasking.md) 가이드입니다.

## <a name="new-contacts-and-contacts-ui-frameworks"></a>새 연락처 및 연락처 UI 프레임 워크

Apple iOS 9의 도입으로 두 개의 새로운 프레임 워크를 출시 했습니다 [연락처](https://developer.xamarin.com/api/namespace/Contacts/) 및 [ContactsUI](https://developer.xamarin.com/api/namespace/ContactsUI/), 기존 주소록을 바꾸는 및 8 및 이전 버전 iOS에서 사용 되는 주소 주소록 UI 프레임 워크입니다.

이러한 새, 개체 지향 프레임 워크 다음 정보를 제공 합니다.

- **연락처** – 사용자의 연락처 정보에 대 한 제공 Xamarin.iOS 액세스 합니다. 대부분의 응용 프로그램에는 읽기 전용 액세스 해야 하기 때문에이 프레임 워크는 스레드로부터 안전 하 고 읽기 전용 액세스를 위해 최적화 되었습니다.
- **ContactsUI** – 제공 Xamarin.iOS UI 요소를 표시 하려면 편집을 선택 하 고 iOS 장치에서 연락처를 만들 합니다.

자세한 내용은 참조는 [연락처 및 연락처 UI](~/ios/platform/contacts.md) 설명서입니다.


## <a name="new-search-apis"></a>새 검색 Api

검색에서 iOS 9 다양 한 방법으로 Xamarin.iOS 앱 내에서 정보에 액세스할 수 있도록 확장 되었습니다. 새 검색 Api를 사용 하 여 가능 응용 프로그램의 콘텐츠 스포트라이트 및 Safari 검색 결과 전달 하 고 Siri 미리 알림 및 제안을 통해 검색할 수 있는 합니다. 따라서 사용자가 활동 및 앱 내에서 전체 정보에 빠르게 액세스할 수 있습니다.

또한 새로운 검색 Api 쉽게 검색 이전 검색 구현 환경 없이 응용 프로그램에 통합할 수 있습니다. 이 인해 Apple iOS 9 응용 프로그램의 콘텐츠를 앱 검색을 사용 하 여 범용으로 검색할 수 있도록 하는 데 몇 시간이 걸릴 일반적으로 클레임입니다.

자세한 내용은 참조 하십시오 우리의 [검색 향상](~/ios/platform/search/index.md) 설명서입니다.

## <a name="new-stack-view"></a>새 스택 보기

스택 뷰 컨트롤 ([UIStackView](https://developer.xamarin.com/api/type/UIKit.UIStackView/)) 이용 하 여 자동 레이아웃 및 크기 클래스 (가로 또는 세로로) 하위 스택을 관리 하려면 iOS 장치의 화면 크기와 방향에 동적으로 응답 하 합니다.

스택 뷰 컨트롤을 사용 하 여 작업의 양을 크게 줄어듭니다 사용자 인터페이스 레이아웃에 필요 합니다. 스택 보기에 연결 된 모든 하위 뷰가 레이아웃 축, 배포, 맞춤 간격 등 개발자 정의 속성에 따라 자동으로 관리 됩니다.

자세한 내용은 참조 하십시오 우리의 [스택 보기 소개](~/ios/user-interface/controls/uistackview.md) 설명서입니다.


## <a name="collection-view-changes"></a>컬렉션 변경 내용 보기

Ios 9, 컬렉션 보기 ([UICollectionView](https://developer.xamarin.com/api/type/UIKit.UICollectionView/)) 지원 끌어 새 기본 제스처 인식기 및 새 지원 메서드가 여러 개 추가 하 여 기본 항목 다시 정렬 합니다.

이러한 새 메서드를 사용 하 여 쉽게 컬렉션 보기에서 끌어서 순서 다시 매기기를 구현 하 고 수 놓기와 프로세스의 모든 단계 항목 모양 사용자 지정 하는 옵션이 있습니다.

IOS 9에 대 한 변경 내용 컬렉션 보기에 대 한 자세한 정보를 확인 하려면를 참조 하십시오 우리의 [변경 내용 컬렉션 보기](~/ios/user-interface/controls/uicollectionview.md) 가이드입니다.

## <a name="game-enhancements"></a>게임의 향상 된 기능

IOS 9와 Apple 쉽게 Xamarin.iOS 앱에서 게임 그래픽 및 오디오를 구현 하는 게임 Api에 몇 가지 기술적 개선을 했습니다. 여기에 두 간편한 개발을 통해 높은 수준의 프레임 워크 및 향상 된 속도 하위 수준 향상 기능이 포함 된 그래픽 기능에 대 한 iOS 장치 GPU의 기능을 통해 포함 됩니다.

여기에 GameplayKit "," ReplayKit "," 모델 I/O "," MetalKit "및" Metal 성능 셰이더 금속, SceneKit 및 SpriteKit 더욱 향상 기능입니다.

자세한 내용은 참조 하십시오 우리의 [게임의 향상 된 기능](~/ios/platform/gaming/index.md) 설명서입니다.

## <a name="homekit-framework-changes"></a>HomeKit Framework 변경 내용

[HomeKit](https://developer.xamarin.com/api/namespace/HomeKit/) iOS 8에에서 도입 된 프레임 워크를 설정 하 고 Xamarin.iOS 앱에서 (예: 자동화 된 광원, 도어 잠금 및 차고문 개폐) 다양 한 활성화 HomeKit 액세서리를 제어 하는 기능을 제공 합니다. 쉽게 설정 하 고 구성할 수 있을 뿐 HomeKit 액세서리 Siri의 구두 명령을 통해 제어할 수 있습니다.

IOS 9, Apple 설치 더 쉽게 수행할 수로, 지원 하 고 (예:는 액세서리 iCloud 통해 원격으로 제어) 자세한 액세서리 상호 작용을 제공 하는 액세서리 유형의 확장 합니다.

자세한 내용은 참조는 [HomeKit 소개](~/ios/platform/homekit.md), [HomeKitIntro iOS 샘플 응용 프로그램](https://developer.xamarin.com/samples/monotouch/HomeKit/HomeKitIntro/) 과 Apple [HomeKit](https://developer.apple.com/homekit/) 설명서입니다.

## <a name="handoff-framework-changes"></a>핸드 오프 Framework 변경 내용

핸드 오프 (연속성 라고도 함)는 사용자가 자신의 장치 (iOS 또는 Mac) 중 하나에 활동을 시작 하 고 다른 장치 (사용자의 iClou에서 식별 한 대로 같은 동작에 계속 하는 방법으로 Apple ios 8 및 OS X Yosemite (10.10)을 통해 시작 된 d 계정)입니다.

핸드 오프 향상 된 검색 기능에서 iOS 9도 새로 만들기를 지원 하도록 확장 되었습니다. 자세한 내용은 참조 하십시오 우리의 [검색 향상](~/ios/platform/search/index.md) 설명서입니다. 전달 하기를 사용 하는 방법은 참조 하십시오 우리의 [핸드 오프 소개](~/ios/platform/handoff.md) 설명서입니다.

## <a name="new-extension-points"></a>새 확장점

IOS 8, 사과 확장을 도입 했습니다. 즉 표시 되는 표준 컨텍스트에서 운영 체제에서와 같은 알림 센터 내에서 때나 사진 편집할 경우 사용자가 키보드를 요청할 때 라이브러리입니다.

Ios 9, 사과 몇 가지 새로운 제공 하 여 확장 프로그램별 지원 기능을 확장 _확장점_ 사용 정책을 정의 하 고 다음과 같이 지정 된 영역 내에서 작업 하기 위한 Api를 제공 합니다.

- **새로운 오디오 단위 확장 지점** –이 확장 지점을 사용 하 여 다른 오디오 장치 호스트 앱 (예: GarageBand) 내에서 사용할 오디오 효과, 음악 기기, 사운드 생성기 등을 제공 합니다. 이 확장 지점 또한 판매할 수 있습니다 _오디오 단위_ (오디오 플러그 인) 앱 스토어에 있습니다.
- **새 인덱스 유지 관리 확장 지점** -앱 롤백합니다 필요 없이 응용 프로그램 데이터의 인덱스를 다시 만들기를 지원 하기 위해이 확장 지점을 사용 합니다.
- **새 네트워크 확장점** (이러한 Apple에서 특별 한 사용 권한 필요):
    - **응용 프로그램 프록시 공급자 확장** -이 확장 지점을 사용 하 여 투명 한 사용자 지정 클라이언트 쪽 네트워크 프록시를 구현 합니다.
    - **데이터 공급자를 필터링 / 필터 제어 공급자 확장** -이러한 확장 점을 사용 하 여 네트워크를 동적 콘텐츠 장치 필터링을 구현 합니다.
    - **패킷 터널 공급자 확장** -확장 지점에이 사용 하 여 사용자 지정 VPN 터널링 프로토콜 클라이언트 쪽을 구현 합니다.
- **새 Safari 확장점**:
    - **차단 확장 콘텐츠** -확장 지점에이 사용 하 여 사용자가 웹을 검색할 때 표시 되지 것입니다 차단 된 콘텐츠의 목록을 정의할 수 있습니다.
    - **링크 확장 공유** -Safari의 공유 링크에 대 한 응용 프로그램의 콘텐츠를 볼 수 있도록이 확장 지점을 사용 합니다.

자세한 내용은 참조 하십시오 우리의 [확장명 소개](~/ios/platform/extensions.md) 과 Apple [App 확장 프로그래밍 가이드](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214) 설명서입니다.

## <a name="keychain-enhancements"></a>키 집합의 향상 된 기능

IOS 9, 사과 다음과 같은 보안 영토 및 더 많은 항목 보호 옵션에 대 한 새 암호화 키 형식을 제공 하는 키 집합을 향상:

- 새 Touch ID 하는 제약 조건 지문 데이터베이스를 수정 하는 경우 키 집합 항목을 무효화 합니다.
- Touch ID 또는 암호도 액세스 제어 목록 항목 만들기만 허용 하는 새 제약 조건입니다.
- 별도의 인증을 호출할 수 있는 새 인증 컨텍스트 `SecItem` 호출 합니다.
- 응용 프로그램에서 제공 되는 키 집합 항목 암호화에 대 한 제어 목록 entropy (응용 프로그램 암호 옵션 사용)에 액세스 합니다.
- 생성 하 고 보안 영토 내 키를 사용 하 여 지원 (통해는 `kSecAttrTokenIDSecureEnclave` 특성).

자세한 내용은 참조 하십시오 우리의 [Touch ID 소개](~/ios/platform/touchid.md) 설명서입니다.


## <a name="right-to-left-language-support"></a>오른쪽에서 왼쪽으로 쓰는 언어 지원

IOS 9 제시 대칭 이동 된 사용자 인터페이스를 보다 쉽게 적이 오른쪽에서 왼쪽 언어에 대 한 모든 지원을 제공 하 여 Apple가 만들었습니다. 여기에는 다음과 같은 사항이 포함됩니다.

- 표준 [UIKit](https://developer.xamarin.com/api/namespace/UIKit/) 컨트롤이 오른쪽에서 왼쪽으로 iOS 장치 로캘 및 언어 설정에 따라 자동으로 대칭 이동 됩니다.
- [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/) 지정된 보기 표시 되는 방식을 시기를 정의할 수 있도록 하는 특성이 오른쪽에서 왼쪽으로 대칭 이동 클래스를 제공 합니다.
- 이미지를 사용 하 여 프로그래밍 방식으로 대칭 수는 [FlipsForRightToLeftLayoutDirection](https://developer.xamarin.com/api/property/UIKit.UIImage.FlipsForRightToLeftLayoutDirection/) 의 속성은 [UIImage](https://developer.xamarin.com/api/type/UIKit.UIImage/) 클래스.

자세한 내용은 Apple의를 참조 하십시오 [지원 오른쪽에서 왼쪽으로 언어](https://developer.apple.com/library/prerelease/ios/documentation/MacOSX/Conceptual/BPInternational/SupportingRight-To-LeftLanguages/SupportingRight-To-LeftLanguages.html#//apple_ref/doc/uid/10000171i-CH17) 설명서입니다.



## <a name="additional-framework-changes"></a>추가 프레임 워크 변경 내용

위에서 설명 했습니다 된 주요 변경, 사과 수정 및 iOS 다음을 포함 하는 9에 대 한 몇 가지 기존 프레임 워크의 향상 된 기능 했습니다.

- AV Foundation 프레임 워크
- AVKit 프레임 워크
- CloudKit Framework
- Foundation 프레임 워크
- 핸드 오프 프레임 워크
- HealthKit 프레임 워크
- HomeKit 프레임 워크
- 로컬 인증 프레임 워크
- MapKit 프레임 워크
- PassKit 프레임 워크
- Safari 서비스 프레임 워크
- UIKit 프레임 워크

자세한 내용은 참조 하십시오 우리의 [추가 iOS 9 Framework 변경 내용](~/ios/platform/introduction-to-ios9/additional-framework-changes.md) 설명서입니다.

## <a name="deprecated-apis-and-functions"></a>사용 되지 않는 Api 및 함수

Api आ स ा iOS 9의에서 함수에는 Apple에 사용 되지 않는:

- **Book 및 주소 주소록 UI 주소** -이러한 Api는 연락처 및 연락처 UI 프레임 워크에 의해 대체 되었습니다. 자세한 내용은 참조는 [연락처 및 연락처 UI](~/ios/platform/contacts.md) 설명서입니다.
- **CBCentralManager** - `RetrievePeripherals` 및 `RetrieveConnectedPeripherals` 의 메서드는 `CBCentralManager` 클래스 iOS 9에서에서 제거 되었습니다. 이러한 메서드를 호출 하는 액세서리 쌍으로 연결 하는 경우 충돌 또는 앱 시작 시 앱이 발생 합니다.
- **FetchAllChanges** - `FetchAllChanges` 의 `CKFetchRecordChangesOperation` 클래스가 제공 되지 않으며 iOS 9에서에서 제거 될 예정입니다.
- **미디어 플레이어** -미디어 플레이어 프레임 워크는 iOS 9에에서 사용 되지 합니다. AVKit 또는 AV Foundation Api 중 하나를 대신 사용 합니다.

특정 API 결함의 전체 목록은, Apple의을 참조 하십시오. [iOS 9.0 API Diffs](https://developer.apple.com/library/prerelease/ios/releasenotes/General/iOS90APIDiffs/index.html#//apple_ref/doc/uid/TP40016222) 설명서입니다.

## <a name="ios-9-sample-apps"></a>iOS 9 샘플 앱

일부는 [iOS 9 관련 샘플](https://developer.xamarin.com/samples/ios/iOS9/) 시작 하려면:

- [AstroLayout](https://github.com/xamarin/monotouch-samples/tree/master/ios9/AstroLayout)
- [CollectionView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/CollectionView)
- [MetalPerformanceShadersHelloWorld](https://developer.xamarin.com/samples/monotouch/ios9/MetalPerformanceShadersHelloWorld/)
- [MusicMotion](https://developer.xamarin.com/samples/monotouch/ios9/MusicMotion/)
- [PhotoProgress](https://developer.xamarin.com/samples/monotouch/ios9/PhotoProgress/)
- [SegueCatalog](https://developer.xamarin.com/samples/monotouch/ios9/SegueCatalog/)
- [StackView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StackView)
- [StickyCorners](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StickyCorners)

또한 이러한 예제 (도우미 Mac OS X 버전 들어오는!)의 iOS 일부 확인해 보세요.

- [AgentsCatalog](https://github.com/xamarin/mac-ios-samples/tree/master/AgentsCatalog)
- [MetalKitEssentials](https://github.com/xamarin/mac-ios-samples/tree/master/MetalKitEssentials)



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://developer.xamarin.com/samples/ios/iOS9/)
- [3D 터치 소개](~/ios/platform/3d-touch.md)
- [앱의 전송 보안](~/ios/app-fundamentals/ats.md)
- [IPad 용 멀티태스킹](~/ios/platform/multitasking.md)
- [연락처 및 연락처 UI](~/ios/platform/contacts.md)
- [새 검색 Api](~/ios/platform/search/index.md)
- [스택 뷰 소개](~/ios/user-interface/controls/uistackview.md)
- [컬렉션 변경 내용 보기](~/ios/user-interface/controls/uicollectionview.md)
- [게임의 향상 된 기능](~/ios/platform/gaming/index.md)
- [HomeKit 소개](~/ios/platform/homekit.md)
- [핸드 오프 소개](~/ios/platform/handoff.md)
- [추가 iOS 9 Framework 변경 내용](~/ios/platform/introduction-to-ios9/additional-framework-changes.md)
- [문제 해결](~/ios/platform/introduction-to-ios9/troubleshooting.md)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [IOS 9.0에 새로 이란](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Xamarin.iOS 앱 iOS9 (비디오)로 업데이트](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
