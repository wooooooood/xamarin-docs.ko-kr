---
title: Xamarin.ios를 사용 하 여 외부 저장소에 대 한 파일 액세스
description: 이 가이드에서는 Xamarin의 외부 저장소에 대 한 파일 액세스에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 40da10b2-a207-4f9c-a2dd-165d9b662f33
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/23/2018
ms.openlocfilehash: 96b0d6a00c7825939b1f89ed63e3e5559ca4ef59
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020476"
---
# <a name="external-storage"></a>외부 저장소

외부 저장소는 내부 저장소에 있지 않고 파일을 담당 하는 앱에 독점적으로 액세스할 수 없는 파일 저장소를 참조 합니다. 외부 저장소의 주요 목적은 앱 간에 공유 될 파일을 배치할 위치를 제공 하는 것입니다. 또는 너무 커서 내부 저장소에 맞출 수 없습니다.

지금까지 외부 저장소는 SD 카드 (이동식 _저장소_라고도 함)와 같은 이동식 미디어의 디스크 파티션을 참조 했습니다. 이러한 차이는 Android 장치가 진화 하는 것과 관련이 없으며, 많은 Android 장치에서 이동식 저장소를 더 이상 지원 하지 않습니다. 대신 일부 장치는 Android에서 동일한 기능 이동식 미디어를 수행 하는 내부 비휘발성 메모리 중 일부를 할당 합니다. 이를 _에뮬레이트된_ 저장소 라고 하며 여전히 외부 저장소로 간주 됩니다. 또는 일부 Android 장치에 외부 저장소 파티션이 여러 개 있을 수 있습니다. 예를 들어 Android 태블릿 (내부 저장소 외)에는 SD 카드에 대해 에뮬레이트된 저장소 및 하나 이상의 슬롯이 있을 수 있습니다. 이러한 모든 파티션은 Android에서 외부 저장소로 처리 됩니다.

여러 사용자가 있는 장치에서 각 사용자는 외부 저장소에 대 한 기본 외부 저장소 파티션에 전용 디렉터리를 갖게 됩니다. 한 명의 사용자로 실행 되는 앱은 장치에 있는 다른 사용자의 파일에 액세스할 수 없습니다. 모든 사용자에 대 한 파일은 전 세계에서 읽고 쓸 수 있습니다. 그러나 Android는 다른 사용자 프로필에서 각 사용자 프로필을 샌드박스를 사용 합니다.

파일 읽기 및 쓰기는 다른 .NET 응용 프로그램에 대 한 것 처럼 Xamarin.ios에서 거의 동일 합니다. Xamarin Android 앱은 조작할 파일의 경로를 결정 한 다음 파일 액세스에 표준 .NET 관용구을 사용 합니다. 내부 및 외부 저장소의 실제 경로는 장치 마다 다르거나 Android 버전에서 Android 버전으로 다를 수 있으므로 파일 경로를 하드 코딩 하는 것은 좋지 않습니다. 대신 Xamarin.ios는 내부 및 외부 저장소에 있는 파일의 경로를 결정 하는 데 도움이 되는 네이티브 Android Api를 노출 합니다.

이 가이드에서는 외부 저장소와 관련 된 Android의 개념 및 Api에 대해 설명 합니다.

## <a name="public-and-private-files-on-external-storage"></a>외부 저장소의 공용 및 개인 파일

앱에서 외부 저장소에 유지할 수 있는 두 가지 유형의 파일은 다음과 같습니다.

* 개인 파일 **&ndash; 개인 파일** 은 응용 프로그램과 관련 된 파일이 며, 전 세계에서 읽고 쓸 수 있는 파일입니다. Android에서는 개인 파일이 외부 저장소의 특정 디렉터리에 저장 되어야 합니다. 파일이 "개인" 이라고 하더라도 장치에 있는 다른 앱에서 계속 표시 되 고 액세스할 수 있으며, Android에서 특별 한 보호를 사용할 수 없습니다.

* **공용** 파일 &ndash; 이러한 파일은 응용 프로그램에만 적용 되는 것으로 간주 되지 않으며 자유롭게 공유할 수 있습니다.

이러한 파일 간의 차이점은 주로 개념입니다. 개인 파일은 응용 프로그램의 일부로 간주 되는 반면 전용 파일은 외부 저장소에 있는 다른 모든 파일입니다. Android는 개인 및 공용 파일의 경로를 확인 하는 두 가지 Api를 제공 합니다. 그렇지 않으면 동일한 .NET Api를 사용 하 여 이러한 파일을 읽고 쓸 수 있습니다. 이러한 Api는 [읽기 및 쓰기](~/android/platform/files/index.md#reading-or-writing-to-files-on-internal-storage)에 대 한 섹션에서 설명 하는 것과 동일한 api입니다.

### <a name="private-external-files"></a>개인 외부 파일

개인 외부 파일은 응용 프로그램에 특정 한 것으로 간주 되지만 (내부 파일과 유사) 여러 가지 이유로 외부 저장소에 유지 됩니다 (예: 내부 저장소에 비해 너무 큼). 내부 파일과 마찬가지로 이러한 파일은 사용자가 앱을 제거 하면 삭제 됩니다.

`Android.Content.Context.GetExternalFilesDir(string type)`메서드를 호출 하 여 전용 외부 파일의 기본 위치를 찾을 수 있습니다. 이 메서드는 앱에 대 한 개인 외부 저장소 디렉터리를 나타내는 `Java.IO.File` 개체를 반환 합니다. 이 메서드에 `null`를 전달 하면 응용 프로그램의 사용자 저장소 디렉터리에 대 한 경로가 반환 됩니다. 예를 들어 패키지 이름이 `com.companyname.app`인 응용 프로그램의 경우 개인 외부 파일의 "root" 디렉터리는 다음과 같습니다.

```bash
/storage/emulated/0/Android/data/com.companyname.app/files/
```

이 문서에서는 외부 저장소에 있는 개인 파일의 저장소 디렉터리를 _개인\_외부\_저장소_로 참조 합니다.

`GetExternalFilesDir()`에 대 한 매개 변수는 _응용 프로그램 디렉터리_를 지정 하는 문자열입니다. 이 디렉터리는 논리적 파일 구성에 대 한 표준 위치를 제공 하기 위한 것입니다. 문자열 값은 `Android.OS.Environment` 클래스의 상수를 통해 사용할 수 있습니다.

| `Android.OS.Environment` | 디렉터리 |
|-|-|
| DirectoryAlarms | **_개인\_외부\_저장소_/경보** |
| DirectoryDcim | **_PRIVATE\_EXTERNAL\_STORAGE_/DCIM** |
| DirectoryDownloads | **_개인\_외부\_저장소_/다운로드** |
| DirectoryDocuments | **_개인\_외부\_저장소_/문서** |
| DirectoryMovies | **_개인\_외부\_저장소_/동영상** |
| DirectoryMusic | **_개인\_외부\_저장소_/음악** |
| DirectoryNotifications | **_개인\_외부\_저장소_/알림** |
| DirectoryPodcasts | **_PRIVATE\_EXTERNAL\_STORAGE_/Podcasts** |
| DirectoryRingtones 소리 | **_개인\_외부\_저장소_/벨 소리** |
| DirectoryPictures | **_개인\_외부\_저장소_/사진** |

외부 저장소 파티션이 여러 개인 장치의 경우 각 파티션에는 개인 파일용 디렉터리가 포함 됩니다. `Android.Content.Context.GetExternalFilesDirs(string type)` 메서드는 `Java.IO.Files`의 배열을 반환 합니다. 각 개체는 응용 프로그램이 소유 하는 파일을 저장할 수 있는 모든 공유/외부 저장 장치에서 개인 응용 프로그램별 디렉터리를 나타냅니다.

> [!IMPORTANT]
> 개인 외부 저장소 디렉터리에 대 한 정확한 경로는 장치 마다 다르며 Android 버전 사이에 다를 수 있습니다. 이로 인해 앱은이 디렉터리의 경로를 하드 코딩 해서는 안 되며, 대신 Xamarin.ios Api (예: `Android.Content.Context.GetExternalFilesDir()`)를 사용 해야 합니다.

### <a name="public-external-files"></a>공용 외부 파일

공용 파일은 Android에서 전용 파일에 대해 할당 하는 디렉터리에 저장 되지 않은 외부 저장소에 있는 파일입니다. 앱을 제거 하면 공용 파일이 삭제 되지 않습니다. Android 앱에는 공용 파일을 읽거나 쓸 수 있는 권한이 부여 되어야 합니다. 공용 파일은 외부 저장소의 어디에 나 존재할 수 있지만, Android 규칙에 따라 속성 `Android.OS.Environment.ExternalStorageDirectory`으로 식별 되는 디렉터리에 공용 파일이 존재할 것으로 예상 됩니다. 이 속성은 기본 외부 저장소 디렉터리를 나타내는 `Java.IO.File` 개체를 반환 합니다. 예를 들어 `Android.OS.Environment.ExternalStorageDirectory`는 다음 디렉터리를 참조할 수 있습니다.

```bash
/storage/emulated/0/
```

이 문서에서는 외부 저장소의 공용 파일에 대 한 저장소 디렉터리를 _공용\_외부\_저장소_로 참조 합니다.

또한 Android는 _공용\_외부\_저장소_에서 응용 프로그램 디렉터리의 개념을 지원 합니다. 이러한 디렉터리는 `PRIVATE_EXTERNAL_STORAGE`의 응용 프로그램 디렉터리와 동일 하며 이전 섹션의 표에 설명 되어 있습니다. `Android.OS.Environment.GetExternalStoragePublicDirectory(string directoryType)` 메서드는 공용 응용 프로그램 디렉터리에 해당 하는 `Java.IO.File` 개체를 반환 합니다. `directoryType` 매개 변수는 필수 매개 변수 이며 `null`수 없습니다.

예를 들어 `Environment.GetExternalStoragePublicDirectory(Environment.DirectoryDocuments).AbsolutePath`를 호출 하면 다음과 같은 문자열이 반환 됩니다.

```bash
/storage/emulated/0/Documents
```

> [!IMPORTANT]
> 공용 외부 저장소 디렉터리에 대 한 정확한 경로는 장치 마다 다르며 Android 버전 사이에 다를 수 있습니다. 이로 인해 앱은이 디렉터리의 경로를 하드 코딩 해서는 안 되며, 대신 Xamarin.ios Api (예: `Android.OS.Environment.ExternalStorageDirectory`)를 사용 해야 합니다.

## <a name="working-with-external-storage"></a>외부 저장소 작업

Xamarin Android 앱은 파일에 대 한 전체 경로를 가져온 후 파일 생성, 읽기, 쓰기 또는 삭제를 위해 표준 .NET Api를 활용 해야 합니다. 앱에 대 한 플랫폼 간 호환 코드의 양을 최대화 합니다. 그러나 파일에 액세스를 시도 하기 전에 Xamarin Android 앱에서 해당 파일에 액세스할 수 있는지 확인 해야 합니다.

1. 외부 저장소 &ndash; **확인** 외부 저장소의 특성에 따라 앱에서 탑재 및 사용 가능 하지 않을 수 있습니다. 모든 앱은 사용을 시도 하기 전에 외부 저장소의 상태를 확인 해야 합니다.
2. Android 앱에서 외부 저장소에 액세스 하기 위해 사용자의 권한을 요청 해야 &ndash; **런타임 권한 검사를 수행** 합니다. 이는 파일 액세스 전에 실행 시간 권한 요청을 수행 해야 함을 의미 합니다. [Xamarin Android의 가이드 권한에](~/android/app-fundamentals/permissions.md) 는 android 사용 권한에 대 한 자세한 내용이 포함 되어 있습니다.

이러한 두 가지 작업에 대해서는 아래에서 설명 합니다.

### <a name="verifying-that-external-storage-is-available"></a>외부 저장소를 사용할 수 있는지 확인 하는 중

외부 저장소에 쓰기 전에 첫 번째 단계는 읽기 또는 쓰기 가능한 지 확인 하는 것입니다. `Android.OS.Environment.ExternalStorageState` 속성은 외부 저장소의 상태를 식별 하는 문자열을 포함 합니다. 이 속성은 상태를 나타내는 문자열을 반환 합니다. 이 표는 `Environment.ExternalStorageState`에서 반환 될 수 있는 `ExternalStorageState` 값의 목록입니다.

| ExternalStorageState | 설명  |
|----------------------|---|
| MediaBadRemoval      | 미디어가 제대로 분리 되지 않고 갑자기 제거 되었습니다. |
| MediaChecking        | 미디어가 있지만 디스크 검사를 진행 하 고 있습니다.  |
| 미디어 꺼내기        | 미디어를 분리 하 고 꺼냈습니다.  |
| MediaMounted         | 미디어를 탑재 하 여 읽거나 쓸 수 있습니다.  |
| MediaMountedReadOnly | 미디어는 탑재 되지만 에서만 읽을 수 있습니다. |
| Medianosf            | 미디어가 있지만 Android에 적합 한 파일 시스템을 포함 하지 않습니다. |
| 미디어 제거 됨         | 미디어가 없습니다. |
| MediaShared          | 미디어가 있지만 탑재 되지 않습니다. 다른 장치와 함께 USB를 통해 공유 됩니다.|
| MediaUnknown         | Android에서 미디어 상태를 인식할 수 없습니다. |
| MediaUnmountable     | 미디어가 있지만 Android에서 탑재할 수 없습니다. |
| 미디어 분리 됨       | 미디어가 있지만 탑재 되지 않았습니다. |

대부분의 Android 앱은 외부 저장소가 탑재 되었는지 확인 하기만 하면 됩니다. 다음 코드 조각에서는 외부 저장소가 읽기 전용 액세스 또는 읽기/쓰기 액세스용으로 탑재 되었는지 확인 하는 방법을 보여 줍니다.

```csharp
bool isReadonly = Environment.MediaMountedReadOnly.Equals(Environment.ExternalStorageState);
bool isWriteable = Environment.MediaMounted.Equals(Environment.ExternalStorageState);
```

## <a name="external-storage-permissions"></a>외부 저장소 사용 권한

Android는 외부 저장소에 대 한 액세스를 일반적으로 사용자가 리소스에 대 한 액세스 권한을 부여 해야 하는 _위험한 사용 권한_으로 간주 합니다. 사용자는 언제 든 지이 사용 권한을 해지할 수 있습니다.  이는 파일 액세스 전에 실행 시간 권한 요청을 수행 해야 함을 의미 합니다. 앱에는 자신의 개인 파일을 읽고 쓸 수 있는 권한이 자동으로 부여 됩니다. 앱이 사용자의 [권한을 부여 받은](~/android/app-fundamentals/permissions.md) 후 다른 앱에 속하는 개인 파일을 읽고 쓸 수 있습니다.

모든 Android 앱은 **Androidmanifest** 에서 외부 저장소에 대 한 두 가지 권한 중 하나를 선언 해야 합니다. 권한을 식별 하려면 다음 두 가지 `uses-permission` 요소 중 하나를 **Androidmanifest**에 추가 해야 합니다.

```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

> [!NOTE]
> 사용자가 `WRITE_EXTERNAL_STORAGE`를 부여 하는 경우 `READ_EXTERNAL_STORAGE`도 암시적으로 부여 됩니다. **Androidmanifest**에서 두 권한을 모두 요청할 필요는 없습니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

**솔루션 속성**의 **Android 매니페스트** 탭을 사용 하 여 사용 권한을 추가할 수도 있습니다.

![솔루션 탐색기-Visual Studio에 필요한 권한](./images/required-permissions.w157.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

**솔루션 속성 패드**의 **Android 매니페스트** 탭을 사용 하 여 사용 권한을 추가할 수도 있습니다.

[![Solution Pad-Mac용 Visual Studio에 필요한 권한](./images/required-permissions.m752-sml.png)](./images/required-permissions.m752.png#lightbox)

-----

일반적으로 모든 위험한 사용 권한은 사용자가 승인 해야 합니다. 외부 저장소에 대 한 사용 권한은 앱이 실행 되는 Android 버전에 따라이 규칙에 대 한 예외가 있습니다.

![외부 저장소 권한 확인 순서도](./images/external-permission-check-flowchart.png)

런타임 권한 요청을 수행 하는 방법에 대 한 자세한 내용은 [Xamarin.ios의 가이드 권한](~/android/app-fundamentals/permissions.md)을 참조 하세요. **Monodroid** [localfiles](https://github.com/xamarin/monodroid-samples/tree/master/LocalFiles) 는 또한 런타임 권한 검사를 수행 하는 한 가지 방법을 보여 줍니다.

#### <a name="granting-and-revoking-permissions-with-adb"></a>ADB를 사용 하 여 사용 권한 부여 및 해지

Android 앱을 개발 하는 과정에서 런타임 권한 검사와 관련 된 다양 한 작업 흐름을 테스트 하기 위한 권한을 부여 하 고 취소 해야 할 수 있습니다. ADB를 사용 하 여 명령 프롬프트에서이 작업을 수행할 수 있습니다. 다음 명령줄 코드 조각에서는 패키지 이름이 **.com**인 Android 앱에 대해 adb를 사용 하 여 권한을 부여 하거나 취소 하는 방법을 보여 줍니다.

```bash
$ adb shell pm grant com.companyname.app android.permission.WRITE_EXTERNAL_STORAGE

$ adb shell pm revoke com.companyname.app android.permission.WRITE_EXTERNAL_STORAGE
```

## <a name="deleting-files"></a>파일 삭제

표준 C# api를 사용 하 여 [`System.IO.File.Delete`](xref:System.IO.File.Delete*)같은 외부 저장소에서 파일을 삭제할 수 있습니다. 또한 코드 이식성을 위해 Java Api를 사용할 수 있습니다. 예를 들면,

```csharp
System.IO.File.Delete("/storage/emulated/0/Android/data/com.companyname.app/files/count.txt");
```

## <a name="related-links"></a>관련 링크

* [Monodroid의 Xamarin Android 로컬 파일 샘플 **-샘플**](https://github.com/xamarin/monodroid-samples/tree/master/LocalFiles)
* [Xamarin Android의 사용 권한](~/android/app-fundamentals/permissions.md)
