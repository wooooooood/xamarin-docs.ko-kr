---
title: Android Beam
ms.prod: xamarin
ms.assetid: 4172A798-89EC-444D-BC0C-0A7DD67EF98C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/06/2017
ms.openlocfilehash: aab121ed5f811baf38eed48cf891ccdf076eaf44
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "76723814"
---
# <a name="android-beam"></a>Android Beam

Android Beam은 애플리케이션이 근접한 거리에서 NFC를 통해 정보를 공유할 수 있는, Android 4.0에 도입된 NFC(근거리 통신) 기술입니다.

[![근접한 두 디바이스가 정보를 공유하는 모습을 보여 주는 다이어그램](android-beam-images/androidbeam.png)](android-beam-images/androidbeam.png#lightbox)

Android Beam은 두 디바이스가 범위 내에 있을 때 NFC를 통해 메시지를 푸시하는 방식으로 작동합니다. 서로 약 4cm에 있는 디바이스는 Android Beam을 사용하여 데이터를 공유할 수 있습니다. 한 디바이스에서 작업이 메시지를 만들고 메시지 푸시를 처리할 수 있는 작업을 지정합니다. 지정된 작업이 포그라운드에 있고 디바이스가 범위 안에 있는 경우 Android Beam은 메시지를 두 번째 디바이스로 푸시합니다. 수신 디바이스에서 메시지 데이터를 포함하는 의도가 호출됩니다.

Android는 Android Beam을 사용하여 메시지를 설정하는 두 가지 방법을 지원합니다.

- `SetNdefPushMessage` -Android Beam이 시작되기 전에 애플리케이션이 SetNdefPushMessage를 호출하여 NFC를 통해 푸시할 NdefMessage와 이를 푸시하는 작업을 지정할 수 있습니다. 애플리케이션을 사용하는 동안 메시지가 변경되지 않는 경우 이 메커니즘을 사용하는 것이 가장 좋습니다.

- `SetNdefPushMessageCallback` - Android Beam이 시작되면 애플리케이션은 콜백을 처리하여 NdefMessage를 생성할 수 있습니다. 이 메커니즘을 사용하면 디바이스가 범위 안에 있을 때까지 메시지 만들기를 지연할 수 있습니다. 이 메커니즘은 애플리케이션에서 발생하는 상황에 따라 메시지가 달라질 수 있는 시나리오를 지원합니다.

어떤 경우든 Android Beam으로 데이터를 전송하기 위해 애플리케이션은 `NdefMessage`를 보내고 데이터를 여러 `NdefRecords`로 패키지합니다. Android Beam을 트리거하기 전에 해결해야 하는 주요 요소에 대해 살펴보겠습니다. 먼저 `NdefMessage`를 만드는 콜백 스타일부터 알아봅니다.

## <a name="creating-a-message"></a>메시지 생성

작업의 `OnCreate` 메서드에서 `NfcAdapter`를 사용하여 콜백을 등록할 수 있습니다. 예를 들어 `mNfcAdapter`라는 `NfcAdapter`가 작업에서 클래스 변수로 선언된다고 가정할 때 다음 코드를 작성하여 메시지를 생성하는 콜백을 만들 수 있습니다.

```csharp
mNfcAdapter = NfcAdapter.GetDefaultAdapter (this);
mNfcAdapter.SetNdefPushMessageCallback (this, this);
```

`NfcAdapter.ICreateNdefMessageCallback`를 구현하는 작업이 위의 `SetNdefPushMessageCallback` 메서드에 전달됩니다. Android Beam이 시작되면 시스템은 아래와 같이 활동이 `NdefMessage`를 생성할 수 있는 `CreateNdefMessage`를 호출합니다.

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

수신 쪽 시스템은 다음과 같이 NdefMessage를 추출할 수 있는 `ActionNdefDiscovered` 작업을 포함하는 의도를 호출합니다.

```csharp
IParcelable [] rawMsgs = intent.GetParcelableArrayExtra (NfcAdapter.ExtraNdefMessages);
NdefMessage msg = (NdefMessage) rawMsgs [0];
```

아래 스크린샷에서 실행 중으로 표시되는 Android Beam을 사용하는 전체 코드 예제는 샘플 갤러리에서 [Android Beam 데모](https://docs.microsoft.com/samples/xamarin/monodroid-samples/androidbeamdemo)를 참조하세요.

[![Android Beam 데모 예제 스크린샷](android-beam-images/24.png)](android-beam-images/24.png#lightbox)

## <a name="related-links"></a>관련 링크

- [Android Beam 데모(샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/androidbeamdemo)
