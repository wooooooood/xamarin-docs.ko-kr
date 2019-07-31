---
title: iOS 보안 및 개인 정보 기능
description: 이 문서에서는 iOS의 보안과 개인 정보 보호 기능에 대해 설명 하 고 Xamarin.ios와 함께 사용 하는 방법을 설명 합니다. IOS 10에서 수행 된 업데이트와 개인 사용자 데이터에 액세스 하는 방법을 살펴봅니다.
ms.prod: xamarin
ms.assetid: 718C8721-C359-4650-878A-D68E159A3F53
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 9fcd4820b5e22254356250ef2d26714dc32a59f4
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655216"
---
# <a name="ios-security-and-privacy-features"></a>iOS 보안 및 개인 정보 기능

_이 문서에서는 iOS에서 보안 및 개인 정보를 사용 하는 방법과 Xamarin.ios 앱에 미치는 영향에 대해 설명 합니다._

Apple에서는 개발자가 앱의 보안을 개선 하 고 최종 사용자의 개인 정보를 확인 하는 데 도움이 되는 iOS 10의 보안 및 개인 정보 모두에 대해 몇 가지 기능이 향상 되었습니다. 이 문서에서는 Xamarin.ios 앱에서 이러한 기능을 구현 하는 방법을 다룹니다.
    
<a name="General-Enhancements" />

## <a name="general-enhancements"></a>일반적인 향상 된 기능

IOS 10의 보안 및 개인 정보에 대 한 다음과 같은 일반적인 변경 내용이 적용 되었습니다.

- CDSA (Common Data Security Architecture) API는 더 이상 사용 되지 않으며, 비대칭 키를 생성 하려면 SecKey API로 바꾸어야 합니다.
- 앱의 `NSAllowsArbitraryLoadsInWebContent` **info.plist** 파일에 새 키를 추가할 수 있으며, 응용 프로그램의 나머지 부분에 대해 ATS (Apple Transport Security) 보호를 사용 하는 동안 웹 페이지를 올바르게 로드할 수 있습니다. 자세한 내용은 [앱 전송 보안](~/ios/app-fundamentals/ats.md) 설명서를 참조 하세요.
- IOS 10 및 macOS Sierra의 새 클립보드를 사용 하면 사용자가 장치 간에 복사 하 여 붙여 넣을 수 있기 때문에 클립보드를 특정 장치로 제한 하 고 지정 된 지점에서 자동으로 지울 타임 스탬프를 허용 하도록 API가 확장 되었습니다. 또한 이름이 지정 된 pasteboards는 더 이상 유지 되지 않으며 공유 된 대지의 컨테이너와 바꾸어야 합니다.
- 모든 SSL/TLS 연결의 경우 이제 RC4 대칭 암호화가 기본적으로 사용 하지 않도록 설정 됩니다. 또한 보안 전송 API는 더 이상 SSLv3을 지원 하지 않으므로 개발자는 가능한 한 빨리 SHA-1 및 3DES 암호화를 사용 하지 않는 것이 좋습니다.

<a name="Accessing-Private-User-Data" />

## <a name="accessing-private-user-data"></a>개인 사용자 데이터 액세스

IOS 10 이상에서 실행 되는 앱은 info.plist 파일에 하나 이상의 개인 정보 보호 키를 입력 하 여 특정 기능 또는 사용자 정보에 액세스 하는 의도를 정적으로 선언 해야 합니다 **.**

> [!IMPORTANT]
> 필요한 키를 제공 하지 못하는 앱은 _오류 없이_제한 된 기능 또는 사용자 정보 중 하나에 액세스 하려고 할 때 시스템에 의해 자동으로 종료 됩니다. IOS 10에서 예기치 않게 앱이 시작 되는 경우 필요한 **info.plist** 를 모두 지정 했는지 확인 합니다.

다음 개인 정보 관련 키를 사용할 수 있습니다.

- **개인 정보-Apple Music 사용 설명** (`NSAppleMusicUsageDescription`)-개발자가 앱이 사용자의 미디어 라이브러리에 액세스 하려는 이유를 설명할 수 있습니다.
- **개인 정보-Bluetooth 주변 장치 사용 설명** (`NSBluetoothPeripheralUsageDescription`)-개발자가 앱이 사용자 장치에서 Bluetooth에 액세스 하려는 이유를 설명할 수 있습니다.
- **개인 정보-일정 사용 설명** (`NSCalendarsUsageDescription`)-개발자가 앱이 사용자의 일정에 액세스 하려는 이유를 설명할 수 있습니다.
- **개인 정보-카메라 사용 설명** (`NSCameraUsageDescription`)-개발자가 장치 카메라에 액세스 하려는 이유를 설명할 수 있습니다.
- **개인 정보-연락처 사용 설명** (`NSContactsUsageDescription`)-개발자가 앱이 사용자의 연락처에 액세스 하려는 이유를 설명할 수 있습니다.
- **개인 정보-상태 공유 사용 설명** (`NSHealthShareUsageDescription`)-개발자가 앱이 사용자의 상태 데이터에 액세스 하려는 이유를 설명할 수 있습니다. 자세한 내용은 Apple의 [HKHealthStore 클래스 참조](https://developer.apple.com/reference/healthkit/hkhealthstore)를 참조 하세요.
- **개인 정보-상태 업데이트 사용 설명** (`NSHealthUpdateUsageDescription`)-개발자가 앱에서 사용자의 상태 데이터를 편집 하려는 이유를 설명할 수 있습니다. 자세한 내용은 Apple의 [HKHealthStore 클래스 참조](https://developer.apple.com/reference/healthkit/hkhealthstore)를 참조 하세요.
- **개인 정보-HomeKit 사용 설명** (`NSHomeKitUsageDescription`)-개발자가 앱이 사용자의 HomeKit 구성 데이터에 액세스 하려는 이유를 설명할 수 있습니다.
- **개인 정보-위치 항상 사용 설명** (`NSLocationAlwaysUsageDescription`)-개발자가 앱에서 항상 사용자의 위치에 액세스할 수 있는 이유를 설명할 수 있습니다.
- Mapi **개인 정보-위치 사용 설명** (`NSLocationUsageDescription`)-개발자가 앱이 사용자 위치에 액세스 하려는 이유를 설명할 수 있습니다. *두고 이 키는 iOS 8에서 더 이상 사용 되지 않습니다. 대신 `NSLocationAlwaysUsageDescription` 또는`NSLocationWhenInUseUsageDescription` 를 사용 합니다.*
- **개인 정보-위치 사용 시 사용 설명** (`NSLocationWhenInUseUsageDescription`)-개발자가 앱이 실행 되는 동안 사용자의 위치에 액세스 하려는 이유를 설명할 수 있습니다.
- Mapi **개인 정보-미디어 라이브러리 사용 설명** -개발자가 앱이 사용자의 미디어 라이브러리에 액세스 하려는 이유를 설명할 수 있습니다. *두고 이 키는 iOS 8에서 더 이상 사용 되지 않습니다. 대신 `NSAppleMusicUsageDescription` 를 사용 합니다.*
- **개인 정보-마이크 사용 설명** (`NSMicrophoneUsageDescription`)-개발자가 앱에서 장치 마이크에 액세스 하려는 이유를 설명할 수 있습니다.
- **개인 정보-동작 사용 설명** (`NSMotionUsageDescription`)-개발자는 앱이 장치의가 속도계에 액세스 하려는 이유를 설명할 수 있습니다.
- **개인 정보-사진 라이브러리 사용 설명** (`NSPhotoLibraryUsageDescription`)-개발자가 앱에서 사용자의 사진 라이브러리에 액세스 하려는 이유를 설명할 수 있습니다.
- **개인 정보-미리 알림 사용 설명** (`NSRemindersUsageDescription`)-개발자가 앱에서 사용자의 미리 알림 액세스를 원하는 이유를 설명할 수 있습니다.
- **개인 정보-Siri 사용 설명** (`NSSiriUsageDescription`)-개발자가 앱에서 siri로 사용자 데이터를 전송 하려는 이유를 설명할 수 있습니다.
- **개인 정보-음성 인식 사용 설명** (`NSSpeechRecognitionUsageDescription`)-개발자가 앱에서 Apple의 음성 인식 서버로 사용자 데이터를 전송 하려는 이유를 설명할 수 있습니다.
- **개인 정보-TV 공급자 사용 설명** (`NSVideoSubscriberAccountUsageDescription`)-개발자가 앱이 사용자의 TV 공급자 계정에 액세스 하려는 이유를 설명할 수 있습니다.

**Info.plist** 키를 사용 하는 방법에 대 한 자세한 내용은 Apple의 [정보 속성 목록 키 참조](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009248-SW1)를 참조 하세요.

<a name="Setting-Privacy-Keys" />

## <a name="setting-privacy-keys"></a>개인 정보 키 설정

IOS 10 (이상)에서 HomeKit에 액세스 하는 다음 예제를 사용 하 여 개발자는 앱의 `NSHomeKitUsageDescription` **info.plist** 파일에 키를 추가 하 고 앱이 사용자의 HomeKit 데이터베이스에 액세스 하는 이유를 선언 하는 문자열을 제공 해야 합니다. 이 문자열은 앱을 처음 실행할 때 사용자에 게 표시 됩니다.

![예제 NSHomeKitUsageDescription 경고](security-privacy-images/info01.png "예제 NSHomeKitUsageDescription 경고")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio 용 xamarin.ios는 현재 기본 iOS 매니페스트 편집기 내에서 **info.plist** 개인 정보 보호 키 편집을 지원 하지 않습니다. 대신 일반 Info.plist 편집기를 사용 해야 하므로 다음을 수행 해야 합니다.

1. **솔루션 탐색기** 에서 **info.plist** 파일을 마우스 오른쪽 단추로 클릭 하 고 **연결 프로그램 ...** 을 선택 합니다.
2. 프로그램 목록에서 **일반 Info.plist 편집기** 를 선택 하 여 파일을 연 다음 **확인**을 클릭 합니다.

    ![일반 Info.plist 편집기를 선택 합니다] . (security-privacy-images/InfoEditorSelectionVs.png "일반 Info.plist 편집기를 선택 합니다") .
3. 편집기의 마지막 행에 있는 **단추를클릭하여목록에새항목을추가합니다.+** 이를 "사용자 지정 속성" 이라고 하며, 유형은로 `String` 설정 되 고 빈 값으로 설정 됩니다.
4. 속성 이름을 클릭 하면 드롭다운이 표시 됩니다.
5. 드롭다운 목록에서 개인 정보 보호 키 (예: **개인 정보-HomeKit 사용 설명**)를 선택 합니다. 

    ![개인 정보 키 선택](security-privacy-images/InfoPListEditorSelectKey.png "개인 정보 키 선택")
6. 앱이 지정 된 기능 또는 사용자 정보에 액세스 하려는 이유에 대 한 설명을 값 열에 입력 합니다. 

    ![설명 입력](security-privacy-images/InfoPListSetValue.png "설명 입력")
7. 파일의 변경 내용을 저장합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

개인 정보 보호 키를 설정 하려면 다음을 수행 합니다.

1. 편집하기 위해 **솔루션 탐색기**에서 **Info.plist** 파일을 두 번 클릭하여 엽니다.
2. 화면 아래쪽에서 **원본** 뷰로 전환 합니다.
3. 목록에 새 **항목** 을 추가 합니다.
4. 드롭다운 목록에서 개인 정보 보호 키 (예: **개인 정보-HomeKit 사용 설명**)를 선택 합니다. 

    ![개인 정보 키 선택](security-privacy-images/info02.png "개인 정보 키 선택")
5. 앱이 지정 된 기능 또는 사용자 정보에 액세스 하려는 이유에 대 한 설명을 입력 합니다. 

    ![설명 입력](security-privacy-images/info03.png "설명 입력")
6. 파일의 변경 내용을 저장합니다.

-----

> [!IMPORTANT]
> 위의 예에서 `NSHomeKitUsageDescription` **info.plist** 파일에 키를 설정 하지 않으면 iOS 10 이상에서 실행 될 때 오류 없이 앱이 _자동으로 실패_ 하 게 됩니다 (런타임에 시스템에서 닫힘).

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Apple에서 iOS 10에 적용 한 보안 및 개인 정보 변경 내용과 Xamarin.ios 앱에 미치는 영향에 대해 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
