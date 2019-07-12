---
title: tvOS 9 소개
description: 이 아티클에서 Xamarin.tvOS 개발자를 위한 모든 새로운 기능과 수정 된 Api 및 tvOS 9에서에서 사용할 수 있는 기능을 소개합니다.
ms.prod: xamarin
ms.assetid: A7E738E1-9F94-489B-918F-7DF8F0810987
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/07/2016
ms.openlocfilehash: c4aea5a35fd5db46c9e7ca245b852c988e82f53e
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67830500"
---
# <a name="introduction-to-tvos-9"></a>tvOS 9 소개

_이 아티클에서 Xamarin.tvOS 개발자를 위한 모든 새로운 기능과 수정 된 Api 및 tvOS 9에서에서 사용할 수 있는 기능을 소개합니다._

Apple 다시 디자인 된, 터치 사용 원격 특징으로, 새 tvOS 운영 체제 (iOS 9에 따라)를 실행 하는 Apple TV 하드웨어의 네 번째 세대를 출시 했습니다.

처음으로 tvOS 열립니다 다양 한 몰입 형 앱을 만들고 작성 하 고 iTunes 앱을 사용 하 여 iOS 용 앱을 해제의 환경과 비슷한 프로세스에 Apple TV 기본 제공 앱 스토어를 통해 해제할 수 있도록 개발자에 게 Apple TV 플랫폼 저장소입니다.

Xamarin.iOS 개발에 익숙한 경우에 tvOS 전환을 매우 간단한 찾아야 합니다. 대부분의 Api 및 기능 동일, 많은 일반적인 Api (예: WebKit) 사용할 수 있지만. 또한 작업을 Siri 원격을 사용 하 여 터치 기반 iOS 장치에 존재 하지 않는 몇 가지 디자인 문제를 제기 합니다.

이 가이드에서는 Xamarin.tvOS 개발자를 위한 모든 새로운 기능과 수정 된 Api 및 tvOS 9에서에서 사용 가능한 기능에 대 한 소개를 제공 합니다. TvOS에 대 한 자세한 내용은 Apple의를 참조 하세요 [에 새 Apple TV 개발](https://developer.apple.com/tvos/) 설명서.

<a name="Supported-and-Unsupported-Capabilities" />

## <a name="supported-and-unsupported-capabilities"></a>지원 되거나 지원 되지 않는 기능

Apple TV에서 실행 중인 tvOS 앱 다음 지원 되는 기능 및 특징:

- 앱 그룹
- 백그라운드 모드
- 데이터 보호
- Game Center
- 게임 컨트롤러
- iCloud
- 앱에서 바로 구매
- 키 집합 공유

다음 기능 및 기능 지원 되지 않습니다.

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

참조 하세요 우리의 [지원 되는 어셈블리](~/ios/tvos/internals/assemblies.md) 하 고 [프레임 워크 지원](~/ios/tvos/internals/frameworks.md) 자세한 설명서.

<a name="Apple-TV-Hardware" />

## <a name="apple-tv-hardware"></a>Apple TV 하드웨어

새 Apple TV에 다음의 하드웨어 사양이 있습니다.

- 64 비트 A8 프로세서
- 32GB 또는 64GB 저장소
- 2GB의 RAM
- 10/100Mbps 이더넷
- WiFi 802.11a/b/g/n/ac
- 해상도 1080p
- HDMI
- USB C 포트 (개발자 및 진단 사용에만)
- 새 Siri 원격 또는 Apple TV 원격 (지역에 따라)

### <a name="siri-remote"></a>Siri 원격

지역에 따라 제공 된 Apple TV 원격 들어옵니다 중 하나의 구성: Siri 원격 또는 Apple TV 원격입니다.

Siri 원격은 현재 다음 국가의

- 오스트레일리아
- 캐나다
- 프랑스
- 독일
- 일본
- 스페인
- 영국
- 미국

다른 모든 국가 Apple TV 원격 검색에 대 한 텍스트 입력을 사용 하 여 기본 검색 화면을 표시 하는 검색 단추를 사용 하 여 Siri 단추를 대체 하는 표시 됩니다.

[![](tvos9-images/remote02.png "Siri 원격")](tvos9-images/remote02.png#lightbox)

자세한 내용은 참조 하십시오 우리의 [Siri 원격 및 Bluetooth 컨트롤러](~/ios/tvos/platform/remote-bluetooth.md) 설명서.

<a name="Apple-TV-Provisioning" />

## <a name="apple-tv-provisioning"></a>Apple TV 프로 비전

IOS 용으로 개발에서 마찬가지로 새 tvOS 개발 및 배포 팀 멤버 자격 및 서명 Id는 이미 설정한 apple에 따라 적절 한 프로 비전 프로필 해야 합니다.

적절 한 프로 비전 KVS iCloud 또는 CloudKit 데이터 저장소와 같이 tvOS 기능에 액세스 하는 데 필요한 이기도 합니다. 하십시오 우리의 [리소스 및 데이터 저장소](~/ios/tvos/app-fundamentals/resources-data-storage.md) Xamarin.tvOS 앱에서 iCloud 지원에 대 한 정보에 대 한 합니다.

프로 비전 프로필 생성 및 Xamarin.iOS 앱을 작업할 때와 동일한 방식으로 설치 됩니다. 따라서 iOS를 참조 하세요 [Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md) 자세한 세부 정보에 대 한 설명서입니다.

<a name="Apple-TV-Apps" />

## <a name="apple-tv-apps"></a>Apple TV 앱

새 Apple TV 하드웨어 및 tvOS 9 두 가지 유형의 앱을 지원 합니다: 기존 및 클라이언트-서버 앱.

<a name="Traditional-Apps" />

### <a name="traditional-apps"></a>기존 앱

기존 앱 Apple TV App Store에서 구매 하 고 장치에서 직접 설치 됩니다. 이러한 앱은 게임, 유틸리티 또는 동일한 기술과 프레임 워크를 사용 하 여 Xamarin.iOS 앱으로 개발 된 미디어 앱 수 있습니다.

Apple TV 앱 200MB의 최대 크기를 가지 며 추가 2GB의 주문형 리소스를 사용 하 여 콘텐츠를 다운로드할 수 있습니다. 참조 하십시오 우리의 [리소스 및 데이터 저장소](~/ios/tvos/app-fundamentals/resources-data-storage.md) 자세한 내용은 합니다.

참조 우리의 [Tvos 빠른 시작 가이드](~/ios/tvos/get-started/hello-tvos.md) 도구 및 Xamarin.tvOS를 사용 하 여 tvOS 앱을 개발 하는 데 필요한 개념을 잘 알고 있습니다.

<a name="Summary" />

### <a name="client-server-apps"></a>클라이언트-서버 앱

설치 된 기존 앱 외에도 Apple TV 쉽게 웹 기술 (HTTPS, XML 및 JavaScript)를 사용 하 여 앱을 스트리밍 클라이언트-서버-웹 기반 미디어를 만듭니다. Apple의 TVML 태그 언어를 사용 하 여 사용자 인터페이스 디자인 되며 JavaScript를 사용 하 여 TVMLKit를 사용 하 여 앱의 동작을 정의 합니다.

자세한 내용은 Apple의를 참조 하세요 [Apple TV 태그 언어 참조](https://developer.apple.com/library/prerelease/tvos/documentation/LanguagesUtilities/Conceptual/ATV_Template_Guide/index.html#//apple_ref/doc/uid/TP40015064), [TVJS 프레임 워크 참조](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLJS/Reference/TVJSFrameworkReference/index.html#//apple_ref/doc/uid/TP40016076)하십시오 [TVMLKit 프레임 워크 참조](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLKit/Reference/TVMLKit_Collection/index.html#//apple_ref/doc/uid/TP40016429), [에 대 한 HTTP 라이브 스트리밍](https://developer.apple.com/library/prerelease/tvos/referencelibrary/GettingStarted/AboutHTTPLiveStreaming/about/about.html#//apple_ref/doc/uid/TP40013978) 하 고 [HLS Apple TV에 대 한 사양을 작성](https://developer.apple.com/services-account/download?path=/Documentation/HLS_Authoring_Specification_for_Apple_TV/HLS_Authoring_Specification_for_Apple_TV.pdf) 설명서.

<a name="User-Interface-Challenges" />

## <a name="user-interface-challenges"></a>사용자 인터페이스 문제

Apple TV가 OS X, iOS와 달리 터치 스크린 또는 마우스 사용자가 직접 선택 하 고 앱을 사용할지 아니면 해당 콘텐츠를 사용 하 여 상호 작용 하도록 허용 되지 않은. 대신 이러한 사용자 앱의 사용자 인터페이스를 탐색 하는 새 Siri 원격 또는 Bluetooth 게임 컨트롤러입니다. 자세한 내용은 참조 하십시오 우리의 [Siri 원격 및 Bluetooth 컨트롤러](~/ios/tvos/platform/remote-bluetooth.md) 설명서.

또한 전반적인 사용자 환경을 iOS 또는 Mac 앱을 단일 사용자 환경을 경향이 있는 보다 크게 다릅니다. Apple TV를 사용 하 여 사용자 환경을 본질적으로 더 많은 소셜 소파 단일 앱 및 서로 상호 작용에 여러 사용자를 차단 수 있는 경우가 많습니다. 환경이 성공적 이면 Apple TV app (새 앱 또는 기존 이식)를 디자인 하려면 이러한 변경 내용은 고려 야 합니다. 

<a name="Summary" />

### <a name="working-with-focus-and-parallax-images"></a>포커스 및 Parallax 이미지 작업

위에서 설명한 대로 Xamarin.tvOS 앱의 사용자가 해당 인터페이스로 직접 ios를 탭 할 하지 장치의 화면에 나타나는 이미지 하지만 직접에서 Siri 원격을 사용 하 여 대화방에서 상호 작용 하지 않는 됩니다. 제공이 사용자 상호 작용을 처리 하 고 Apple TV 포커스 기반 모델을 사용 합니다. 

포커스가 변경 될 때 미묘한 애니메이션과 부작용 (예: 이미지에 시차 적용) 현재 포커스가 있는 사용자 인터페이스 항목을 명확 하 게 식별 하는 데 사용 됩니다.

사용자가 느린, 순환 제스처 Siri 원격 경우 포커스가 있는 항목이 이동이에 대 한 응답으로 실시간 sway 됩니다. sway 발생을 표시등이 광택 우수한 성능을 나타내는 표시 화면을 만드는 이미지에 적용 됩니다. 지정 된 시간이 지나면 비활성, 포커스를 벗어난 내용을 흐리게 표시 및 Focused 항목은 더 커집니다.

자세한 내용은 참조 하십시오 우리의 [탐색 및 포커스 작업](~/ios/tvos/app-fundamentals/navigation-focus.md) 및 [아이콘 및 이미지 작업](~/ios/tvos/app-fundamentals/icons-images.md) 설명서.

<a name="The-Home-Screen" />

### <a name="the-home-screen"></a>홈 화면

Apple TV 홈 화면에는 설치 되 고 사용자 기본 설정에 액세스 하는 방법을 제공 하는 모든 앱 표시 됩니다.

[![](tvos9-images/home01.png "홈 화면")](tvos9-images/home01.png#lightbox)

사용자가 터치 제스처를 사용 하 여 포커스를 사용 하 여 앱을 선택 하 고 실행 하려면 Siri 원격 앱 아이콘이 포함 된 표를 탐색 합니다. 앱 아이콘은의 첫 번째 잠재적 사용자에서 매우 깊은 인상을 나타내고 한눈에 앱의 용도 나타내야 합니다.

모든 앱에는 작은 해당 앱 아이콘의 큰 버전을 제공 해야 합니다. 작은 아이콘은 앱을 설치할 때 Apple TV 홈 화면에서 사용 됩니다. 큰 버전을 앱 스토어에서 사용 됩니다. 큰 앱 아이콘에는 작은 아이콘 버전의 모양과 느낌와 비슷해야 합니다.

자세한 내용은 참조 하십시오 우리의 [아이콘 및 이미지 작업](~/ios/tvos/app-fundamentals/icons-images.md) 설명서.

<a name="The-Top-Shelf" />

### <a name="the-top-shelf"></a>위쪽 선반

Apple TV 홈 화면의 위쪽 행에 Xamarin.tvOS 앱을 배치 하는 경우 사용자가 앱을 선택한 경우 큰 인기 코너 이미지 표시 됩니다. 이 이미지는 앱의 기능을 강조 하거나 해당 내용에 대 한 직접 링크를 제공 해야 합니다.

[![](tvos9-images/topshelf01.png "위쪽 선반")](tvos9-images/topshelf01.png#lightbox)

단일 정적으로 인기 코너 이미지 제공 `.png` 또는 `.lsr` 파일 이거나 동적으로 런타임 시 포커스 가능 항목의 단일 행으로 합니다.

정적 인기 코너 이미지를 표시 하는 대신 동적 행 또는 포커스 항목 스크롤 배너의 동적 집합을 포함할 수 있습니다. 둘 다 이러한 동적 스타일의 앱 또는 자주 사용 하는 기능에 대 한 점프 하 여 제공 된 콘텐츠를 강조 표시할 수 있습니다.

자세한 내용은 참조 하십시오 우리의 [아이콘 및 이미지 작업](~/ios/tvos/app-fundamentals/icons-images.md) 설명서와 Apple [TVServices 프레임 워크 참조](https://developer.apple.com/library/prerelease/tvos/documentation/TVServices/Reference/TVServices_Ref/index.html#//apple_ref/doc/uid/TP40016412) 위쪽 선반 확장을 제공 하기 위해 앱으로 추가 하는 방법은 동적 인기 코너 콘텐츠입니다.






## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [Xamarin (비디오)를 사용 하 여 tvOS에 대 한 앱 빌드](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
