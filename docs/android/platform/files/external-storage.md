---
title: Xamarin.Android에서 외부 스토리지의 파일 액세스
description: 이 가이드에서는 Xamarin.Android에서 외부 스토리지의 파일에 액세스하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 40da10b2-a207-4f9c-a2dd-165d9b662f33
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/23/2018
ms.openlocfilehash: 96b0d6a00c7825939b1f89ed63e3e5559ca4ef59
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73020476"
---
# <a name="external-storage"></a>외부 스토리지

외부 스토리지는 내부 스토리지에 있지 않고 해당 파일을 담당하는 앱에서 독점적으로 액세스할 수 있지 않은 파일 스토리지를 가리킵니다. 외부 스토리지의 주된 목적은 앱 간에 공유되어야 하는 파일이나 내부 스토리지에 저장하기에 너무 큰 파일을 저장할 공간을 제공하는 것입니다.

지금까지 외부 스토리지는 SD 카드와 같은 이동식 미디어(_휴대용 스토리지_라고도 함)에 있는 디스크 파티션을 가리켰습니다. 그러나 Android 디바이스가 점점 더 발전되고 더 이상 이동식 스토리지를 지원하지 않는 Android 디바이스가 늘어남에 따라 이제 해당 구분은 더 이상 무의미해졌습니다. 이제 어떤 디바이스에서는 Android가 이동식 미디어에 대해 동일한 기능을 수행하도록 얼마간의 내부 비휘발성 메모리를 할당하기도 합니다. 이를 _에뮬레이트된_ 스토리지라고 하며, 해당 스토리지도 외부 스토리지로 간주됩니다. 어떤 Android 디바이스에는 외부 스토리지 파티션이 여러 개인 경우도 있습니다. 예를 들어, 하나의 Android 태블릿에 내부 스토리지에 더해 에뮬레이트된 스토리지와 하나 이상의 SD 카드용 슬롯이 있을 수 있습니다. Android는 해당 파티션을 모두 외부 스토리지로 취급합니다.

여러 사용자가 있는 디바이스에서는 각 사용자의 기본 외부 스토리지 파티션에 외부 스토리지를 위한 전용 디렉터리가 있습니다. 특정 사용자로 실행되는 앱은 디바이스에 있는 다른 사용자의 파일에 액세스할 수 없습니다. 모든 사용자를 위한 파일은 여전히 전역적으로 읽기 및 쓰기 가능하나, Android는 각 사용자 프로필을 다른 사용자 프로필로부터 샌드박싱합니다.

Xamarin.Android의 파일 읽기 및 쓰기는 다른 모든 .NET 애플리케이션의 파일 읽기 및 쓰기와 거의 동일합니다. Xamarin.Android 앱이 조작할 파일의 경로를 확인한 다음에 표준 .NET 관용구를 사용하여 파일에 액세스합니다. 내부 스토리지와 외부 스토리지의 실제 경로는 디바이스별로 또는 Android 버전별로 다를 수 있으므로 파일 경로를 하드 코딩하는 것은 권장되지 않습니다. 그 대신 Xamarin.Android는 내부 스토리지와 외부 스토리지의 파일 경로를 확인하는 데 도움이 되는 네이티브 Android API를 노출합니다.

이 가이드에서는 Android에서 외부 스토리지에 해당하는 개념과 API를 살펴봅니다.

## <a name="public-and-private-files-on-external-storage"></a>외부 스토리지의 퍼블릭 및 프라이빗 파일

앱이 외부 스토리지에 보관할 수 있는 파일에는 두 가지 형식이 있습니다.

* **프라이빗** 파일 &ndash; 프라이빗 파일은 특정 애플리케이션에만 관련된 파일입니다(단, 전역적인 읽기 및 쓰기는 가능함). Android는 프라이빗 파일이 외부 스토리지의 특정 디렉터리에 저장되어 있다고 간주합니다. 해당 파일은 “프라이빗”이라고 불리긴 해도, 디바이스의 다른 앱이 보고 액세스할 수 있으며 Android에 의해 특수 보호가 제공되지 않습니다.

* **퍼블릭** 파일 &ndash; 특정 애플리케이션에만 관련되었다고 간주되지 않는 파일로, 자유로운 공유가 가능합니다.

프라이빗 파일과 퍼블릭 파일 사이의 차이는 주로 개념상의 차이입니다. 프라이빗 파일은 애플리케이션의 일부로 간주된다는 측면에서 프라이빗하며, 퍼블릭 파일은 외부 스토리지에 존재하는 다른 모든 파일입니다. Android는 프라이빗 및 퍼블릭 파일의 경로를 확인하기 위한 서로 다른 두 가지 API를 제공하지만, 이 외에는 해당 파일을 읽고 쓰는 데 동일한 .NET API가 사용됩니다. 이는 바로 [읽기 및 쓰기](~/android/platform/files/index.md#reading-or-writing-to-files-on-internal-storage)에 대한 섹션에서 설명하는 동일한 API입니다.

### <a name="private-external-files"></a>프라이빗 외부 파일

프라이빗 외부 파일은 (내부 파일처럼) 특정 애플리케이션에만 관련된 것으로 간주되나 다양한 이유로 인해 외부 스토리지에 보관되는 파일입니다(예: 내부 스토리지에 저장하기에는 크기가 너무 큼). 해당 파일은 내부 파일과 마찬가지로 사용자가 앱을 제거하면 삭제됩니다.

프라이빗 외부 파일의 기본 위치는 `Android.Content.Context.GetExternalFilesDir(string type)` 메서드를 호출하여 확인합니다. 이 메서드는 해당 앱의 프라이빗 외부 스토리지 디렉터리를 나타내는 `Java.IO.File` 개체를 반환합니다. 이 메서드에 `null`을 전달하면 해당 애플리케이션에 대한 사용자의 스토리지 디렉터리 경로가 반환됩니다. 예를 들어, 패키지 이름이 `com.companyname.app`인 애플리케이션에 대해 프라이빗 외부 파일의 “루트” 디렉터리는 다음과 같습니다.

```bash
/storage/emulated/0/Android/data/com.companyname.app/files/
```

이 문서에서는 외부 스토리지에 있는 프라이빗 파일의 스토리지 디렉터리를 _PRIVATE\_EXTERNAL\_STORAGE_라고 부르겠습니다.

`GetExternalFilesDir()`의 매개 변수는 _애플리케이션 디렉터리_를 지정하는 문자열입니다. 이는 파일의 논리적인 조직에 대한 표준 위치를 제공하기 위한 디렉터리입니다. 문자열 값은 다음과 같은 `Android.OS.Environment` 클래스의 상수를 통해 사용할 수 있습니다.

| `Android.OS.Environment` | 디렉터리 |
|-|-|
| DirectoryAlarms | **_PRIVATE\_EXTERNAL\_STORAGE_/Alarms** |
| DirectoryDcim | **_PRIVATE\_EXTERNAL\_STORAGE_/DCIM** |
| DirectoryDownloads | **_PRIVATE\_EXTERNAL\_STORAGE_/Download** |
| DirectoryDocuments | **_PRIVATE\_EXTERNAL\_STORAGE_/Documents** |
| DirectoryMovies | **_PRIVATE\_EXTERNAL\_STORAGE_/Movies** |
| DirectoryMusic | **_PRIVATE\_EXTERNAL\_STORAGE_/Music** |
| DirectoryNotifications | **_PRIVATE\_EXTERNAL\_STORAGE_/Notifications** |
| DirectoryPodcasts | **_PRIVATE\_EXTERNAL\_STORAGE_/Podcasts** |
| DirectoryRingtones | **_PRIVATE\_EXTERNAL\_STORAGE_/Ringtones** |
| DirectoryPictures | **_PRIVATE\_EXTERNAL\_STORAGE_/Pictures** |

외부 스토리지 파티션이 여러 개인 디바이스의 경우, 각 파티션에는 프라이빗 파일을 위한 디렉터리가 있습니다. `Android.Content.Context.GetExternalFilesDirs(string type)` 메서드는 `Java.IO.Files`로 구성된 배열을 반환합니다. 각 개체는 모든 공유/외부 스토리지 디바이스에서 애플리케이션이 해당 애플리케이션에서 소유하는 파일을 저장할 수 있는 해당 애플리케이션 전용 프라이빗 디렉터리를 나타냅니다.

> [!IMPORTANT]
> 프라이빗 외부 스토리지 디렉터리의 정확한 경로는 디바이스별로, 그리고 Android 버전별로 다를 수 있습니다. 이 때문에 앱에서 이 디렉터리의 경로를 하드 코딩해서는 안 되며, 그 대신 `Android.Content.Context.GetExternalFilesDir()`와 같은 Xamarin.Android API를 사용해야 합니다.

### <a name="public-external-files"></a>퍼블릭 외부 파일

퍼블릭 파일은 외부 스토리지에 존재하며 Android가 프라이빗 파일을 위해 할당한 디렉터리에 저장되지 않는 파일입니다. 퍼블릭 파일은 앱을 제거해도 삭제되지 않습니다. Android 앱에서 퍼블릭 파일을 읽거나 쓰려면 먼저 앱에 권한이 부여되어야 합니다. 퍼블릭 파일은 외부 스토리지의 어느 곳에나 존재할 수 있지만, Android는 규칙상 퍼블릭 파일이 속성 `Android.OS.Environment.ExternalStorageDirectory`로 식별되는 디렉터리에 존재한다고 간주합니다. 이 속성은 기본 외부 스토리지 디렉터리를 나타내는 `Java.IO.File` 개체를 반환합니다. 예를 들어, `Android.OS.Environment.ExternalStorageDirectory`는 다음 디렉터리를 가리킬 수 있습니다.

```bash
/storage/emulated/0/
```

이 문서에서는 외부 스토리지에 있는 퍼블릭 파일의 스토리지 디렉터리를 _PUBLIC\_EXTERNAL\_STORAGE_라고 부르겠습니다.

Android는 _PUBLIC\_EXTERNAL\_STORAGE_에 있는 애플리케이션 디렉터리라는 개념도 지원합니다. 해당 디렉터리는 `PRIVATE_EXTERNAL_STORAGE`의 애플리케이션 디렉터리와 정확하게 동일하며, 이전 섹션의 표에 설명되어 있습니다. `Android.OS.Environment.GetExternalStoragePublicDirectory(string directoryType)` 메서드는 퍼블릭 애플리케이션 디렉터리에 해당하는 `Java.IO.File` 개체를 반환합니다. `directoryType` 매개 변수는 필수 매개 변수이며 `null`일 수 없습니다.

예를 들어, `Environment.GetExternalStoragePublicDirectory(Environment.DirectoryDocuments).AbsolutePath`를 호출하면 다음과 같은 문자열이 반환됩니다.

```bash
/storage/emulated/0/Documents
```

> [!IMPORTANT]
> 퍼블릭 외부 스토리지 디렉터리의 정확한 경로는 디바이스별로, 그리고 Android 버전별로 다를 수 있습니다. 이 때문에 앱에서 이 디렉터리의 경로를 하드 코딩해서는 안 되며, 그 대신 `Android.OS.Environment.ExternalStorageDirectory`와 같은 Xamarin.Android API를 사용해야 합니다.

## <a name="working-with-external-storage"></a>외부 저장소 사용

Xamarin.Android 앱이 파일의 전체 경로를 확인했으면 이제부터는 파일 만들기, 읽기, 쓰기 또는 삭제를 위한 표준 .NET API를 사용해야 합니다. 이렇게 하면 앱의 플랫폼 간 호환 가능한 코드의 양이 최대화됩니다. 그러나 Xamarin.Android 앱은 파일 액세스를 시도하기 전에 해당 파일에 대한 액세스가 가능한지를 먼저 확인해야 합니다.

1. **외부 저장소 확인** &ndash; 외부 저장소의 성격에 따라 앱에서 사용할 수 있도록 탑재되는 것이 가능하지 않을 수 있습니다. 모든 앱은 외부 스토리지를 사용하려고 시도하기 전에 먼저 외부 스토리지의 상태를 확인해야 합니다.
2. **런타임 권한 확인 수행** &ndash; Android 앱이 외부 스토리지에 액세스하려면 사용자에게 권한을 요청해야 합니다. 즉, 모든 파일 액세스에 앞서 런타임 권한 요청이 수행되어야 합니다. [Xamarin.Android의 사용 권한](~/android/app-fundamentals/permissions.md) 가이드에서 Android 권한에 대해 자세히 설명합니다.

아래에서 이 두 가지 작업에 대해 자세히 설명합니다.

### <a name="verifying-that-external-storage-is-available"></a>외부 스토리지를 사용할 수 있는지 확인

외부 스토리지에 쓰기에 앞서 수행해야 할 첫 번째 단계는 외부 스토리지 읽기 또는 쓰기가 가능한지 확인하는 것입니다. `Android.OS.Environment.ExternalStorageState` 속성은 외부 스토리지의 상태를 식별하는 문자열을 포함합니다. 이 속성은 상태를 나타내는 문자열을 반환합니다. 다음 표에는 `Environment.ExternalStorageState`가 반환하는 `ExternalStorageState` 값이 나와 있습니다.

| ExternalStorageState | 설명  |
|----------------------|---|
| MediaBadRemoval      | 미디어가 올바르게 분리되지 않고 갑자기 제거되었습니다. |
| MediaChecking        | 미디어가 있긴 하나 현재 디스크 검사가 진행되고 있습니다.  |
| MediaEjecting        | 현재 미디어의 분리 및 꺼내기가 진행되고 있습니다.  |
| MediaMounted         | 미디어가 탑재되어 읽기 또는 쓰기가 가능합니다.  |
| MediaMountedReadOnly | 미디어가 탑재되었으나 읽기만 가능합니다. |
| MediaNofs            | 미디어가 있긴 하나 Android에 적합한 파일 시스템을 포함하지 않습니다. |
| MediaRemoved         | 미디어가 없습니다. |
| MediaShared          | 미디어가 있지만 탑재되지 않았습니다. USB를 통해 다른 디바이스와 공유되고 있습니다.|
| MediaUnknown         | 미디어가 Android에서 인식할 수 없는 상태에 있습니다. |
| MediaUnmountable     | 미디어가 있긴 하나 Android에 탑재할 수 없습니다. |
| MediaUnmounted       | 미디어가 있지만 탑재되지 않았습니다. |

대부분의 Android 앱에서는 외부 스토리지가 탑재되었는지만 확인하면 됩니다. 다음 코드 조각에서는 읽기 전용 액세스 또는 읽기-쓰기 액세스를 위해 외부 스토리지가 탑재되었는지를 확인하는 방법을 보여 줍니다.

```csharp
bool isReadonly = Environment.MediaMountedReadOnly.Equals(Environment.ExternalStorageState);
bool isWriteable = Environment.MediaMounted.Equals(Environment.ExternalStorageState);
```

## <a name="external-storage-permissions"></a>외부 저장소 권한

Android는 외부 스토리지에 액세스하는 것을 _위험한 권한_으로 간주합니다. 위험한 권한은 일반적으로 사용자가 리소스에 액세스할 수 있도록 권한을 부여해야 합니다. 사용자는 언제든지 이 권한을 철회할 수 있습니다.  즉, 모든 파일 액세스에 앞서 런타임 권한 요청이 수행되어야 합니다. 앱에는 앱 자체의 프라이빗 파일에 대한 읽기 및 쓰기 권한이 자동으로 부여됩니다. 사용자가 [권한을 부여](~/android/app-fundamentals/permissions.md)한 후에는 앱에서 다른 앱에 속하는 프라이빗 파일을 읽고 쓸 수 있습니다.

모든 Android 앱은 **AndroidManifest.xml**에서 외부 스토리지에 대한 두 가지 권한 중 하나를 선언해야 합니다. 권한을 식별하려면 다음 두 개의 `uses-permission` 요소 중 하나가 **AndroidManifest.xml**에 추가되어야 합니다.

```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

> [!NOTE]
> 사용자가 `WRITE_EXTERNAL_STORAGE`를 부여하면 `READ_EXTERNAL_STORAGE`도 암시적으로 부여됩니다. **AndroidManifest.xml**에서 두 가지 권한을 모두 요청할 필요는 없습니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

**솔루션 속성**의 **Android 매니페스트** 탭을 사용하여 권한을 추가할 수도 있습니다.

![솔루션 탐색기 - Visual Studio에 필요한 권한](./images/required-permissions.w157.png)

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

**솔루션 속성 패드**의 **Android 매니페스트** 탭을 사용하여 권한을 추가할 수도 있습니다.

[![Solution Pad - Mac용 Visual Studio에 필요한 권한](./images/required-permissions.m752-sml.png)](./images/required-permissions.m752.png#lightbox)

-----

일반적으로, 모든 위험한 권한은 사용자가 승인해야 합니다. 외부 스토리지에 대한 권한은 앱이 실행 중인 Android 버전에 따라 이 규칙에 예외가 존재한다는 점에서 변칙입니다.

![외부 스토리지 권한 확인 순서도](./images/external-permission-check-flowchart.png)

런타임 권한 요청 수행에 대한 자세한 내용은 [Xamarin.Android의 사용 권한](~/android/app-fundamentals/permissions.md) 가이드를 참조하세요. **monodroid-sample** [LocalFiles](https://github.com/xamarin/monodroid-samples/tree/master/LocalFiles)에서도 런타임 권한 확인을 수행하는 한 가지 방법을 보여 줍니다.

#### <a name="granting-and-revoking-permissions-with-adb"></a>ADB를 사용한 권한 부여 및 철회

Android 앱을 개발하는 과정에서 런타임 권한 확인과 관련된 다양한 워크플로를 테스트하기 위해 권한을 부여하거나 철회하는 것이 필요할 수 있습니다. ADB를 사용하여 명령 프롬프트에서 권한을 부여하거나 철회할 수 있습니다. 다음 명령줄 조각에서는 ADB를 사용하여 패키지 이름이 **com.companyname.app**인 Android 앱의 권한을 부여하거나 철회하는 방법을 보여 줍니다.

```bash
$ adb shell pm grant com.companyname.app android.permission.WRITE_EXTERNAL_STORAGE

$ adb shell pm revoke com.companyname.app android.permission.WRITE_EXTERNAL_STORAGE
```

## <a name="deleting-files"></a>파일 삭제

[`System.IO.File.Delete`](xref:System.IO.File.Delete*)를 비롯한 모든 표준 C# API를 외부 스토리지에서 파일을 삭제하는 데 사용할 수 있습니다. 코드 이식성을 희생하고 Java API를 사용하는 것도 가능합니다. 예를 들어:

```csharp
System.IO.File.Delete("/storage/emulated/0/Android/data/com.companyname.app/files/count.txt");
```

## <a name="related-links"></a>관련 링크

* [**monodroid-samples**의 Xamarin.Android Local Files 샘플](https://github.com/xamarin/monodroid-samples/tree/master/LocalFiles)
* [Xamarin.Android의 사용 권한](~/android/app-fundamentals/permissions.md)
