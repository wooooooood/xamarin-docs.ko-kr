---
title: Xamarin.ios의 핵심 NFC
description: 이 문서에서는 iOS 11에 도입 된 Api를 사용 하 여 Xamarin.ios에서 근거리 통신 태그를 읽는 방법을 설명 합니다.
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 846B59D3-F66A-48F3-A78C-84217697194E
author: lobrien
ms.author: laobri
ms.date: 09/25/2017
ms.openlocfilehash: c82861a0b4ca00f7c664cd6a920f250ad937d306
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656456"
---
# <a name="core-nfc-in-xamarinios"></a>Xamarin.ios의 핵심 NFC

_IOS 11을 사용 하 여 NFC (근거리 통신) 태그 읽기_

CoreNFC는 앱 내에서 태그를 읽기 위해 NFC ( _근거리 통신_ ) 라디오에 액세스할 수 있게 해 주는 iOS 11의 새로운 프레임 워크입니다. IPhone 7, 7 Plus, 8, 8 Plus 및 X에서 작동 합니다.

IOS 장치의 NFC 태그 판독기는 NDEF ( _Nfc Data Exchange Format_ ) 정보를 포함 하는 모든 nfc 태그 유형 1 ~ 5를 지원 합니다.

유의 해야 할 몇 가지 제한 사항이 있습니다.

- CoreNFC는 태그 읽기 (쓰기 또는 서식 지정 안 함)만 지원 합니다.
- 태그 검색은 사용자가 시작 해야 하며, 60 초 후에 시간 초과 됩니다.
- 앱은 검색을 위해 포그라운드에서 표시 되어야 합니다.
- CoreNFC는 시뮬레이터가 아닌 실제 장치 에서만 테스트할 수 있습니다.

이 페이지에서는 CoreNFC를 사용 하는 데 필요한 구성에 대해 설명 하 고 ["NFCTagReader" 샘플 코드](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-nfctagreader)를 사용 하 여 API를 사용 하는 방법을 보여 줍니다.

## <a name="configuration"></a>Configuration

CoreNFC를 사용 하도록 설정 하려면 프로젝트에서 다음 세 가지 항목을 구성 해야 합니다.

- **Info.plist** 개인 정보 보호 키입니다.
- **Info.plist** 항목입니다.
- **NFC 태그 읽기** 기능이 있는 프로 비전 프로필입니다.

### <a name="infoplist"></a>Info.plist

검색을 수행 하는 동안 사용자에 게 표시 되는 **NFCReaderUsageDescription** 개인 정보 보호 키 및 텍스트를 추가 합니다. 응용 프로그램에 적합 한 메시지를 사용 합니다 (예: 검색의 목적 설명).

```xml
<key>NFCReaderUsageDescription</key>
<string>NFC tag to read NDEF messages into the application</string>
```

### <a name="entitlementsplist"></a>Entitlements.plist

앱은 **info.plist**에서 다음 키/값 쌍을 사용 하 여 **근거리 통신 태그 읽기** 기능을 요청 해야 합니다.

```xml
<key>com.apple.developer.nfc.readersession.formats</key>
<array>
  <string>NDEF</string>
</array>
```

### <a name="provisioning-profile"></a>프로비전 프로필

새 **앱 ID** 를 만들고 **NFC 태그 읽기** 서비스가 선택 인지 확인 합니다.

[![개발자 포털 NFC 태그를 선택 하 여 새 앱 ID 페이지 선택](corenfc-images/app-services-nfc-sml.png)](corenfc-images/app-services-nfc.png#lightbox)

그런 다음이 앱 ID에 대 한 새 프로 비전 프로필을 만든 다음, 개발 Mac에 다운로드 하 여 설치 해야 합니다.

## <a name="reading-a-tag"></a>태그 읽기

프로젝트가 구성 되 면 파일의 맨 `using CoreNFC;` 위에를 추가 하 고 다음 세 단계를 수행 하 여 NFC 태그 읽기 기능을 구현 합니다.

### <a name="1-implement-infcndefreadersessiondelegate"></a>1. 구현한`INFCNdefReaderSessionDelegate`

인터페이스에는 다음과 같은 두 가지 메서드를 구현할 수 있습니다.

- `DidDetect`– 태그를 성공적으로 읽으면 호출 됩니다.
- `DidInvalidate`– 오류가 발생 하거나 60 초 제한 시간에 도달 하면 호출 됩니다.

#### <a name="diddetect"></a>DidDetect

샘플 코드에서 검색 된 각 메시지는 테이블 뷰에 추가 됩니다.

```csharp
public void DidDetect(NFCNdefReaderSession session, NFCNdefMessage[] messages)
{
    foreach (NFCNdefMessage msg in messages)
    {  // adds the messages to a list view
        DetectedMessages.Add(msg);
    }
    DispatchQueue.MainQueue.DispatchAsync(() =>
    {
        this.TableView.ReloadData();
    });
}
```

세션에서 여러 태그 읽기를 허용 하는 경우이 메서드는 여러 번 호출 될 수 있으며 메시지 배열이 전달 될 수도 있습니다. 이는 `Start` 메서드의 세 번째 매개 변수 ( [2 단계](#step2)에서 설명)를 사용 하 여 설정 됩니다.

#### <a name="didinvalidate"></a>DidInvalidate

무효화는 다음과 같은 다양 한 이유로 발생할 수 있습니다.

- 검색 하는 동안 오류가 발생 했습니다.
- 앱이 전경에 있지 않습니다.
- 사용자가 검색을 취소 하도록 선택 했습니다.
- 앱에서 검사를 취소 했습니다.

아래 코드에서는 오류를 처리 하는 방법을 보여 줍니다.

```csharp
public void DidInvalidate(NFCNdefReaderSession session, NSError error)
{
    var readerError = (NFCReaderError)(long)error.Code;
    if (readerError != NFCReaderError.ReaderSessionInvalidationErrorFirstNDEFTagRead &&
        readerError != NFCReaderError.ReaderSessionInvalidationErrorUserCanceled)
    {
      // some error handling
    }
}
```

세션이 무효화 되 면 다시 검색 하려면 새 세션 개체를 만들어야 합니다.

<a name="step2" />

### <a name="2-start-an-nfcndefreadersession"></a>2. 시작`NFCNdefReaderSession`

검색은 단추 누름과 같은 사용자 요청으로 시작 해야 합니다.
다음 코드는 검색 세션을 만들고 시작 합니다.

```csharp
Session = new NFCNdefReaderSession(this, null, true);
Session?.BeginSession();
```

`NFCNdefReaderSession` 생성자에 대 한 매개 변수는 다음과 같습니다.

- `delegate`–의 `INFCNdefReaderSessionDelegate`구현입니다. 샘플 코드에서 대리자는 테이블 뷰 컨트롤러에서 구현 되므로 `this` 대리자 매개 변수로 사용 됩니다.
- `queue`– 콜백이 처리 되는 큐입니다. 이 경우 예제와 같이 사용자 인터페이스 컨트롤을 업데이트할 `DispatchQueue.MainQueue` 때를 사용 해야 합니다. `null`
- `invalidateAfterFirstRead`– 인 `true`경우 검색을 처음 성공한 `false` 후 검색을 중지 하 고, 검색을 취소 하거나 60 초 시간 제한에 도달할 때까지 검색을 계속 하 고 여러 결과를 반환 합니다.


### <a name="3-cancel-the-scanning-session"></a>3. 검사 세션 취소

사용자는 사용자 인터페이스에서 시스템이 제공 하는 단추를 통해 검색 세션을 취소할 수 있습니다.

![검사 하는 동안 취소 단추](corenfc-images/scan-cancel-sml.png)

앱은 메서드를 `InvalidateSession` 호출 하 여 프로그래밍 방식으로 검색을 취소할 수 있습니다.

```csharp
Session.InvalidateSession();
```

두 경우 모두 대리자의 `DidInvalidate` 메서드가 호출 됩니다.

## <a name="summary"></a>요약

CoreNFC를 사용 하면 앱에서 NFC 태그의 데이터를 읽을 수 있습니다. 다양 한 태그 형식 (NDEF 형식 1 ~ 5)을 읽을 수 있지만 쓰기 또는 서식 지정은 지원 하지 않습니다.


## <a name="related-links"></a>관련 링크

- [NFCTagReader (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-nfctagreader)
- [코어 NFC (WWDC) 소개 (비디오)](https://developer.apple.com/videos/play/wwdc2017/718/)
