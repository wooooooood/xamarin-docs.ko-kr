---
title: Android 빔
ms.topic: article
ms.prod: xamarin
ms.assetid: 4172A798-89EC-444D-BC0C-0A7DD67EF98C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/06/2017
ms.openlocfilehash: 08d15095c8d1d08a65f18d5ad867efdd89d3b795
ms.sourcegitcommit: 17a9cf246a4d33cfa232016992b308df540c8e4f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/29/2018
---
# <a name="android-beam"></a>Android 빔

Android 빔에 응용 프로그램을 통해 NFC 서로 가까이 있는 경우 정보를 공유할 수 있는 Android 4.0에서 도입 된 근처 NFC (근거리 통신) 기술입니다.

[![정보 공유 가까이 있는 두 개의 장치를 보여 주는 다이어그램](android-beam-images/androidbeam.png)](android-beam-images/androidbeam.png#lightbox)

Android 빔 두 장치 범위에 있는 경우 NFC를 통해 메시지를 푸시 하 여 작동 합니다. 장치를 서로 약 4 c m Android 빔을 사용 하 여 데이터를 공유할 수 있습니다. 한 장치에서 작업 메시지를 만들고 활동 (또는 활동)을 지정 푸시합니다 처리할 수 있는 합니다. 지정된 된 활동이 포그라운드에서 때 장치 범위에는 Android 빔은 두 번째 장치에 메시지를 푸시합니다. 수신 하는 장치에서 메시지 데이터를 포함 하는 의도 호출 합니다.

Android는 Android 빔을 사용 하 여 설정 메시지의 두 가지 방법으로 지원합니다.

-   `SetNdefPushMessage` -Android 보가 시작 되 면 전에 응용 프로그램 NFC, 및가 푸시 해야 하는 작업을 통해 적용할 NdefMessage 지정 하려면 SetNdefPushMessage 호출할 수 있습니다. 이 메커니즘은 메시지는 응용 프로그램을 사용 하는 동안 변경 되지 않는 경우에 가장 적합 합니다.

-   `SetNdefPushMessageCallback` -Android 빔 시작 되 면 응용 프로그램에 NdefMessage 만들기에 대 한 콜백을 처리할 수 있습니다. 이 메커니즘 메시지 작성 범위에 있는 장치 될 때까지 지연 될 수 있습니다. 응용 프로그램에서 발생 하는 기능에 따라 메시지 달라질 수 있습니다 시나리오를 지원 합니다.


두 경우 모두 Android 빔으로 데이터를 보내는 응용 프로그램 보냅니다는 `NdefMessage`을 몇 개에 데이터를 패키징 `NdefRecords`합니다. Android 빔 트리거할 수도 하기 전에 해결 해야 하는 주요 사항에 살펴보겠습니다. 먼저 만드는 콜백 스타일을 사용 합니다 우리는 `NdefMessage`합니다.


## <a name="creating-a-message"></a>메시지 만들기

와 콜백을 등록할 수 있습니다는 `NfcAdapter` 활동의에서 `OnCreate` 메서드. 예를 들어 가정는 `NfcAdapter` 라는 `mNfcAdapter` 은 변수로 선언 된 클래스 활동에서 메시지를 생성 하는 콜백을 만들려면 다음 코드를 작성할 수 있습니다.

```csharp
mNfcAdapter = NfcAdapter.GetDefaultAdapter (this);
mNfcAdapter.SetNdefPushMessageCallback (this, this);
```

구현 하는 활동 `NfcAdapter.ICreateNdefMessageCallback`에 전달 되는 `SetNdefPushMessageCallback` 위의 메서드. Android 보가 시작 되 면 시스템은 호출 `CreateNdefMessage`에서 활동 생성할 수는 `NdefMessage` 아래와 같이:

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

수신 측에서 시스템에서 호출 된 의도 `ActionNdefDiscovered` 올 수 추출할는 NdefMessage 다음과 같이 동작 합니다.

```csharp
IParcelable [] rawMsgs = intent.GetParcelableArrayExtra (NfcAdapter.ExtraNdefMessages);
NdefMessage msg = (NdefMessage) rawMsgs [0];
```

아래 스크린샷에서에서 실행 중인 Android 광선을 사용 하는 전체 코드 예제에 대 한 참조는 [Android 빔 데모](https://developer.xamarin.com/samples/monodroid/AndroidBeamDemo/) 샘플 갤러리에서 합니다.

[![Android 빔 데모의 예제 스크린 샷](android-beam-images/24.png)](android-beam-images/24.png#lightbox)



## <a name="related-links"></a>관련 링크

- [Android 빔 데모 (샘플)](https://developer.xamarin.com/samples/monodroid/AndroidBeamDemo/)
- [아이스크림 샌드위치 소개](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 플랫폼](http://developer.android.com/sdk/android-4.0.html)
