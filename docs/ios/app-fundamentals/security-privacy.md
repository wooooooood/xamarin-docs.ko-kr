---
title: iOS 보안 및 개인 정보 보호 기능
description: 이 문서는 보안 및 개인 정보 보호 기능의 iOS 설명 하 고 Xamarin.iOS와 함께 사용 하는 방법에 설명 합니다. IOS 10 및 개인 사용자 데이터에 액세스 하는 방법에 대 한 업데이트 살펴보겠습니다를 걸립니다.
ms.prod: xamarin
ms.assetid: 718C8721-C359-4650-878A-D68E159A3F53
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 119fd478727a002622861c94462b93b75720a992
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61400621"
---
# <a name="ios-security-and-privacy-features"></a>iOS 보안 및 개인 정보 보호 기능

_이 문서에서는 보안 및 개인 정보 보호에 iOS 및 Xamarin.iOS 앱에 미치는 영향을 사용 하 여 작업을 설명 합니다._

Apple는 개발자가 앱의 보안을 강화 하 고 최종 사용자의 개인 정보 보호에 도움이 되는 보안 및 개인 정보 보호 iOS 10 개 (이상)에 모두 몇 가지 향상을 했습니다. 이 문서에서는 Xamarin.iOS 앱에서 이러한 기능을 구현 다룰 것입니다.
    
<a name="General-Enhancements" />

## <a name="general-enhancements"></a>일반적인 개선 사항

IOS 10에서에서 보안 및 개인 정보 보호는 다음과 같은 일반 변경이 적용 했습니다.

- 일반적인 데이터 보안 아키텍처 (CDSA) API는 사용 되지 않으며 비대칭 키를 생성 하는 SecKey API로 바꿔야 합니다.
- 새 `NSAllowsArbitraryLoadsInWebContent` 키를 추가할 수는 앱의 **Info.plist** 파일과 웹 페이지 앱의 나머지 부분에 대 한 Apple 전송 보안 (ATS) 보호를 사용 하도록 계속 하는 동안 올바르게 로드를 사용 하면 합니다. 자세한 내용은 참조 하십시오 우리의 [앱 전송 보안](~/ios/app-fundamentals/ats.md) 설명서.
- 새 클립보드에 10 iOS 및 macOS Sierra를 허용 하므로 사용자 장치 간에 복사 및 붙여넣기를 API는 클립보드 특정 장치로 제한 되며 타임 스탬프가 적용 된 지정된 된 시점에 자동으로 지울 수 있도록 확장 되었습니다. 또한 명명 된 대지의 오브젝트 더 이상 유지 됩니다 및 임시 보드 공유 컨테이너를 사용 하 여 대체 해야 합니다.
- 모든 SSL/TLS 연결에 대해 RC4 대칭 암호화는 이제 기본적으로 비활성화 됩니다. 또한 보안 전송 API SSLv3을 더 이상 지원 및 개발자는 sha-1 및 3DES 암호화를 사용 하 여 가능한 한 빨리 중지 하는 것이 좋습니다.

<a name="Accessing-Private-User-Data" />

## <a name="accessing-private-user-data"></a>개인 사용자 데이터에 액세스

IOS 10 이상에서 실행 되는 앱에서 하나 이상의 개인 키를 입력 하 여 특정 기능 또는 사용자 정보에 액세스 하려면 의도 정적으로 선언 해야 자신의 **Info.plist** 앱을 확보 하고자 하는 이유는 사용자에 게 설명 하는 파일 액세스 합니다.

> [!IMPORTANT]
> 필요한 키를 자동으로 종료 됩니다 시스템에서 사용자 정보를 제한 된 기능 중 하나에 액세스 하려고 할 때 제공 되지 않는 응용 프로그램 _오류 없이_! IOS 10에서 예기치 않게 실패 하는 앱을 시작 하는 경우 필요한 모든 되도록 **Info.plist** 지정 되었습니다.

다음 개인 키가 사용 가능한 관련:

- **개인 정보-Apple 음악 사용 설명** (`NSAppleMusicUsageDescription`)-앱 사용자의 미디어 라이브러리에 액세스 하려고 하는 이유를 설명 하기 위해 개발자가 있습니다.
- **개인 정보-Bluetooth 주변 장치 사용 설명** (`NSBluetoothPeripheralUsageDescription`)-앱 사용자의 장치에서 Bluetooth를 액세스 하려고 하는 이유를 설명 하기 위해 개발자가 있습니다.
- **개인 정보-일정 사용 설명** (`NSCalendarsUsageDescription`)-앱 사용자의 일정에 액세스 하려고 하는 이유를 설명 하기 위해 개발자가 있습니다.
- **개인 정보-카메라 사용 설명** (`NSCameraUsageDescription`)-앱을 장치의 카메라에 액세스 하려고 하는 이유를 설명 하기 위해 개발자가 있습니다.
- **개인 정보-연락처 사용 설명** (`NSContactsUsageDescription`)-앱 사용자의 연락처에 액세스 하려고 하는 이유를 설명 하기 위해 개발자가 있습니다.
- **개인 정보-건강 공유 사용 설명** (`NSHealthShareUsageDescription`)-앱 사용자 상태 데이터에 액세스 하려고 하는 이유를 설명 하기 위해 개발자가 있습니다. 자세한 내용은 Apple의를 참조 하세요 [HKHealthStore 클래스 참조](https://developer.apple.com/reference/healthkit/hkhealthstore)합니다.
- **개인 정보-건강 업데이트 사용 설명** (`NSHealthUpdateUsageDescription`)-앱 사용자 상태 데이터를 편집 하려는 이유를 설명 하기 위해 개발자가 있습니다. 자세한 내용은 Apple의를 참조 하세요 [HKHealthStore 클래스 참조](https://developer.apple.com/reference/healthkit/hkhealthstore)합니다.
- **개인 정보-HomeKit 사용 설명** (`NSHomeKitUsageDescription`)-앱 사용자의 HomeKit 구성 데이터에 액세스 하려고 하는 이유는 무엇을 설명 하기 위해 개발자가 있습니다.
- **개인 정보-위치 항상 사용 설명** (`NSLocationAlwaysUsageDescription`)-앱 사용자의 위치에 항상 액세스할 하려는 이유를 설명 하기 위해 개발자가 있습니다.
- [사용 되지 않음] **개인 정보-위치 사용 설명** (`NSLocationUsageDescription`)-앱 사용자 위치에 액세스 하려고 하는 이유를 설명 하기 위해 개발자가 있습니다. *참고: IOS 8 (이상)에서이 키가 사용 되지 않습니다. 사용 하 여 `NSLocationAlwaysUsageDescription` 또는 `NSLocationWhenInUseUsageDescription` 대신 합니다.*
- **개인 정보-위치는 경우에 사용 하 여 사용 설명** (`NSLocationWhenInUseUsageDescription`)-앱이 실행 되는 동안 사용자의 위치에 액세스 하려고 하는 이유를 설명 하기 위해 개발자가 있습니다.
- [사용 되지 않음] **개인 정보-미디어 라이브러리 사용 설명** -개발자가 앱이 사용자의 미디어 라이브러리에 액세스 하려고 하는 이유를 설명 하는 수 있습니다. *참고: IOS 8 (이상)에서이 키가 사용 되지 않습니다. 사용 하 여 `NSAppleMusicUsageDescription` 대신 합니다.*
- **개인 정보-마이크 사용 설명** (`NSMicrophoneUsageDescription`)-앱을 장치의 마이크에 액세스 하려고 하는 이유를 설명 하기 위해 개발자가 있습니다.
- **개인 정보-동작 사용 설명** (`NSMotionUsageDescription`)-앱을가 속도계는 장치에 액세스 하려고 하는 이유를 설명 하기 위해 개발자가 있습니다.
- **개인 정보-사진 라이브러리 사용 설명** (`NSPhotoLibraryUsageDescription`)-앱 사용자의 사진 라이브러리에 액세스 하려고 하는 이유를 설명 하기 위해 개발자가 있습니다.
- **개인 정보-미리 알림 사용 설명** (`NSRemindersUsageDescription`)-앱 사용자의 미리 알림을 액세스 하려고 하는 이유는 무엇을 설명 하기 위해 개발자가 있습니다.
- **개인 정보-Siri 사용 설명** (`NSSiriUsageDescription`)-Siri 사용자 데이터를 보낼 앱 하려고 하는 이유를 설명 하기 위해 개발자가 있습니다.
- **개인 정보-음성 인식 사용 설명** (`NSSpeechRecognitionUsageDescription`)-앱을 Apple의 음성 인식 서버에 사용자 데이터를 전송 하려고 하는 이유를 설명 하기 위해 개발자가 있습니다.
- **개인 정보-TV 공급자 사용 설명** (`NSVideoSubscriberAccountUsageDescription`)-앱 사용자의 TV 공급자 계정에 액세스 하려고 하는 이유는 무엇을 설명 하기 위해 개발자가 있습니다.

작업에 대 한 자세한 내용은 **Info.plist** 키를 참조 하세요. Apple [정보 속성 목록 키 참조](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009248-SW1)합니다.

<a name="Setting-Privacy-Keys" />

## <a name="setting-privacy-keys"></a>개인 키를 설정합니다.

HomeKit ios 10 개 (이상)에 액세스 하는 예로 다음, 개발자를 추가 해야 합니다는 `NSHomeKitUsageDescription` 앱의 키 **Info.plist** 파일 및 앱 사용자의 HomeKit에 액세스 하려고 하는 이유는 무엇을 선언 문자열을 제공 합니다. 데이터베이스입니다. 이 문자열은 사용자가 처음에 앱을 실행 하기에 표시 됩니다.

![경고 예제 NSHomeKitUsageDescription](security-privacy-images/info01.png "NSHomeKitUsageDescription 경고 예제")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio 용 Xamarin.iOS를 현재 편집을 지원 하지 않습니다 합니다 **Info.plist** 매니페스트 편집기 기본 iOS 내에서 개인 키입니다. 제네릭 PList 편집기를 사용 해야 하는 대신 하므로 다음을 수행 합니다.

1. 마우스 오른쪽 단추로 클릭 합니다 **Info.plist** 파일을 **솔루션 탐색기** 선택한 **로 열기...** .
2. 선택 된 **제네릭 PList 편집기** 파일을 열고 프로그램 목록에서 클릭 **확인**합니다.

    ![제네릭 PList 편집기를 선택](security-privacy-images/InfoEditorSelectionVs.png "제네릭 PList 편집기를 선택 합니다.")
3. 클릭 합니다 **+** 목록에 새 항목을 추가 하기 위해 편집기에서 마지막 행에는 단추입니다. "사용자 지정 속성"로 설정 하는 형식 사용 하 여 호출 됩니다 `String` 및 빈 값입니다.
4. 속성 이름을 클릭 하 고 드롭다운이 표시 됩니다.
5. 드롭다운 목록에서 개인 키를 선택 합니다 (같은 **개인 정보-HomeKit 사용 설명**): 

    ![개인 키 선택](security-privacy-images/InfoPListEditorSelectKey.png "개인 키를 선택 합니다.")
6. 앱이 지정된 된 기능 또는 사용자 정보에 액세스 하려고 하는 이유의 값 열에 설명을 입력 합니다. 

    ![설명 입력](security-privacy-images/InfoPListSetValue.png "설명을 입력")
7. 파일의 변경 내용을 저장합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

개인 키 중 하나를 설정 하려면 다음을 수행 합니다.

1. 편집하기 위해 **솔루션 탐색기**에서 **Info.plist** 파일을 두 번 클릭하여 엽니다.
2. 화면 아래쪽으로 전환 합니다 **원본** 보기.
3. 새 **항목이** 목록에 있습니다.
4. 드롭다운 목록에서 개인 키를 선택 합니다 (같은 **개인 정보-HomeKit 사용 설명**): 

    ![개인 키 선택](security-privacy-images/info02.png "개인 키를 선택 합니다.")
5. 앱이 지정된 된 기능 또는 사용자 정보에 액세스 하려고 하는 이유는 무엇에 대 한 설명을 입력 합니다. 

    ![설명 입력](security-privacy-images/info03.png "설명을 입력")
6. 파일의 변경 내용을 저장합니다.

-----

> [!IMPORTANT]
> 설정 하지 못했습니다 위에 주어진 예제에서는 `NSHomeKitUsageDescription` 키를 **Info.plist** 앱 파일을 초래 _자동으로 실패_ (되 런타임 시 시스템에 의해 닫힘) (iOS 10에서에서 실행 하는 경우 오류 없이 또는 그 이상).

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 보안 및 개인 정보 보호 변경은 Apple에 iOS 10 및 Xamarin.iOS 앱에 미치는 영향의 검사가 수행 합니다.

## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://developer.xamarin.com/samples/ios/iOS10/)
