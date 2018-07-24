---
title: Xamarin.Android 사용 하 여 외부 저장소에 대 한 파일 액세스
description: 이 가이드에서는 Xamarin.Android에서 외부 저장소에 파일 액세스를 설명 하는
ms.prod: xamarin
ms.assetid: 40da10b2-a207-4f9c-a2dd-165d9b662f33
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 07/23/2018
ms.openlocfilehash: 380100d38febf567fde94096455fd846d9d3d2d3
ms.sourcegitcommit: 9bb9e8297d3edd9a50585f4ba53c1b4f0bcd1d3e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39212209"
---
# <a name="external-storage"></a>외부 저장소

외부 저장소 참조 파일 및 저장소를 내부 저장소에 없는 파일을 담당 하는 앱에만 액세스할 수 없습니다. 외부 저장소의 기본 목적은를 앱 간에 공유할 수 즉 또는 너무 커서 내부 저장소에 파일을 넣을 위치를 제공 하도록 합니다.

지금 말하기, 외부 저장소 참조 SD 카드와 같은 이동식 미디어에 디스크 파티션 (라고도 _휴대용 저장소_). 이러한 차이점으로이 인해 더 이상 관련이 없는으로 Android 장치 발전와 많은 Android 장치 이동식 저장소를 더 이상 지원 합니다. 대신 일부 장치는 할당 내부 비휘발성 메모리가 중 일부를 동일한 함수 이동식 미디어를 수행 하는 Android 합니다. 이 이라고 _에뮬레이트된_ 저장소 외부 저장소로 간주 됩니다. 또는 일부 Android 장치에는 여러 외부 저장소 파티션이 있을 수 있습니다. 예를 들어, Android 태블릿 (내부 저장소) 외에도 저장소 에뮬레이트가 수 및 SD 카드에 대 한 하나 이상의 슬롯입니다. 이러한 파티션의 모든 외부 저장소로 Android에서 처리 됩니다.

여러 사용자가 장치에서 각 사용자는 해당 외부 저장소에 대 한 기본 외부 저장소 파티션에 전용된 디렉터리를 갖게 됩니다. 명의 사용자로 실행 되는 앱 파일에 액세스할 장치에서 다른 사용자 로부터 수 없습니다 됩니다. 모든 사용자에 대 한 파일은 여전히 읽을 수 있는 전 세계 및 world-쓰기 가능한; 그러나 Android 각 사용자 프로필에서 다른 샌드박스를 됩니다.

다른.NET 응용 프로그램에 그대로 읽고 파일에 쓰기는 Xamarin.Android에서 거의 동일 합니다. Xamarin.Android 앱에는 다음 사용 하 여 표준.NET 관용구 파일 액세스에 대 한 파일을 조작할 수는 있는 경로를 결정합니다. Android 버전에서 Android 또는 내부 및 외부 저장소에 실제 경로 장치 마다 다를 수 있으므로 좋지 않습니다 하드 코드 된 파일의 경로를 합니다. 대신, Xamarin.Android 내부 및 외부 저장소에 파일의 경로를 지정 하는 데 도움이 되는 네이티브 Android Api를 노출 합니다.

이 가이드에서는 개념 및 Android에서 외부 저장소에 관련 된 Api를 설명 합니다.

## <a name="public-and-private-files-on-external-storage"></a>외부 저장소에 공용 및 개인 파일

두 가지 유형의 앱 외부 저장소에 유지할 수 있는 파일에는

* **개인** 파일 &ndash; 개인 파일은 응용 프로그램 (그러나 여전히 세계-읽고 쓸 수 있는 전 세계는) 관련 된 파일입니다. Android는 개인 파일 외부 저장소에서 특정 디렉터리에 저장 되는 필요 합니다. 파일 "private" 이라고 하는 경우에 계속 표시 하 고 장치의 다른 앱에서 액세스할 수 있는, Android에서 특별 한 보호를 제공 하지는 됩니다.

* **공용** 파일 &ndash; 응용 프로그램에 구체적으로 간주 되지 않습니다 및 자유롭게 공유 되며 사용 되는 파일입니다.

이러한 파일의 차이점은 주로 개념입니다. 개인 파일이 점에서 개인는 공용 파일은 외부 저장소에 있는 다른 파일의 응용 프로그램에 속할 것으로 간주 됩니다. Android 개인 및 공용 파일 경로 해결 하기 위한 두 가지 Api를 제공 하지만 그렇지 않으면 동일한.NET Api는 이러한 파일에 읽기 및 쓰기에 사용 됩니다. 에 섹션에서 설명 하는 동일한 Api를 이들은 [읽기 및 쓰기](~/android/platform/files/index.md#reading-or-writing-to-files-on-internal-storage)합니다.

### <a name="private-external-files"></a>개인 외부 파일

외부 파일을 개인 것으로 간주 됩니다 (내부 파일 유사) 응용 프로그램에 특정 하지만 다양 한 이유로 (예: 내부 저장소에 비해 너무 큰)에 대 한 외부 저장소에 유지 되 고 합니다. 내부 파일 비슷하게, 이러한 파일이 삭제 됩니다 사용자가 앱을 제거 하는 경우.

메서드를 호출 하 여 개인 외부 파일에 대 한 기본 위치가 `Android.Content.Context.GetExternalFilesDir(string type)`합니다. 이 메서드는 반환 된 `Java.IO.File` 앱에 대 한 개인 외부 저장소 디렉터리를 나타내는 개체입니다. 전달 `null` 이 메서드는 반환 경로 응용 프로그램에 대 한 사용자의 저장소 디렉터리에 있습니다. 예를 들어 패키지 이름을 사용 하 여 응용 프로그램에 대 한 `com.companyname.app`, 개인 외부 파일의 "루트" 디렉터리 됩니다.

```bash
/storage/emulated/0/Android/data/com.companyname.app/files/
```

이 문서는 외부 저장소에서 개인 파일에 대 한 저장소 디렉터리에 참조 _사설\_외부\_저장소_합니다.


매개 변수가 `GetExternalFilesDir()` 지정 하는 문자열을 _응용 프로그램 디렉터리_합니다. 이 파일의 논리적 구성에 대 한 표준 위치를 제공 하기 위한 디렉터리입니다. 문자열 값에 상수를 통해 사용할 수는 `Android.OS.Environment` 클래스:

| `Android.OS.Environment` | 디렉터리 |
|-|-|
| DirectoryAlarms | **_개인\_외부\_저장소_경보 /** |
| DirectoryDcim | **_개인\_외부\_저장소_/DCIM** |
| DirectoryDownloads | **_개인\_외부\_저장소_다운로드 /** |
| DirectoryDocuments | **_개인\_외부\_저장소_문서 /** |
| DirectoryMovies | **_개인\_외부\_저장소_/Movies** |
| DirectoryMusic | **_개인\_외부\_저장소_/Music** |
| DirectoryNotifications | **_개인\_외부\_저장소_/Notifications** |
| DirectoryPodcasts | **_개인\_외부\_저장소_/Podcasts** |
| DirectoryRingtones | **_개인\_외부\_저장소_/Ringtones** |
| DirectoryPictures | **_개인\_외부\_저장소_그림 /** |

여러 외부 저장소 파티션을 포함 하는 장치의 경우 각 파티션에 개인 파일을 위한 디렉터리를 갖습니다. 메서드 `Android.Content.Context.GetExternalFilesDirs(string type)` 배열을 반환 합니다 `Java.IO.Files`합니다. 각 개체가 나타내는 개인 응용 프로그램별 디렉터리 응용 프로그램 파일을 배치할 수 있는 모든 공유/외부 저장 장치에 소유 합니다.

> [!IMPORTANT]
> 장치를 Android 버전 간의 장치에서 개인 exteral 저장소 디렉터리에 정확한 경로 달라질 수 있습니다. 이 인해 앱 해야 하지 하드이 디렉터리의 경로를 코드 및와 같은 Xamarin.Android Api를 대신 사용 `Android.Content.Context.GetExternalFilesDir()`합니다.

### <a name="public-external-files"></a>공용 외부 파일

공용 파일은 Android 개인 파일에 대 한 할당 하는 디렉터리에 저장 되지 않은 외부 저장소에 있는 파일입니다. 앱을 제거할 때 공용 파일 삭제 되지 않습니다. Android 앱 읽기 설정 하거나 모든 공용 파일을 작성 하기 전에 권한을 부여 되어야 합니다. 외부 저장소에 존재 하는 공용 파일에 대 한 가능한 것 이지만 규칙에 따라 Android 속성으로 식별 되는 디렉터리에 존재 하는 공용 파일 `Android.OS.Environment.ExternalStorageDirectory`합니다. 이 속성은 반환 된 `Java.IO.File` 기본 외부 저장소 디렉터리를 나타내는 개체입니다. 예를 들어 `Android.OS.Environment.ExternalStorageDirectory` 다음 디렉터리를 참조할 수 있습니다.

```bash
/storage/emulated/0/
```

이 문서는 공용 파일을 외부 저장소에 대 한 저장소 디렉터리에 참조 _공개\_외부\_저장소_합니다.


Android에서 응용 프로그램 디렉터리의 개념을도 지원 _공개\_외부\_저장소_합니다. 이러한 디렉터리는 정확 하 게에 대 한 응용 프로그램 diretories 동일 `_PRIVATE\_EXTERNAL\_STORAGE_` 하며 이전 섹션의 표에 설명 되어 있습니다. 메서드 `Android.OS.Environment.GetExternalStoragePublicDirectory(string directoryType)` 돌아갑니다는 `Java.IO.File` 공용 응용 프로그램 디렉터리에 해당 하는 개체입니다. 합니다 `directoryType` 매개 변수는 필수 매개 변수 및 안 `null`합니다.

예를 들어 호출 `Environment.GetExternalStoragePublicDirectory(Environment.DirectoryDocuments).AbsolutePath` 는 비슷합니다는 문자열을 반환 합니다.

```bash
/storage/emulated/0/Documents
```

> [!IMPORTANT]
> 장치를 Android 버전 간의 정확한 경로 외부 공용 저장소 디렉터리에 장치에서 달라질 수 있습니다. 이 인해 앱 해야 하지 하드이 디렉터리의 경로를 코드 및와 같은 Xamarin.Android Api를 대신 사용 `Android.OS.Environment.ExternalStorageDirectory`합니다.

## <a name="working-with-external-storage"></a>외부 저장소를 사용 하 여 작업

Xamarin.Android 앱의 전체 경로를 파일에 가져오면 만들기, 읽기, 쓰기 또는 파일 삭제에 대 한 표준.NET api를 사용 해야 합니다. 이 크로스 플랫폼 앱에 대 한 호환 코드의 크기를 최대화 합니다. 그러나 Xamarin.Android 앱을는 파일에 액세스 하기 전에 해당 파일에 액세스할 수를 확인 해야 합니다.

1. **외부 저장소를 확인 하십시오** &ndash; 외부 저장소의 특성에 따라 것이 가능 하지 탑재 될 수 있습니다 하 고 앱에서 사용할 수 있습니다. 모든 앱은 사용 하기 전에 외부 저장소의 상태를 확인 해야 합니다.
2. **런타임 권한 검사를 수행** &ndash; 는 Android 앱 외부 저장소에 액세스 하기 위해 사용자의 권한을 요청 해야 합니다. 따라서 런타임 전에 모든 파일 액세스 권한 요청을 수행 해야 합니다. 이 가이드 [Xamarin.Android에서 사용 권한을](~/android/app-fundamentals/permissions.md) Android 권한에 대 한 자세한 세부 정보를 포함 합니다.

이러한 두 작업의 각 아래 설명 합니다.

### <a name="verifying-that-external-storage-is-available"></a>외부 저장소를 사용할 수 있는지 확인 합니다.

외부 저장소에 쓰기 전에 먼저 읽기 가능 하거나 쓰기 가능 인지 확인 하는 것입니다. `Android.OS.Environment.ExternalStorageState` 속성 외부 저장소의 상태를 식별 하는 문자열을 포함 합니다. 이 속성은 상태를 나타내는 문자열을 반환 합니다. 이 테이블의 목록은 합니다 `ExternalStorageState` 에서 반환 되는 값 `Environment.ExternalStorageState`:

| ExternalStorageState | 설명  |
|----------------------|---|
| MediaBadRemoval      | 미디어 제대로 분리 되지 않고 갑자기 제거 되었습니다. |
| MediaChecking        | 미디어는 있지만 디스크 중에 확인 됩니다.  |
| MediaEjecting        | 미디어는 분리 되 고 꺼낼 중입니다.  |
| MediaMounted         | 미디어 마운트되어 및 읽거나 쓸 수 수 있습니다.  |
| MediaMountedReadOnly | 미디어 탑재 되어 있지만에서 읽을 수만 있습니다. |
| MediaNofs            | 미디어는 있지만 Android에 대 한 적합 한 파일 시스템을 포함 하지 않습니다. |
| MediaRemoved         | 미디어가 존재 하지 않는 경우 |
| MediaShared          | 미디어는 있지만 탑재 되어 있지 않습니다. 다른 장치를 사용 하 여 USB를 통해 공유 되 고 됩니다.|
| MediaUnknown         | 미디어의 상태는 Android에서 인식할 수 없습니다. |
| MediaUnmountable     | 미디어는 있지만 Android에서 탑재할 수 없습니다. |
| MediaUnmounted       | 미디어는 있지만 탑재 되어 있지 않습니다. |


대부분의 Android 앱 외부 저장소 탑재 된 경우 확인에 필요 합니다. 다음 코드 조각은 읽기 전용 액세스 또는 읽기 / 쓰기 액세스에 대 한 외부 저장소 탑재 되는 확인 하는 방법을 보여 줍니다.

```csharp
bool isReadonly = Environment.MediaMountedReadOnly.Equals(Environment.ExternalStorageState);
bool isWriteable = Environment.MediaMounted.Equals(Environment.ExternalStorageState);
```

## <a name="external-storage-permissions"></a>외부 저장소 사용 권한

Android를 외부 저장소에 액세스 하는 것으로 간주는 _위험한 권한_, 일반적으로 사용자가 리소스에 액세스 하기 위한 권한을 부여 해야 합니다. 사용자는 언제 든 지가이 권한을 해지할 수 있습니다.  따라서 런타임 전에 모든 파일 액세스 권한 요청을 수행 해야 합니다. 앱에는 자신의 개인 파일 읽기 및 쓰기 권한은 자동으로 부여 됩니다. 앱이 된 후 다른 앱에 속해 있는 개인 파일 읽기 및 쓰기에 대 한 있기 [권한이](~/android/app-fundamentals/permissions.md) 사용자가.

모든 Android 앱 외부 저장소에 대 한 두 가지 권한 중 하나를 선언 해야 합니다 **AndroidManifest.xml** 합니다. 다음 두 가지 사용 권한을 식별 `uses-permission` 요소를 추가 해야 합니다 **AndroidManifest.xml**:

```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

> [!NOTE]
> 사용자가 부여 하는 경우 `WRITE_EXTERNAL_STORAGE`, 다음 `READ_EXTERNAL_STORAGE` 도 암시적으로 부여 됩니다. 모두 권한을 요청 하려면 필요한 경우가 아니라면 **AndroidManifest.xml**합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

사용 하 여 사용 권한을 추가할 수도 있습니다는 **Android 매니페스트** 탭의 **솔루션 속성**:

![솔루션 탐색기-Visual Studio 2017에 대 한 필요한 권한](./images/required-permissions.w157.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

사용 하 여 사용 권한을 추가할 수도 있습니다는 **Android 매니페스트** 탭의 **솔루션 속성 패드**:

[![Solution Pad-Mac 용 Visual Studio에 필요한 권한](./images/required-permissions.m752-sml.png)](./images/required-permissions.m752.png#lightbox)

-----

일반적으로 모든 위험한 권한은 사용자가 승인 해야 합니다. 외부 저장소에 대 한 권한은 비정상 응용 프로그램을 실행 하는 android 버전에 따라이 규칙에 대 한 예외는 같습니다.

![외부 저장소 권한 검사의 순서도](./images/external-permission-check-flowchart.png)

런타임 권한 요청을 수행 하는 방법은 가이드를 참조 하세요 [사용 권한에서 Xamarin.Android](~/android/app-fundamentals/permissions.md)합니다. 합니다 **monodroid 샘플** [LocalFiles](https://github.com/xamarin/monodroid-samples/tree/master/LocalFiles) 도 런타임 권한 검사를 수행 하는 방법을 보여 줍니다.

#### <a name="granting-and-revoking-permissions-with-adb"></a>부여 및 ADB 사용 하 여 권한 취소

Android 앱을 개발 하는 과정에서 부여 하 고 런타임 권한 검사와 관련 된 다양 한 작업 흐름을 테스트 하는 권한을 취소 해야 할 수도 있습니다. ADB를 사용 하 여 명령 프롬프트에서이 작업을 수행 하는 것이 가능 합니다. 다음 명령줄 조각에 부여 하거나 ADB를 사용 하 여 패키지 이름이 Android 앱에 대 한 권한을 취소 하는 방법을 보여 줍니다 **com.companyname.app**:

```bash
$ adb shell pm grant com.companyname.app android.permission.WRITE_EXTERNAL_STORAGE

$ adb shell pm revoke com.companyname.app android.permission.WRITE_EXTERNAL_STORAGE
```

## <a name="deleting-files"></a>파일 삭제

와 같은 외부 저장소에서 파일을 삭제 하려면 C# Api를 사용할 수 있지만 표준 하나라 [ `System.IO.File.Delete` ](xref:System.IO.File.Delete*)합니다. 코드 이식성 희생 Java Api를 사용 하는 것도 가능 합니다. 예를 들어:

```csharp
System.IO.File.Delete("/storage/emulated/0/Android/data/com.companyname.app/files/count.txt");
```

## <a name="related-links"></a>관련 링크

* [Xamarin.Android 로컬 파일에서 샘플 **monodroid 샘플**](https://github.com/xamarin/monodroid-samples/tree/master/LocalFiles)
* [Xamarin.Android에서 사용 권한](~/android/app-fundamentals/permissions.md)
