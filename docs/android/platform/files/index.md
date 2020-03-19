---
title: Xamarin.Android를 사용한 파일 액세스
description: 이 가이드에서는 Xamarin.Android에서의 파일 액세스에 대해 설명합니다.
ms.prod: xamarin
ms.assetid: FC1CFC58-B799-4DD6-8ED1-DE36B0E56856
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/23/2018
ms.openlocfilehash: 1bb0fae73a1e3647cdc0e3266c7b44ac04fcc1ee
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79303668"
---
# <a name="file-storage-and-access-with-xamarinandroid"></a>Xamarin.Android를 사용한 파일 스토리지 및 액세스

Android 앱에 적용되는 공통된 요구 사항은 그림 저장, 문서 다운로드, 다른 프로그램과의 공유를 위한 데이터 내보내기와 같은 파일 조작을 수행할 수 있어야 한다는 것입니다.&ndash; Linux를 기반으로 하는 Android는 파일 스토리지를 위한 공간을 제공하여 이 요구 사항을 지원합니다. Android는 파일 시스템을 다음과 같은 두 가지 유형의 스토리지로 분류합니다.

* **내부 스토리지** &ndash; 파일 시스템에서 애플리케이션 또는 운영 체제에 의해서만 액세스 가능한 부분입니다.
* **외부 스토리지** &ndash; 모든 앱, 사용자, 그리고 (경우에 따라) 다른 디바이스에서 액세스 가능한 파일을 저장하기 위한 파티션입니다. 일부 디바이스에서는 외부 스토리지가 이동식(예: SD 카드)인 경우도 있습니다.

이는 단지 개념상의 분류일 뿐이며, 반드시 디바이스의 특정 파티션 또는 디렉터리를 가리키는 것은 아닙니다. Android 디바이스는 항상 내부 스토리지와 내부 스토리지를 위한 파티션을 제공합니다. 일부 디바이스에는 외부 스토리지로 간주되는 파티션이 여러 개 있을 수 있습니다. 파티션의 구성과 관계없이 파일 읽기, 쓰기 또는 만들기를 위한 API는 동일합니다. Xamarin.Android 애플리케이션이 파일 액세스를 위해 사용할 수 있는 API에는 다음과 같은 두 가지 종류가 있습니다.

1. **.NET API(Mono에서 제공하고 Xamarin.Android에 의해 래핑됨)** &ndash;  여기에는 [Xamarin.Essentials](~/essentials/index.md?context=xamarin/android)에서 제공하는 [파일 시스템 도우미](~/essentials/file-system-helpers.md?context=xamarin/android)가 포함됩니다. .NET API는 가장 좋은 플랫폼 간 호환성을 제공합니다. 따라서 이 가이드에서는 .NET API를 중점적으로 살펴봅니다.
1. **네이티브 Java 파일 액세스 API(Java에서 제공하고 Xamarin.Android에 의해 래핑됨)** &ndash; Java는 파일 읽기 및 쓰기를 위한 자체 API를 제공합니다. 네이티브 Java 파일 액세스 API는 얼마든지 .NET API 대신 사용할 수 있긴 하나, Android에만 적용되며 플랫폼 간 호환성을 지원하지 않습니다.

Xamarin.Android의 파일 읽기 및 쓰기는 다른 모든 .NET 애플리케이션의 파일 읽기 및 쓰기와 거의 동일합니다. Xamarin.Android 앱이 조작할 파일의 경로를 확인한 다음에 표준 .NET 관용구를 사용하여 파일에 액세스합니다. 내부 스토리지와 외부 스토리지의 실제 경로는 디바이스별로 또는 Android 버전별로 다를 수 있으므로 파일 경로를 하드 코딩하는 것은 권장되지 않습니다. 그 대신 Xamarin.Android API를 사용하여 파일 경로를 확인하시기 바랍니다. 이렇게 하면 파일 읽기 및 쓰기를 위한 .NET API가 내부 스토리지와 외부 스토리지의 파일 경로를 확인하는 데 도움이 되는 네이티브 Android API를 노출합니다.

파일 액세스와 관련된 API를 살펴보기에 앞서 내부 스토리지 및 외부 스토리지에 관한 몇 가지 정보를 이해하는 것이 중요합니다. 다음 섹션에서 내부 스토리지와 외부 스토리지에 대해 알아봅니다.

## <a name="internal-vs-external-storage"></a>내부 스토리지와 외부 스토리지

내부 스토리지와 외부 스토리지는 둘 다 Xamarin.Android 앱이 파일을 저장할 수 있는 공간이라는 점에서 개념상 매우 비슷합니다.&ndash; 이와 같은 유사성으로 인해 앱에서 내부 스토리지와 외부 스토리지를 각각 어떤 경우에 사용해야 하는지 명확하지 않기 때문에 Android에 익숙하지 않은 개발자라면 혼란스러울 수 있습니다.

내부 스토리지는 Android가 운영 체제, APK 및 개별 앱에 할당하는 비휘발성 메모리를 가리킵니다. 이 공간은 운영 체제와 앱에서만 액세스할 수 있습니다. Android는 내부 스토리지 파티션에 각 앱의 디렉터리를 할당합니다. 앱이 제거되면 내부 스토리지에서 해당 디렉터리에 저장되었던 모든 파일도 삭제됩니다. 내부 스토리지는 앱에서만 액세스할 수 있고 다른 앱과 공유되지 않으며 앱이 제거되면 쓸모가 없어지는 파일에 가장 적합합니다. Android 6.0 이상에서는 내부 스토리지에 있는 파일이 [Android 6.0의 자동 백업 기능](https://developer.android.com/guide/topics/data/autobackup)을 사용하여 Google에 의해 자동으로 백업될 수 있습니다. 내부 스토리지에는 다음과 같은 단점이 있습니다.

* 파일이 공유될 수 없습니다.
* 앱이 제거되면 파일이 삭제됩니다.
* 내부 스토리지에서 사용할 수 있는 공간이 제한되어 있습니다.

외부 스토리지는 내부 스토리지가 아니며 앱이 아니어도 액세스 가능한 파일 스토리지를 가리킵니다. 외부 스토리지의 주된 목적은 앱 간에 공유되어야 하는 파일이나 내부 스토리지에 저장하기에 너무 큰 파일을 저장할 공간을 제공하는 것입니다. 외부 스토리지의 장점은 일반적으로 내부 스토리지보다 훨씬 많은 파일 저장 공간이 있다는 것입니다. 그러나 디바이스에 항상 외부 스토리지가 있을 것이라고 보장할 수 없으며, 사용자가 외부 스토리지에 액세스하려면 특수 권한이 필요할 수 있습니다.

> [!NOTE]
> 여러 사용자를 지원하는 디바이스의 경우, Android는 내부 스토리지와 외부 스토리지 양쪽에서 각 사용자에게 자체 디렉터리를 제공합니다. 이 디렉터리는 해당 디바이스를 사용하는 다른 사용자가 액세스할 수 없습니다. 이와 같은 분리는 내부 스토리지 또는 외부 스토리지에 있는 파일의 경로가 하드 코딩되어 있지 않은 한 앱에서 구분할 수 없습니다.

경험적인 규칙상, Xamarin.Android 앱은 적절한 경우 항상 내부 스토리지에 파일을 저장해야 하며, 파일을 다른 앱과 공유해야 하는 경우, 파일이 매우 큰 경우 또는 앱이 제거된 후에도 파일이 유지되어야 하는 경우에 외부 스토리지를 사용해야 합니다. 예를 들어, 구성 파일은 해당 파일을 만든 앱에만 중요하므로 내부 스토리지에 저장하는 것이 적합합니다.  반면에 사진은 외부 스토리지에 저장하기 좋은 데이터입니다. 사진은 크기가 매우 클 수 있으며 사용자가 사진을 공유하거나 앱이 제거된 후에도 액세스하려는 경우가 많기 때문입니다.

이 가이드에서는 내부 스토리지를 중점적으로 다룹니다. Xamarin.Android 애플리케이션에서 외부 스토리지를 사용하는 방법은 [외부 스토리지](~/android/platform/files/external-storage.md) 가이드를 참조하세요.

## <a name="working-with-internal-storage"></a>내부 스토리지 사용

애플리케이션의 내부 스토리지 디렉터리는 운영 체제에 의해 결정되며 `Android.Content.Context.FilesDir` 속성을 통해 Android 앱에 노출됩니다. 이 속성은 Android가 해당 앱 전용으로 지정한 디렉터리를 나타내는 `Java.IO.File` 개체를 반환합니다.  예를 들어, 패키지 이름이 **com.companyname**인 앱의 내부 스토리지 디렉터리는 다음과 같을 수 있습니다.

```bash
/data/user/0/com.companyname/files
```

이 문서에서는 내부 스토리지 디렉터리를 _INTERNAL\_STORAGE_라고 부르겠습니다.

> [!IMPORTANT]
> 내부 스토리지 디렉터리의 정확한 경로는 디바이스별로, 그리고 Android 버전별로 다를 수 있습니다. 이 때문에 앱에서 내부 파일 스토리지 디렉터리의 경로를 하드 코딩해서는 안 되며, 그 대신 `System.Environment.GetFolderPath()`와 같은 Xamarin.Android API를 사용해야 합니다.

코드 공유를 최대화할 수 있도록 Xamarin.Android 앱(또는 Xamarin.Android를 대상으로 하는 Xamarin.Forms 앱)은 [`System.Environment.GetFolderPath()`](xref:System.Environment.GetFolderPath*) 메서드를 사용해야 합니다. Xamarin.Android에서 이 메서드는 `Android.Content.Context.FilesDir`과 동일한 위치인 디렉터리를 나타내는 문자열을 반환합니다. 이 메서드는 운영 체제가 사용하는 특수 폴더의 경로를 나타내는 일련의 열거형 상수를 식별하는 데 사용되는 열거형 `System.Environment.SpecialFolder`를 받습니다. `System.Environment.SpecialFolder` 값이 전부 Xamarin.Android의 유효한 디렉터리에 매핑되는 것은 아닙니다. 다음 표에서는 지정된 `System.Environment.SpecialFolder` 값에 대해 예상할 수 있는 경로를 보여 줍니다.

| System.Environment.SpecialFolder | 경로  |
|----------------------|---|
| `ApplicationData` | **_INTERNAL\_STORAGE_/.config** |
| `Desktop` | **_INTERNAL\_STORAGE_/Desktop** |
| `LocalApplicationData` | **_INTERNAL\_STORAGE_/.local/share** |
| `MyDocuments` | **_INTERNAL\_STORAGE_** |
| `MyMusic` | **_INTERNAL\_STORAGE_/Music** |
| `MyPictures` | **_INTERNAL\_STORAGE_/Pictures** |
| `MyVideos` | **_INTERNAL\_STORAGE_/Videos** |
| `Personal` | **_INTERNAL\_STORAGE_** |

### <a name="reading-or-writing-to-files-on-internal-storage"></a>내부 스토리지에 있는 파일 읽기 또는 쓰기

[파일 쓰기 C# API](https://docs.microsoft.com/dotnet/csharp/programming-guide/file-system/how-to-write-to-a-text-file)라면 모두 사용할 수 있습니다. 이때 필요한 것은 애플리케이션에 할당된 디렉터리에 있는 파일의 경로를 가져오는 것입니다. 파일 액세스가 주 스레드를 차단하여 발생할 수 있는 문제를 최소화하기 위해 .NET API의 비동기 버전을 사용하는 것이 좋습니다.

다음 코드 조각은 애플리케이션의 내부 스토리지 디렉터리에 있는 UTF-8 텍스트 파일에 정수를 쓰는 예입니다.

```csharp
public async Task SaveCountAsync(int count)
{
    var backingFile = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.Personal), "count.txt");
    using (var writer = File.CreateText(backingFile))
    {
        await writer.WriteLineAsync(count.ToString());
    }
}
```

다음 코드 조각은 텍스트 파일에 저장된 정수 값을 읽는 한 가지 방법을 보여 줍니다.

```csharp
public async Task<int> ReadCountAsync()
{
    var backingFile = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.Personal), "count.txt");

    if (backingFile == null || !File.Exists(backingFile))
    {
        return 0;
    }

    var count = 0;
    using (var reader = new StreamReader(backingFile, true))
    {
        string line;
        while ((line = await reader.ReadLineAsync()) != null)
        {
            if (int.TryParse(line, out var newcount))
            {
                count = newcount;
            }
        }
    }

    return count;
}
```

## <a name="using--xamarinessentials-ndash-file-system-helpers"></a>Xamarin.Essentials 사용하기 &ndash; 파일 시스템 도우미

[Xamarin.Essentials](~/essentials/file-system-helpers.md?context=xamarin/android)는 플랫폼 간 호환되는 코드를 쓰기 위한 API 집합입니다. [파일 시스템 도우미](~/essentials/file-system-helpers.md?context=xamarin/android)는 애플리케이션의 캐시 및 데이터 디렉터리를 찾는 작업을 간소화해 주는 일련의 도우미가 포함된 클래스입니다. 다음 코드 조각은 앱의 내부 스토리지 디렉터리와 캐시 디렉터리를 찾는 방법을 보여 주는 예입니다.

```csharp
// Get the path to a file on internal storage
var backingFile = Path.Combine(Xamarin.Essentials.FileSystem.AppDataDirectory, "count.txt");

// Get the path to a file in the cache directory
var cacheFile = Path.Combine(Xamarin.Essentials.FileSystem.CacheDirectory, "count.txt");
```

## <a name="hiding-files-from-the-mediastore"></a>`MediaStore`로부터 파일 숨기기

`MediaStore`는 Android 디바이스의 미디어 파일(동영상, 음악, 이미지)에 대한 메타데이터를 수집하는 Android 구성 요소입니다. MediaStore의 목적은 디바이스에 있는 모든 Android 앱 간의 파일 공유를 간소화하는 것입니다.

프라이빗 파일은 공유 가능한 미디어로 표시되지 않습니다. 예를 들어, 앱이 프라이빗 외부 스토리지에 저장한 그림 파일은 미디어 스캐너(`MediaStore`)에 표시되지 않습니다.

퍼블릭 파일은 `MediaStore`에 표시됩니다. 0바이트 파일 이름 **.NOMEDIA**를 갖는 디렉터리는 `MediaStore`에 의해 검색되지 않습니다.

## <a name="related-links"></a>관련 링크

* [외부 스토리지](~/android/platform/files/external-storage.md)
* [디바이스 스토리지에 파일 저장](https://developer.android.com/training/data-storage/files)
* [Xamarin.Essentials: 파일 시스템 도우미](~/essentials/file-system-helpers.md?context=xamarin/android)
* [자동 백업으로 사용자 데이터 백업](https://developer.android.com/guide/topics/data/autobackup)
* [채택 가능한 스토리지](https://source.android.com/devices/storage/adoptable)
