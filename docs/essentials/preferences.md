---
title: 'Xamarin.Essentials: 기본 설정'
description: 이 문서에서는 키/값 저장소에 응용 프로그램 기본 설정을 저장하는 Xamarin.Essentials의 Preferences 클래스를 설명합니다. 또한 해당 클래스 및 저장할 수 있는 데이터 형식을 사용하는 방법을 설명합니다.
ms.assetid: AA81BCBD-79BA-448F-942B-BA4415CA50FF
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: d50fe3853ab87d5bc14ac15a442140218a1b0fe0
ms.sourcegitcommit: 6be6374664cd96a7d924c2e0c37aeec4adf8be13
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51617555"
---
# <a name="xamarinessentials-preferences"></a>Xamarin.Essentials: 기본 설정

![시험판 NuGet](~/media/shared/pre-release.png)

**Preferences** 클래스를 사용하여 키/값 저장소에 응용 프로그램 기본 설정을 저장할 수 있습니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-preferences"></a>기본 설정 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

기본 설정에서 지정된 ‘키’의 값을 저장합니다.

```csharp
Preferences.Set("my_key", "my_value");
```

기본 설정에서 값을 검색하거나 설정되지 않은 경우 기본값을 검색합니다.

```csharp
var myValue = Preferences.Get("my_key", "default_value");
```

기본 설정에서 ‘키’를 제거합니다.

```csharp
Preferences.Remove("my_key");
```

모든 기본 설정을 제거합니다.

```csharp
Preferences.Clear();
```

이러한 메서드 외에도 각각은 기본 설정의 추가 컨테이너를 만드는 데 사용할 수 있는 선택적 `sharedName`을 사용합니다. 아래 플랫폼 구현 관련 정보를 참조하세요.

## <a name="supported-data-types"></a>지원되는 데이터 형식

다음 데이터 형식은 **기본 설정**에서 지원됩니다.

- **bool**
- **double**
- **int**
- **float**
- **long**
- **string**
- **DateTime**

## <a name="implementation-details"></a>구현 세부 정보

`DateTime` 값은 `DateTime` 클래스에서 정의한 두 가지 메서드를 사용하여 64비트 이진(long 정수) 형식으로 저장됩니다. [`ToBinary`](xref:System.DateTime.ToBinary) 메서드는 `DateTime` 값을 인코드하는 데 사용되고 [`FromBinary`](xref:System.DateTime.FromBinary(System.Int64)) 메서드는 값을 디코드합니다. UTC(협정 세계시) 값이 아닌 `DateTime`이 저장될 때 디코드된 값으로 설정할 수 있는 조정에 대해서는 이러한 메서드의 문서를 참조하세요.

## <a name="platform-implementation-specifics"></a>플랫폼 구현 관련 정보

# <a name="androidtabandroid"></a>[Android](#tab/android)

모든 데이터는 [공유 기본 설정](https://developer.android.com/training/data-storage/shared-preferences.html)에 저장됩니다. `sharedName`을 지정하지 않으면 기본 공유 기본 설정이 사용되고, 이외의 경우에는 지정된 이름을 사용하여 **개인** 공유 기본 설정을 가져오는 데 이름이 사용됩니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

[NSUserDefaults](https://docs.microsoft.com/xamarin/ios/app-fundamentals/user-defaults)는 iOS 장치에 값을 저장하는 데 사용됩니다. `sharedName`을 지정하지 않으면 `StandardUserDefaults`가 사용되고, 이외의 경우에는 `NSUserDefaultsType.SuiteName`에 사용된 지정된 이름을 사용하여 새 `NSUserDefaults`을 만드는 데 이름이 사용됩니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[ApplicationDataContainer](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer)는 장치에 값을 저장하는 데 사용됩니다. `sharedName`을 지정하지 않으면 `LocalSettings`가 사용되고, 이외의 경우에는 `LocalSettings` 내부에 새 컨테이너를 만드는 데 이름이 사용됩니다.

--------------

## <a name="persistence"></a>지속성

응용 프로그램을 제거하면 모든 ‘기본 설정’이 제거됩니다. 한 가지 예외는 Android 6.0(API 레벨 23) 이상을 대상으로 하고 이 레벨에서 실행되며 [__자동 백업__](https://developer.android.com/guide/topics/data/autobackup)을 사용 중인 앱의 경우입니다. 이 기능은 기본적으로 켜지고 **기본 설정** API에서 사용하는 __공유 기본 설정__을 포함한 앱 데이터를 유지합니다. 다음 Google의 [문서](https://developer.android.com/guide/topics/data/autobackup)를 사용하여 이 기능을 사용하지 않도록 설정할 수 있습니다.

## <a name="limitations"></a>제한 사항

문자열을 저장할 때 이 API는 소량 텍스트를 저장하기 위한 것입니다.  대량 텍스트를 저장하는 데 사용하려고 하면 성능이 저하될 수 있습니다.

## <a name="api"></a>API

- [기본 설정 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Preferences)
- [기본 설정 API 문서](xref:Xamarin.Essentials.Preferences)
