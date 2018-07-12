---
title: 'Xamarin.Essentials: 파일 시스템 도우미'
description: Xamarin.Essentials에서 파일 시스템 클래스는 다양 한 응용 프로그램의 캐시 및 데이터 디렉터리를 찾고 앱 패키지 내에서 파일을 열기 위해 도우미를 포함 합니다.
ms.assetid: B3EC2DE0-EFC0-410C-AF71-7410AE84CF84
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 13293ec05261cbdc1e70fd278002d1af18654851
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815620"
---
# <a name="xamarinessentials-file-system-helpers"></a>Xamarin.Essentials: 파일 시스템 도우미

![시험판 버전 NuGet](~/media/shared/pre-release.png)

합니다 **FileSystem** 클래스 일련의 응용 프로그램의 캐시 및 데이터 디렉터리를 찾고 앱 패키지 내에서 파일을 열기 위해 도우미를 포함 합니다.

## <a name="using-file-system-helpers"></a>파일 시스템 도우미를 사용 하 여

클래스에서 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

응용 프로그램을 저장할 디렉터리를 가져오려고 **데이터 캐시**합니다. 임시 데이터를 보다 오래 유지 해야 하지만 제대로 작동 하는 데 필요한 데이터 하지 않아야 하는 모든 데이터에 대해 캐시 데이터를 사용할 수 있습니다.

```csharp
var cacheDir = FileSystem.CacheDirectory;
```

사용자 데이터 파일 되지 않은 모든 파일에 대 한 응용 프로그램의 최상위 디렉터리를 가져오려면. 이러한 파일은 프레임 워크를 동기화 하는 운영 체제를 사용 하 여 백업 됩니다. 플랫폼 구현 세부 사항은 아래를 참조 하세요.

```csharp
var mainDir = FileSystem.AppDataDirectory;
```

응용 프로그램 패키지에 번들로 제공 되는 파일을 열려면:

```csharp
 using (var stream = await FileSystem.OpenAppPackageFileAsync(templateFileName))
 {
    using (var reader = new StreamReader(stream))
    {
        var fileContents = await reader.ReadToEndAsync();
    }
 }
```

## <a name="platform-implementation-specifics"></a>플랫폼 구현 세부 정보

# <a name="androidtabandroid"></a>[Android](#tab/android)

- **CacheDirectory** -반환 된 [CacheDir](https://developer.android.com/reference/android/content/Context.html#getCacheDir) 현재 컨텍스트의 합니다.
- **AppDataDirectory** – 반환 합니다 [FilesDir](https://developer.android.com/reference/android/content/Context.html#getFilesDir) 되며 현재 컨텍스트에서의 사용 하 여 백업할 [자동 백업](https://developer.android.com/guide/topics/data/autobackup.html) API 23 이상 시작 합니다.

모든 파일을 추가 합니다 **자산** 폴더 Android에서 프로젝트 및 빌드 작업으로 표시 **AndroidAsset** 와 함께 사용 하 `OpenAppPackageFileAsync`합니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

- **CacheDirectory** -반환 된 [라이브러리/캐시](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) 디렉터리입니다.
- **AppDataDirectory** -반환 된 [라이브러리](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) iTunes 및 iCloud 백업 되는 디렉터리입니다.

모든 파일을 추가 합니다 **리소스** iOS에서 폴더 프로젝트 및 빌드 작업으로 표시 **BundledResource** 와 함께 사용 하 `OpenAppPackageFileAsync`합니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

- **CacheDirectory** -반환 된 [LocalCacheFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localcachefolder#Windows_Storage_ApplicationData_LocalCacheFolder) 디렉터리...
- **AppDataDirectory** -반환 된 [LocalFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localfolder#Windows_Storage_ApplicationData_LocalFolder) 클라우드로 백업 되는 디렉터리입니다.

UWP 프로젝트의 루트에 파일을 추가 하 고 빌드 작업으로 표시 **콘텐츠** 하는 데 `OpenAppPackageFileAsync`합니다.

--------------

## <a name="api"></a>API

- [파일 시스템 도우미 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/FileSystem)
- [파일 시스템 API 설명서](xref:Xamarin.Essentials.FileSystem)
