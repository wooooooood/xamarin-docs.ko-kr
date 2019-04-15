---
title: Xamarin.Android 사용 하 여 파일 액세스
description: 이 가이드에서는 Xamarin.Android에서 파일 액세스에 설명
ms.prod: xamarin
ms.assetid: FC1CFC58-B799-4DD6-8ED1-DE36B0E56856
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/23/2018
ms.openlocfilehash: 2978f0b2bcbdd463876784a9addd7dec055b8af9
ms.sourcegitcommit: 91a4fcb715506e18e8070bc89bf2cb14d079ad32
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/15/2019
ms.locfileid: "59574821"
---
# <a name="file-storage-and-access-with-xamarinandroid"></a>파일 저장소 및 Xamarin.Android 사용 하 여 액세스

파일을 조작 하는 Android 앱에 대 한 일반적인 요구 사항은 &ndash; 그림을 저장, 문서, 다운로드 또는 다른 프로그램과 공유 하는 데이터 내보내기. Android (기반으로 하는 Linux) 파일 저장소에 대 한 공간을 제공 함으로써이 지원 합니다. Android는 두 가지 유형의 저장소에 파일 시스템을 그룹:

* **내부 저장소** &ndash; 응용 프로그램 또는 운영 체제에 의해서만 액세스할 수 있는 파일 시스템의 일부입니다.
* **외부 저장소** &ndash; 이 모든 앱, 사용자 및 다른 장치에서 액세스할 수 있는 파일의 저장소에 대 한 파티션입니다. 일부 장치에서 외부 저장소 (예: SD 카드) 이동식 수 있습니다.

이러한 그룹화는 개념 뿐 및 단일 파티션 또는 장치의 디렉터리에 반드시 참조 하지 않습니다. Android 장치는 항상 내부 및 외부 저장소에 대 한 파티션을 제공 합니다. 특정 장치 여러 파티션으로 간주 되는 외부 저장소에 있을 가능성이 있습니다. 파티션을 읽기에 대 한 Api에 관계 없이 파일을 만들거나를 작성 하는 같습니다. Xamarin.Android 응용 프로그램 파일 액세스에 사용할 수 있는 Api의 두 가지 있습니다.

1. **(Mono에서 제공 하 고 Xamarin.Android에서 래핑될).NET Api** &ndash; 포함 된 [파일 시스템 도우미](~/essentials/file-system-helpers.md?context=xamarin/android) 제공한 [Xamarin.Essentials](~/essentials/index.md?context=xamarin/android)합니다. .NET Api는 플랫폼 간 호환성을 제공 하 고 이러한 Api에서이 가이드의 포커스를 같이 사용 됩니다.
1. **네이티브 Java 파일 액세스 Api (Java 제공한 및 Xamarin.Android에서 래핑된)** &ndash; Java 파일 읽기 및 쓰기에 대 한 자체 Api를 제공 합니다. 이러한.NET Api 대안을 완전히 허용 되는 있지만 관련이 있으며 Android 플랫폼 간 수 있는 응용 프로그램에 적합 하지 않습니다.

다른.NET 응용 프로그램에 그대로 읽고 파일에 쓰기는 Xamarin.Android에서 거의 동일 합니다. Xamarin.Android 앱에는 다음 사용 하 여 표준.NET 관용구 파일 액세스에 대 한 파일을 조작할 수는 있는 경로를 결정합니다. Android 버전에서 Android 또는 내부 및 외부 저장소에 실제 경로 장치 마다 다를 수 있으므로 좋지 않습니다 하드 코드 된 파일의 경로를 합니다. 대신, 파일의 경로를 확인 하려면 Xamarin.Android Api를 사용 합니다. 이런 방식으로 파일 읽기 및 쓰기에 대 한.NET Api를 내부 및 외부 저장소에 파일의 경로를 지정 하는 데 도움이 되는 네이티브 Android Api를 노출 합니다.

가 파일 액세스와 관련 된 Api를 다루기 전에 내부 및 외부 저장소 관련 세부 정보 중 일부를 이해 해야 합니다. 다음 섹션에서 설명 합니다.

## <a name="internal-vs-external-storage"></a>내부 vs 외부 저장소

내부 및 외부 저장소는 매우 유사 개념적으로 &ndash; 는 두 위치 모두는 Xamarin.Android 앱 파일을 저장할 수 있습니다. 이 유사성 앱 내부 vs 외부 저장소를 사용 해야 하는 경우 명확 하지 않습니다 하는 대로 Android에 익숙하지 않은 개발자에 대 한 혼동을 줄 수 있습니다.

내부 저장소 Android Apk 운영 체제에 대 한 개별 앱에 대해 할당 하는 비휘발성 메모리를 가리킵니다. 이 공간을 운영 체제 또는 앱에서 제외 하 고 액세스할 수 없는 경우 Android에는 각 앱에 대 한 디렉터리 내부 저장소 파티션에 할당 됩니다. 앱을 제거 하면 해당 디렉터리에 내부 저장소에 유지 되는 모든 파일 삭제도 됩니다. 내부 저장소만 앱에 액세스할 수 있으며 다른 앱과 공유 되지 것입니다 또는 앱이 제거 되 면 거의 값이 포함 됩니다는 파일에 가장 적합 합니다. Android 6.0 이상에서는 파일 내부 저장소에 백업할 수 있는 자동으로 Google을 사용 하 여 합니다 [Android 6.0에 자동 백업 기능](https://developer.android.com/guide/topics/data/autobackup)합니다. 내부 저장소에 다음과 같은 단점이 있습니다.

* 파일을 공유할 수 없습니다.
* 앱을 제거할 때 파일이 삭제 됩니다.
* 아마도 제한 하는 내부 저장소에서 사용할 수 있는 공간입니다.

외부 저장소 참조를 내부 저장소 없는 파일 저장소로 앱에만 액세스할 수 없습니다. 외부 저장소의 기본 목적은를 앱 간에 공유할 수 즉 또는 너무 커서 내부 저장소에 파일을 넣을 위치를 제공 하도록 합니다. 외부 저장소의 장점은 내부 저장소 보다 훨씬 더 많은 공간이 파일에 대 한 일반적으로 있는지 한다는 점입니다. 그러나 외부 저장소 장치에 표시 하기 위해 항상 보장 되지 않으며 액세스 하는 데 사용자의 특별 한 권한이 필요할 수 있습니다.

> [!NOTE]
> 여러 사용자를 지 원하는 장치를 Android 제공 됩니다 각 사용자가 자신의 디렉터리 내부 및 외부 저장소에서. 이 디렉터리에 다른 사용자가 장치에서 액세스할 수 없는 경우 이 분리 파일 내부 또는 외부 저장소에 하드 코드 경로가 아니라 그럴으로 앱에 표시 되지 않습니다.

일반적으로 Xamarin.Android 앱, 적절 한 경우 내부 저장소에 파일을 저장 하는 것을 선호 하 고 파일 다른 앱과 공유 해야 할 매우 큰 또는 해당 앱을 제거 하는 경우에 유지 해야 하는 경우 외부 저장소를 사용 해야 합니다. 예를 들어 구성 파일은가 만든 앱을 제외 하 고 중요 하지 내부 저장소에 가장 적합 합니다.  반면, 사진은 외부 저장소에 대 한 좋은 후보입니다. 매우 큰 수 있으며 대부분의 사용자 수를 공유 하거나 앱을 제거 하는 경우에 액세스할.

이 가이드는 내부 저장소를 중점적으로 합니다. 이 가이드를 참조 하세요 [외부 저장소](~/android/platform/files/external-storage.md) 외부 저장소를 사용 하 여 Xamarin.Android 응용 프로그램에 대 한 자세한 내용은 합니다.

## <a name="working-with-internal-storage"></a>내부 저장소를 사용 하 여 작업

응용 프로그램에 대 한 내부 저장소 디렉터리 운영 체제에 의해 결정 됩니다 및 Android 앱에 노출 되는 `Android.Content.Context.FilesDir` 속성입니다. 이 반환 됩니다는 `Java.IO.File` Android 앱에 대 한 단독으로 전용가 있는 디렉터리를 나타내는 개체입니다.  예를 들어 패키지 이름을 사용 하 여 앱 **com.companyname** 내부 저장소 디렉터리 수 있습니다.

```bash
/data/user/0/com.companyname/files
```

이 문서는 내부 저장소 디렉터리를 가리킵니다 _내부\_저장소_합니다.

> [!IMPORTANT]
> 내부 저장소 디렉터리에 정확한 경로 장치를 Android 버전 간의 장치에서 달라질 수 있습니다. 이 인해 앱 해야 하지 하드 코드 내부 파일 저장소 디렉터리에 경로 및와 같은 Xamarin.Android Api를 대신 사용 `System.Environment.GetFolderPath()`합니다.

코드 공유 최대화, Xamarin.Android 앱 (또는 Xamarin.Android를 대상으로 하는 Xamarin.Forms 앱) 사용 해야 합니다 [ `System.Environment.GetFolderPath()` ](xref:System.Environment.GetFolderPath*) 메서드. 이 메서드는 디렉터리와 동일한 위치에 대 한 문자열을 반환 하는 데 xamarin.android에서 `Android.Content.Context.FilesDir`합니다. 이 메서드는 열거형을 `System.Environment.SpecialFolder`에 운영 체제에서 사용 되는 특수 폴더의 경로 나타내는 열거 된 상수의 집합을 식별 하는 데 사용 됩니다. 일부는 `System.Environment.SpecialFolder` 값 Xamarin.Android에서 올바른 디렉터리에 매핑됩니다. 다음 표에서 지정된 된 값에 대 한 어떤 경로 사용할 수 `System.Environment.SpecialFolder`:

| System.Environment.SpecialFolder | Path  |
|----------------------|---|
| `ApplicationData` | **_INTERNAL\_STORAGE_/.config** |
| `Desktop` | **_INTERNAL\_STORAGE_/Desktop** |
| `LocalApplicationData` | **_INTERNAL\_STORAGE_/.local/share** |
| `MyDocuments` | **_INTERNAL\_STORAGE_** |
| `MyMusic` | **_INTERNAL\_STORAGE_/Music** |
| `MyPictures` | **_INTERNAL\_STORAGE_/Pictures** |
| `MyVideos` | **_INTERNAL\_STORAGE_/Videos** |
| `Personal` | **_INTERNAL\_STORAGE_** |


### <a name="reading-or-writing-to-files-on-internal-storage"></a>읽기 또는 쓰기를 내부 저장소의 파일

중 하나는 [ C# 작성 하기 위한 Api](https://docs.microsoft.com/dotnet/csharp/programming-guide/file-system/how-to-write-to-a-text-file) 파일에는 응용 프로그램에 할당 된 디렉터리에 있는 파일의 경로 필요한 모든 것 만으로는 충분 합니다. .NET Api의 버전은 수 있는 문제를 최소화 하는 데 사용 되는 비동기 주 스레드를 차단 하는 파일 액세스를 사용 하 여 연결 하는 것이 좋습니다.

이 코드 조각은 다음과 같습니다. 응용 프로그램의 내부 저장소 디렉터리에 utf-8 텍스트 파일에는 정수를 작성 하는 한 가지 예

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

다음 코드 조각은 제공 텍스트 파일에 저장 된 정수 값을 읽을 수 있습니다.

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

## <a name="using--xamarinessentials-ndash-file-system-helpers"></a>Xamarin.Essentials를 사용 하 여 &ndash; 파일 시스템 도우미

[Xamarin.Essentials](~/essentials/file-system-helpers.md?context=xamarin/android) 집합이 Api 플랫폼 간 호환 코드를 작성 합니다. 합니다 [파일 시스템 도우미](~/essentials/file-system-helpers.md?context=xamarin/android) 도우미를 간단히 응용 프로그램의 캐시 및 데이터 디렉터리를 찾기 위해 계열을 포함 하는 클래스입니다. 이 코드 조각은 앱에 대 한 내부 저장소 디렉터리 및 캐시 디렉터리를 찾는 방법의 예제를 제공 합니다.

```csharp
// Get the path to a file on internal storage
var backingFile = Path.Combine(Xamarin.Essentials.FileSystem.AppDataDirectory, "count.txt");

// Get the path to a file in the cache directory
var cacheFile = Path.Combine(Xamarin.Essentials.FileSystem.CacheDirectory, "count.txt");
```

## <a name="hiding-files-from-the-mediastore"></a>파일을 숨기는 `MediaStore`

`MediaStore` 는 Android 장치에서 미디어 파일 (비디오, 음악, 이미지)에 대 한 메타 데이터를 수집 하는 Android 구성 요소입니다. 용도 장치의 모든 Android 앱에서 이러한 파일의 공유를 간소화 합니다.

개인 파일 공유할 수 있는 미디어로 표시 되지 않습니다. 예를 들어, 그림 해당 개인 외부 저장소에 저장 하는 앱, 하는 경우 다음 해당 파일은 선택 되지 미디어 스캐너에서 (`MediaStore`).

공용 파일에서 선택 됩니다 `MediaStore`합니다. 0 바이트 파일 이름을 가진 디렉터리 **합니다. NOMEDIA** 하 여 검색 되지 것입니다 `MediaStore`합니다.

## <a name="related-links"></a>관련 링크

* [외부 스토리지](~/android/platform/files/external-storage.md)
* [장치 저장소에 파일을 저장](https://developer.android.com/training/data-storage/files)
* [Xamarin.Essentials 파일 시스템 도우미](~/essentials/file-system-helpers.md?context=xamarin/android)
* [자동 백업을 사용 하 여 백업 사용자 데이터](https://developer.android.com/guide/topics/data/autobackup)
* [만드는 저장소](https://source.android.com/devices/storage/adoptable)
