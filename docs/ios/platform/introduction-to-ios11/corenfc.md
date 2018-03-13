---
title: "NFC 코어"
description: "11 iOS를 사용 하 여 읽기 근처 NFC (근거리 통신) 태그"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 846B59D3-F66A-48F3-A78C-84217697194E
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/25/2016
ms.openlocfilehash: 72c19ef09843c137514983b1d7ee7104e3cb32c5
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="core-nfc"></a>NFC 코어

_11 iOS를 사용 하 여 읽기 근처 NFC (근거리 통신) 태그_

CoreNFC은 ios 11에 대 한 액세스를 제공 하는 새로운 프레임 워크는 _근거리 통신_ (NFC) 라디오 앱 내에서 태그를 읽을 수 있습니다. IPhone 7에서 작동 더하기 7, 8, 8, 더하기 및 X.

IOS 장치에서 NFC 태그 판독기가 NFC 태그 형식을 1-5 포함 하는 모든 지원 _NFC 데이터 교환 형식인_ (NDEF) 정보입니다.

알아두어야 할 몇 가지 제한 사항이 있습니다.

- CoreNFC만 태그 읽기 (작성 또는 서식)를 지원 합니다.
- 사용자가 시작한 태그 검사 해야 및 60 초 후 시간 제한입니다.
- 앱 검색에 대 한 포그라운드에서 표시 되어야 합니다.
- CoreNFC (시뮬레이터)에 없는 실제 장치에만 테스트할 수 있습니다.

이 페이지 CoreNFC를 사용 하는 데 필요한 구성에 설명 하 고 사용 하 여 API를 사용 하는 방법을 보여 줍니다.는 ["TFCTagReader" 샘플 코드](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/)합니다.

## <a name="configuration"></a>구성

CoreNFC를 사용 하려면 프로젝트의 항목 3 개를 구성 해야 합니다.

- **Info.plist** 개인 키입니다.
- **Entitlements.plist** 항목입니다.
- 포함 된 프로비저닝 프로필 **NFC 태그 읽기** 기능입니다.

### <a name="infoplist"></a>Info.plist

추가 **NFCReaderUsageDescription** 개인 키 및 검색이 진행 되는 동안 사용자에 게 표시 되는 텍스트입니다. 응용 프로그램에 대 한 적절 한 메시지를 사용 하 여 (검색이 용도 설명 하기 예를 들어):

```xml
<key>NFCReaderUsageDescription</key>
<string>NFC tag to read NDEF messages into the application</string>
```

### <a name="entitlementsplist"></a>Entitlements.plist

응용 프로그램을 요청 해야는 **필드 통신 태그 읽기 근처** 의 다음 키/값을 사용 하 여 기능 쌍 프로그램 **Entitlements.plist**:

```xml
<key>com.apple.developer.nfc.readersession.formats</key>
<array>
  <string>NDEF</string>
</array>
```

### <a name="provisioning-profile"></a>프로비전 프로필

새 **앱 ID** 있는지 확인는 **NFC 태그 읽기** 서비스 선택 되어 있습니다.

[![개발자 포털 새 앱 ID 페이지를 선택 하는 NFC 태그 읽기](corenfc-images/app-services-nfc-sml.png)](corenfc-images/app-services-nfc.png#lightbox)

그런 다음이 응용 프로그램 ID에 대 한 새 프로비저닝 프로필을 만들 다음 다운로드 하 고 해야 사용자가 개발 Mac.에 설치

## <a name="reading-a-tag"></a>태그 읽기

프로젝트를 구성한 후 추가할 `using CoreNFC;` 수행 있고 파일의 맨 위에 NFC를 구현 하는 이러한 세 단계 태그 읽기 기능:

### <a name="1-implement-infcndefreadersessiondelegate"></a>1. 구현 `INFCNdefReaderSessionDelegate`

인터페이스에 두 개의 메서드를 구현 해야 합니다.

- `DidDetect` –는 태그를 성공적으로 읽을 때 호출 됩니다.
- `DidInvalidate` -오류가 발생 하거나 60 두 번째 시간 제한에 도달할 때 호출 됩니다.

#### <a name="diddetect"></a>DidDetect

샘플 코드에서 스캔 한 각 메시지는 한 테이블 보기에 추가 됩니다.

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

이 메서드는 여러 번 호출할 수 있습니다 (및 메시지의 배열에 전달 될 수) 세션 여러 태그 읽기에 대 한 허용 하는 경우. 이 값의 세 번째 매개 변수를 사용 하 여 설정 됩니다는 `Start` 메서드 (에 설명 된 [2 단계](#step2)).

#### <a name="didinvalidate"></a>DidInvalidate

무효화 다양 한 이유로 발생할 수 있습니다.

- 검색 하는 동안 오류가 발생 했습니다.
- 앱이 포그라운드에서 수 중지 합니다.
- 사용자 검색을 취소 하도록 선택 합니다.
- 응용 프로그램에서 검색이 취소 되었습니다.

아래 코드는 오류를 처리 하는 방법을 보여 줍니다.

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

세션을 무효화 된 후 새 세션 개체를 다시 스캔를 만들어야 합니다.

<a name="step2" />

### <a name="2-start-an-nfcndefreadersession"></a>2. 시작 프로그램 `NFCNdefReaderSession`

검색 단추 누르기와 같은 사용자 요청으로 시작 해야 합니다.
다음 코드를 만들고 검색 세션을 시작 합니다.

```csharp
Session = new NFCNdefReaderSession(this, null, true);
Session?.BeginSession();
```

에 대 한 매개 변수는 `NFCNdefReaderSession` 생성자는 다음과 같습니다.

- `delegate` –의 구현을 `INFCNdefReaderSessionDelegate`합니다. 샘플 코드에서 대리자에서에서 구현 되 테이블 뷰 컨트롤러 따라서 `this` 대리자 매개 변수로 사용 됩니다.
- `queue` –에서 콜백을 처리 하는 큐입니다. 것이 `null`,이 경우 사용 해야는 `DispatchQueue.MainQueue` (에서처럼 샘플) 사용자 인터페이스 컨트롤을 업데이트할 때.
- `invalidateAfterFirstRead` – 때 `true`, 첫 번째 성공적으로 검색; 검색이 중지 때 `false` 계속 검색 하 고 검색을 취소 하거나 60 두 번째 제한 시간이 경과할 때까지 여러 결과가 반환 합니다.


### <a name="3-cancel-the-scanning-session"></a>3. 스캔 세션 취소

사용자 인터페이스에서 시스템 제공 단추를 통해 검색 세션을 취소할 수 있습니다.

![검색 하는 동안 취소 단추](corenfc-images/scan-cancel-sml.png)

응용 프로그램 수를 호출 하 여 검색을 프로그래밍 방식으로 취소 된 `InvalidateSession` 메서드:

```csharp
Session.InvalidateSession();
```

두 경우 모두 대리자의 `DidInvalidate` 메서드가 호출 됩니다.

## <a name="summary"></a>요약

CoreNFC 앱이 NFC 태그에서 데이터를 읽을 수 있도록 합니다. 것 태그 형식 (NDEF 유형 1-5)의 다양 한 읽기를 지원 하지만 작성 하거나 서식을 지원 하지 않습니다.


## <a name="related-links"></a>관련 링크

- [NFCTagReader (샘플)](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/)
- [코어 NFC (WWDC) (비디오)를 소개합니다.](https://developer.apple.com/videos/play/wwdc2017/718/)
