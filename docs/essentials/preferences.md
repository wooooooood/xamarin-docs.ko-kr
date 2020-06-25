---
title: 'Xamarin.Essentials: 기본 설정'
description: 이 문서에서는 키/값 저장소에 애플리케이션 기본 설정을 저장하는 Xamarin.Essentials의 Preferences 클래스를 설명합니다. 또한 해당 클래스 및 저장할 수 있는 데이터 형식을 사용하는 방법을 설명합니다.
ms.assetid: AA81BCBD-79BA-448F-942B-BA4415CA50FF
author: jamesmontemagno
ms.author: jamont
ms.date: 01/15/2019
ms.custom: video
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: acc0c48776c7a91e9e5a060928564bc6e0c1d775
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84801818"
---
# <a name="xamarinessentials-preferences"></a>Xamarin.Essentials: 기본 설정

**Preferences** 클래스를 사용하여 키/값 저장소에 애플리케이션 기본 설정을 저장할 수 있습니다.

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

지정된 ‘키’가 기본 설정에 있는지 확인하려면 다음을 수행합니다.

```csharp
bool hasKey = Preferences.ContainsKey("my_key");
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

## <a name="integrate-with-system-settings"></a>시스템 설정과 통합

기본 설정은 기본적으로 저장되므로 설정을 네이티브 시스템 설정에 통합할 수 있습니다. 플랫폼 설명서 및 샘플을 따라 플랫폼과 통합합니다.

* Apple: [iOS 설정 번들 구현](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/UserDefaults/Preferences/Preferences.html)
* [iOS 애플리케이션 기본 설정 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/appprefs/)
* [watchOS 설정](https://developer.xamarin.com/guides/ios/watch/working-with/settings/)
* Android: [설정 화면 시작](https://developer.android.com/guide/topics/ui/settings.html)

## <a name="implementation-details"></a>구현 세부 정보

`DateTime` 값은 `DateTime` 클래스에서 정의된 두 가지 메서드를 사용하여 64 비트 이진(긴 정수) 형식으로 저장됩니다. [`ToBinary`](xref:System.DateTime.ToBinary) 메서드는 `DateTime` 값을 인코딩하는 데 사용되며 [`FromBinary`](xref:System.DateTime.FromBinary(System.Int64)) 메서드는 값을 디코딩합니다. UTC(협정 세계시) 값이 아닌 `DateTime`이 저장될 때 디코드된 값으로 설정할 수 있는 조정에 대해서는 이러한 메서드의 문서를 참조하세요.

## <a name="platform-implementation-specifics"></a>플랫폼 구현 관련 정보

# <a name="android"></a>[Android](#tab/android)

모든 데이터는 [공유 기본 설정](https://developer.android.com/training/data-storage/shared-preferences.html)에 저장됩니다. `sharedName`을 지정하지 않으면 기본 공유 기본 설정이 사용되고, 이외의 경우에는 지정된 이름을 사용하여 **개인** 공유 기본 설정을 가져오는 데 이름이 사용됩니다.

# <a name="ios"></a>[iOS](#tab/ios)

[NSUserDefaults](https://docs.microsoft.com/xamarin/ios/app-fundamentals/user-defaults)는 iOS 디바이스에 값을 저장하는 데 사용됩니다. `sharedName`을 지정하지 않으면 `StandardUserDefaults`가 사용되고, 이외의 경우에는 `NSUserDefaultsType.SuiteName`에 사용된 지정된 이름을 사용하여 새 `NSUserDefaults`을 만드는 데 이름이 사용됩니다.

# <a name="uwp"></a>[UWP](#tab/uwp)

[ApplicationDataContainer](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer)는 디바이스에 값을 저장하는 데 사용됩니다. `sharedName`을 지정하지 않으면 `LocalSettings`가 사용되고, 이외의 경우에는 `LocalSettings` 내부에 새 컨테이너를 만드는 데 이름이 사용됩니다.

또한 `LocalSettings`에는 각 설정의 이름 길이가 최대 255자 이하로 제한하는 사항이 포함되어 있습니다. 각 설정의 크기는 최대 8K 바이트이며 각 복합 설정은 크기는 최대 64K 바이트까지 가능합니다.

--------------

## <a name="persistence"></a>지속성

애플리케이션을 제거하면 모든 ‘기본 설정’이 제거됩니다. 한 가지 예외는 Android 6.0(API 레벨 23) 이상을 대상으로 하고 이 레벨에서 실행되며 [__자동 백업__](https://developer.android.com/guide/topics/data/autobackup)을 사용 중인 앱의 경우입니다. 이 기능은 기본적으로 켜지고 **기본 설정** API에서 사용하는 __공유 기본 설정__을 포함한 앱 데이터를 유지합니다. 다음 Google의 [문서](https://developer.android.com/guide/topics/data/autobackup)를 사용하여 이 기능을 사용하지 않도록 설정할 수 있습니다.

## <a name="limitations"></a>제한 사항

문자열을 저장할 때 이 API는 소량 텍스트를 저장하기 위한 것입니다.  대량 텍스트를 저장하는 데 사용하려고 하면 성능이 저하될 수 있습니다.

## <a name="api"></a>API

- [기본 설정 소스 코드](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Preferences)
- [기본 설정 API 문서](xref:Xamarin.Essentials.Preferences)

## <a name="related-video"></a>관련 동영상

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Preferences-Essential-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
