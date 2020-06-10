---
title: macOS Sierra 소개
description: 이 문서에서는 Xamarin.ios 개발자를 위한 macOS Sierra에서 사용할 수 있는 새로운 Api 및 수정 된 Api 및 기능을 모두 소개 합니다.
ms.prod: xamarin
ms.assetid: 71A8A737-F310-4320-BD23-743AA1E9033C
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 4dde8941b09ac7b235b94e73e86dc60167fb6bd4
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84574459"
---
# <a name="introduction-to-macos-sierra"></a>macOS Sierra 소개

새 macOS Sierra 개발자는 최종 사용자가 이전에 사용할 수 없는 방식으로 앱 및 웹 사이트와 상호 작용할 수 있도록 하는 새로운 Api를 활용할 수 있습니다. 예를 들어 Apple은 이제 고객이 Apple Pay를 통해 안전 하 게 요금을 지불할 수 있는 옵션을 제공 하 고 금속 프레임 워크에 대 한 향상 된 기능을 통해 앱의 그래픽 및 컴퓨팅 잠재력을 높일 수 있습니다 

MacOS Sierra에 대 한 자세한 내용은 Apple의 [Macos + 앱](https://developer.apple.com/macos/) 설명서를 참조 하세요.

<a name="Whats-New-in-macOS-Sierra"></a>

## <a name="whats-new-in-macos-sierra"></a>macOS Sierra의 새로운 기능

Apple은 다음을 비롯 한 기존 기능에 대 한 여러 향상 된 기능을 비롯 한 여러 가지 새로운 Api 및 서비스를 macOS Sierra에 추가 했습니다.

<a name="Apple-File-System"></a>

### <a name="apple-file-system"></a>Apple 파일 시스템

MacOS Sierra를 사용 하 여 Apple은 새 Apple 파일 시스템을 iOS, macOS, tvOS 및 watchOS에 대 한 최신 파일 시스템으로 출시 했습니다. Apple 파일 시스템은 Flash 및 SSD 저장소에 대해 최적화 되었으며, 강력한 암호화, 복사 시 쓰기 메타 데이터, 공간 공유, 파일 및 디렉터리 복제, 스냅숏, 빠른 디렉터리 크기 조정 및 원자성 안전 저장 기본 형식 등의 기능을 제공 합니다.

자세한 내용은 Apple의 [Apple 파일 시스템 가이드](https://developer.apple.com/library/prerelease/content/documentation/FileManagement/Conceptual/APFS_Guide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40016999)를 참조 하세요.

<a name="Apple-Pay-Enhancements"></a>

### <a name="apple-pay-enhancements"></a>향상 된 Apple Pay

Apple은 사용자가 웹 사이트에서 보안 지불을 수행할 수 있도록 하는 macOS Sierra Apple Pay에 대 한 몇 가지 향상 된 기능을 만들었습니다.

MacOS Sierra를 사용 하 여 동적 지불 네트워크 및 새 샌드박스 테스트 환경을 지원 하기 위해 macOS Sierra, iOS 및 watchOS와 함께 작동 하는 몇 가지 새로운 Api가 추가 되었습니다.

macOS Sierra에는 개발자가 iOS 및 macOS Safari 기반 웹 사이트에 직접 Apple Pay를 통합할 수 있도록 하는 새로운 새 기능 Javascript 프레임 워크가 포함 되어 있습니다. Apple Pay를 지 원하는 웹 사이트의 경우 사용자는 iPhone 또는 Apple Watch를 사용 하 여 결제 권한을 부여할 수 있습니다.

자세한 내용은 Apple의 [사과 Epay JS 프레임 워크](https://developer.apple.com/reference/applepayjs) 참조를 참조 하세요.

<a name="Building-Modern-macOS-Apps"></a>

### <a name="building-modern-macos-apps"></a>최신 macOS 앱 빌드

Apple의 Safari 웹 브라우저, 페이지 워드 프로세서 및 숫자 스프레드 시트와 같은 최신 macOS 앱은 여러 가지 새로운 기술을 사용 하 여 부동 패널 및 여러 개의 열린 창과 같은 일반적인 UI 요소를 사용 하지 않는 통합 된 상황에 맞는 사용자 인터페이스를 제공 합니다.

[![탭 Mac 창의 예](images/content08.png)](images/content08.png#lightbox)

[최신 Macos 앱 구축](~/mac/platform/introduction-to-macos-sierra/modern-cocoa-apps.md) 가이드에서는 개발자가 xamarin.ios에서 최신 macos 앱을 빌드하는 데 사용할 수 있는 몇 가지 팁, 기능 및 기술을 설명 합니다.

<a name="CloudKit-Data-Sharing"></a>

### <a name="cloudkit-data-sharing"></a>CloudKit 데이터 공유

CloudKit 프레임 워크는 사용자가 자신의 개인 iCloud 데이터베이스에서 빠르고 쉽게 레코드 또는 레코드 집합을 공유할 수 있도록 macOS Sierra 확장 되었습니다.

CloudKit는 공유 레코드 초대를 보내고 받기 위한 전체 UI를 제공 하 고 사용자는 레코드에 대 한 액세스 권한이 있는 사용자에 대해 완전 한 읽기/쓰기 제어를 제공 합니다.

자세한 내용은 Apple의 [Cloudkit 프레임 워크 참조](https://developer.apple.com/reference/clockkit) 및 [Cloudkit JS 프레임 워크 참조](https://developer.apple.com/reference/cloudkitjs)를 참조 하세요.

> [!IMPORTANT]
> Apple에서는 개발자가 유럽 연합의 GDPR(일반 데이터 보호 규정)을 제대로 처리하는 데 도움이 되는 [도구를 제공합니다](https://developer.apple.com/support/allowing-users-to-manage-data/).

<a name="Safari-App-Extensions-Support"></a>

### <a name="safari-app-extensions-support"></a>Safari 앱 확장 지원

Safari 앱 확장을 사용 하면 앱이 macOS Sierra와 긴밀 하 게 통합 되는 동안 Safari 웹 브라우저의 동작을 확장할 수 있습니다. MacOS Safari 앱 확장은 iOS Safari 앱 확장과 유사 하 게 작동 하므로 한 시스템에서 다른 시스템으로 쉽게 이식할 수 있습니다.

자세한 내용은 Apple의 [Safari 앱 확장 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternetWeb/Conceptual/SafariAppExtension_PG/index.html#//apple_ref/doc/uid/TP40017319)를 참조 하세요.

<a name="Security-and-Privacy-Enhancements"></a>

### <a name="security-and-privacy-enhancements"></a>보안 및 개인 정보 향상

Apple은 앱의 보안을 개선 하 고 최종 사용자의 개인 정보를 다음을 비롯 하 여 앱의 보안을 개선 하는 데 도움이 되는 macOS Sierra의 보안과 개인 정보에 대해 몇 가지 향상 된 기능을 만들었습니다.

- 앱의 `NSAllowsArbitraryLoadsInWebContent` 파일에 새 키를 추가할 수 `Info.plist` 있으며, 앱의 나머지 부분에 대해 ATS (Apple Transport Security) 보호를 사용 하는 동안 웹 페이지를 올바르게 로드할 수 있습니다.
- CDSA (Common Data Security Architecture) API는 더 이상 사용 되지 않으며, 비대칭 키를 생성 하려면 SecKey API로 바꾸어야 합니다.
- 모든 SSL/TLS 연결의 경우 이제 RC4 대칭 암호화가 기본적으로 사용 하지 않도록 설정 됩니다. 또한 보안 전송 API는 더 이상 SSLv3을 지원 하지 않으며, 가능한 한 빨리 SHA-1 및 3DES 암호화를 사용 하 여 앱을 중지 하는 것이 좋습니다.
- IOS 10 및 macOS Sierra의 새 클립보드를 사용 하면 사용자가 장치 간에 복사 하 여 붙여 넣을 수 있기 때문에 클립보드를 특정 장치로 제한 하 고 지정 된 지점에서 자동으로 지울 타임 스탬프를 허용 하도록 API가 확장 되었습니다. 또한 이름이 지정 된 pasteboards는 더 이상 유지 되지 않으며 공유 된 대지의 컨테이너와 바꾸어야 합니다.
- 앱에서 사용자의 달력과 같은 보호 된 데이터에 액세스 하는 경우 해당 파일에서 올바른 용도의 문자열 값 키를 사용 하 여 해당 의도를 선언 _해야_ 합니다 `Info.plist` ( `NSCalendarUsageDescription` 달력의 경우).
- Mac 앱 스토어를 통해 제공 되지 않는 개발자 서명 된 앱은 이제 CloudKit, iCloud 키 집합, iCloud 드라이브, 원격 푸시 알림, MapKit 및 VPN 자격을 활용할 수 있습니다.
- 런타임에 런타임 경로를 알 수 없으므로 zip 보관 파일 또는 서명 되지 않은 디스크 이미지에서 코드 서명자 앱과 함께 외부 코드 또는 데이터 배달을 더 이상 지원 하지 않습니다. macOS Sierra

또한 macOS Sierra (이상)에서 실행 되는 앱은 해당 파일에 하나 이상의 개인 정보 키를 입력 하 여 특정 기능 또는 사용자 정보에 액세스 하 여 `Info.plist` 앱에서 액세스 권한을 얻으려고 하는 이유를 명확 하 게 선언 해야 합니다.

MacOS Sierra는 이러한 변경 내용을 iOS 10과 공유 하므로 iOS 10 [보안 및 개인 정보 향상](~/ios/app-fundamentals/security-privacy.md) 가이드를 참조 하 여 자세한 내용을 확인 하세요.

<a name="Smart-Card-Driver-Extension-Support"></a>

### <a name="smart-card-driver-extension-support"></a>스마트 카드 드라이버 확장 지원

MacOS Sierra를 사용 하 여 앱은 `NSExtension` 특정 유형의 스마트 카드에서 콘텐츠에 대 한 읽기 전용 액세스를 허용 하는 기반 스마트 카드 드라이버를 만들 수 있습니다. 이 정보는 사용 되지 않는 Common Data Security Architecture 메서드를 대체 하는 시스템 키 집합 안에 표시 됩니다.

자세한 내용은 Apple의 [Cryptotokenkit 프레임 워크 참조](https://developer.apple.com/reference/cryptotokenkit)를 참조 하세요.

<a name="Unified-Logging"></a>

### <a name="unified-logging"></a>통합 로깅

통합 로깅은 모든 시스템 수준에서 효율적인 메시징을 위한 단일 API를 앱에 제공 합니다. 통합 로깅을 사용 하 여 앱은 더 쉽게 디버깅 하기 위해 개인 정보 제어 및 활동 추적을 포함 하는 여러 수준의 로깅에 대해 세분화 된 제어를 제공 합니다. 

작업 추적 및 로깅이 함께 사용 되는 경우 로깅은 자동 메시지 상관 관계를 제공 합니다.

macOS Sierra에는 연결 된 장치를 비롯 한 여러 원본의 로그 데이터를 표시할 수 있는 새로운 콘솔 앱 (응용 프로그램/유틸리티)이 포함 되어 있습니다. 또한 토큰화 및 저장 된 검색을 지원 하 고 여러 프로세스에서 관련 메시지 간의 연결을 표시 합니다.

또한 명령줄 도구를 사용 하 여 로그 메시지를 보고 관리할 수 있습니다.

자세한 내용은 Apple의 [로깅 참조](https://developer.apple.com/documentation/os/logging)를 참조 하세요.

<a name="Wide-Color"></a>

### <a name="wide-color"></a>와이드 컬러

macOS Sierra는 핵심 그래픽, 핵심 이미지, 금속 및 AVFoundation과 같은 프레임 워크를 포함 하 여 시스템 전체의 확장 범위 픽셀 형식 및 넓은 색 영역 색 공간에 대 한 지원을 확장 합니다. 넓은 색 표시를 사용 하는 장치에 대 한 지원은 전체 그래픽 스택에이 동작을 제공 하 여 추가로 줄어들 됩니다.

또한는 `AppKit` 새로운 확장 된 **sRGB** colorspace에서 작동 하도록 수정 되어 상당한 성능 손실 없이 광범위 한 색 gamuts 색을 더 쉽게 혼합할 수 있게 되었습니다.

Apple은 넓은 색으로 작업할 때 다음과 같은 모범 사례를 제공 합니다.

- `NSColor`이제는 sRGB 색 공간을 사용 하며이 값은 더 이상 to 범위로 값을 클램프 하지 않습니다 `0.0` `1.0` . 앱이 이전 클램프 동작에 의존 하는 경우 macOS Sierra에 대해 수정 해야 합니다.
- 핵심 그래픽 또는 금속과 같은 하위 수준 API를 사용 하 여 이미지 처리를 제공 하는 경우 앱은 16 비트 부동 소수점 값을 지 원하는 확장 된 범위 색 공간과 픽셀 형식을 사용 해야 합니다. 필요한 경우 앱은 색 구성 요소 값을 수동으로 클램프 해야 합니다.
- 핵심 그래픽, 핵심 이미지 및 금속 성능 셰이더는 모두 두 색상 공간 간을 변환 하기 위한 새로운 방법을 제공 합니다.

자세히 알아보려면 [Wide Color 소개](~/ios/platform/wide-color.md) 가이드를 참조 하세요.

<a name="Additional-Framework-Changes"></a>

## <a name="additional-framework-changes"></a>추가 프레임 워크 변경

Apple은 위에 나열 된 주요 프레임 워크 변경 및 추가 기능 외에도 macOS Sierra에서 많은 추가 사소한 프레임 워크 변경 사항을 만들었습니다.

자세히 알아보려면 [추가 프레임 워크 변경](~/mac/platform/introduction-to-macos-sierra/additional-framework-changes.md) 가이드를 참조 하세요.

<a name="Deprecated-APIs"></a>

## <a name="deprecated-apis"></a>사용되지 않는 API

다음 Api는 macOS Sierra에서 더 이상 사용 되지 않습니다.

- HFS 표준 파일 시스템은 더 이상 지원 되지 않습니다.

결함 및 변경 사항의 전체 목록은 Apple의 [Macos v 10.12 API 차이](https://developer.apple.com/library/archive/releasenotes/General/APIDiffsMacOS10_12/index.html) 설명서를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [Mac 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Mac)
- [MacOS 10.12의 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
