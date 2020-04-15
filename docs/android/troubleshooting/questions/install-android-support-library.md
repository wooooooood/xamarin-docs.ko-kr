---
title: Xamarin.Android.Support 패키지에 필요한 Android 지원 라이브러리를 수동으로 설치하려면 어떻게 할까요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A9CB8CA8-8A6D-405E-B84C-A16CE452C0F7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 99571e0b62592597bb1fffdc8d3ed8336fe050b2
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73026935"
---
# <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packages"></a>Xamarin.Android.Support 패키지에 필요한 Android 지원 라이브러리를 수동으로 설치하려면 어떻게 할까요?

## <a name="example-steps-for-xamarinandroidsupportv4"></a>Xamarin.Android.Support.v4의 단계 예 

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

원하는 Xamarin.Android.Support NuGet 패키지를 다운로드합니다(예: NuGet 패키지 관리자를 사용하여 설치).

`ildasm`을 사용하여 NuGet 패키지에 필요한 **android_m2repository.zip** 버전을 확인합니다.

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr SourceUrl
```

예제 출력:

```cmd
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
```

**ildasm**에서 반환된 URL을 사용하여 Google에서 **android\_m2repository.zip**을 다운로드합니다. 또는 현재 Android SDK Manager에 설치되어 있는 _Android 지원 리포지토리_ 버전을 확인할 수 있습니다.

!["Android 지원 리포지토리 버전 32가 설치된 Android SDK 관리자"](install-android-support-library-images/sdk-extras.png)

이 버전이 NuGet 패키지에 필요한 버전과 일치한다면 새로 다운로드할 필요가 없습니다. _SDK 경로_(Android SDK 관리자 창의 상단에 표시됨)의 **extras\\android** 아래에 있는 기존 **m2repository** 디렉터리를 대신 다시 압축할 수 있습니다.

**ildasm**에서 반환된 URL의 MD5 해시를 계산합니다. 모두 대문자를 사용하고 공백을 사용하지 않도록 결과 문자열의 형식을 지정합니다. 예를 들어 필요에 따라 `$url` 변수를 조정한 다음, PowerShell에서 다음 2줄([Xamarin.Android의 원본 C# 코드](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208) 기반)을 실행합니다.

```powershell
$url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"
(([System.Security.Cryptography.MD5]::Create()).ComputeHash([System.Text.Encoding]::UTF8.GetBytes($url)) | %{ $_.ToString("X02") }) -join ""
```

예제 출력:

```powershell
F16A3455987DBAE5783F058F19F7FCDF
```

**%LOCALAPPDATA%\\Xamarin\\zips\\** 폴더에 **android\_m2repository.zip**을 복사합니다. 이전 MD5 해시 계산 단계의 MD5 해시를 사용하도록 파일의 이름을 바꿉니다. 예를 들어:

**%LOCALAPPDATA%\\Xamarin\\zips\\F16A3455987DBAE5783F058F19F7FCDF.zip**

(선택 사항) **%LOCALAPPDATA%\\Xamarin\\Xamarin.Android.Support.v4\\23.4.0.0\\content\\** 에 파일의 압축을 풉니다(**content\\m2repository** 하위 디렉터리 생성). 이 단계를 건너뛰면 라이브러리를 사용하는 첫 번째 빌드에서 이 단계를 완료해야 하기 때문에 시간이 좀 더 오래 걸립니다.
하위 디렉터리의 버전 번호(이 예에서는 **23.4.0.0**)는 NuGet 패키지 버전과 동일하지 않습니다. `ildasm`을 사용하여 올바른 버전 번호를 찾을 수 있습니다.

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr /C:"string 'Version'"
```

예제 출력:

```cmd
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
```

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

원하는 Xamarin.Android.Support NuGet 패키지를 다운로드합니다(예: NuGet 패키지 관리자를 사용하여 설치).

Mac용 Visual Studio에서 Android 프로젝트의 _참조_ 섹션에서 _Xamarin.Android.Support.v4_ 어셈블리를 두 번 클릭하여 어셈블리 브라우저에서 어셈블리를 엽니다. _언어_ 드롭다운이 _C#_ 으로 설정되어 있는지 확인하고 어셈블리 브라우저 탐색 트리에서 최상위 _Xamarin.Android.Support.v4_ 어셈블리를 선택합니다. `IncludeAndroidResourcesFrom` 또는 `JavaLibraryReference` 특성 중 하나에서 `SourceUrl` 속성을 찾습니다.

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

**ildasm**에서 반환된 `SourceUrl`을 사용하여 Google에서 **android\_m2repository.zip**을 다운로드합니다. 또는 현재 Android SDK Manager에 설치되어 있는 _Android 지원 리포지토리_ 버전을 확인할 수 있습니다.

!["Android 지원 리포지토리 버전 32가 설치된 Android SDK 관리자"](install-android-support-library-images/sdk-extras.png)

이 버전이 NuGet 패키지에 필요한 버전과 일치한다면 새로 다운로드할 필요가 없습니다. _SDK 경로_(Android SDK 관리자 창의 상단에 표시됨)의 **extras/android** 아래에 있는 기존 **m2repository** 디렉터리를 대신 다시 압축할 수 있습니다.

**ildasm**에서 반환된 URL의 MD5 해시를 계산합니다. 모두 대문자를 사용하고 공백을 사용하지 않도록 결과 문자열의 형식을 지정합니다. 예를 들어 필요에 따라 URL 문자열을 조정한 후 **Terminal.app** 명령 프롬프트에서 다음 명령을 실행합니다.

```bash
echo -n "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip" | md5 | tr '[:lower:]' '[:upper:]'
```

또 다른 옵션은 `csharp` 인터프리터를 사용하여 [Xamarin.Android 자체에서 사용하는 것과 동일한 C# 코드](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208)를 실행하는 것입니다.
이렇게 하려면 필요에 따라 `url` 변수를 조정한 후 **Terminal.app** 명령 프롬프트에서 다음 명령을 실행합니다.

```bash
csharp -e 'var url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"; string.Concat((System.Security.Cryptography.MD5.Create().ComputeHash(System.Text.Encoding.UTF8.GetBytes(url))).Select(b => b.ToString("X02")))'
```

예제 출력:

```bash
F16A3455987DBAE5783F058F19F7FCDF
```

**android\_m2repository.zip**를 **$HOME/.local/share/Xamarin/zips/** 폴더에 복사합니다. 이전 MD5 해시 계산 단계의 MD5 해시를 사용하도록 파일의 이름을 바꿉니다. 예를 들어:

**$HOME/.local/share/Xamarin/zips/F16A3455987DBAE5783F058F19F7FCDF.zip**

(선택 사항) 다음 위치에 파일의 압축을 풉니다. 

**$HOME/.local/share/Xamarin/Xamarin.Android.Support.v4/23.4.0.0/content/**

(**content/m2repository** 하위 디렉터리 생성). 이 단계를 건너뛰면 라이브러리를 사용하는 첫 번째 빌드에서 이 단계를 완료해야 하기 때문에 시간이 좀 더 오래 걸립니다.

하위 디렉터리의 버전 번호(이 예에서는 **23.4.0.0**)는 NuGet 패키지 버전과 동일하지 않습니다. 이전 **ildasm** 단계와 마찬가지로 Mac용 Visual Studio에서 어셈블리 브라우저를 사용하여 올바른 버전 번호를 찾을 수 있습니다. `IncludeAndroidResourcesFrom` 또는 `JavaLibraryReference` 특성 중 하나에서 `Version` 속성을 찾습니다.

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

-----

## <a name="additional-references"></a>추가 참조

- [버그 43245](https://bugzilla.xamarin.com/show_bug.cgi?id=43245) – (부정확) "다운로드에 실패했습니다. {0}을(를) 다운로드하고 {1} 디렉터리에 저장하세요." 및 "SDK 설치 프로그램에 제공되는 '{0}' 패키지를 설치하세요"라는 Xamarin.Android.Support 패키지 관련 오류 메시지

### <a name="next-steps"></a>다음 단계

이 문서에서는 2016년 8월 현재 동작에 대해 설명합니다. 이 문서에서 설명하는 기법은 안정적인 Xamarin용 테스트 도구 모음의 일부가 아니므로 나중에 중단될 수 있습니다.

추가 지원이 필요하면 Microsoft에 문의하고, 위의 정보를 참조한 후에도 이 문제가 계속 발생하는 경우 [Xamarin에 사용할 수 있는 지원 옵션은 무엇인가요?](~/cross-platform/troubleshooting/support-options.md)에서 문의 옵션, 제안 사항 및 필요한 경우 새 버그를 보고하는 방법에 대한 정보를 참조하세요.
