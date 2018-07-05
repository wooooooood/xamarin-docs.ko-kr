---
title: 'Xamarin.Essentials: 기본 설정'
description: 이 문서에서는 Xamarin.Essentials 응용 프로그램의 기본 키/값 저장소에 저장 하는 기본 클래스를 설명 합니다. 클래스 및 저장할 수 있는 데이터 형식을 사용 하는 방법을 설명 합니다.
ms.assetid: AA81BCBD-79BA-448F-942B-BA4415CA50FF
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: ca6d4f1ec60a80b483c79dd75267144e67d80c0b
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403497"
---
# <a name="xamarinessentials-preferences"></a>Xamarin.Essentials: 기본 설정

![시험판 버전 NuGet](~/media/shared/pre-release.png)

합니다 **기본 설정** 클래스는 응용 프로그램의 기본 키/값 저장소에 저장 합니다.

## <a name="using-preferences"></a>기본 설정을 사용 하 여

클래스에서 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

에 대 한 값을 저장 하는 주어진 _키_ 기본 설정에서:

```csharp
Preferences.Set("my_key", "my_value");
```

그렇지 않은 경우 기본 설정이 나 기본값에서 값을 검색 하려면 다음을 설정 합니다.

```csharp
var myValue = Preferences.Get("my_key", "default_value");
```

제거할 합니다 _키_ 기본 설정에서:

```csharp
Preferences.Remove("my_key");
```

모든 기본 설정을 제거 합니다.

```csharp
Preferences.Clear();
```

이러한 방법 외에도 각각 사용 된 선택적 `sharedName` 기본 설정에 대 한 추가 컨테이너를 만드는 데 사용할 수는 있습니다. 아래 플랫폼 구현 세부 정보를 읽습니다.

## <a name="supported-data-types"></a>지원 되는 데이터 형식

다음 데이터 형식에서 지원 됩니다 **기본 설정**:

- **bool**
- **double**
- **int**
- **float**
- **long**
- **string**
- **DateTime**

## <a name="implementation-details"></a>구현 세부 정보

값 `DateTime` 정의한 두 가지 메서드를 사용 하는 64 비트 이진 파일 (long 정수) 형식으로 저장 됩니다는 `DateTime` 클래스: 합니다 [ `ToBinary` ](xref:System.DateTime.ToBinary) 메서드는 인코딩하는 데는 `DateTime` 값과는 [ `FromBinary` ](xref:System.DateTime.FromBinary(System.Int64)) 메서드 값을 디코딩합니다. 경우에 디코딩된 생성 될 수 있는 조정에 대 한 이러한 메서드의 설명서 값 참조를 `DateTime` 는 저장 된 utc (협정 세계시) 값이 아닙니다.

## <a name="platform-implementation-specifics"></a>플랫폼 구현 세부 정보

# <a name="androidtabandroid"></a>[Android](#tab/android)

모든 데이터에 저장 됩니다 [기본 설정 공유](https://developer.android.com/training/data-storage/shared-preferences.html)합니다. 없으면 `sharedName` 지정 공유 기본 설정을 사용 되 고, 그렇지 않으면 이름을 가져오는 데 사용 되는 **개인** 지정 된 이름의 기본 설정을 공유 합니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

[NSUserDefaults](https://docs.microsoft.com/en-us/xamarin/ios/app-fundamentals/user-defaults) iOS 장치에서 값을 저장 하는 데 사용 됩니다. 없으면 `sharedName` 지정 합니다 `StandardUserDefaults` 는 사용 하 고, 그렇지 않으면 이름을 만드는 데 사용 됩니다 새 `NSUserDefaults` 에 사용 되는 지정 된 이름의 `NSUserDefaultsType.SuiteName`합니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[ApplicationDataContainer](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdatacontainer) 장치의 값을 저장 하는 데 사용 됩니다. 없으면 `sharedName` 지정 합니다 `LocalSettings` 는 사용 하면 다른 이름이 사용 됩니다 내에 새 컨테이너를 만들려면 `LocalSettings`합니다.

--------------

## <a name="limitations"></a>제한 사항

문자열에 저장 하는 경우이 API는 적은 양의 텍스트를 저장할 것입니다.  성능을 사용 하 여 많은 양의 텍스트를 저장 하려는 경우 거쳤으며 평균 이하의 수도 있습니다.

## <a name="api"></a>API

- [기본 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Preferences)
- [기본 설정 API 설명서](xref:Xamarin.Essentials.Preferences)
