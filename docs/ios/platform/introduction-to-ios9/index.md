---
title: iOS 9 소개
description: 이 문서에서는 Xamarin.ios 개발자를 위한 iOS 9에서 제공 되는 새로운 Api 및 수정 된 Api 및 기능을 모두 소개 합니다.
ms.prod: xamarin
ms.assetid: 4D71BBD9-B948-4B59-9AF5-F199C51CBEB3
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/19/2017
ms.openlocfilehash: c162912d6762ac1ee9d2896f96bbb35e9fef06f4
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70285203"
---
# <a name="introduction-to-ios-9"></a>iOS 9 소개

_이 문서에서는 Xamarin.ios 개발자를 위한 iOS 9에서 제공 되는 새로운 Api 및 수정 된 Api 및 기능을 모두 소개 합니다._

![](images/ios9-sml.png "IOS 9 로고")

Apple은 기존 기능에 대 한 여러 향상 된 기능을 비롯 하 여 iOS 9에서 몇 가지 새로운 Api 및 서비스를 추가 했습니다.

## <a name="3d-touch"></a>3D 터치

IOS 9 및 iPhone 6s와 iPhone 6s Plus를 처음 접하는 3D Touch는 iOS 앱에 압력 민감한 제스처를 추가 합니다. 3D Touch를 사용 하는 경우 iPhone 앱은 사용자가 장치 화면에 접촉 하 고 있음을 알 수 있을 뿐만 아니라 사용자가 얼마나 많은 압력을 있어 서버의 하 고 다른 압력 수준에 반응 하 고 있음을 알 수 있습니다.

3D Touch는 앱에 다음과 같은 기능을 제공 합니다.

- **압력 민감도** -이제 앱은 사용자가 화면을 터치 하는 속도를 측정 하 고 해당 정보를 활용할 수 있습니다. 예를 들어 그리기 앱은 사용자가 화면에 접촉 하는 정도에 따라 선을 더 두껍게 하거나 더 가늘게 만들 수 있습니다.
- 이제 응용 프로그램을 **피킹 (peeking** ) 하 여 현재 컨텍스트에서 이동 하지 않고도 사용자가 해당 데이터와 상호 작용할 수 있게 할 수 있습니다. 화면에서 하드 키를 누르면 관심 있는 항목을 볼 수 있습니다 (예: 메시지 *미리 보기)* . 더 어렵게 누르면 *항목을 볼 수 있습니다* .
- **빠른 작업** -사용자가 데스크톱 앱에서 항목을 마우스 오른쪽 단추로 클릭할 때 팝업 될 수 있는 상황에 맞는 메뉴와 같은 빠른 작업을 생각 합니다. 빠른 작업을 사용 하 여 iOS 장치의 홈 화면 아이콘에서 앱의 함수에 대 한 쉽고 빠르게 액세스 하는 바로 가기를 추가할 수 있습니다.

자세히 알아보려면 [3D 터치 가이드 소개](~/ios/platform/3d-touch.md) 를 참조 하세요.

## <a name="app-transport-security"></a>앱 전송 보안

IOS 9를 처음 접하는 ATS (App Transport Security)는 인터넷 리소스 (예: 앱의 백 엔드 서버)와 앱 간 보안 연결을 적용 합니다. ATS를 사용 하면 모든 인터넷 통신이 안전한 연결 모범 사례를 준수 하 여 앱 또는 사용 중인 라이브러리를 통해 직접 중요 한 정보가 노출 되지 않도록 방지할 수 있습니다.

ATS는 iOS 9 및 OS X 10.11 (El Capitan) 용으로 빌드된 앱에서 기본적으로 사용 하도록 설정 되어 있으므로 [NSUrlConnection](xref:Foundation.NSUrlConnection), [Cfurl](xref:CoreFoundation.CFUrl) 또는 [NSUrlSession](xref:Foundation.NSUrlSession) 를 사용 하는 모든 연결에는 ATS 보안 요구 사항이 적용 됩니다. 연결이 이러한 요구 사항을 충족 하지 않는 경우에는 예외와 함께 실패 합니다.

ATS에 대해 자세히 알아보려면 [앱 전송 보안](~/ios/app-fundamentals/ats.md) 가이드를 참조 하세요.

<a name="multitasking" />

## <a name="multitasking-for-ipad"></a>iPad용 멀티태스킹

IOS 9를 사용 하는 경우 Apple은 특정 iPad 하드웨어에서 두 개의 앱을 동시에 실행 하는 멀티태스킹 지원을 추가 했습니다. 따라서 Xamarin.ios 앱은 지정 된 시간에 실행 되는 유일한 앱 이라고 가정할 수 없으며 장치의 전체 화면이 나 리소스에 액세스할 수 있습니다.

IPad 용 멀티태스킹은 다음 기능을 통해 지원 됩니다.

- **밀기** -사용자는 현재 실행 중인 주 앱의 약 25%에 해당 하는 슬라이드 아웃 패널 (언어 방향에 따라 화면의 오른쪽 또는 왼쪽)에서 두 번째 iOS 앱을 일시적으로 실행할 수 있습니다. 밀기는 iPad Pro, iPad Air, iPad Air 2, iPad 미니 2, iPad 미니 3 또는 iPad 미니 4 에서만 사용할 수 있습니다.
- **분할 보기** -지원 되는 iPad 하드웨어 (ipad Air 2, ipad 미니 4 및 iPad Pro만 해당)에서 사용자는 두 번째 앱을 선택 하 고 분할 화면 모드에서 현재 실행 중인 앱과 나란히 실행할 수 있습니다. 사용자는 각 앱이 차지 하는 주 화면의 백분율을 제어할 수 있습니다.
- **그림의 그림** -비디오 콘텐츠를 재생 하는 앱의 경우 이제 iOS 장치에서 현재 실행 되 고 있는 다른 앱 위에 배치 되는 크기 조정 가능한 창에서 비디오를 재생할 수 있습니다. 사용자는이 창의 크기와 위치에 대 한 모든 권한을 가집니다. 그림의 그림은 iPad Pro, iPad Air, iPad Air 2, iPad 미니 2, iPad 미니 3 또는 iPad 미니 4 에서만 사용할 수 있습니다.

IOS 9의 새로운 멀티태스킹 기능에 대해 자세히 알아보려면 [iPad 용 멀티태스킹](~/ios/platform/multitasking.md) 가이드를 참조 하세요.

## <a name="new-contacts-and-contacts-ui-frameworks"></a>새 연락처 및 연락처 UI 프레임 워크

IOS 9가 도입 됨에 따라 Apple은 iOS 8 및 이전 버전에서 사용 하는 기존 주소록 및 주소록 UI 프레임 워크를 대체 하는 두 개의 새로운 프레임 워크, [연락처 및 동료](xref:Contacts) [sui](xref:ContactsUI)를 출시 했습니다.

이러한 새로운 개체 지향 프레임 워크는 다음과 같은 기능을 제공 합니다.

- **연락처** – 사용자의 연락처 정보에 대 한 xamarin.ios 액세스를 제공 합니다. 대부분의 앱은 읽기 전용 액세스만 필요 하므로이 프레임 워크는 스레드로부터 안전한 읽기 전용 액세스를 위해 최적화 되었습니다.
- 지 **수-ios** 장치에서 연락처를 표시, 편집, 선택 및 만들기 위한 xamarin.ios UI 요소를 제공 합니다.

자세한 내용은 [연락처 및 연락처 UI](~/ios/platform/contacts.md) 설명서를 참조 하세요.


## <a name="new-search-apis"></a>새 검색 Api

검색은 Xamarin.ios 앱 내의 정보에 액세스 하는 뛰어난 새 방법을 제공 하기 위해 iOS 9에서 확장 되었습니다. 새 검색 Api를 사용 하 여 스포트라이트 및 Safari 검색 결과, 전달 및 Siri 미리 알림 및 제안을 통해 앱의 콘텐츠를 검색 가능 하도록 만들 수 있습니다. 그러면 사용자가 앱 내에서 작업 및 정보에 빠르게 액세스할 수 있습니다.

또한 새 검색 Api를 사용 하면 사전 검색 구현 환경을 사용 하지 않고도 앱에서 검색을 보다 쉽게 통합할 수 있습니다. 따라서 Apple은 일반적으로 앱 검색을 사용 하 여 iOS 9 앱 콘텐츠를 검색 하는 데 몇 시간 정도 걸립니다.

자세한 내용은 [향상 된 검색 기능](~/ios/platform/search/index.md) 설명서를 참조 하세요.

## <a name="new-stack-view"></a>새 스택 뷰

[Uistackview](xref:UIKit.UIStackView) 는 자동 레이아웃 및 크기 클래스의 강력한 기능을 활용 하 여 iOS 장치의 방향 및 화면 크기에 동적으로 응답 하는 하위 뷰 (가로 또는 세로) 스택을 관리 합니다.

스택 뷰 컨트롤을 사용 하면 사용자 인터페이스를 레이아웃 하는 데 필요한 작업 양이 크게 줄어듭니다. 스택 보기에 연결 된 모든 하위 뷰의 레이아웃은 축, 분포, 맞춤 및 간격과 같은 개발자 정의 속성에 따라 자동으로 관리 됩니다.

자세한 내용은 [Stack 뷰 소개](~/ios/user-interface/controls/uistackview.md) 설명서를 참조 하세요.


## <a name="collection-view-changes"></a>컬렉션 뷰 변경 내용

IOS 9에서 컬렉션 뷰 (이제[UICollectionView](xref:UIKit.UICollectionView) 는 새 기본 제스처 인식자와 몇 가지 새로운 지원 메서드를 추가 하 여 항목을 즉시 다시 정렬 하도록 지원 합니다.

이러한 새 메서드를 사용 하 여 컬렉션 뷰에서 순서를 변경 하는 작업을 쉽게 구현할 수 있으며, 다시 정렬 프로세스 단계 중에 항목 모양을 사용자 지정 하는 옵션을 사용할 수 있습니다.

IOS 9의 컬렉션 뷰 변경 내용에 대 한 자세한 내용은 [컬렉션 뷰 변경 내용](~/ios/user-interface/controls/uicollectionview.md) 가이드를 참조 하세요.

## <a name="game-enhancements"></a>향상 된 게임

IOS 9를 사용 하 여 Apple은 Xamarin.ios 앱에서 게임 그래픽 및 오디오를 보다 쉽게 구현할 수 있도록 하는 게임 Api에 대 한 몇 가지 기술 개선을 만들었습니다. 여기에는 고급 프레임 워크를 통한 손쉬운 개발이 모두 포함 되며, 낮은 수준의 고급 기능을 통해 향상 된 속도 및 그래픽 기능을 제공 하기 위해 iOS 장치 GPU의 기능을 활용 합니다.

여기에는 GameplayKit, ReplayKit, Model i/o, MetalKit 및 메탈 Performance 셰이더가 포함 되며, 이러한 기능에는 금속, SceneKit 및 SpriteKit의 새롭고 향상 된 기능이 포함 됩니다.

자세한 내용은 [게임의 향상 된 기능](~/ios/platform/gaming/index.md) 설명서를 참조 하세요.

## <a name="homekit-framework-changes"></a>HomeKit 프레임 워크 변경

IOS 8에 도입 된 [HomeKit](xref:HomeKit) 프레임 워크는 xamarin.ios 앱에서 다양 한 HomeKit 사용 액세서리 (예: 자동화 된 조명, 도어 잠금 및 중고품 도어 openers)를 설정 하 고 제어 하는 기능을 제공 합니다. HomeKit 액세서리는 설정 하 고 구성 하는 것 외에도 음성 Siri 명령을 통해 제어할 수 있습니다.

IOS 9에서 Apple은 더 쉽게 설정 하 고, 지원 되는 액세서리 유형을 확장 하 고, 더 많은 액세서리 상호 작용 (예: iCloud를 통해 원격으로 액세서리 제어)을 제공 했습니다.

자세한 내용은 HomeKit, [HomeKitIntro IOS 샘플 앱](https://docs.microsoft.com/samples/xamarin/ios-samples/homekit-homekitintro) 및 Apple의 [HomeKit](https://developer.apple.com/homekit/) 설명서 [소개](~/ios/platform/homekit.md)를 참조 하세요.

## <a name="handoff-framework-changes"></a>핸드 프레임 워크 변경

사용자가 장치 (iOS 또는 Mac) 중 하나에서 활동을 시작 하 고 사용자의 iClou로 식별 된 다른 장치에서 동일한 활동을 계속 진행 하는 방식으로 iOS 8 및 OS X Yosemite (10.10)에서 Apple에 의해 전달 (연속성이 라고도 함)이 도입 되었습니다. d 계정).

핸드 오프는 새로운 향상 된 검색 기능을 지원 하기 위해 iOS 9에서 확장 되었습니다. 자세한 내용은 [향상 된 검색 기능](~/ios/platform/search/index.md) 설명서를 참조 하세요. 핸드 오프를 사용 하는 방법에 대 한 자세한 내용은 전달 설명서 [소개](~/ios/platform/handoff.md) 를 참조 하세요.

## <a name="new-extension-points"></a>새 확장점

IOS 8에서 Apple은 확장을 도입 했습니다. 예를 들어 알림 센터 내에서, 사용자가 키보드를 요청 하거나 사진을 편집 하는 경우와 같은 표준 컨텍스트의 운영 체제에서 제공 하는 라이브러리입니다.

IOS 9에서 Apple은 다음과 같이 사용 정책을 정의 하 고 지정 된 영역 내에서 작업 하기 위한 Api를 제공 하는 몇 가지 새로운 _확장 요소_ 를 제공 하 여 확장 지원을 확장 합니다.

- **새 오디오 장치 확장 지점** -이 확장 지점을 사용 하 여 오디오 효과, 악기, 사운드 생성기 등을 다른 오디오 단위 호스트 앱 (예: GarageBand) 내에서 사용할 수 있도록 제공 합니다. 이 확장 지점을 사용 하 여 앱 스토어에서 _오디오 장치_ (오디오 플러그 인)를 판매할 수도 있습니다.
- **새 인덱스 유지 관리 확장 지점** -이 확장 지점을 사용 하 여 앱을 다시 실행할 필요 없이 앱 데이터의 다시 인덱스를 지원할 수 있습니다.
- **새 네트워크 확장 위치** (Apple의 특별 한 권한이 필요 함):
  - **앱 프록시 공급자 확장** -이 확장 지점을 사용 하 여 사용자 지정 투명 클라이언트 쪽 네트워크 프록시를 구현 합니다.
  - **필터 Data Provider/필터 컨트롤 공급자 확장** -이러한 확장 요소를 사용 하 여 장치에서 동적 네트워크 콘텐츠 필터링을 구현 합니다.
  - **패킷 터널 공급자 확장** -이 확장 지점을 사용 하 여 사용자 지정 VPN 터널링 프로토콜 클라이언트 쪽을 구현 합니다.
- **새 Safari 확장 지점은**다음과 같습니다.
  - **콘텐츠 차단 확장** -이 확장 지점을 사용 하 여 사용자가 웹을 찾아볼 때 표시 되지 않을 차단 된 콘텐츠의 목록을 정의 합니다.
  - **공유 링크 확장** -이 확장 지점을 사용 하 여 Safari의 공유 링크에서 앱 콘텐츠를 볼 수 있습니다.

자세한 내용은 [Extensions 소개](~/ios/platform/extensions.md) 및 Apple의 [앱 확장 프로그래밍 가이드](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214) 문서를 참조 하세요.

## <a name="keychain-enhancements"></a>키 집합 기능 향상

IOS 9에서 Apple은 다음과 같이 Secure Enclave 및 기타 항목 보호 옵션에 대 한 새 암호화 키 유형을 제공 하도록 키 집합을 향상 시켰습니다.

- 지문 데이터베이스가 수정 될 때 키 집합 항목을 무효화 하는 새 Touch ID 제약 조건입니다.
- Touch ID 또는 암호를 사용 하 여 Access Control 목록 항목을 만들 수 있도록 하는 새 제약 조건입니다.
- `SecItem` 호출과 별도로 인증을 호출할 수 있도록 하는 새 인증 컨텍스트입니다.
- 앱에서 제공 하는 키 집합 항목 암호화에 대해 엔트로피 (응용 프로그램 암호 옵션 사용)를 Access Control 합니다.
- 특성을 `kSecAttrTokenIDSecureEnclave` 통해 secure enclave 내에서 키를 생성 하 고 사용할 수 있도록 지원 합니다.

자세한 내용은 [TOUCH ID 소개 설명서를](~/ios/platform/touchid.md) 참조 하세요.


## <a name="right-to-left-language-support"></a>오른쪽에서 왼쪽으로 쓰기 언어 지원

IOS 9에서 Apple은 오른쪽에서 왼쪽으로 진행 되는 언어에 대 한 완전 한 지원을 제공 하 여 이전 보다 더 쉽게 대칭 이동 된 사용자 인터페이스를 제공 했습니다. 여기에는 다음이 포함됩니다.

- 표준 [Uikit](xref:UIKit) 컨트롤은 iOS 장치 로캘 및 언어 설정에 따라 오른쪽에서 왼쪽으로 자동 전환 됩니다.
- [Uiview](xref:UIKit.UIView) 클래스는 오른쪽에서 왼쪽으로 대칭 이동 하는 경우 지정 된 뷰가 표시 되는 방법을 정의할 수 있는 특성을 제공 합니다.
- [Uiimage](xref:UIKit.UIImage) 클래스의 [FlipsForRightToLeftLayoutDirection](xref:UIKit.UIImage.FlipsForRightToLeftLayoutDirection) 속성을 사용 하 여 프로그래밍 방식으로 이미지를 대칭 이동 하는 기능입니다.

자세한 내용은 Apple의 [지원 오른쪽에서 왼쪽 언어](https://developer.apple.com/library/prerelease/ios/documentation/MacOSX/Conceptual/BPInternational/SupportingRight-To-LeftLanguages/SupportingRight-To-LeftLanguages.html#//apple_ref/doc/uid/10000171i-CH17) 설명서를 참조 하세요.



## <a name="additional-framework-changes"></a>추가 프레임 워크 변경

위에서 설명한 주요 변경 사항 외에도 Apple에서는 다음을 포함 하 여 iOS 9에 대 한 몇 가지 기존 프레임 워크를 수정 및 개선 했습니다.

- AV 기반 프레임 워크
- AVKit 프레임 워크
- CloudKit Framework
- Foundation Framework
- 핸드 오프 프레임 워크
- HealthKit 프레임 워크
- HomeKit 프레임 워크
- 로컬 인증 프레임 워크
- MapKit 프레임 워크
- PassKit 프레임 워크
- Safari 서비스 프레임 워크
- UIKit Framework

자세한 내용은 [추가 iOS 9 프레임 워크 변경](~/ios/platform/introduction-to-ios9/additional-framework-changes.md) 설명서를 참조 하세요.

## <a name="deprecated-apis-and-functions"></a>사용 되지 않는 Api 및 함수

Apple은 iOS 9에서 다음 Api 및 함수를 사용 하지 않습니다.

- 주소록 **& 주소록 ui** -이러한 Api는 연락처 및 연락처 UI 프레임 워크로 대체 되었습니다. 자세한 내용은 [연락처 및 연락처 UI](~/ios/platform/contacts.md) 설명서를 참조 하세요.
- **CBCentralManager** - `RetrievePeripherals` 클래스`CBCentralManager` 의 `RetrieveConnectedPeripherals` 및 메서드는 iOS 9에서 제거 되었습니다. 이러한 메서드를 호출 하면 액세서리를 페어링 하거나 앱을 시작할 때 앱이 충돌 합니다.
- **FetchAllChanges** - `CKFetchRecordChangesOperation` 클래스 `FetchAllChanges` 의는 사용 되었으며 iOS 9에서 제거 될 예정입니다.
- **Media Player** -Media Player Framework는 iOS 9에서 더 이상 사용 되지 않습니다. 대신 AVKit 또는 AV 기반 Api를 사용 합니다.

특정 API 결함의 전체 목록은 Apple의 [iOS 9.0 API 차이](https://developer.apple.com/library/prerelease/ios/releasenotes/General/iOS90APIDiffs/index.html#//apple_ref/doc/uid/TP40016222) 설명서를 참조 하세요.

## <a name="ios-9-sample-apps"></a>iOS 9 샘플 앱

시작 하기 위한 몇 가지 [iOS 9 관련 샘플이](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9) 있습니다.

- [AstroLayout](https://github.com/xamarin/monotouch-samples/tree/master/ios9/AstroLayout)
- [CollectionView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/CollectionView)
- [MetalPerformanceShadersHelloWorld](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-metalperformanceshadershelloworld)
- [MusicMotion](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-musicmotion)
- [PhotoProgress](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-photoprogress)
- [SegueCatalog](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-seguecatalog)
- [StackView](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StackView)
- [StickyCorners](https://github.com/xamarin/monotouch-samples/tree/master/ios9/StickyCorners)

또한 이러한 샘플의 iOS 부분을 확인 하세요 (부록 Mac OS X 버전).

- [AgentsCatalog](https://github.com/xamarin/mac-ios-samples/tree/master/AgentsCatalog)
- [MetalKitEssentials](https://github.com/xamarin/mac-ios-samples/tree/master/MetalKitEssentials)



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [3D 터치 소개](~/ios/platform/3d-touch.md)
- [앱 전송 보안](~/ios/app-fundamentals/ats.md)
- [iPad용 멀티태스킹](~/ios/platform/multitasking.md)
- [연락처 및 연락처 UI](~/ios/platform/contacts.md)
- [새 검색 Api](~/ios/platform/search/index.md)
- [스택 뷰 소개](~/ios/user-interface/controls/uistackview.md)
- [컬렉션 뷰 변경 내용](~/ios/user-interface/controls/uicollectionview.md)
- [게임 향상](~/ios/platform/gaming/index.md)
- [HomeKit 소개](~/ios/platform/homekit.md)
- [핸드 오프 소개](~/ios/platform/handoff.md)
- [추가 iOS 9 프레임워크 추가 변경 내용](~/ios/platform/introduction-to-ios9/additional-framework-changes.md)
- [문제 해결](~/ios/platform/introduction-to-ios9/troubleshooting.md)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [IOS 9.0의 새로운 기능](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [IOS9에 Xamarin.ios 앱 업데이트 (비디오)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
