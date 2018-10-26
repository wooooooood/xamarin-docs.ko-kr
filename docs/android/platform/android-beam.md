---
title: Android 보
ms.prod: xamarin
ms.assetid: 4172A798-89EC-444D-BC0C-0A7DD67EF98C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/06/2017
ms.openlocfilehash: 13a0a0d9c6a9d1d5f49020b1a8096f5e054d415c
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114927"
---
# <a name="android-beam"></a>Android 보

Android 광선에 응용 프로그램 가까이에서 NFC 정보를 공유할 수 있도록 하는 Android 4.0에서 도입 된 거의 NFC (근거리 통신) 기술입니다.

[![정보를 공유 하는 인접에서 한 두 개의 장치를 보여 주는 다이어그램](android-beam-images/androidbeam.png)](android-beam-images/androidbeam.png#lightbox)

Android 보 두 장치가 범위의 경우 NFC를 통해 메시지를 밀어 작동 합니다. 장치에 대 한 4 cm 서로 Android 광선을 사용 하 여 데이터를 공유할 수 있습니다. 하나의 장치에서 작업 메시지를 만들고 작업 (또는 작업)를 지정 합니다. 해당 푸시를 처리할 수 있는 합니다. 지정 된 작업이 포그라운드에서 범위에 있는 장치를 Android 광선은 두 번째 장치에 메시지를 푸시합니다. 수신 장치에 메시지 데이터를 포함 하는 의도 호출 됩니다.

Android는 Android 광선을 사용 하 여 설정 메시지의 두 가지 방법으로 지원합니다.

-   `SetNdefPushMessage` -Android 광선에 시작 전에 응용 프로그램 NFC, 및 푸시하는 작업을 통해 푸시하는 NdefMessage 지정 SetNdefPushMessage 호출할 수 있습니다. 메시지는 응용 프로그램을 사용 하는 동안 변경 되지 않습니다 하는 경우에 가장이 메커니즘이 사용 됩니다.

-   `SetNdefPushMessageCallback` -Android 보 시작 되 면 응용 프로그램을 NdefMessage 만들기에 대 한 콜백을 처리할 수 있습니다. 이 메커니즘 메시지 만들기 장치를 범위에 있을 때까지 지연 될 수 있습니다. 응용 프로그램에서 일어나 고 있는 내용에 따라 메시지 달라질 수 있는 시나리오를 지원 합니다.


두 경우 모두 Android 광선을 사용 하 여 데이터를 보내도록 응용 프로그램이 보낼를 `NdefMessage`, 몇 가지 데이터 패키징 `NdefRecords`합니다. Android 보 트리거할 수 전에 해결 해야 하는 주요 사항에 살펴보겠습니다. 먼저 위해 함께 노력할 만드는 콜백 스타일을 `NdefMessage`입니다.


## <a name="creating-a-message"></a>메시지 만들기

사용 하 여 콜백을 등록할 수 있습니다는 `NfcAdapter` 활동에서 `OnCreate` 메서드. 예를 들어, 가정 하 고는 `NfcAdapter` 라는 `mNfcAdapter` 메시지를 생성 하는 콜백을 만듭니다에 다음 코드를 작성할 수 있습니다 작업에서 클래스 변수로 선언 됩니다.

```csharp
mNfcAdapter = NfcAdapter.GetDefaultAdapter (this);
mNfcAdapter.SetNdefPushMessageCallback (this, this);
```

구현 하는 활동을 `NfcAdapter.ICreateNdefMessageCallback`, 전달 되는 `SetNdefPushMessageCallback` 위의 메서드. Android 보 시작 되 면 시스템은 호출 `CreateNdefMessage`에서 활동 생성할 수는 `NdefMessage` 아래와 같이:

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


## <a name="receiving-a-message"></a>메시지 수신

받는 쪽에서는 시스템 호출 사용 하 여 의도 `ActionNdefDiscovered` 를 추출할 수 있습니다는 NdefMessage 다음과 같은 작업을 합니다.

```csharp
IParcelable [] rawMsgs = intent.GetParcelableArrayExtra (NfcAdapter.ExtraNdefMessages);
NdefMessage msg = (NdefMessage) rawMsgs [0];
```

아래 스크린샷에서에서 실행 중인 Android 광선을 사용 하는 전체 코드 예제를 참조 하세요. 합니다 [Android 보 데모](https://developer.xamarin.com/samples/monodroid/AndroidBeamDemo/) 샘플 갤러리에서.

[![Android 보 데모에서 예제 스크린샷](android-beam-images/24.png)](android-beam-images/24.png#lightbox)



## <a name="related-links"></a>관련 링크

- [Android 보 데모 (샘플)](https://developer.xamarin.com/samples/monodroid/AndroidBeamDemo/)
- [Ice Cream Sandwich 소개](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 플랫폼](http://developer.android.com/sdk/android-4.0.html)
