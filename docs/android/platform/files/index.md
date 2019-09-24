---
title: Xamarin.ios를 사용 하 여 파일 액세스
description: 이 가이드에서는 Xamarin Android의 파일 액세스에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: FC1CFC58-B799-4DD6-8ED1-DE36B0E56856
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/23/2018
ms.openlocfilehash: bf4b0f4ed6ade69808314ac7e7a51270aa3a847e
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70758904"
---
# <a name="file-storage-and-access-with-xamarinandroid"></a>Xamarin Android를 사용 하 여 File Storage 및 액세스

Android 앱에 대 한 일반적인 요구 사항은 사진을 저장 &ndash; 하는 파일을 조작 하거나, 문서를 다운로드 하거나, 다른 프로그램과 공유 하기 위해 데이터를 내보내는 것입니다. Android (Linux 기반)는 파일 저장소에 공간을 제공 하 여이를 지원 합니다. Android는 파일 시스템을 다음과 같은 두 가지 유형의 저장소로 그룹화 합니다.

* **내부 저장소** &ndash; 응용 프로그램 또는 운영 체제 에서만 액세스할 수 있는 파일 시스템의 일부입니다.
* **외부 저장소** &ndash; 모든 앱, 사용자 및 기타 장치에서 액세스할 수 있는 파일의 저장소에 대 한 파티션입니다. 일부 장치에서는 외부 저장소 (예: SD 카드)를 분리할 수 있습니다.

이러한 그룹은 개념적 으로만 사용할 수 있으며 장치에서 단일 파티션 또는 디렉터리를 반드시 참조할 필요는 없습니다. Android 장치는 항상 내부 저장소 및 외부 저장소에 대 한 파티션을 제공 합니다. 특정 장치에 외부 저장소로 간주 되는 파티션이 여러 개 있을 수 있습니다. 파티션에 관계 없이 파일 읽기, 쓰기 또는 만들기를 위한 Api가 동일 합니다. Xamarin Android 응용 프로그램에서 파일 액세스에 사용할 수 있는 두 가지 Api 집합이 있습니다.

1. **.Net api (Mono에서 제공되고 xamarin.ios로 래핑된)** &ndash; 여기에는 [Xamarin.Essentials](~/essentials/index.md?context=xamarin/android)에서 제공하는 [파일 시스템 도우미가](~/essentials/file-system-helpers.md?context=xamarin/android) 포함됩니다. .NET Api는 최고의 플랫폼 간 호환성을 제공 하며이 가이드는 이러한 Api에 중점을 둡니다.
1. **네이티브 java 파일 액세스 api (java에서 제공 하 고 xamarin.ios로 래핑된)** &ndash; Java는 파일 읽기 및 쓰기를 위한 자체 api를 제공 합니다. 이러한 항목은 .NET Api에 대 한 완전 한 대안 이지만 Android에만 해당 되며 플랫폼 간으로 사용 되는 앱에는 적합 하지 않습니다.

파일 읽기 및 쓰기는 다른 .NET 응용 프로그램에 대 한 것 처럼 Xamarin.ios에서 거의 동일 합니다. Xamarin Android 앱은 조작할 파일의 경로를 결정 한 다음 파일 액세스에 표준 .NET 관용구을 사용 합니다. 내부 및 외부 저장소의 실제 경로는 장치 마다 다르거나 Android 버전에서 Android 버전으로 다를 수 있으므로 파일 경로를 하드 코딩 하는 것은 좋지 않습니다. 대신, Xamarin.ios Api를 사용 하 여 파일 경로를 확인 합니다. 이러한 방식으로 파일을 읽고 쓰기 위한 .NET Api는 내부 및 외부 저장소에 있는 파일의 경로를 결정 하는 데 도움이 되는 네이티브 Android Api를 노출 합니다.

파일 액세스와 관련 된 Api에 대해 설명 하기 전에 내부 및 외부 저장소에 대 한 일부 세부 정보를 이해 하는 것이 중요 합니다. 이에 대해서는 다음 섹션에서 설명 합니다.

## <a name="internal-vs-external-storage"></a>내부 vs 외부 저장소

개념적으로 내부 저장소와 외부 저장소는 매우 유사 &ndash; 합니다 .이는 Xamarin Android 앱이 파일을 저장할 수 있는 위치입니다. 이 유사성은 앱에서 내부 저장소와 외부 저장소를 사용 해야 하는 경우 분명 하지 않으므로 Android에 익숙하지 않은 개발자에 게 혼란 스 러 울 수 있습니다.

내부 저장소는 Android에서 운영 체제, APKs 및 개별 앱에 대해 할당 하는 비휘발성 메모리를 나타냅니다. 이 공간은 운영 체제 또는 앱을 제외 하 고는 액세스할 수 없습니다. Android는 각 앱에 대 한 내부 저장소 파티션에 디렉터리를 할당 합니다. 앱이 제거 되 면 해당 디렉터리의 내부 저장소에 유지 되는 모든 파일도 삭제 됩니다. 내부 저장소는 앱에만 액세스할 수 있으며 다른 앱과 공유 되지 않거나 앱이 제거 된 후에는 값이 거의 없는 파일에 가장 적합 합니다. Android 6.0 이상에서는 [android 6.0의 자동 백업 기능](https://developer.android.com/guide/topics/data/autobackup)을 사용 하 여 Google에서 내부 저장소의 파일을 자동으로 백업할 수 있습니다. 내부 저장소의 단점은 다음과 같습니다.

* 파일은 공유할 수 없습니다.
* 앱이 제거 되 면 파일이 삭제 됩니다.
* 내부 저장소에서 사용 가능한 공간이 제한 될 수도 있습니다.

외부 저장소는 내부 저장소가 아니고 앱에 독점적으로 액세스할 수 없는 파일 저장소를 참조 합니다. 외부 저장소의 주요 목적은 앱 간에 공유 될 파일을 배치할 위치를 제공 하는 것입니다. 또는 너무 커서 내부 저장소에 맞출 수 없습니다. 외부 저장소의 장점은 일반적으로 내부 저장소 보다 파일에 대해 훨씬 더 많은 공간을 제공 한다는 것입니다. 그러나 외부 저장소는 항상 장치에 존재 하는 것은 아니므로 사용자가 액세스할 수 있는 특별 한 권한이 필요할 수 있습니다.

> [!NOTE]
> 여러 사용자를 지 원하는 장치의 경우 Android는 내부 및 외부 저장소 모두에서 각 사용자에 게 고유한 디렉터리를 제공 합니다. 장치에 있는 다른 사용자가이 디렉터리에 액세스할 수 없습니다. 이러한 분리는 내부 또는 외부 저장소의 파일에 대 한 경로를 하드 코딩 하지 않는 한 앱에 표시 되지 않습니다.

일반적으로 Xamarin Android 앱은 적절 한 경우에는 내부 저장소에 파일을 저장 하는 것이 좋습니다. 또한 파일이 다른 앱과 공유 되어야 하거나 매우 크거나 앱이 제거 된 경우에도 유지 해야 하는 경우에는 외부 저장소를 사용 하는 것이 좋습니다. 예를 들어 구성 파일은 해당 파일을 만드는 앱을 제외 하 고는 중요 하지 않으므로 내부 저장소에 가장 적합 합니다.  반면 사진은 외부 저장소에 적합 한 후보입니다. 사용자는 앱을 제거 하는 경우에도 매우 클 수 있으며, 사용자가 공유 하거나 액세스할 수 있는 경우가 많습니다.

이 가이드는 내부 저장소에 집중 합니다. Xamarin Android 응용 프로그램에서 외부 저장소를 사용 하는 방법에 대 한 자세한 내용은 [외부 저장소](~/android/platform/files/external-storage.md) 가이드를 참조 하세요.

## <a name="working-with-internal-storage"></a>내부 저장소 작업

응용 프로그램에 대 한 내부 저장소 디렉터리는 운영 체제에 의해 결정 되며, `Android.Content.Context.FilesDir` 속성에 의해 Android 앱에 노출 됩니다. 그러면 Android에서 앱 `Java.IO.File` 전용으로 전용으로 사용 하는 디렉터리를 나타내는 개체가 반환 됩니다.  예를 들어, 이름이 .com 인 응용 프로그램의 경우 내부 저장소 디렉터리는 다음과 같을 수 있습니다 **.**

```bash
/data/user/0/com.companyname/files
```

이 문서에서는 내부 저장소 디렉터리를 _내부\_저장소_로 참조 합니다.

> [!IMPORTANT]
> 내부 저장소 디렉터리에 대 한 정확한 경로는 장치 마다 다르며 Android 버전 사이에 다를 수 있습니다. 따라서 앱은 내부 파일 저장소 디렉터리에 대 한 경로를 하드 코딩 해서는 안 되며, 대신 Xamarin.ios Api를 `System.Environment.GetFolderPath()`사용 해야 합니다.

코드 공유를 최대화 하려면 xamarin android 앱 (또는 xamarin.ios를 대상으로 하는 xamarin.ios 앱)에서 메서드를 [`System.Environment.GetFolderPath()`](xref:System.Environment.GetFolderPath*) 사용 해야 합니다. Xamarin Android에서이 메서드는와 `Android.Content.Context.FilesDir`동일한 위치인 디렉터리에 대 한 문자열을 반환 합니다. 이 메서드는 운영 체제에서 `System.Environment.SpecialFolder`사용 하는 특수 폴더의 경로를 나타내는 열거형 상수 집합을 식별 하는 데 사용 되는 열거형을 사용 합니다. 일부 값은 `System.Environment.SpecialFolder` xamarin.ios의 올바른 디렉터리에 매핑되지 않습니다. 다음 표에서는 지정 된 값 `System.Environment.SpecialFolder`에 대해 예상할 수 있는 경로를 설명 합니다.

| System.Environment.SpecialFolder | 경로  |
|----------------------|---|
| `ApplicationData` | **_INTERNAL\_STORAGE_/.config** |
| `Desktop` | **_INTERNAL\_STORAGE_/Desktop** |
| `LocalApplicationData` | **_INTERNAL\_STORAGE_/.local/share** |
| `MyDocuments` | **_내부\_저장소_** |
| `MyMusic` | **_INTERNAL\_STORAGE_/Music** |
| `MyPictures` | **_내부\_저장소_/사진** |
| `MyVideos` | **_INTERNAL\_STORAGE_/Videos** |
| `Personal` | **_내부\_저장소_** |

### <a name="reading-or-writing-to-files-on-internal-storage"></a>내부 저장소에서 파일 읽기 또는 쓰기

파일에 [ C# 쓰기 위한 api](https://docs.microsoft.com/dotnet/csharp/programming-guide/file-system/how-to-write-to-a-text-file) 는 충분 합니다. 응용 프로그램에 할당 된 디렉터리에 있는 파일의 경로를 가져오는 것만 있으면 됩니다. 비동기 버전의 .NET Api는 주 스레드를 차단 하는 파일 액세스와 관련 될 수 있는 모든 문제를 최소화 하는 데 사용 하는 것이 좋습니다.

이 코드 조각은 UTF-8 텍스트 파일에 정수를 응용 프로그램의 내부 저장소 디렉터리에 기록 하는 한 가지 예입니다.

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

다음 코드 조각은 텍스트 파일에 저장 된 정수 값을 읽을 수 있는 한 가지 방법을 제공 합니다.

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

## <a name="using--xamarinessentials-ndash-file-system-helpers"></a>Xamarin.ios &ndash; 파일 시스템 도우미 사용

[Xamarin.ios](~/essentials/file-system-helpers.md?context=xamarin/android) 는 플랫폼 간 호환 코드를 작성 하는 api 집합입니다. [파일 시스템 도우미](~/essentials/file-system-helpers.md?context=xamarin/android) 는 응용 프로그램의 캐시 및 데이터 디렉터리 찾기를 간소화 하는 일련의 도우미가 포함 된 클래스입니다. 이 코드 조각은 앱에 대 한 내부 저장소 디렉터리 및 캐시 디렉터리를 찾는 방법의 예제를 제공 합니다.

```csharp
// Get the path to a file on internal storage
var backingFile = Path.Combine(Xamarin.Essentials.FileSystem.AppDataDirectory, "count.txt");

// Get the path to a file in the cache directory
var cacheFile = Path.Combine(Xamarin.Essentials.FileSystem.CacheDirectory, "count.txt");
```

## <a name="hiding-files-from-the-mediastore"></a>파일 숨기기`MediaStore`

는 `MediaStore` android 장치에서 미디어 파일 (비디오, 음악, 이미지)에 대 한 메타 데이터를 수집 하는 android 구성 요소입니다. 이를 위해 장치의 모든 Android 앱에서 이러한 파일을 쉽게 공유할 수 있습니다.

개인 파일은 공유 가능한 미디어로 표시 되지 않습니다. 예를 들어 앱이 개인 외부 저장소에 사진을 저장 하는 경우 해당 파일은 미디어 스캐너 (`MediaStore`)에서 선택 되지 않습니다.

공용 파일은에서 `MediaStore`선택 됩니다. 0 바이트 파일 이름이 있는 디렉터리입니다 **. NOMEDIA** 는에 의해 `MediaStore`검색 되지 않습니다.

## <a name="related-links"></a>관련 링크

* [외부 스토리지](~/android/platform/files/external-storage.md)
* [장치 저장소에 파일 저장](https://developer.android.com/training/data-storage/files)
* [Xamarin.ios 파일 시스템 도우미](~/essentials/file-system-helpers.md?context=xamarin/android)
* [자동 백업으로 사용자 데이터 백업](https://developer.android.com/guide/topics/data/autobackup)
* [Adoptable 저장소](https://source.android.com/devices/storage/adoptable)
