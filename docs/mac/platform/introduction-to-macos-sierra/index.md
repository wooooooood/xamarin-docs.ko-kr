---
title: MacOS Sierra 소개
description: 이 문서에서는 Xamarin.Mac 개발자를 위한 모든 새로운 기능과 수정 된 Api 및 macOS Sierra에서에서 사용할 수 있는 기능을 소개합니다.
ms.prod: xamarin
ms.assetid: 71A8A737-F310-4320-BD23-743AA1E9033C
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 5a944fd8f7dcfdcbb3f025c92b4afac35673416f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50111014"
---
# <a name="introduction-to-macos-sierra"></a>MacOS Sierra 소개

개발자는 새 macOS Sierra 사용 하 여 최종 사용자가 해당 앱 및 웹 사이트를 사용 하 여 이전에 사용할 수 없는 방법으로 상호 작용할 수 있도록 새로운 Api 활용을 걸릴 수 있습니다. 예를 들어 Apple 이제 허용 컴퓨팅 및 그래픽 앱의 Apple Pay 및 금속 프레임 워크 향상의 향상 된 기능을 통해 안전 하 게 지불 옵션을 제공 하기 위해 웹 사이트 잠재적인 합니다. 

MacOS Sierra에 대 한 자세한 내용은 Apple의를 참조 하세요 [macOS + 앱](https://developer.apple.com/macos/) 설명서.

<a name="Whats-New-in-macOS-Sierra" />

## <a name="whats-new-in-macos-sierra"></a>MacOS Sierra에서 새 란

Apple이 Api 및 서비스에 대 한 몇 가지 새 포함 하 여 기존 기능에 많은 향상 된 macOS Sierra에에서 추가 합니다.

<a name="Apple-File-System" />

### <a name="apple-file-system"></a>Apple 파일 시스템

Macos Sierra, Apple iOS, macOS, tvOS 및 watchOS에 대 한 최신 파일 시스템으로 새 Apple 파일 시스템을 출시 했습니다. Apple 파일 시스템 플래시와 SSD 저장소에 최적화 된 및 다음 기능을 제공 합니다: 강력한 암호화를 기록 중 복사 메타 데이터를 공간 공유, 파일 및 디렉터리, 스냅숏, 빠른 디렉터리 크기 조정 및 원자성 안전한 저장 기본 형식에 대 한 복제 합니다.

자세한 내용은 Apple의를 참조 하세요 [Apple 파일 시스템 설명서](https://developer.apple.com/library/prerelease/content/documentation/FileManagement/Conceptual/APFS_Guide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40016999)합니다.

<a name="Apple-Pay-Enhancements" />

### <a name="apple-pay-enhancements"></a>Apple Pay 향상 된 기능

Apple 웹 사이트의 보안 지불 하는 사용자를 허용 하는 macOS Sierra에서에서 Apple Pay에 몇 가지 향상 된 기능을 했습니다.

MacOS Sierra 사용 하 여 몇 가지 새로운 Api가 macOS Sierra, iOS 및 watchOS 동적 결제 네트워크 및 새 샌드박스 테스트 환경을 지원 하기 위해 사용 되는 추가 되었습니다.

macOS Sierra 개발자가 iOS 및 macOS Safari 기반 웹 사이트에 직접 Apple Pay를 통합할 수 있도록 새 ApplePay Javascript 프레임 워크를 포함 합니다. Apple Pay 지 웹 사이트에 대 한 사용자에 해당 iPhone 또는 Apple Watch 사용 하 여 지불이 권한을 부여할 수 있습니다.

자세한 내용은 Apple의를 참조 하세요 [ApplePay JS 프레임 워크](https://developer.apple.com/reference/applepayjs) 참조 합니다.

<a name="Building-Modern-macOS-Apps" />

### <a name="building-modern-macos-apps"></a>최신 macOS 앱 빌드

Apple Safari 웹 브라우저, 페이지 워드 프로세서 및 숫자 스프레드 시트 사용 떨어진 부동 패널 같은 일반적인 UI 요소를 사용 하 여 수행 하는 통합, 상황에 맞는 사용자 인터페이스를 제공 하는 많은 새로운 기술 및 여러 열과 같은 최신 macOS 앱 windows입니다.

[![탭 Mac 창의 예](images/content08.png)](images/content08.png#lightbox)

우리의 [건물 최신 macOS 앱](~/mac/platform/introduction-to-macos-sierra/modern-cocoa-apps.md) 가이드에서는 몇 가지 팁, 기능 및 기술을 xamarin.mac에서 최신 macOS 앱을 빌드하는 개발자를 사용할 수 있습니다.

<a name="CloudKit-Data-Sharing" />

### <a name="cloudkit-data-sharing"></a>CloudKit 데이터 공유

CloudKit 프레임 워크에서 macOS Sierra 사용자 레코드 또는 개인 iCloud 데이터베이스에서 레코드 집합 빠르고 쉽게 공유할 수 있도록 확장 되었습니다.

사용자가 레코드에 대 한 액세스 권한이 있는 사람에 대 한 전체 읽기/쓰기 제어 및 CloudKit 보내고 레코드 공유 초대를 수락 하는 것에 대 한 전체 UI를 제공 합니다.

자세한 내용은 Apple의를 참조 하세요 [CloudKit 프레임 워크 참조](https://developer.apple.com/reference/clockkit) 하 고 [CloudKit JS 프레임 워크 참조](https://developer.apple.com/reference/cloudkitjs)합니다.

> [!IMPORTANT]
> Apple에서는 개발자가 유럽 연합의 GDPR(일반 데이터 보호 규정)을 제대로 처리하는 데 도움이 되는 [도구를 제공합니다](https://developer.apple.com/support/allowing-users-to-manage-data/).

<a name="Safari-App-Extensions-Support" />

### <a name="safari-app-extensions-support"></a>Safari 앱 확장 지원

MacOS Sierra 긴밀 하 게 통합 되는 동안 Safari 웹 브라우저의 동작을 확장 하도록 앱을 허용 하는 safari 앱 확장 합니다. MacOS의 Safari 앱 확장 iOS Safari 앱 확장 비슷하게 작동 하므로 쉽기 때문 포트를 다른 시스템에서.

자세한 내용은 Apple의를 참조 하세요 [Safari 앱 확장 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternetWeb/Conceptual/SafariAppExtension_PG/index.html#//apple_ref/doc/uid/TP40017319)합니다.

<a name="Security-and-Privacy-Enhancements" />

### <a name="security-and-privacy-enhancements"></a>보안 및 개인 정보 보호 향상

Apple은 모두 보안 및 개인 정보 보호에 macOS Sierra 앱은 앱의 보안을 개선 하 고 다음과 같은 최종 사용자의 개인 정보 보호에 도움이 되는 몇 가지 향상 되었습니다.

- 새 `NSAllowsArbitraryLoadsInWebContent` 앱의 키를 추가할 수 `Info.plist` 파일과 웹 페이지 앱의 나머지 부분에 대 한 Apple 전송 보안 (ATS) 보호를 사용 하도록 계속 하는 동안 올바르게 로드를 사용 하면 됩니다.
- 일반적인 데이터 보안 아키텍처 (CDSA) API는 사용 되지 않으며 비대칭 키를 생성 하는 SecKey API로 바꿔야 합니다.
- 모든 SSL/TLS 연결에 대해 RC4 대칭 암호화는 이제 기본적으로 비활성화 됩니다. 또한 보안 전송 API SSLv3을 더 이상 지원 및 앱 sha-1 및 3DES 암호화를 사용 하 여 가능한 한 빨리 중지 하는 것이 좋습니다.
- 새 클립보드에 10 iOS 및 macOS Sierra를 허용 하므로 사용자 장치 간에 복사 및 붙여넣기를 API는 클립보드 특정 장치로 제한 되며 타임 스탬프가 적용 된 지정된 된 시점에 자동으로 지울 수 있도록 확장 되었습니다. 또한 명명 된 대지의 오브젝트 더 이상 유지 됩니다 및 임시 보드 공유 컨테이너를 사용 하 여 대체 해야 합니다.
- 앱에서 보호 된 데이터 (예: 사용자의 일정의 경우)에 액세스 하는 경우 해당 _해야 합니다_ 의 올바른 용도 문자열 값 키를 사용 하 여 해당 의도 선언 해당 `Info.plist` 파일 (`NSCalendarUsageDescription` 일정의 경우).
- Mac 앱 스토어를 통해 배달 되지 않는 개발자 서명 앱 CloudKit, iCloud 키 집합, iCloud 드라이브, 원격 푸시 알림, MapKit 및 VPN 자격 활용 이제 수 있습니다.
- macOS Sierra 런타임 경로 런타임 전에 알 수 없는 대로 외부 코드 또는 zip 보관 파일 또는 서명 되지 않은 디스크 이미지 코드 서명자 앱과 함께 데이터를 제공 하는 더 이상 지원 합니다.

MacOS Sierra 실행 하는 앱 또한 (또는 이상)에서 하나 이상의 개인 특정 키를 입력 하 여 특정 기능 또는 사용자 정보에 액세스 하려면 의도 정적으로 선언 해야 합니다. 해당 `Info.plist` 앱을 확보 하고자 하는 이유는 사용자에 게 설명 하는 파일 액세스 합니다.

IOS 10 사용 하 여 이러한 변경 내용을 공유 하는 macOS Sierra를 하므로 iOS 10을 참조 하세요 [보안 및 개인 정보 보호 향상](~/ios/app-fundamentals/security-privacy.md) 자세한 가이드입니다.

<a name="Smart-Card-Driver-Extension-Support" />

### <a name="smart-card-driver-extension-support"></a>스마트 카드 드라이버 확장 지원

MacOS Sierra, 앱을 만들 수 있습니다 `NSExtension` 특정 유형의 스마트 카드에서 콘텐츠를 읽기 전용 액세스를 허용 하는 스마트 카드 드라이버를 기반으로 합니다. 이 정보 (사용 되지 않는 일반적인 데이터 보안 아키텍처 메서드를 대체) 시스템 키 집합 내에서 제공 됩니다.

자세한 내용은 Pleas Apple의 [CryptoTokenKit 프레임 워크 참조](https://developer.apple.com/reference/cryptotokenkit)합니다.

<a name="Unified-Logging" />

### <a name="unified-logging"></a>통합 된 로깅

통합된 로깅 시스템의 모든 수준에서 효율적인 메시징에 대 한 단일 API 사용 하 여 앱을 제공 합니다. 통합 로깅을 사용 하 여 앱 개인 정보 및 활동 쉬운 디버깅에 대 한 추적을 포함 하는 여러 가지 로깅 수준이 세밀 하 게 제어할을 있습니다. 

로깅 추적 및 로깅 작업을 함께 사용 하는 경우 자동 메시지 상관 관계를 제공 합니다.

macOS Sierra (응용 프로그램/유틸리티)에 연결 된 장치를 비롯 한 여러 소스의 로그 데이터를 표시할 수 있는 새 콘솔 앱을 포함 합니다. 또한 토큰화 및 저장 된 검색을 지원 하 고 여러 프로세스에서 관련된 메시지 간의 연결을 표시 합니다.

또한 로그 메시지를 볼 수 있고 명령줄 도구를 사용 하 여 유지 관리 합니다.

자세한 내용은 Apple의를 참조 하세요 [로깅 참조](https://developer.apple.com/reference/os/1891852-logging)합니다.

<a name="Wide-Color" />

### <a name="wide-color"></a>와이드 컬러

macOS Sierra 확장 범위 픽셀 형식 및 AVFoundation 핵심 그래픽, Core 이미지, 금속 등 프레임 워크를 비롯 한 시스템 전체에서 광범위 한 색상 공간에 대 한 지원도 확장 되었습니다. 와이드 컬러 디스플레이 사용 하 여 장치에 대 한 지원은 전체 그래픽 스택 전체에서이 동작을 제공 하 여 용이 추가 됩니다.

또한 `AppKit` 수정 된 새 작업을 확장 **sRGB** 색상 공간을 간편 하 게 중요 한 성능 손실 없이 광범위 한 색 색상의 색을 혼합 합니다.

Apple에서는 다양 한 색을 사용 하 여 작업 하는 경우 다음 모범 사례를 제공 합니다.

- `NSColor` 사용 하는 sRGB 색 공간을 더 이상 고정 값을 이제는 `0.0` 에 `1.0` 범위입니다. 앱을 이전 하는 제한 동작에 의존 하는 경우 macOS Sierra에 대 한 수정 해야 합니다.
- 핵심 그래픽 또는 체제 미 설치 컴퓨터와 같은 하위 수준 API를 사용 하 여 이미지 처리를 앱을 확장된 범위 색 공간과 픽셀 지 원하는 형식으로 16 비트 부동 소수점 값을 사용 해야 합니다. 필요한 경우 앱을 수동으로 색 구성 요소 값을 고정 해야 합니다.
- 핵심 그래픽, Core 이미지 및 체제 미 설치 컴퓨터 성능 셰이더 모든 두 색 공간 변환 하기 위한 새 메서드를 제공 합니다.

자세한 내용을 참조 하세요 우리의 [와이드 컬러 소개](~/ios/platform/wide-color.md) 가이드입니다.

<a name="Additional-Framework-Changes" />

## <a name="additional-framework-changes"></a>프레임 워크 추가 변경 내용

주요 프레임 워크 변경 내용 및 위에 나열 된 추가 외에 Apple macOS Sierra에서에서 많은 사소한 프레임 워크 추가 변경을 했습니다.

자세한 내용을 참조 하세요 우리의 [추가 프레임 워크 변경 내용](~/mac/platform/introduction-to-macos-sierra/additional-framework-changes.md) 가이드입니다.

<a name="Deprecated-APIs" />

## <a name="deprecated-apis"></a>사용되지 않는 API

MacOS Sierra에서에서 다음과 같은 Api가 사용 되지 않습니다.

- HFS 표준 파일 시스템 지원 하지 않습니다.

Apple의를 참조 하세요 [OS X v10.12 API 차이](https://developer.apple.com/library/prerelease/content/releasenotes/Miscellaneous/APIDiffsMacOS10_12/index.html) 결함 및 변경 내용 목록은 설명서.

## <a name="related-links"></a>관련 링크

- [Mac 샘플](https://developer.xamarin.com/samples/mac/)
- [OS X 10.12의 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
