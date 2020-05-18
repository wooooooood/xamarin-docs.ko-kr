---
ms.topic: include
author: jamesmontemagno
ms.author: jamont
ms.date: 05/11/2020
ms.openlocfilehash: b5396061de657690fa3f4cdf68e8a94382918613
ms.sourcegitcommit: 83cf2a4d99546751c6394510a463a2b2a8bf75b8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/13/2020
ms.locfileid: "83149751"
---
이 API는 Android에서 런타임 권한을 사용합니다. Xamarin.Essentials가 완전히 초기화되고 앱에서 사용 권한 처리가 설정되어 있는지 확인하세요.

Android 프로젝트의 `MainLauncher` 또는 시작된 `Activity`의 `OnCreate` 메서드에서 Xamarin.Essentials를 초기화해야 합니다.

```csharp
protected override void OnCreate(Bundle savedInstanceState) 
{
    //...
    base.OnCreate(savedInstanceState);
    Xamarin.Essentials.Platform.Init(this, savedInstanceState); // add this line to your code, it may also be called: bundle
    //...
}    
```

Android에서 런타임 권한을 처리하려면 Xamarin.Essentials가 `OnRequestPermissionsResult`를 받아야 합니다. 모든 `Activity` 클래스에 다음 코드를 추가합니다.

```csharp
public override void OnRequestPermissionsResult(int requestCode, string[] permissions, Android.Content.PM.Permission[] grantResults)
{
    Xamarin.Essentials.Platform.OnRequestPermissionsResult(requestCode, permissions, grantResults);

    base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
}
```
