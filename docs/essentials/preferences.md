---
title: 'Xamarin.Essentials: 기본 설정'
description: 이 문서에서는 Xamarin.Essentials 응용 프로그램의 기본 키/값 저장소에 저장에서 기본 클래스를 설명 합니다. 클래스 및 저장할 수 있는 데이터 형식을 사용 하는 방법을 설명 합니다.
ms.assetid: AA81BCBD-79BA-448F-942B-BA4415CA50FF
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: e453c04a953e60be2508670723d175bde3dc7c42
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782849"
---
# <a name="xamarinessentials-preferences"></a>Xamarin.Essentials: 기본 설정

![시험판 NuGet](~/media/shared/pre-release.png)

**기본 설정** 클래스는 응용 프로그램의 기본 키/값 저장소에 저장 하는 데 도움이 됩니다.

## <a name="using-secure-storage"></a>보안 저장소를 사용 하 여

클래스에 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

에 대 한 값을 저장 하는 지정 된 _키_ 기본 설정에서:

```csharp
Preferences.Set("my_key", "my_value");
```

그렇지 않은 경우 기본 설정 또는 기본값에서 값을 검색 하려면 설정 합니다.

```csharp
var myValue = Preferences.Get("my_key", "default_value");
```

제거 하는 _키_ 기본 설정에서:

```csharp
Preferences.Remove("my_key");
```

제거 하려면 모든 기본 설정:

```csharp
Preferences.Clear();
```

이러한 방법 외에도 각각 사용 된 선택적 `sharedName` 선호도에 따라 추가 컨테이너를 만드는 데 사용할 수 있는 합니다. 플랫폼 구현 세부 아래를 읽습니다.

## <a name="supported-data-types"></a>지원 되는 데이터 형식

다음과 같은 데이터 형식이 지원 됩니다 **기본 설정**:

- **bool**
- **double**
- **int**
- **float**
- **long**
- **string**

## <a name="platform-implementation-specifics"></a>플랫폼 구현 세부 사항

# <a name="androidtabandroid"></a>[Android](#tab/android)

모든 데이터에 저장 됩니다 [공유 기본 설정을](https://developer.android.com/training/data-storage/shared-preferences.html)합니다. 없는 경우 `sharedName` 공유 기본 설정을 사용 되 고, 이름을 가져오는 데 사용 됩니다 다른 지정 된 한 **개인** 지정 된 이름의 기본 설정을 공유 합니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

[NSUserDefaults](https://docs.microsoft.com/en-us/xamarin/ios/app-fundamentals/user-defaults) iOS 장치에서 값을 저장 하는 데 사용 됩니다. 없는 경우 `sharedName` 지정는 `StandardUserDefaults` 는 사용 하는 다른 이름을 만드는 데 사용 되는 새 `NSUserDefaults` 에 사용 되는 지정 된 이름의 `NSUserDefaultsType.SuiteName`합니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[ApplicationDataContainer](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdatacontainer) 장치에서 값을 저장 하는 데 사용 됩니다. 없는 경우 `sharedName` 지정는 `LocalSettings` 는 사용 하는 다른 이름이 사용 됩니다 내에 새 컨테이너를 만들려면 `LocalSettings`합니다.

--------------

## <a name="limitations"></a>제한 사항

문자열에 저장 하는 경우이 API는 적은 양의 텍스트를 저장 하는 데 사용 됩니다.  성능에 많은 양의 텍스트를 저장 하려면 사용 하려는 경우 거쳤으며 평균 이하의 수 있습니다.

## <a name="api"></a>API

- [기본 설정 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Preferences)
- [기본 설정 API 설명서](xref:Xamarin.Essentials.Preferences)
