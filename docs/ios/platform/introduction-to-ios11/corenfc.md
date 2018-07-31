---
title: Xamarin.iOS에서 NFC 코어
description: 이 문서에는 거의 iOS 11에에서 도입 된 Api를 사용 하 여 Xamarin.iOS에서 필드 통신 태그 읽기 방법을 설명 합니다.
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 846B59D3-F66A-48F3-A78C-84217697194E
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/25/2017
ms.openlocfilehash: 1381a4564f93fd091f181949454df3f06b31ae6b
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350835"
---
# <a name="core-nfc-in-xamarinios"></a>Xamarin.iOS에서 NFC 코어

_IOS 11을 사용 하 여 태그를 읽는 거의 NFC (근거리 통신)_

CoreNFC에 대 한 액세스를 제공 하는 iOS 11의에서 새로운 프레임 워크는를 _근거리 통신_ (NFC) 라디오 앱 내에서 태그를 읽을 수 있습니다. IPhone 7에서 작동 더하기 7, 8, 또한 및 X 8입니다.

IOS 장치에서 NFC 태그 판독기 지원 모든 NFC 태그 형식을 포함 하는 1-5 _NFC 데이터 교환 형식인_ (NDEF) 정보입니다.

알아야 할 몇 가지 제한 사항이 있습니다.

- CoreNFC 태그 읽기 (작성 또는 서식 지정)을 지원 합니다.
- 사용자가 시작한 태그 검색 해야 하 고 60 초 후 시간 제한 합니다.
- 앱 검색에 대 한 전경에 표시 되어야 합니다.
- CoreNFC (시뮬레이터)에 없는 실제 장치에서 테스트할 수만 있습니다.

이 페이지 CoreNFC를 사용 하는 데 필요한 구성을 설명 하 고 사용 하 여 API를 사용 하는 방법을 보여 줍니다 합니다 ["TFCTagReader" 샘플 코드](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/)합니다.

## <a name="configuration"></a>구성

CoreNFC을 사용 하려면 프로젝트에 세 개의 항목이 구성 해야 합니다.

- **Info.plist** 개인 키입니다.
- **Entitlements.plist** 항목입니다.
- 프로 비전 프로필과 **NFC 태그 읽기** 기능입니다.

### <a name="infoplist"></a>Info.plist

추가 된 **NFCReaderUsageDescription** 개인 키 및 검색 진행 되는 동안 사용자에 게 표시 되는 텍스트입니다. 응용 프로그램에 대 한 적절 한 메시지를 사용 하 여 (예를 들어 검색의 용도 설명).

```xml
<key>NFCReaderUsageDescription</key>
<string>NFC tag to read NDEF messages into the application</string>
```

### <a name="entitlementsplist"></a>Entitlements.plist

앱 요청 해야 합니다 **필드 통신 태그 읽기 근처** 쌍에 다음 키/값을 사용 하 여 기능에 **Entitlements.plist**:

```xml
<key>com.apple.developer.nfc.readersession.formats</key>
<array>
  <string>NDEF</string>
</array>
```

### <a name="provisioning-profile"></a>프로비전 프로필

새 **앱 ID** 있는지 확인 합니다 **NFC 태그 읽기** 서비스는 선택:

[![선택한 NFC 태그 읽기를 사용 하 여 개발자 포털에 새 앱 ID 페이지](corenfc-images/app-services-nfc-sml.png)](corenfc-images/app-services-nfc.png#lightbox)

그런 다음이 앱 ID에 대 한 새 프로비저닝 프로필을 만드는 다음 다운로드 하 고 해야 Mac. 개발에 설치

## <a name="reading-a-tag"></a>태그 읽기

프로젝트 구성 되 면 추가 `using CoreNFC;` 다음 확인 하 고 파일의 맨 위에 NFC를 구현 하려면 다음 세 단계 태그 읽기 기능:

### <a name="1-implement-infcndefreadersessiondelegate"></a>1. 구현 `INFCNdefReaderSessionDelegate`

인터페이스를 구현 하는 두 가지 방법에 있습니다.

- `DidDetect` -태그를 성공적으로 읽을 때 호출 됩니다.
- `DidInvalidate` -오류가 발생 하거나 60 초 제한 시간에 도달한 경우 호출 됩니다.

#### <a name="diddetect"></a>DidDetect

샘플 코드에서 검색 한 각 메시지는 표 보기에 추가 됩니다.

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

이 메서드가 여러 번 호출할 수 있습니다 (및 메시지의 배열에 전달 될 수) 여러 개의 태그 읽기에 대 한 세션을 허용 하는 경우. 세 번째 매개 변수를 사용 하 여 설정 됩니다는 `Start` 메서드 (에서 설명한 [2 단계](#step2)).

#### <a name="didinvalidate"></a>DidInvalidate

무효화 다양 한 이유로 발생할 수 있습니다.

- 검색 하는 동안 오류가 발생 했습니다.
- 앱이 포그라운드에서 수 중지 합니다.
- 사용자 검색을 취소 하기로 했습니다.
- 앱에서 검색이 취소 되었습니다.

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

세션이 무효화 되었습니다 되 면 새 세션 개체를 다시 스캔을 만들어야 합니다.

<a name="step2" />

### <a name="2-start-an-nfcndefreadersession"></a>2. 시작 프로그램 `NFCNdefReaderSession`

검색 단추 누름 등의 사용자 요청을 시작 해야 합니다.
다음 코드를 만들고 검색 세션을 시작 합니다.

```csharp
Session = new NFCNdefReaderSession(this, null, true);
Session?.BeginSession();
```

매개 변수는 `NFCNdefReaderSession` 생성자는 다음과 같습니다.

- `delegate` -구현의 `INFCNdefReaderSessionDelegate`합니다. 샘플 코드는 대리자에서에서 구현 됩니다 테이블 뷰 컨트롤러를 따라서 `this` 대리자 매개 변수로 사용 됩니다.
- `queue` –에 콜백을 처리 하는 큐입니다. 수 `null`,이 경우에 사용 해야는 `DispatchQueue.MainQueue` 예제의 같이 사용자 인터페이스 컨트롤을 업데이트 하는 경우.
- `invalidateAfterFirstRead` – 때 `true`, 첫 번째 성공적인 검색; 검색 중지 때 `false` 계속 검색 하 고 검색을 취소 하거나 60 초 제한 시간에 도달 될 때까지 여러 결과 반환 합니다.


### <a name="3-cancel-the-scanning-session"></a>3. 검색 세션 취소

사용자 인터페이스에서 시스템 제공 단추를 통해 검색 세션을 취소할 수 있습니다.

![검색 하는 동안 취소 단추](corenfc-images/scan-cancel-sml.png)

앱을 호출 하 여 프로그래밍 방식으로 검색을 취소할 수는 `InvalidateSession` 메서드:

```csharp
Session.InvalidateSession();
```

두 경우 모두 대리자의 `DidInvalidate` 메서드가 호출 됩니다.

## <a name="summary"></a>요약

CoreNFC 앱이 NFC 태그에서 데이터를 읽을 수 있도록 합니다. 이 태그 형식 (NDEF 형식 1 ~ 5)의 다양 한 읽기를 지원 하지만 쓰거나 서식 지정을 지원 하지 않습니다.


## <a name="related-links"></a>관련 링크

- [NFCTagReader (샘플)](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/)
- [Core NFC (WWDC) (비디오)를 소개합니다.](https://developer.apple.com/videos/play/wwdc2017/718/)
