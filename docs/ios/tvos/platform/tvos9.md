---
title: tvOS 9 소개
description: 이 문서에서는 tvOS 개발자를 위해 tvOS 9에서 제공 되는 새로운 Api 및 수정 된 Api 및 기능을 모두 소개 합니다.
ms.prod: xamarin
ms.assetid: A7E738E1-9F94-489B-918F-7DF8F0810987
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/07/2016
ms.openlocfilehash: 2da3e919ec792297f26670c43275bb0c54040835
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290716"
---
# <a name="introduction-to-tvos-9"></a>tvOS 9 소개

_이 문서에서는 tvOS 개발자를 위해 tvOS 9에서 제공 되는 새로운 Api 및 수정 된 Api 및 기능을 모두 소개 합니다._

Apple은 새로 디자인 된 터치 가능 원격 기능을 제공 하는 4 세대 Apple TV 하드웨어를 출시 했습니다 (iOS 9 기반).

처음으로 tvOS는 개발자에 게 Apple TV 플랫폼을 열어 iTunes 앱을 사용 하 여 iOS 용 앱을 작성 하 고 해제 하는 환경과 비슷한 프로세스에서 다양 한 몰입 형 앱을 만들고 Apple TV의 기본 제공 앱 스토어를 통해 릴리스 하는 것을 허용 합니다. 보관.

Xamarin.ios 개발에 익숙한 경우 tvOS 매우 간단한 전환을 찾아야 합니다. 대부분의 Api와 기능은 동일 하지만 많은 공통 Api (예: WebKit)를 사용할 수 없습니다. 또한 Siri 원격으로을 사용 하 여 작업 하는 경우 터치 스크린 기반 iOS 장치에는 없는 몇 가지 디자인 과제가 있습니다.

이 가이드에서는 tvOS 개발자를 위한 tvOS 9에서 제공 되는 모든 새로운 및 수정 된 Api 및 기능에 대해 소개 합니다. TvOS에 대 한 자세한 내용은 Apple의 [새로운 APPLE TV 설명서 개발](https://developer.apple.com/tvos/) 을 참조 하세요.

<a name="Supported-and-Unsupported-Capabilities" />

## <a name="supported-and-unsupported-capabilities"></a>지원 되거나 지원 되지 않는 기능

Apple TV에서 실행 되는 tvOS apps에는 다음과 같은 지원 되는 기능 및 기능이 있습니다.

- 앱 그룹
- 백그라운드 모드
- 데이터 보호
- Game Center
- 게임 컨트롤러
- iCloud
- 앱에서 바로 구매
- 키 집합 공유

지원 되지 않는 기능 및 기능은 다음과 같습니다.

- Apple Pay
- 앱 샌드박스
- 연결된 도메인
- HealthKit
- HomeKit
- 내부 앱 오디오
- 지도
- 개인 VPN
- 푸시 알림
- Wallet
- 무선 액세서리 구성

자세한 내용은 [지원 되는 어셈블리](~/ios/tvos/internals/assemblies.md) 및 [지원 되는 프레임 워크](~/ios/tvos/internals/frameworks.md) 설명서를 참조 하세요.

<a name="Apple-TV-Hardware" />

## <a name="apple-tv-hardware"></a>Apple TV 하드웨어

새 Apple TV에는 다음과 같은 하드웨어 사양이 있습니다.

- 64 비트 A8 프로세서
- 32GB 또는 64GB의 저장소
- 2GB RAM
- 10/100Mbps 이더넷
- WiFi 802.11 a/b/g/n/ac
- 1080p 해상도
- HDMI
- USB C 포트 (개발자 및 진단 사용에만 해당)
- 새 Siri 원격 또는 Apple TV 원격 (지역 기반)

### <a name="siri-remote"></a>Siri 원격

제공 된 Apple TV 원격은 지역에 따라 하나의 구성으로 제공 됩니다. Siri 원격 또는 Apple TV 원격.

Siri 원격은 현재 다음 국가에서 사용할 수 있습니다.

- 오스트레일리아
- 캐나다
- 프랑스
- 독일
- 일본
- 스페인
- 영국
- 미국

다른 모든 국가에서는 Siri 단추를 검색 단추로 대체 하는 Apple TV 원격을 받게 됩니다 .이 단추를 클릭 하면 검색 단추를 사용 하 여 기본 검색 화면에서

[![](tvos9-images/remote02.png "Siri 원격")](tvos9-images/remote02.png#lightbox)

자세한 내용은 [Siri 원격 및 Bluetooth 컨트롤러](~/ios/tvos/platform/remote-bluetooth.md) 설명서를 참조 하세요.

<a name="Apple-TV-Provisioning" />

## <a name="apple-tv-provisioning"></a>Apple TV 프로 비전

IOS에 대 한 개발과 마찬가지로 새 tvOS에는 이미 Apple으로 설정한 팀 멤버 자격 및 서명 Id를 기반으로 개발 및 배포 모두에 적합 한 프로 비전 프로필이 필요 합니다.

또한 iCloud KVS 또는 CloudKit 데이터 저장소와 같은 tvOS 기능에 액세스 하려면 적절 한 프로 비전이 필요 합니다. TvOS 앱에서 iCloud를 지 원하는 방법에 대 한 자세한 내용은 [리소스 및 데이터 저장소](~/ios/tvos/app-fundamentals/resources-data-storage.md) 를 참조 하세요.

프로 비전 프로필은 Xamarin.ios 앱과 함께 사용 하는 것과 동일한 방식으로 생성 및 설치 됩니다. 따라서 자세한 내용은 iOS [장치 프로 비전](~/ios/get-started/installation/device-provisioning/index.md) 설명서를 참조 하세요.

<a name="Apple-TV-Apps" />

## <a name="apple-tv-apps"></a>Apple TV 앱

새 Apple TV 하드웨어 및 tvOS 9는 기존 및 클라이언트-서버 앱 이라는 두 가지 유형의 앱을 지원 합니다.

<a name="Traditional-Apps" />

### <a name="traditional-apps"></a>기존 앱

기존 앱은 Apple TV 앱 스토어에서 구매 하 여 장치에 직접 설치 됩니다. 이러한 앱은 Xamarin.ios 앱과 동일한 프레임 워크 및 기술을 사용 하 여 개발 된 게임, 유틸리티 또는 미디어 앱이 될 수 있습니다.

Apple TV 앱은 최대 크기가 200MB 이며 주문형 리소스를 사용 하 여 추가 2GB의 콘텐츠를 다운로드할 수 있습니다. 자세한 내용은 [리소스 및 데이터 저장소](~/ios/tvos/app-fundamentals/resources-data-storage.md) 를 참조 하세요.

TvOS를 사용 하 여 tvOS apps를 개발 하는 데 필요한 도구 및 개념을 숙지 하려면 [Hello, tvOS 빠른 시작 가이드](~/ios/tvos/get-started/hello-tvos.md) 를 참조 하세요.

<a name="Summary" />

### <a name="client-server-apps"></a>클라이언트-서버 앱

Apple TV는 기존 앱을 설치 하는 것 외에도 웹 기술 (HTTPS, XML 및 JavaScript)을 사용 하 여 웹 기반 클라이언트-서버 미디어 스트리밍 앱을 쉽게 만들 수 있습니다. Apple의 TVML 태그 언어를 사용 하 여 사용자 인터페이스를 디자인 하 고 JavaScript를 사용 하 여 TVMLKit를 사용 하 여 앱의 동작을 정의 합니다.

자세한 내용은 apple의 [APPLE Tv 마크업 언어 참조](https://developer.apple.com/library/prerelease/tvos/documentation/LanguagesUtilities/Conceptual/ATV_Template_Guide/index.html#//apple_ref/doc/uid/TP40015064), [Tvjs 프레임](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLJS/Reference/TVJSFrameworkReference/index.html#//apple_ref/doc/uid/TP40016076)워크 참조, [Tvmlkit 프레임 워크 참조](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLKit/Reference/TVMLKit_Collection/index.html#//apple_ref/doc/uid/TP40016429), [Apple Tv에 대 한 HTTP 라이브 스트리밍 및 HLS 제작 사양](https://developer.apple.com/services-account/download?path=/Documentation/HLS_Authoring_Specification_for_Apple_TV/HLS_Authoring_Specification_for_Apple_TV.pdf) [정보](https://developer.apple.com/library/prerelease/tvos/referencelibrary/GettingStarted/AboutHTTPLiveStreaming/about/about.html#//apple_ref/doc/uid/TP40013978) 를 참조 하세요. 설명을.

<a name="User-Interface-Challenges" />

## <a name="user-interface-challenges"></a>사용자 인터페이스 문제

IOS 또는 OS X와 달리 Apple TV에는 사용자가 직접 앱 이나 해당 콘텐츠를 선택 하 고 상호 작용할 수 있도록 하는 터치 스크린 또는 마우스가 없습니다. 대신, 새 Siri 원격 또는 Bluetooth 게임 컨트롤러를 사용자가 앱의 사용자 인터페이스를 탐색 합니다. 자세한 내용은 [Siri 원격 및 Bluetooth 컨트롤러](~/ios/tvos/platform/remote-bluetooth.md) 설명서를 참조 하세요.

또한 전체 사용자 환경은 단일 사용자 환경을 위한 iOS 또는 Mac 앱과 크게 다르지 않습니다. Apple TV를 사용 하는 경우 사용자 환경은 여러 사람이 단일 앱과 상호 작용 하는 소파에 앉아 있을 수 있는 본질적으로 교류 하는 경향이 있습니다. 성공적인 Apple TV 앱 환경을 설계 (새 앱 또는 기존 앱 포팅) 하려면 이러한 변경 사항을 고려해 야 합니다. 

<a name="Summary" />

### <a name="working-with-focus-and-parallax-images"></a>포커스 및 시차 이미지 작업

위에서 설명한 것 처럼 tvOS 앱의 사용자는 iOS를 사용 하 여 장치 화면에서 이미지를 탭 하지만 Siri 원격을 사용 하 여 대화방에서 간접적으로 상호 작용 하지 않습니다. 이 사용자 상호 작용을 제공 하 고 처리 하기 위해 Apple TV는 포커스 기반 모델을 사용 합니다. 

포커스가 변경 되 면 미세한 애니메이션 및 효과 (예: 이미지에 대 한 시차 효과)를 사용 하 여 현재 포커스가 있는 사용자 인터페이스 항목을 명확 하 게 식별 합니다.

사용자가 Siri 원격에서 천천히 원형 제스처를 수행 하면 포커스가 있는 항목이이 이동에 대 한 응답으로 실시간으로 설정 됩니다. Sway가 발생 하면 표면이 빛이 보이도록 표시 되는 이미지에 불 광택이 적용 됩니다. 지정 된 비활성 시간이 지나면 포커스를 벗어난 콘텐츠가 희미 하 고 포커스가 있는 항목이 훨씬 커집니다.

자세한 내용은 [탐색 사용 및](~/ios/tvos/app-fundamentals/navigation-focus.md) [아이콘 및 이미지 작업](~/ios/tvos/app-fundamentals/icons-images.md) 설명서를 참조 하세요.

<a name="The-Home-Screen" />

### <a name="the-home-screen"></a>홈 화면

Apple TV 홈 화면에는 설치 된 모든 앱이 표시 되며 사용자 기본 설정에 액세스 하는 방법이 제공 됩니다.

[![](tvos9-images/home01.png "홈 화면")](tvos9-images/home01.png#lightbox)

사용자는 포커스를 사용 하는 Siri 원격에서 터치 제스처를 사용 하 여 앱 아이콘 그리드를 탐색 하 여 앱을 선택 하 고 시작 합니다. 앱 아이콘은 잠재적 사용자에 게 훌륭한 인상을 줄 수 있는 첫 번째 기회 이며, 앱의 용도를 한눈에 파악할 수 있습니다.

모든 앱은 앱 아이콘의 작은 버전과 많은 버전을 모두 제공 해야 합니다. 앱이 설치 되 면 Apple TV 홈 화면에서 작은 아이콘이 사용 됩니다. 앱 스토어에서 많은 버전이 사용 됩니다. 크게 된 앱 아이콘은 작은 아이콘 버전의 모양과 느낌을 모방 해야 합니다.

자세한 내용은 [아이콘 및 이미지 작업](~/ios/tvos/app-fundamentals/icons-images.md) 설명서를 참조 하세요.

<a name="The-Top-Shelf" />

### <a name="the-top-shelf"></a>위쪽 선반

사용자가 Apple TV 홈 화면의 맨 위 행에 tvOS 앱을 배치 하는 경우 사용자가 앱을 선택 하면 최상위 선반 이미지가 표시 됩니다. 이 이미지는 앱의 기능을 강조 표시 하거나 콘텐츠에 대 한 직접 링크를 제공 해야 합니다.

[![](tvos9-images/topshelf01.png "위쪽 선반")](tvos9-images/topshelf01.png#lightbox)

Top 선반 이미지는 단일 정적 `.png` 또는 `.lsr` 파일로 제공 될 수도 있고 런타임에 포커스를 받을 수 있는 항목의 단일 행으로 동적으로 만들 수도 있습니다.

정적 최상위 선반 이미지를 표시 하는 대신 동적 행 또는 포커스를 받을 수 있는 항목 또는 동적 스크롤 배너 집합을 포함할 수 있습니다. 이러한 동적 스타일을 사용 하면 앱에서 제공 하는 콘텐츠를 강조 표시 하거나 가장 많이 사용 하는 기능으로 바로 이동할 수 있습니다.

자세한 내용은 [아이콘 및 이미지 작업](~/ios/tvos/app-fundamentals/icons-images.md) 설명서 및 Apple의 [Tvservices 프레임 워크 참조](https://developer.apple.com/library/prerelease/tvos/documentation/TVServices/Reference/TVServices_Ref/index.html#//apple_ref/doc/uid/TP40016412) 를 참조 하세요. 앱에 최상위 선반 확장을 추가 하 여 동적 최고 선반 콘텐츠를 제공 하는 방법에 대 한 자세한 내용은을 참조 하세요.






## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 가이드](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [Xamarin을 사용 하 여 tvOS 용 앱 빌드 (비디오)](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
