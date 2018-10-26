---
title: IOS 9 소개
description: 이 문서에서는 Xamarin.iOS 개발자를 위한 모든 새로운 기능과 수정 된 Api 및 iOS 9에서에서 사용할 수 있는 기능을 소개합니다.
ms.prod: xamarin
ms.assetid: 4D71BBD9-B948-4B59-9AF5-F199C51CBEB3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 2f9cc72dcbe506d22c8a986bcf59ddaa6355f043
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103259"
---
# <a name="introduction-to-ios-9"></a>IOS 9 소개

_이 문서에서는 Xamarin.iOS 개발자를 위한 모든 새로운 기능과 수정 된 Api 및 iOS 9에서에서 사용할 수 있는 기능을 소개합니다._

![](images/ios9-sml.png "IOS 9 로고")

Apple ios 9 기존 기능에 대 한 많은 향상 된 Api 및 서비스에 대 한 몇 가지 새로운 추가 되었습니다.

## <a name="3d-touch"></a>3D 터치

IOS 9 iPhone 6s와 iPhone 6s 새 또한 3D 터치 하 여 iOS 앱에가 중 중요 한 제스처를 추가 합니다. 3D를 사용 하 여 터치 iPhone 앱 뿐만 아니라 사용자 장치의 화면에 접촉 되어 있는지를 확인할 수 있습니다 수 됩니다. 또한 사용자 적용은 얼마나 부족 감지 및 서로 다른 압력 수준으로 응답 합니다.

3D 터치 앱에 다음 기능을 제공 합니다.

- **압력 감지** -앱을 얼마나 철저 측정 이제 수 또는 light 사용자 정보는 화면 및 take 활용 접촉 되어 있습니다. 예를 들어, 그리기 앱 두꺼운 또는 사용자의 화면을 터치가 얼마나 철저 살 따라 줄을 만들 수 있습니다.
- **보기 및 팝** -앱 사용자가 현재 컨텍스트에 밖으로 이동 하지 않고도 해당 데이터와 상호 작용할 수 있도록 이제 수 있습니다. 화면에 하드 눌러 수 있습니다 *피킹* 들은 (예: 메시지 미리 보기)에서 관심 있는 항목입니다. 어렵습니다 눌러 수 있습니다 *팝* 항목입니다.
- **빠른 작업** -는 수 수 팝 접속 데스크톱 앱에서 해당 항목의 단추로 클릭할 때 상황에 맞는 메뉴와 같은 빠른의 작업의 생각 합니다. 빠른 작업을 사용 하 여 추가할 수 있습니다 일반 빠르고 쉽게 함수에 대 한 액세스 바로 가기에 앱의 iOS 장치의 홈 화면 아이콘에서.

자세한 내용을 참조 하세요 우리의 [3D 터치 소개](~/ios/platform/3d-touch.md) 가이드입니다.

## <a name="app-transport-security"></a>앱 전송 보안

새 앱 전송 보안 (ATS) 하 여 iOS 9, 인터넷 리소스 (예: 앱의 백 엔드 서버)와 앱 간에 보안 연결을 적용 합니다. 앱 또는 사용 하는 라이브러리를 통해 직접 중요 한 정보가 실수로 유출 방지 모든 인터넷 통신 보안 연결 모범 사례를 준수 하는지 ATS에서 확인 합니다.

ATS는 모든 연결을 사용 하 여 iOS 9 및 OS X 10.11 (El Capitan) 용으로 빌드된 앱에서 기본적으로 활성화 되어 있으므로 [NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/)를 [CFUrl](https://developer.xamarin.com/api/type/CoreFoundation.CFUrl/) 하거나 [NSUrlSession](https://developer.xamarin.com/api/type/Foundation.NSUrlSession/) 적용 됩니다 ATS 보안 요구 사항입니다. 연결에 이러한 요구 사항에 맞지 않는 경우 예외가 발생 하 여 실패 합니다.

ATS 대 한 자세한 정보를 확인 하려면 하세요 우리의 [앱 전송 보안](~/ios/app-fundamentals/ats.md) 가이드입니다.

<a name="multitasking" />

## <a name="multitasking-for-ipad"></a>IPad 용 멀티태스킹

IOS 9 사용 하 여 Apple에는 특정 iPad 하드웨어에서 동시에 두 개의 앱을 실행 하기 위한 멀티태스킹 지원이 추가 되었습니다. 결과적으로, Xamarin.iOS 앱 특정된 시점에 실행 중인 유일한 앱 되는지 또는 전체 화면 또는 장치 리소스에 액세스할 수 있는 더 이상 가정할 수 없습니다.

IPad 용 멀티태스킹 다음 기능을 통해 사용할 수 있습니다.

- **슬라이드 통해** -일시적으로 슬라이드 아웃 약 25%를 현재 실행 중인 기본 앱을 포함 하는 (언어 방향에 따라 화면 오른쪽 이나 왼쪽에 하나) 패널에에서는 두 번째 iOS 앱을 실행할 수 있습니다. 이동 되는 iPad Pro, iPad 항공, iPad 공기 2, iPad 미니 2, 미니 3, iPad 또는 iPad 미니 4 에서만 사용할 수 있습니다.
- **분할 보기** -지원 되는 iPad 하드웨어 (iPad 공기 2, iPad 미니 4 및 iPad Pro만 해당)에서 사용자는 두 번째 앱을 선택 하 고 실행할 수 side-by-side-분할 화면 모드에서 현재 실행 중인 앱을 사용 하 여 합니다. 사용자는 각 앱 차지 하는 주 화면 비율을 제어할 수 있습니다.
- **그림에는 그림** -비디오 콘텐츠가 재생, 비디오 수 이제 iOS 장치에서 현재 실행 중인 다른 앱을 통해 배치 되는 이동 가능한 및 크기 조정 가능한 창에서 재생할 수 있는 앱입니다. 사용자에는 크기와이 창의 위치를 통해 모든 권한을 가집니다. 그림에는 그림은 iPad Pro, iPad 항공, iPad 공기 2, iPad 미니 2, iPad 미니 3 또는 iPad 미니 4 에서만 사용할 수 있습니다.

IOS 9의 새로운 멀티태스킹 기능에 대 한 자세한 내용을 알아보려면, 하세요 우리의 [iPad 용 멀티태스킹](~/ios/platform/multitasking.md) 가이드입니다.

## <a name="new-contacts-and-contacts-ui-frameworks"></a>새 연락처 및 연락처 UI 프레임 워크

Apple iOS 9의 도입으로 두 가지 새 프레임 워크를 출시 했습니다 [연락처](https://developer.xamarin.com/api/namespace/Contacts/) 하 고 [ContactsUI](https://developer.xamarin.com/api/namespace/ContactsUI/)기존 주소록을 대체 하 고 주소 책 UI 프레임 워크에서 사용 하는 iOS 8 및 이전 버전입니다.

이러한 새, 개체 지향 프레임 워크 다음 정보를 제공 합니다.

- **연락처** – 사용자의 연락처 정보를 제공 하는 Xamarin.iOS 액세스 합니다. 대부분의 앱만 읽기 전용 액세스를 되어야 하므로이 프레임 워크는 스레드 안전 하 고 읽기 전용 액세스에 최적화 되었습니다.
- **ContactsUI** – Xamarin.iOS UI 제공 요소를 표시 하려면 편집 선택 하 고 iOS 장치에서 연락처를 만듭니다.

자세한 내용은 참조 하세요. 당사의 [연락처 및 연락처 UI](~/ios/platform/contacts.md) 설명서.


## <a name="new-search-apis"></a>새 검색 Api

검색에서 iOS 9 다양 한 방법으로 Xamarin.iOS 앱 내에서 정보에 액세스할 수 있도록 확장 되었습니다. 새 검색 Api를 사용 가능 응용 프로그램의 콘텐츠 추천 및 Safari 검색 결과 핸드 오프 및 Siri 미리 알림 및 제안을 통해 검색할 수 있는 합니다. 이렇게 하면 사용자 활동 및 앱 내에서 심층 정보에 빠르게 액세스할 수 있습니다.

또한 새 검색 Api를 쉽게 이전 검색 구현 환경 없이 앱에서 검색을 통합 합니다. 이 때문에 Apple는 일반적으로 몇 시간이 소요 iOS 9 앱의 콘텐츠를 앱 검색을 사용 하 여 전체적으로 검색할 수 있도록 클레임입니다.

자세한 내용은 참조 하십시오 우리의 [검색 기능 향상](~/ios/platform/search/index.md) 설명서.

## <a name="new-stack-view"></a>새 스택 보기

스택 뷰 컨트롤 ([UIStackView](https://developer.xamarin.com/api/type/UIKit.UIStackView/)) 활용 자동 레이아웃 및 크기 클래스 (가로 또는 세로로) 하위 뷰의 스택을 관리 하는 iOS 장치의 화면 크기와 방향에 동적으로 응답 합니다.

스택 보기 컨트롤을 사용 하면 작업량이 사용자 인터페이스를 크게 감소 하는 레이아웃에 필요 합니다. 스택 보기에 연결 하는 모든 하위 뷰의 레이아웃 축, 배포, 맞춤 간격 등 개발자 정의 속성에 따라 자동으로 관리 됩니다.

자세한 내용은 참조 하십시오 우리의 [스택 뷰 소개](~/ios/user-interface/controls/uistackview.md) 설명서.


## <a name="collection-view-changes"></a>변경 내용 컬렉션 보기

Ios 9 컬렉션 뷰 ([UICollectionView](https://developer.xamarin.com/api/type/UIKit.UICollectionView/)) 지 원하는 새 기본 제스처 인식기 및 몇 가지 새로운 지원 메서드를 추가 하 여 즉시 항목 다시 정렬 끕니다.

이러한 새 메서드를 사용 하 쉽게 끌어서 다시 정렬 컬렉션 보기에 구현 하 고 수 다시 정렬 하는 프로세스의 모든 단계 동안 항목 모양을 사용자 지정할 수 있습니다.

IOS 9에 대 한 변경 내용 컬렉션 보기에 대 한 자세한 내용을 알아보려면를 참조 하십시오 우리의 [변경 내용 컬렉션 보기](~/ios/user-interface/controls/uicollectionview.md) 가이드입니다.

## <a name="game-enhancements"></a>게임 향상 된 기능

IOS 9 사용 하 여 Apple 쉽게 Xamarin.iOS 앱에서 오디오 및 게임 그래픽을 구현 하는 게임 Api에 몇 가지 기술적 개선을 했습니다. 여기에 고급 프레임 워크 및 향상 된 속도 및 하위 수준 기능이 향상 된 그래픽 기능에 대 한 iOS 장치의 GPU의 강력한 기능 활용을 통해 개발의 용이성 모두 포함 됩니다.

SpriteKit 체제 미 설치 컴퓨터, SceneKit을 더욱 향상 기능과 함께 GameplayKit, ReplayKit, 모델 I/O, MetalKit 및 체제 미 설치 컴퓨터 성능 셰이더 포함 됩니다.

자세한 내용은 참조 하십시오 우리의 [게임 향상](~/ios/platform/gaming/index.md) 설명서.

## <a name="homekit-framework-changes"></a>HomeKit 프레임 워크 변경

합니다 [HomeKit](https://developer.xamarin.com/api/namespace/HomeKit/) iOS 8에에서 도입 된 프레임 워크를 설정 하 고 Xamarin.iOS 앱에서 (예: 자동화 된 광원, 도어 잠금 및 차고문 개폐) 다양 한 사용 HomeKit 액세서리를 제어 하는 기능을 제공 합니다. 쉽게 설치 및 구성할 수 있을 뿐만 HomeKit 액세서리 음성된 Siri 명령을 통해 제어할 수 있습니다.

Ios 9에서 Apple 설치 프로그램을 쉽게에 지원 하 고 (예: iCloud 통해 원격으로 액세서리 제어) 자세한 액세서리 상호 작용을 제공 하는 보조 프로그램 유형을 확장 합니다.

자세한 내용은 참조 하세요. 당사의 [HomeKit 소개](~/ios/platform/homekit.md)를 [HomeKitIntro iOS 샘플 앱](https://developer.xamarin.com/samples/monotouch/HomeKit/HomeKitIntro/) 과 Apple [HomeKit](https://developer.apple.com/homekit/) 설명서.

## <a name="handoff-framework-changes"></a>핸드 오프 Framework 변경 내용

핸드 오프 (라고도: 연속성) 장치 (iOS 또는 Mac) 중 하나에 활동을 시작 하 고 다른 장치 (사용자의 iClou에서 식별 한 대로 동일한 작업을 계속 하려면 사용자에 대 한 방법으로 Apple ios 8 및 OS X Yosemite(10.10 ()에 의해 정의 된 d 계정)입니다.

핸드 오프 검색 기능 향상 된 새로운 기능도 9 iOS에서 확장 되었습니다. 자세한 내용은 참조 하십시오 우리의 [검색 기능 향상](~/ios/platform/search/index.md) 설명서. 핸드 오프를 사용 하 여에 대 한 자세한 내용은 참조 하십시오 우리의 [핸드 오프 소개](~/ios/platform/handoff.md) 설명서.

## <a name="new-extension-points"></a>새 확장점

IOS 8, Apple 확장을 도입 했습니다.-표시 되는 표준 컨텍스트에서 운영 체제에서와 같이 알림 센터 내에서 때나 사진을 편집할 경우 사용자가 키보드를 요청할 때 라이브러리입니다.

IOS 9 사용 하 여 Apple 함으로써 여러 새 확장 지원을 확장 _확장점_ 사용 정책을 정의 하 고 지정된 된 영역 내에서 다음과 같은 작업에 대 한 Api를 제공 합니다.

- **새 오디오 장치 확장 지점을** –이 확장 지점을 사용 하 여 (예: GarageBand) 앱을 다른 오디오 장치 호스트 내에서 사용할 오디오 효과, 악기, 사운드 생성기 등을 제공 합니다. 이 확장 지점을 또한을 판매할 수 있도록 _오디오 단위_ (오디오 플러그 인) App Store에서.
- **새 인덱스 유지 관리 확장 지점을** -이 확장 지점을 사용 하 여 앱 데이터를 앱 다시 하지 않고도 다시 인덱싱 지원 합니다.
- **새 네트워크 확장점** (이러한 Apple에서 특별 한 권한이 필요).
    - **앱 프록시 공급자 확장** -이 확장 지점을 사용 하 여 투명 한 클라이언트 쪽 네트워크 사용자 지정 프록시를 구현 합니다.
    - **데이터 공급자를 필터링 / 필터 제어 공급자 확장** -이러한 확장 점을 사용 하 여 장치를 필터링 하는 동적 네트워크 콘텐츠를 구현 합니다.
    - **패킷 터널 공급자 확장** -이 확장 지점을 사용 하 여 사용자 지정 VPN 터널링 프로토콜 클라이언트 쪽을 구현 합니다.
- **새 Safari 확장점**:
    - **콘텐츠 차단 확장** -이 확장 지점을 사용 하 여 사용자가 웹을 검색할 때 표시 되지 것입니다는 차단 된 콘텐츠의 목록을 정의 합니다.
    - **공유 링크 확장** -Safari의 공유 링크에서 앱의 콘텐츠를 볼 수 있도록이 확장 지점을 사용 합니다.

자세한 내용은 참조 하십시오 우리의 [확장 프로그램 소개](~/ios/platform/extensions.md) 과 Apple [앱 확장의 프로그래밍 가이드](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214) 설명서.

## <a name="keychain-enhancements"></a>키 집합 향상 된 기능

IOS 9 Apple이 같이 보안 Enclave와 더 많은 항목 보호 옵션에 대 한 새 암호화 키 유형을 제공 키 집합을 향상 합니다.

- 새 Touch ID 하는 제약 조건 지문 데이터베이스를 수정 하는 경우 키 집합 항목을 무효화 합니다.
- Touch ID 또는 암호를 사용 하 여 Access Control 목록 항목 만들기만 허용 하는 새 제약 조건입니다.
- 별도 인증을 호출할 수 있는 새 인증 컨텍스트 `SecItem` 호출 합니다.
- 앱에서 제공한 키 집합 항목 암호화에 대 한 제어 목록 엔트로피 (응용 프로그램 암호 옵션 사용)에 액세스 합니다.
- 생성 하 고 보안 enclave 내에서 키를 사용 하 여에 대 한 지원 (통해는 `kSecAttrTokenIDSecureEnclave` 특성).

자세한 내용은 참조 하십시오 우리의 [Touch ID 소개](~/ios/platform/touchid.md) 설명서.


## <a name="right-to-left-language-support"></a>오른쪽에서 왼쪽 언어 지원

Ios 9에서 Apple 했습니다 하는 데 대칭 이동 된 사용자 인터페이스를 보다 쉽게 적이 오른쪽에서 왼쪽 언어에 대 한 전체 지원을 제공 합니다. 여기에는 다음과 같은 사항이 포함됩니다.

- 표준 [UIKit](https://developer.xamarin.com/api/namespace/UIKit/) 컨트롤 오른쪽에서 왼쪽 iOS 장치 로캘 및 언어 설정에 따라 자동으로 대칭 이동 됩니다.
- 합니다 [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/) 클래스는 오른쪽에서 왼쪽을 대칭 이동 된 경우 지정된 된 보기 표시 되어야 하는 방법을 정의할 수 있도록 하는 특성을 제공 합니다.
- 기능을 사용 하 여 프로그래밍 방식으로 이미지를 대칭 합니다 [FlipsForRightToLeftLayoutDirection](https://developer.xamarin.com/api/property/UIKit.UIImage.FlipsForRightToLeftLayoutDirection/) 의 속성을 [UIImage](https://developer.xamarin.com/api/type/UIKit.UIImage/) 클래스.

자세한 내용은 Apple의를 참조 하세요 [지원 오른쪽에서 왼쪽 언어](https://developer.apple.com/library/prerelease/ios/documentation/MacOSX/Conceptual/BPInternational/SupportingRight-To-LeftLanguages/SupportingRight-To-LeftLanguages.html#//apple_ref/doc/uid/10000171i-CH17) 설명서.



## <a name="additional-framework-changes"></a>프레임 워크 추가 변경 내용

위의 살펴 봤는 주요 변경 내용 외에도 Apple ios 9 다음과 같은 몇 가지 기존 프레임 워크의 향상 된 기능 및 수정 했습니다.

- AV Foundation 프레임 워크
- AVKit 프레임 워크
- CloudKit 프레임 워크
- Foundation 프레임 워크
- 핸드 오프 프레임 워크
- HealthKit 프레임 워크
- HomeKit 프레임 워크
- 로컬 인증 프레임 워크
- MapKit 프레임 워크
- PassKit Framework
- Safari 서비스 프레임 워크
- UIKit 프레임 워크

자세한 내용은 참조 하십시오 우리의 [추가 iOS 9 프레임 워크 변경 내용](~/ios/platform/introduction-to-ios9/additional-framework-changes.md) 설명서.

## <a name="deprecated-apis-and-functions"></a>사용 되지 않는 Api 및 함수

다음 Api와 iOS 9의에서 함수에는 Apple에 사용 되지 않음:

- **도 서 및 북 UI 주소 주소** -이러한 Api는 연락처 및 연락처 UI 프레임 워크에 의해 대체 되었습니다. 자세한 내용은 참조 하세요. 당사의 [연락처 및 연락처 UI](~/ios/platform/contacts.md) 설명서.
- **CBCentralManager** - `RetrievePeripherals` 하 고 `RetrieveConnectedPeripherals` 의 메서드는 `CBCentralManager` 클래스 iOS 9에서에서 제거 되었습니다. 이러한 메서드를 호출 하면 보조 프로그램을 연결 하는 경우 충돌 또는 앱 시작 시 앱.
- **FetchAllChanges** - `FetchAllChanges` 의 `CKFetchRecordChangesOperation` 클래스 상각액이 되었습니다 및 iOS 9에서에서 제거 됩니다.
- **Media Player** -iOS 9에서에서의 미디어 플레이어 프레임 워크를 더 이상 사용 되지 되었습니다. AVKit 또는 AV Foundation Api를 대신 사용 합니다.

특정 API 결함의 전체 목록은, Apple의을 참조 하세요 [iOS 9.0 API 차이](https://developer.apple.com/library/prerelease/ios/releasenotes/General/iOS90APIDiffs/index.html#//apple_ref/doc/uid/TP40016222) 설명서.

## <a name="ios-9-sample-apps"></a>iOS 9 샘플 앱

고객 들 [iOS 9 관련 샘플](https://developer.xamarin.com/samples/ios/iOS9/) 시작 하려면:

- [AstroLayout](https://github.com/xamarin/monotouch-samples/tree/master/ios9/AstroLayout)
- [CollectionView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/CollectionView)
- [MetalPerformanceShadersHelloWorld](https://developer.xamarin.com/samples/monotouch/ios9/MetalPerformanceShadersHelloWorld/)
- [MusicMotion](https://developer.xamarin.com/samples/monotouch/ios9/MusicMotion/)
- [PhotoProgress](https://developer.xamarin.com/samples/monotouch/ios9/PhotoProgress/)
- [SegueCatalog](https://developer.xamarin.com/samples/monotouch/ios9/SegueCatalog/)
- [StackView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StackView)
- [StickyCorners](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StickyCorners)

또한 iOS 부분 (도우미 Mac OS X 버전 출시!) 이러한 샘플을 확인해 보세요.

- [AgentsCatalog](https://github.com/xamarin/mac-ios-samples/tree/master/AgentsCatalog)
- [MetalKitEssentials](https://github.com/xamarin/mac-ios-samples/tree/master/MetalKitEssentials)



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://developer.xamarin.com/samples/ios/iOS9/)
- [3D 터치 소개](~/ios/platform/3d-touch.md)
- [앱 전송 보안](~/ios/app-fundamentals/ats.md)
- [iPad용 멀티태스킹](~/ios/platform/multitasking.md)
- [연락처 및 연락처 UI](~/ios/platform/contacts.md)
- [새 검색 Api](~/ios/platform/search/index.md)
- [스택 뷰 소개](~/ios/user-interface/controls/uistackview.md)
- [변경 내용 컬렉션 보기](~/ios/user-interface/controls/uicollectionview.md)
- [게임 향상 된 기능](~/ios/platform/gaming/index.md)
- [HomeKit 소개](~/ios/platform/homekit.md)
- [핸드 오프 소개](~/ios/platform/handoff.md)
- [추가 iOS 9 프레임워크 추가 변경 내용](~/ios/platform/introduction-to-ios9/additional-framework-changes.md)
- [문제 해결](~/ios/platform/introduction-to-ios9/troubleshooting.md)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [새로운 iOS 9.0에서](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [IOS9 (비디오)를 Xamarin.iOS 앱을 업데이트 하는 중입니다.](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
