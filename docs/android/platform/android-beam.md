---
title: Android Beam
ms.prod: xamarin
ms.assetid: 4172A798-89EC-444D-BC0C-0A7DD67EF98C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/06/2017
ms.openlocfilehash: aab121ed5f811baf38eed48cf891ccdf076eaf44
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76723814"
---
# <a name="android-beam"></a>Android Beam

Android 빔은 가까운 근접 한 경우 응용 프로그램이 NFC를 통해 정보를 공유할 수 있도록 하는 Android 4.0에 도입 된 NFC (근거리 통신) 기술입니다.

[가까운 근접 공유 정보에서 두 장치를 보여 주는 ![다이어그램](android-beam-images/androidbeam.png)](android-beam-images/androidbeam.png#lightbox)

Android 무선은 두 장치가 범위 내에 있을 때 NFC를 통해 메시지를 푸시하는 방식으로 작동 합니다. 서로 약 4cm의 장치는 Android 보를 사용 하 여 데이터를 공유할 수 있습니다. 한 장치의 활동은 메시지를 만들고이를 푸시하는 작업을 처리할 수 있는 활동을 지정 합니다. 지정 된 활동이 포그라운드에 있고 장치가 범위 내에 있는 경우 Android 빔은 메시지를 두 번째 장치로 푸시합니다. 수신 장치에서 메시지 데이터를 포함 하는 의도가 호출 됩니다.

Android는 Android 빔을 사용 하 여 메시지를 설정 하는 두 가지 방법을 지원 합니다.

- `SetNdefPushMessage`-Android 무선 연결을 시작 하기 전에 응용 프로그램은 SetNdefPushMessage를 호출 하 여 NFC를 통해 푸시할 NdefMessage와이를 푸시하는 활동을 지정할 수 있습니다. 응용 프로그램을 사용 하는 동안 메시지가 변경 되지 않는 경우이 메커니즘을 사용 하는 것이 가장 좋습니다.

- `SetNdefPushMessageCallback`-Android 빔이 시작 되 면 응용 프로그램은 콜백을 처리 하 여 NdefMessage를 만들 수 있습니다. 이 메커니즘을 사용 하면 장치가 범위 내에 있을 때까지 메시지를 만들 수 있습니다. 응용 프로그램에서 발생 하는 상황에 따라 메시지가 달라질 수 있는 시나리오를 지원 합니다.

어떤 경우 든 Android 빔으로 데이터를 전송 하기 위해 응용 프로그램은 `NdefMessage`를 보내고 여러 `NdefRecords`데이터를 패키지화 합니다. Android 보를 트리거하기 전에 해결 해야 하는 주요 요소에 대해 살펴보겠습니다. 먼저 `NdefMessage`를 만드는 콜백 스타일을 사용 합니다.

## <a name="creating-a-message"></a>메시지 만들기

활동의 `OnCreate` 메서드에서 `NfcAdapter`를 사용 하 여 콜백을 등록할 수 있습니다. 예를 들어 이름이 `mNfcAdapter` 인 `NfcAdapter` 작업에서 클래스 변수로 선언 된다고 가정할 때 다음 코드를 작성 하 여 메시지를 생성 하는 콜백을 만들 수 있습니다.

```csharp
mNfcAdapter = NfcAdapter.GetDefaultAdapter (this);
mNfcAdapter.SetNdefPushMessageCallback (this, this);
```

`NfcAdapter.ICreateNdefMessageCallback`를 구현 하는 활동은 위의 `SetNdefPushMessageCallback` 메서드에 전달 됩니다. Android 빔이 시작 되 면 시스템은 아래와 같이 활동에서 `NdefMessage`를 생성할 수 있는 `CreateNdefMessage`를 호출 합니다.

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

받는 쪽에서 시스템은 다음과 같이 NdefMessage를 추출할 수 있는 `ActionNdefDiscovered` 작업을 사용 하 여 의도를 호출 합니다.

```csharp
IParcelable [] rawMsgs = intent.GetParcelableArrayExtra (NfcAdapter.ExtraNdefMessages);
NdefMessage msg = (NdefMessage) rawMsgs [0];
```

아래 스크린샷에서 실행 되는 Android 보를 사용 하는 전체 코드 예제는 샘플 갤러리의 [Android 빔 데모](https://docs.microsoft.com/samples/xamarin/monodroid-samples/androidbeamdemo) 를 참조 하세요.

[Android 빔 데모의 ![예제 스크린샷](android-beam-images/24.png)](android-beam-images/24.png#lightbox)

## <a name="related-links"></a>관련 링크

- [Android 빔 데모 (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/androidbeamdemo)
