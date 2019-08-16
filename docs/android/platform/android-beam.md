---
title: Android Beam
ms.prod: xamarin
ms.assetid: 4172A798-89EC-444D-BC0C-0A7DD67EF98C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/06/2017
ms.openlocfilehash: 83fa64ca207358b712341e1923a3a9a67a449e1f
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69524731"
---
# <a name="android-beam"></a>Android Beam

Android 빔은 가까운 근접 한 경우 응용 프로그램이 NFC를 통해 정보를 공유할 수 있도록 하는 Android 4.0에 도입 된 NFC (근거리 통신) 기술입니다.

[![가까운 근접 공유 정보에서 두 장치를 보여 주는 다이어그램](android-beam-images/androidbeam.png)](android-beam-images/androidbeam.png#lightbox)

Android 무선은 두 장치가 범위 내에 있을 때 NFC를 통해 메시지를 푸시하는 방식으로 작동 합니다. 서로 약 4cm의 장치는 Android 보를 사용 하 여 데이터를 공유할 수 있습니다. 한 장치의 활동은 메시지를 만들고이를 푸시하는 작업을 처리할 수 있는 활동을 지정 합니다. 지정 된 활동이 포그라운드에 있고 장치가 범위 내에 있는 경우 Android 빔은 메시지를 두 번째 장치로 푸시합니다. 수신 장치에서 메시지 데이터를 포함 하는 의도가 호출 됩니다.

Android는 Android 빔을 사용 하 여 메시지를 설정 하는 두 가지 방법을 지원 합니다.

- `SetNdefPushMessage`-Android 무선 연결을 시작 하기 전에 응용 프로그램은 SetNdefPushMessage를 호출 하 여 NFC를 통해 푸시할 NdefMessage와이를 푸시하는 활동을 지정할 수 있습니다. 응용 프로그램을 사용 하는 동안 메시지가 변경 되지 않는 경우이 메커니즘을 사용 하는 것이 가장 좋습니다.

- `SetNdefPushMessageCallback`-Android 빔이 시작 되 면 응용 프로그램은 콜백을 처리 하 여 NdefMessage를 만들 수 있습니다. 이 메커니즘을 사용 하면 장치가 범위 내에 있을 때까지 메시지를 만들 수 있습니다. 응용 프로그램에서 발생 하는 상황에 따라 메시지가 달라질 수 있는 시나리오를 지원 합니다.


두 경우 모두 Android 빔를 사용 하 여 데이터를 전송 하기 위해 응용 `NdefMessage`프로그램은를 전송 하 고 `NdefRecords`데이터를 여러 패키지로 패키지화 합니다. Android 보를 트리거하기 전에 해결 해야 하는 주요 요소에 대해 살펴보겠습니다. 먼저를 만드는 `NdefMessage`콜백 스타일을 사용 합니다.


## <a name="creating-a-message"></a>메시지 만들기

활동의 `NfcAdapter` `OnCreate` 메서드에서를 사용 하 여 콜백을 등록할 수 있습니다. 예를 `NfcAdapter` 들어 라는 `mNfcAdapter` 가 활동에서 클래스 변수로 선언 된 경우 메시지를 생성 하는 콜백을 만들기 위해 다음 코드를 작성할 수 있습니다.

```csharp
mNfcAdapter = NfcAdapter.GetDefaultAdapter (this);
mNfcAdapter.SetNdefPushMessageCallback (this, this);
```

을 구현 `NfcAdapter.ICreateNdefMessageCallback`하는 활동은 위의 `SetNdefPushMessageCallback` 메서드에 전달 됩니다. Android 빔이 시작 되 면 시스템은 아래 `CreateNdefMessage` `NdefMessage` 와 같이 활동에서을 생성할 수 있는를 호출 합니다.

```csharp
public NdefMessage CreateNdefMessage (NfcEvent evt)
{
    DateTime time = DateTime.Now;
    var text = ("Beam me up!\n\n" + "Beam Time: " +
        time.ToString ("HH:mm:ss"));
    NdefMessage msg = new NdefMessage (
        new NdefRecord[]{ CreateMimeRecord (
            "application/com.example.android.beam",
            Encoding.UTF8.GetBytes (text)) });
        } };
    return msg;
}

public NdefRecord CreateMimeRecord (String mimeType, byte [] payload)
{
    byte [] mimeBytes = Encoding.UTF8.GetBytes (mimeType);
    NdefRecord mimeRecord = new NdefRecord (
        NdefRecord.TnfMimeMedia, mimeBytes, new byte [0], payload);
    return mimeRecord;
}
```


## <a name="receiving-a-message"></a>메시지 받기

받는 쪽에서 시스템은 다음과 같이 ndefmessage를 추출할 `ActionNdefDiscovered` 수 있는 작업을 사용 하 여 의도를 호출 합니다.

```csharp
IParcelable [] rawMsgs = intent.GetParcelableArrayExtra (NfcAdapter.ExtraNdefMessages);
NdefMessage msg = (NdefMessage) rawMsgs [0];
```

아래 스크린샷에서 실행 되는 Android 보를 사용 하는 전체 코드 예제는 샘플 갤러리의 [Android 빔 데모](https://docs.microsoft.com/samples/xamarin/monodroid-samples/androidbeamdemo) 를 참조 하세요.

[![Android 빔 데모의 예제 스크린샷](android-beam-images/24.png)](android-beam-images/24.png#lightbox)



## <a name="related-links"></a>관련 링크

- [Android 빔 데모 (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/androidbeamdemo)
- [아이스크림 및 사우스 샌드위치](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 플랫폼](https://developer.android.com/sdk/android-4.0.html)
