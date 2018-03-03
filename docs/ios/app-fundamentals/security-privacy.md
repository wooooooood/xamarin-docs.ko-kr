---
title: "iOS 보안 및 개인 정보 보호 기능"
description: "이 문서에서는 보안 및 개인 정보 iOS 및 Xamarin.iOS 앱에 미치는 영향에 포함 된 작업을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 718C8721-C359-4650-878A-D68E159A3F53
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: acdcdc2b76a995ca324532c6a034b2fdf8e21db5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="ios-security-and-privacy-features"></a>iOS 보안 및 개인 정보 보호 기능

_이 문서에서는 보안 및 개인 정보 iOS 및 Xamarin.iOS 앱에 미치는 영향에 포함 된 작업을 설명 합니다._

Apple는 개발자가 앱의 보안을 개선 하 고 최종 사용자의 개인 정보 보호에 도움이 되는 보안과 iOS 10 (및 큰)의 개인 정보에 몇 가지 개선 되어를 했습니다. 이 문서에서는 Xamarin.iOS 앱에서 이러한 기능을 구현 설명 합니다.

다음 항목에 대해 자세히 설명합니다.

- [일반 향상 된 기능](#General-Enhancements)
- [개인 사용자 데이터에 액세스](#Accessing-Private-User-Data)
    - [개인 키 설정](#Setting-Privacy-Keys)
    
<a name="General-Enhancements" />

## <a name="general-enhancements"></a>일반 향상 된 기능

IOS 10의에서 보안 및 개인 정보에는 다음과 같은 일반적인 방식으로 변경 적용 했습니다.

- 일반적인 데이터 보안 아키텍처 (CDSA) API 사용 되지 않으며 비대칭 키를 생성 하는 SecKey API로 대체 해야 합니다.
- 새 `NSAllowsArbitraryLoadsInWebContent` 에 키를 추가할 수는 응용 프로그램 `Info.plist` 파일을 웹 페이지 응용 프로그램의 나머지 부분에 대해 아직 사용 되 고 Apple 전송 보안 (AT) 보호 하는 동안 올바르게 로드를 허용 합니다. 자세한 내용은 참조 하십시오 우리의 [앱 전송 보안](~/ios/app-fundamentals/ats.md) 설명서입니다.
- IOS 10와 macOS에서 새 클립보드 시에라를 허용 하므로 사용자를 장치 간에 복사 및 붙여넣을 API는 특정 장치로 제한 되 고 지정된 된 지점에서 자동으로 삭제 된 타임 스탬프를 지정 하는 클립보드를 허용 하도록 확장 되었습니다. 또한 명명 된 대지의 오브젝트 더 이상 유지 되는 공유 임시 보드 컨테이너와 교체 해야 합니다.
- 모든 SSL/TLS 연결에 대 한 RC4 대칭 암호화는 이제 기본적으로 비활성화 됩니다. 또한 보안 전송 API는 더 이상 SSLv3 지원 하 고 개발자는 s h A-1과 3DES 암호화를 사용 하 여 가능한 한 빨리 중지 하는 것이 좋습니다.

<a name="Accessing-Private-User-Data" />

## <a name="accessing-private-user-data"></a>개인 사용자 데이터에 액세스

IOS 10 (또는 이후 버전)에서 실행 되는 앱에 하나 이상의 개인 키를 입력 하 여 특정 기능 또는 사용자 정보에 액세스 하는 기능의 의도 정적으로 선언 해야 자신의 `Info.plist` 사용자 응용 프로그램에 액세스 하려는 이유를 설명 하는 파일입니다.

> [!IMPORTANT]
> **참고** 필요한 키 자동으로 종료 됩니다 시스템에 의해 제한 된 기능이 나 사용자 정보 중 하나에 액세스 하려고 할 때 제공 되지 않은 앱 _오류 없이_! 응용 프로그램 시작 iOS 10에서 예기치 않게 실패 하는 경우 모든 필수 되도록 `Info.plist` 지정 되었습니다.

다음과 같은 개인 정보 관련 키를 사용할 수 있습니다.

- **개인정보 취급 방침-Apple 음악 사용 설명** (`NSAppleMusicUsageDescription`)-응용 프로그램 사용자의 미디어 라이브러리에 액세스 하려는 이유를 설명 하기 위해 개발자가 있습니다.
- **개인정보 취급 방침-Bluetooth 주변 사용 설명을** (`NSBluetoothPeripheralUsageDescription`)-응용 프로그램 사용자의 장치에서 Bluetooth를 액세스 하려고 하는 이유를 설명 하기 위해 개발자가 있습니다.
- **개인정보 취급 방침-달력 사용 설명** (`NSCalendarsUsageDescription`)-응용 프로그램 사용자의 일정에 액세스 하려는 이유를 설명 하기 위해 개발자가 있습니다.
- **개인정보 취급 방침-카메라 사용 설명** (`NSCameraUsageDescription`)-응용 프로그램을 장치의 카메라에 액세스 하려고 하는 이유를 설명 하기 위해 개발자가 있습니다.
- **개인정보 취급 방침-연락처 사용 설명** (`NSContactsUsageDescription`)-응용 프로그램을 사용자의 연락처에 액세스 하려고 하는 이유를 설명 하기 위해 개발자가 있습니다.
- **개인정보 취급 방침-상태 공유 사용 설명** (`NSHealthShareUsageDescription`)-응용 프로그램에서 사용자 상태 데이터에 액세스 하려는 이유를 설명 하기 위해 개발자가 있습니다. 자세한 내용은 Apple의를 참조 하십시오 [HKHealthStore 클래스 참조](https://developer.apple.com/reference/healthkit/hkhealthstore)합니다.
- **개인정보 취급 방침-상태 업데이트 사용 설명** (`NSHealthUpdateUsageDescription`)-응용 프로그램 사용자 상태 데이터를 편집 하려는 이유를 설명 하기 위해 개발자가 있습니다. 자세한 내용은 Apple의를 참조 하십시오 [HKHealthStore 클래스 참조](https://developer.apple.com/reference/healthkit/hkhealthstore)합니다.
- **개인정보 취급 방침-HomeKit 사용 설명** (`NSHomeKitUsageDescription`)-응용 프로그램 사용자의 HomeKit 구성 데이터에 액세스 하려는 이유를 설명 하기 위해 개발자가 있습니다.
- **개인정보 취급 방침-항상 사용 설명 위치** (`NSLocationAlwaysUsageDescription`)-응용 프로그램 사용자의 위치에 권한이 항상 하려는 이유를 설명 하기 위해 개발자가 있습니다.
- [사용 되지 않음] **개인정보 취급 방침-위치 사용 설명** (`NSLocationUsageDescription`)-응용 프로그램 사용자 위치에 액세스 하려는 이유를 설명 하기 위해 개발자가 있습니다. *참고:이 키에서에서 사용 되지 iOS 8 (및 큰). 사용 하 여 `NSLocationAlwaysUsageDescription` 또는 `NSLocationWhenInUseUsageDescription` 대신 합니다.*
- **개인 정보 보호-위치 때에 사용 하 여 사용 설명** (`NSLocationWhenInUseUsageDescription`)-응용 프로그램을 실행 하는 동안 사용자의 위치에 액세스 하려고 하는 이유를 설명 하기 위해 개발자가 있습니다.
- [사용 되지 않음] **개인정보 취급 방침-미디어 라이브러리 사용 설명을** -개발자가 응용 프로그램 사용자의 미디어 라이브러리에 액세스 하려는 이유를 설명 합니다. *참고:이 키에서에서 사용 되지 iOS 8 (및 큰). 사용 하 여 `NSAppleMusicUsageDescription` 대신 합니다.*
- **개인정보 취급 방침-마이크 사용 설명** (`NSMicrophoneUsageDescription`)-응용 프로그램을 장치 마이크에 액세스 하려고 하는 이유를 설명 하기 위해 개발자가 있습니다.
- **개인정보 취급 방침-동작 사용 설명** (`NSMotionUsageDescription`)-응용 프로그램이 속도계 장치의 액세스 하려고 하는 이유를 설명 하기 위해 개발자가 있습니다.
- **개인정보 취급 방침-사진 라이브러리 사용 설명** (`NSPhotoLibraryUsageDescription`)-응용 프로그램 사용자의 사진 라이브러리 액세스 하려는 이유를 설명 하기 위해 개발자가 있습니다.
- **개인정보 취급 방침-알림 사용 설명** (`NSRemindersUsageDescription`)-응용 프로그램을 사용자의 미리 알림을 액세스 하려고 하는 이유를 설명 하기 위해 개발자가 있습니다.
- **개인정보 취급 방침-Siri 사용 설명** (`NSSiriUsageDescription`)-응용 프로그램 Siri를 사용자 데이터를 전송 하려는 이유를 설명 하기 위해 개발자가 있습니다.
- **개인정보 취급 방침-음성 인식을 사용 설명** (`NSSpeechRecognitionUsageDescription`)-Apple의 음성 인식 서버에 사용자 데이터를 보내지 앱 하려고 하는 이유를 설명 하기 위해 개발자가 있습니다.
- **개인정보 취급 방침-TV 공급자 사용 설명** (`NSVideoSubscriberAccountUsageDescription`)-응용 프로그램 사용자의 TV 공급자 계정에 액세스 하려는 이유를 설명 하기 위해 개발자가 있습니다.

작업에 대 한 자세한 내용은 `Info.plist` 키를 참조 하세요. Apple의 [정보 속성 목록 키 참조](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009248-SW1)합니다.

<a name="Setting-Privacy-Keys" />

## <a name="setting-privacy-keys"></a>개인 키 설정

IOS 10 (이상)에서 HomeKit에 액세스 하는 다음 예제에서는, 개발자는를 추가 해야 합니다는 `NSHomeKitUsageDescription` 응용 프로그램의 키 `Info.plist` 파일 및 응용 프로그램 사용자의 HomeKit 데이터베이스에 액세스 하려는 이유 선언 문자열을 제공 합니다. 이 문자열은 응용 프로그램을 실행 하는 사용자는 첫 번째 시간에 나타납니다.

[ ![](security-privacy-images/info01.png "예제 NSHomeKitUsageDescription 경고")](security-privacy-images/info01.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

현재 Visual Studio 용 Xamarin.iOS 향상 된 보안 편집을 지원 하지 않는 `Info.plist` 다음 문제를 해결 해야 하므로 IDE 내에서 설정 합니다.

1. 열기는 `Info.plist` 외부 텍스트 편집기에서에서 파일에 있습니다.
2. 지난 기간 이전 `</dict>` 노드를 다음 노드를 추가 합니다. `<key>NSHomeKitUsageDescription</key>`
3. 필요한 설명을 제공 하는 다음 노드를 추가 합니다. `<string>Allows the app to control HomeKit enabled devices.</string>`
4. `Info.plist` 파일은 다음과 같습니다. 

    [ ![](security-privacy-images/info02vs.png "Info.plist 파일은 다음과 같이 표시")](security-privacy-images/info02vs.png)
4. 파일의 변경 내용을 저장합니다.
5. Visual Studio로 반환 하 고 다시 컴파일한 응용 프로그램의 키를 누릅니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

개인 키를 설정 하려면 다음을 수행 합니다.

1. 두 번 클릭는 `Info.plist` 파일에 **솔루션 탐색기** 를 편집 하기 위해 엽니다.
2. 화면 맨 아래에 전환 하는 **소스** 보기.
3. 새로 추가 **항목** 목록에 있습니다.
4. 드롭다운 목록에서 개인 키를 선택 합니다 (예: **개인정보 취급 방침-HomeKit 사용 설명을**): 

    [ ![](security-privacy-images/info02.png "개인 키를 선택 합니다.")](security-privacy-images/info02.png)
5. 응용 프로그램이 지정된 된 기능 또는 사용자 정보에 액세스 하려고 하는 이유에 대 한 설명을 입력 합니다. 

    [ ![](security-privacy-images/info03.png "설명을 입력합니다")](security-privacy-images/info03.png)
6. 파일의 변경 내용을 저장합니다.

-----

> [!IMPORTANT]
> **참고:** 설정 실패 위에 제공 된 예제에서는 `NSHomeKitUsageDescription` 키에 `Info.plist` 앱에서 파일을 초래 _자동으로 실패_ (되 런타임 시 시스템에 의해 닫힘) iOS 10에서에서 실행할 때 오류 없이 (또는 많은).

<a name="Summary" />

## <a name="summary"></a>요약

이 문서는 보안 및 개인 정보 보호는 Apple에에서 변경 내용을 iOS 10 및 Xamarin.iOS 앱에 어떻게 영향을 검사가 수행 됩니다.



## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://developer.xamarin.com/samples/ios/iOS10/)
