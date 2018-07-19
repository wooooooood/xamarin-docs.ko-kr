---
title: MacOS 시에라 소개
description: 이 문서 Xamarin.Mac 개발자를 위한 모든 새로운 기능과 수정 된 Api 및 시에라 macOS에서 사용할 수 있는 기능을 소개합니다.
ms.prod: xamarin
ms.assetid: 71A8A737-F310-4320-BD23-743AA1E9033C
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 3b8211e4c38fd37040fab5b35be4709d4b926c91
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31044982"
---
# <a name="introduction-to-macos-sierra"></a>MacOS 시에라 소개

새 macOS 시에라 개발자 활용할 수 있습니다의 최종 사용자가 이전에 사용할 수 없는 같은 방법으로 해당 앱 및 웹 사이트와 상호 작용 하도록 허용 하는 새 Api. 예를 들어, 사과 이제 수 있습니다는 응용 프로그램의 그래픽 Apple Pay 및 금속 프레임 워크 높임에 향상 된 기능을 통해 안전 하 게 지불 하 고 컴퓨팅를 고객에 대 한 웹 사이트를 잠재적. 

대 한 자세한 내용은 macOS 시에라, Apple의를 참조 하십시오 [macOS + 앱](https://developer.apple.com/macos/) 설명서입니다.

<a name="Whats-New-in-macOS-Sierra" />

## <a name="whats-new-in-macos-sierra"></a>시에라 macOS에서 새로운 이란

Apple이 macOS 시에라 포함 하 여 기존 기능에는 많은 향상 된 구성 요소가에서 몇 가지 새로운 Api 및 서비스를 추가 합니다.

<a name="Apple-File-System" />

### <a name="apple-file-system"></a>Apple 파일 시스템

시에라 macOS Apple iOS, macOS, tvOS 및 watchOS 최신 파일 시스템으로 새로운 Apple 파일 시스템을 출시 했습니다. Apple 파일 시스템 플래시와 SSD 저장소에 맞게 최적화 된 및 다음과 같은 기능 제공: 공유, 파일 및 디렉터리, 스냅숏, 빠른 디렉터리 크기 조정 및 원자성 저장 안전한 기본 형식에 대 한 복제 간격을 강력한 암호화를 쓰기 시 복사 메타 데이터입니다.

자세한 내용은 Apple의를 참조 하십시오 [Apple 파일 시스템 설명서](https://developer.apple.com/library/prerelease/content/documentation/FileManagement/Conceptual/APFS_Guide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40016999)합니다.

<a name="Apple-Pay-Enhancements" />

### <a name="apple-pay-enhancements"></a>Apple Pay 향상 된 기능

Apple 웹 사이트의 보안 지불 확인을 사용할 수 있는 시에라 macOS에서 Apple 비용을 지불 하려면 여러 개선 된 기능을 했습니다.

시에라 macOS와 여러 가지 새로운 Api가 macOS 시에라, iOS 및 watchOS 동적 지불 네트워크와 새 샌드박스 테스트 환경을 지원 하기 위해 작동 하는 추가 되었습니다.

macOS 시에라 Apple Pay iOS와 macOS Safari 기반 웹 사이트에 직접 통합 하는 개발자는 새 ApplePay Javascript 프레임 워크를 포함 합니다. Apple Pay 지원 웹 사이트에 대 한 사용자가 iPhone 또는 Apple Watch 사용 하 여 지불을 인증할 수 있습니다.

자세한 내용은 Apple의를 참조 하십시오 [ApplePay JS 프레임 워크](https://developer.apple.com/reference/applepayjs) 참조 합니다.

<a name="Building-Modern-macOS-Apps" />

### <a name="building-modern-macos-apps"></a>최신 macOS 앱 빌드

Apple Safari 웹 브라우저, 페이지 워드 프로세서 및 숫자 스프레드 시트 사용에서는 부동 패널 같은 기존의 UI 요소와 통합 된 상황에 맞는 사용자 인터페이스를 제공 하는 많은 새로운 기술 및 여러 열과 같은 최신 macOS 앱 창입니다.

[![탭된 창 Mac의 예](images/content08.png)](images/content08.png#lightbox)

우리의 [건물 최신 macOS 앱](~/mac/platform/introduction-to-macos-sierra/modern-cocoa-apps.md) 가이드에서는 몇 가지 팁, 기능 및 기술 Xamarin.Mac에 최신 macOS 앱을 빌드하기 위해 개발자가 사용할 수 있습니다.

<a name="CloudKit-Data-Sharing" />

### <a name="cloudkit-data-sharing"></a>CloudKit 데이터 공유

사용자 레코드 또는 개인 iCloud 데이터베이스에서 레코드 집합 빠르고 쉽게 공유할 수 있도록 시에라 macOS에서 CloudKit 프레임 워크 확장 되었습니다.

사용자에 게 레코드에 대 한 액세스 권한이 있는 사람에 대 한 전체 읽기/쓰기 제어 및 CloudKit 보내고 레코드 공유 초대를 수락 하는 것에 대 한 전체 UI를 제공 합니다.

자세한 내용은 Apple의를 참조 하십시오 [CloudKit 프레임 워크 참조](https://developer.apple.com/reference/clockkit) 및 [CloudKit JS 프레임 워크 참조](https://developer.apple.com/reference/cloudkitjs)합니다.

> [!IMPORTANT]
> Apple [도구 제공](https://developer.apple.com/support/allowing-users-to-manage-data/) 제대로 유럽 연합 일반 데이터 보호 규정 (GDPR)를 처리 하는 개발자가 수 있도록 합니다.

<a name="Safari-App-Extensions-Support" />

### <a name="safari-app-extensions-support"></a>Safari 응용 프로그램 확장 지원

Safari 응용 프로그램 확장 macOS 시에라과 긴밀 하 게 통합 되는 동안 Safari 웹 브라우저의 동작을 확장 하는 앱을 사용 합니다. MacOS Safari 앱 확장이 iOS Safari 응용 프로그램 확장 비슷하게 작동, 이후 서로 쉽게 포트를 다른 시스템 간에입니다.

자세한 내용은 Apple의를 참조 하십시오 [Safari App 확장 프로그래밍 가이드](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternetWeb/Conceptual/SafariAppExtension_PG/index.html#//apple_ref/doc/uid/TP40017319)합니다.

<a name="Security-and-Privacy-Enhancements" />

### <a name="security-and-privacy-enhancements"></a>보안 및 개인 정보 보호 향상 된 기능

Apple은 보안 및 응용 프로그램의 보안을 향상 하 고 다음을 포함 하는 최종 사용자의 개인 정보 보호는 응용 프로그램에 도움이 되는 시에라 macOS에서 개인 정보를 모두에 몇 가지 향상 되었습니다.

- 새 `NSAllowsArbitraryLoadsInWebContent` 는 앱에 키를 추가할 수 `Info.plist` 파일을 웹 페이지 응용 프로그램의 나머지 부분에 대해 아직 사용 되 고 Apple 전송 보안 (AT) 보호 하는 동안 올바르게 로드를 허용 합니다.
- 일반적인 데이터 보안 아키텍처 (CDSA) API 사용 되지 않으며 비대칭 키를 생성 하는 SecKey API로 대체 해야 합니다.
- 모든 SSL/TLS 연결에 대 한 RC4 대칭 암호화는 이제 기본적으로 비활성화 됩니다. 또한 보안 전송 API는 더 이상 SSLv3 지원 있고 응용 프로그램 s h A-1과 3DES 암호화를 사용 하 여 가능한 한 빨리 중지 하는 것이 좋습니다.
- IOS 10와 macOS에서 새 클립보드 시에라를 허용 하므로 사용자를 장치 간에 복사 및 붙여넣을 API는 특정 장치로 제한 되 고 지정된 된 지점에서 자동으로 삭제 된 타임 스탬프를 지정 하는 클립보드를 허용 하도록 확장 되었습니다. 또한 명명 된 대지의 오브젝트 더 이상 유지 되는 공유 임시 보드 컨테이너와 교체 해야 합니다.
- 앱에서 보호 된 데이터 (예: 사용자의 일정의 경우)에 액세스 하는 경우 그 _해야_ 올바른 용도 문자열 값 키와 해당 의도 선언 해당 `Info.plist` 파일 (`NSCalendarUsageDescription` 일정의 경우).
- Mac 앱 스토어를 통해 배달 되지 않는 개발자 서명 됨 앱 수 이제 활용 CloudKit, 키 집합 iCloud, iCloud 드라이브, 원격 푸시 알림, MapKit 및 VPN 자격.
- macOS 시에라 더 이상 지원 하지 런타임 경로 런타임 전에 알 수 없는 대로 외부 코드 또는 서명 되지 않은 디스크 이미지 또는 zip 보관 파일에 코드 서명자 앱과 함께 데이터를 제공 합니다.

시에라 macOS에서 실행 되는 앱 또한 (또는 이상)에서 하나 이상의 개인 정보 보호 특정 키를 입력 하 여 특정 기능 또는 사용자 정보에 액세스 하는 기능의 의도 정적으로 선언 해야 합니다는 `Info.plist` 응용 프로그램 권한을 얻으려고 이유 않고자 한다면 사용자에 설명 하는 파일 액세스 합니다.

Ios 10 이러한 변경 내용을 공유 하는 macOS 시에라, 하므로 iOS 10을 참조 하십시오 [보안 및 개인 정보 보호 향상 된 기능](~/ios/app-fundamentals/security-privacy.md) 자세한 정보에 대 한 가이드입니다.

<a name="Smart-Card-Driver-Extension-Support" />

### <a name="smart-card-driver-extension-support"></a>스마트 카드 드라이버 확장 프로그램별 지원 기능

응용 프로그램 시에라 macOS를 사용 하 여 만들 수 `NSExtension` 스마트 카드의 특정 형식에서 콘텐츠를 읽기 전용 액세스를 허용 하는 스마트 카드 드라이버를 기반으로 합니다. 이 정보 (사용 되지 않는 공통 데이터 보안 아키텍처 메서드 교체) 시스템 키 집합 내 제공 됩니다.

자세한 내용은, Pleas 참조 Apple의 [CryptoTokenKit 프레임 워크 참조](https://developer.apple.com/reference/cryptotokenkit)합니다.

<a name="Unified-Logging" />

### <a name="unified-logging"></a>통합 된 로깅

통합된 로깅 시스템의 모든 수준에서 효율적인 메시징에 대 한 응용 프로그램을 단일 API 제공 합니다. 통합 로깅을 사용 하 여 응용 프로그램에는 개인 정보 관리 및 활동 보다 쉽게 디버깅을 위해 추적을 포함 하는 여러 로깅 수준에 대해 세분화 된 제어를 있습니다. 

로깅 작업 추적 및 로깅 함께 사용 되는 경우 자동 메시지 상관 관계를 제공 합니다.

macOS 시에라 (응용 프로그램/유틸리티)에 연결 된 장치를 포함 하 여 여러 원본에서 로그 데이터를 표시할 수 있는 새 콘솔 앱을 포함 합니다. 또한 토큰화 된 및 저장 된 검색을 지원 하 고 여러 프로세스에서 관련된 메시지 간의 연결을 표시 합니다.

또한 로그 메시지를 볼 수 있으며 명령줄 도구를 사용 하 여 유지 관리 합니다.

자세한 내용은 Apple의를 참조 하십시오 [로깅 참조](https://developer.apple.com/reference/os/1891852-logging)합니다.

<a name="Wide-Color" />

### <a name="wide-color"></a>광범위 한 색

macOS 시에라 확장 범위 픽셀 형식 및 코어 그래픽, Core 이미지, 금속 및 AVFoundation 등의 프레임 워크를 비롯 하 여 시스템 전체에서 광범위 한 색상 공간에 대 한 지원을 확장 합니다. 전체 전체 그래픽 스택에서이 동작을 제공 하 여 광범위 한 색 표시를 사용 하 여 장치에 대 한 지원은 쉽게 할 추가 합니다.

또한 `AppKit` 수정 된 새에서 작동 하도록 확장 **sRGB** 상당한 성능 손실 없이 색상 광범위 한 색 영역에서 색을 혼합를 더욱 쉽게 색상 공간입니다.

Apple에서는 와이드 색으로 작업 하는 경우 다음 모범 사례를 제공 합니다.

- `NSColor` 이제는 sRGB 색 공간을 더 이상 사용 하 여 값을 clamp는 `0.0` 를 `1.0` 범위입니다. 응용 프로그램 이전 제한 동작에 의존 하는 경우 macOS 시에라에 맞게 수정 해야 합니다.
- 코어 그래픽, 금속 등의 하위 수준 API를 사용 하 여 제공 이미지 처리를 응용 프로그램에는 확장된 범위가 색 공간 및 픽셀 형식으로 16 비트 부동 소수점 값을 지 원하는 사용 해야 합니다. 필요한 경우 응용 프로그램 색상 구성 요소 값을 수동으로 clamp 해야 합니다.
- 코어 그래픽, Core 이미지 및 금속 성능 셰이더 모두 두 개의 색 공간 형식 사이의 변환에 대 한 새로운 메서드를 제공 합니다.

자세한 내용은 참조 하세요 우리의 [광범위 한 색 소개](~/ios/platform/wide-color.md) 가이드입니다.

<a name="Additional-Framework-Changes" />

## <a name="additional-framework-changes"></a>추가 프레임 워크 변경 내용

주요 framework 변경 내용 및 위에 나열 된 추가 기능 뿐만 아니라 Apple 시에라 macOS에서 많은 추가 부 프레임 워크 변경을 했습니다.

자세한 내용은 참조 하세요 우리의 [추가 프레임 워크 변경](~/mac/platform/introduction-to-macos-sierra/additional-framework-changes.md) 가이드입니다.

<a name="Deprecated-APIs" />

## <a name="deprecated-apis"></a>사용되지 않는 API

다음 Api에서에서 사용 되지 macOS 시에라:

- HFS 표준 파일 시스템은 더 이상 지원 됩니다.

Apple의 참조 [OS X API Diffs v10.12](https://developer.apple.com/library/prerelease/content/releasenotes/Miscellaneous/APIDiffsMacOS10_12/index.html) 결함 및 변경의 전체 목록은 대 한 설명서입니다.

## <a name="related-links"></a>관련 링크

- [Mac 샘플](https://developer.xamarin.com/samples/mac/)
- [OS X 10.12의 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
