---
title: "9 tvOS 소개"
description: "이 문서 Xamarin.tvOS 개발자를 위한 모든 새로운 기능과 수정 된 Api 및 tvOS 9에서에서 사용할 수 있는 기능을 소개합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: A7E738E1-9F94-489B-918F-7DF8F0810987
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
ms.openlocfilehash: 55e83658e09bc7e5c12bb3ef3f508497651ec46c
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="introduction-to-tvos-9"></a>9 tvOS 소개

_이 문서 Xamarin.tvOS 개발자를 위한 모든 새로운 기능과 수정 된 Api 및 tvOS 9에서에서 사용할 수 있는 기능을 소개합니다._


Apple 다시 디자인 된, 터치 기능을 사용 하도록 원격 특징으로, 새 tvOS 운영 체제 (iOS 9에 기반)를 실행 하는 Apple TV 하드웨어의 4 번째 세대를 출시 했습니다.

처음으로 tvOS 열립니다 다양 앱을 만들고 작성 하 고 iTunes 응용 프로그램을 사용 하 여 iOS 용 앱을 해제 하는 환경을 비슷한 프로세스에 Apple TV 기본 제공 앱 스토어를 통해 해제할 수 있도록 개발자에 게 Apple TV 플랫폼 저장소입니다.

Xamarin.iOS 개발에 익숙한 경우 찾아야 tvOS로의 전환을 매우 간단 합니다. 그러나 대부분의 Api 및 기능을 동일 합니다. 대부분의 일반적인 Api는 (예: WebKit) 사용할 수 없습니다. 또한 작업에서 Siri 원격와 되지만 터치 스크린을 기반으로 iOS 장치에 존재 하지 않는 몇 가지 디자인 문제가 발생 합니다.

이 가이드는 Xamarin.tvOS 개발자를 위한 모든 새로운 기능과 수정 된 Api 및 tvOS 9에서에서 사용할 수 있는 기능에 대 한 소개를 제공 합니다. TvOS에 대 한 자세한 내용은 Apple의를 참조 하십시오 [새 Apple TV에 대 한 개발](https://developer.apple.com/tvos/) 설명서입니다.

<a name="Supported-and-Unsupported-Capabilities" />

## <a name="supported-and-unsupported-capabilities"></a>지원 되는 / 지원 되지 않는 기능

Apple TV에서 실행 되는 tvOS 앱 다음 지원 되는 기능 및 특징:

 - 앱 그룹
 - 백그라운드 모드
 - 데이터 보호
 - Game Center
 - 게임 컨트롤러
 - iCloud
 - 앱에서 바로 구매
 - 키 집합 공유

다음 기능 및 특성은 지원 되지 않습니다.

 - Apple Pay
 - 샌드박스 응용 프로그램
 - 연결된 도메인
 - HealthKit
 - HomeKit
 - 내부 앱 오디오
 - 맵
 - 개인 VPN
 - 푸시 알림
 - Wallet
 - 무선 액세서리 구성

참조 하십시오 우리의 [지원 어셈블리](~/ios/tvos/internals/assemblies.md) 및 [프레임 워크 지원](~/ios/tvos/internals/frameworks.md) 자세한 정보에 대 한 설명서입니다.

<a name="Apple-TV-Hardware" />

## <a name="apple-tv-hardware"></a>Apple TV 하드웨어

새 Apple TV에 다음의 하드웨어 사양이 있습니다.

 - 64 비트 A8 프로세서
 - 32GB 또는 64GB의 저장소
 - 2GB RAM
 - 10/100Mbps 이더넷
 - WiFi 802.11a/b/g/n/ac
 - 1080p 해상도
 - HDMI
 - USB C 포트 (개발자 및 진단 전용)
 - 새 Siri 원격 또는 Apple TV 원격 (지역에 따라)

### <a name="siri-remote"></a>Siri 원격

지역에 따라 제공 된 Apple TV 원격 나올지 하나의 구성 중 하나에서: Siri 원격 또는 Apple TV 원격 합니다.

Siri 원격은 다음과 같은 국가에서 현재 제공 됩니다.

 - 오스트레일리아
 - 캐나다
 - 프랑스
 - 독일
 - 일본
 - 스페인
 - 영국
 - 미국

다른 모든 국가 Apple TV 원격 검색을 위해 텍스트 입력이 포함 된 기본 검색 화면을 열 수 있는 검색 단추와 Siri 단추를 대체 하는 표시 됩니다.

[![](tvos9-images/remote02.png "Siri 원격")](tvos9-images/remote02.png#lightbox)

자세한 내용은 참조 하십시오 우리의 [Siri 원격 인스턴스 및 Bluetooth 컨트롤러](~/ios/tvos/platform/remote-bluetooth.md) 설명서입니다.

<a name="Apple-TV-Provisioning" />

## <a name="apple-tv-provisioning"></a>Apple TV 프로 비전

IOS 용으로 개발할 때와 마찬가지로 새 tvOS 개발 및 배포 서명 Id는 이미 설정한 apple와 팀 멤버 자격에 따라에 모두에 대 한 적절 한 프로 비전 프로필이 필요 합니다.

적절 한 프로 비전 iCloud은 KVS 또는 CloudKit 데이터 저장소와 같이 tvOS 기능에 액세스 하는 데 필요한 이기도 합니다. 참조 하십시오 우리의 [리소스 및 데이터 저장소](~/ios/tvos/app-fundamentals/resources-data-storage.md) 지원 iCloud Xamarin.tvOS 앱에 대 한 자세한 내용은 합니다.

프로 비전 프로필 생성 및 Xamarin.iOS 앱 작업할 때와 동일한 방식으로 설치 됩니다. 따라서 iOS를 참조 하세요 [장치 프로 비전](~/ios/get-started/installation/device-provisioning/index.md) 자세한 세부 정보에 대 한 설명서입니다.

<a name="Apple-TV-Apps" />

## <a name="apple-tv-apps"></a>Apple TV 앱

새 Apple TV 하드웨어와 tvOS 9 두 가지 유형의 앱을 지원: 일반적인 클라이언트-서버 응용 프로그램 및입니다.

<a name="Traditional-Apps" />

### <a name="traditional-apps"></a>일반적인 응용 프로그램

일반적인 응용 프로그램 Apple TV 앱 스토어에서 구입한 하 고 장치에서 직접 설치 됩니다. 이러한 앱은 게임, 유틸리티 또는 Xamarin.iOS 앱으로 동일한 프레임 워크 및 기법을 사용 하 여 개발 된 미디어 응용 프로그램 수 있습니다.

Apple TV 앱 200MB의 최대 크기 및 추가 2GB의 요청 시 리소스를 사용 하 여 콘텐츠를 다운로드할 수 있습니다. 참조 하십시오 우리의 [리소스 및 데이터 저장소](~/ios/tvos/app-fundamentals/resources-data-storage.md) 자세한 정보에 대 한 합니다.

참조 우리의 [Hello, tvOS 퀵 스타트 가이드](~/ios/tvos/get-started/hello-tvos.md) 개념 Xamarin.tvOS를 사용 하 여 tvOS 앱을 개발 하는 데 필요한 도구와 잘 알아두는 것입니다.

<a name="Summary" />

### <a name="client-server-apps"></a>클라이언트-서버 응용 프로그램

설치 된 기존 응용 프로그램 뿐만 아니라 Apple TV 쉽게 웹 기술 (HTTPS, XML 및 JavaScript)를 사용 하 여 앱을 스트리밍 클라이언트-서버-웹 기반 미디어를 만드는 있습니다. Apple의 TVML 표시 언어를 사용 하 여 사용자 인터페이스 디자인 되 고 TVMLKit를 사용 하 여 응용 프로그램의 동작을 정의 하려면 JavaScript를 사용 합니다.

자세한 내용은 Apple의를 참조 하십시오 [Apple TV 태그 언어 참조](https://developer.apple.com/library/prerelease/tvos/documentation/LanguagesUtilities/Conceptual/ATV_Template_Guide/index.html#//apple_ref/doc/uid/TP40015064), [TVJS 프레임 워크 참조](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLJS/Reference/TVJSFrameworkReference/index.html#//apple_ref/doc/uid/TP40016076), [TVMLKit 프레임 워크 참조](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLKit/Reference/TVMLKit_Collection/index.html#//apple_ref/doc/uid/TP40016429), [에 대 한 HTTP 라이브 스트리밍](https://developer.apple.com/library/prerelease/tvos/referencelibrary/GettingStarted/AboutHTTPLiveStreaming/about/about.html#//apple_ref/doc/uid/TP40013978) 및 [Apple TV에 대 한 제작 사양이 HLS](https://developer.apple.com/services-account/download?path=/Documentation/HLS_Authoring_Specification_for_Apple_TV/HLS_Authoring_Specification_for_Apple_TV.pdf) 설명서입니다.

<a name="User-Interface-Challenges" />

## <a name="user-interface-challenges"></a>사용자 인터페이스 문제

IOS 또는 OS X와 달리 Apple TV 않아도 터치 스크린 또는 사용자가 직접 선택 하 고 응용 프로그램 또는 해당 콘텐츠가와 상호 작용 하도록 허용 하는 마우스 됩니다. 대신 이러한 사용자는 응용 프로그램의 사용자 인터페이스를 탐색 하는 새로운 Siri 원격 또는 Bluetooth 게임 컨트롤러입니다. 자세한 내용은 참조 하십시오 우리의 [Siri 원격 인스턴스 및 Bluetooth 컨트롤러](~/ios/tvos/platform/remote-bluetooth.md) 설명서입니다.

또한 전체 사용자 환경이 iOS 또는 Mac 앱을 단일 사용자 환경을 경향이 있는 보다 크게 차이가 있습니다. Apple tv와 사용자 환경에는 여기서 여러 사람이 있습니다 수 소파에 앉아 단일 응용 프로그램 및 서로 상호 작용 본질적으로 더 소셜 경우가 많습니다. 성공적인 Apple TV 앱 환경을 (새 앱 또는 기존 이식)를 디자인 하려면 이러한 변경 내용은 고려 사항에 고려해 야 합니다. 

<a name="Summary" />

### <a name="working-with-focus-and-parallax-images"></a>포커스 및 시차 이미지 작업

위에서 설명 했 듯이 Xamarin.tvOS 응용 프로그램의 사용자의 인터페이스를 직접으로 ios 여기서은 탭 하지 장치의 화면에 나타나는 이미지 하지만 직접에서 Siri 원격을 사용 하 여 대화방에서 상호 작용 하지 않는 됩니다. 나타내고이 사용자 상호 작용 처리 Apple TV 포커스 기반 모델을 사용 합니다. 

포커스 변경에 따라 미묘한 애니메이션 및 효과 (예: 이미지에 시차 효과) 현재 포커스가 있는 사용자 인터페이스 항목을 명확 하 게 식별 하는 데 사용 됩니다.

사용자 Siri 원격 느린, 순환 제스처를 만드는 경우 포커스가 있는 항목이 이동이에 대 한 응답으로 실시간 라 합니다. 거주 발생 조명된 광택을 표시 하는 화면을 만드는 해당 이미지에 적용 됩니다. 비활성가 주어진된 시간 후 포커스를 벗어난 내용이 흐리게 표시 하 고 Focused 항목도 커집니다.

자세한 내용은 참조 하십시오 우리의 [탐색 및 포커스 작업](~/ios/tvos/app-fundamentals/navigation-focus.md) 및 [아이콘과 이미지 작업을](~/ios/tvos/app-fundamentals/icons-images.md) 설명서입니다.

<a name="The-Home-Screen" />

### <a name="the-home-screen"></a>홈 화면

Apple TV 홈 화면 설치 되 고 사용자 기본 설정에 액세스 하는 방법을 제공 하는 모든 앱에 표시 됩니다.

[![](tvos9-images/home01.png "홈 화면")](tvos9-images/home01.png#lightbox)

사용자가 포커스를 사용 하 여 응용 프로그램을 선택 하 고 응용 프로그램을 시작 하 고 Siri 원격에서 터치 제스처를 사용 하 여 응용 프로그램 아이콘의 표를 탐색 합니다. 앱 아이콘은는 첫 번째 잠재적 사용자에 매우 깊은 인상을 나타내고를 한 눈에 응용 프로그램의 용도 나타내야 합니다.

모든 응용 프로그램은 작 및 해당 응용 프로그램 아이콘의 큰 버전을 모두 제공 해야 합니다. 작은 아이콘에는 앱을 설치 하는 경우 Apple TV 홈 화면에서 사용 됩니다. 큰 버전 앱 스토어에서 사용 됩니다. 큰 응용 프로그램 아이콘에는 작은 아이콘 버전의 모양과 느낌 유사 해야 합니다.

자세한 내용은 참조 하십시오 우리의 [아이콘과 이미지 작업을](~/ios/tvos/app-fundamentals/icons-images.md) 설명서입니다.

<a name="The-Top-Shelf" />

### <a name="the-top-shelf"></a>위쪽 선반

사용자가을 배치 하 Xamarin.tvOS 앱 Apple TV 홈 화면에서 첫 행에는 사용자가 앱을 선택 하는 큰 위쪽 선반 이미지 표시 됩니다. 이 이미지는 응용 프로그램의 기능을 강조 표시 하거나 해당 내용에 대 한 직접 링크를 제공 해야 합니다.

[![](tvos9-images/topshelf01.png "위쪽 선반")](tvos9-images/topshelf01.png#lightbox)

위쪽 선반 이미지 수를 단일 정적으로 제공 되거나 `.png` 또는 `.lsr` 파일 이거나 동적으로 런타임에 포커스 항목의 단일 행으로 합니다.

정적 위쪽 선반 이미지를 표시 하는 대신 동적 행 또는 포커스 항목 또는 동적 집합 배너 스크롤을 포함할 수 있습니다. 이러한 동적 스타일의 둘 다를 통해 응용 프로그램 또는 가장 사용 되는 해당 기능에 대 한 점프 하 여 제공 된 콘텐츠를 강조 표시할 수 있습니다.

자세한 내용은 참조 하십시오 우리의 [아이콘과 이미지 작업을](~/ios/tvos/app-fundamentals/icons-images.md) 설명서 및 Apple의 [TVServices 프레임 워크 참조](https://developer.apple.com/library/prerelease/tvos/documentation/TVServices/Reference/TVServices_Ref/index.html#//apple_ref/doc/uid/TP40016412) 위쪽 선반 확장을 제공 하기 위해 앱에 추가 하는 방법에 대 한 자세한 내용은 동적 위쪽 선반 콘텐츠입니다.






## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 응용 프로그램 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [Xamarin (비디오)는 tvOS에 대 한 응용 프로그램 구축](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
