---
title: Xamarin.Android.Support 패키지에 필요한 Android 지원 라이브러리를 수동으로 설치하려면 어떻게 할까요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A9CB8CA8-8A6D-405E-B84C-A16CE452C0F7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: a43a2ed4498be76a99ab4b6b54d3048f2f80af5c
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69887653"
---
# <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packages"></a>Xamarin.Android.Support 패키지에 필요한 Android 지원 라이브러리를 수동으로 설치하려면 어떻게 할까요?

## <a name="example-steps-for-xamarinandroidsupportv4"></a>Xamarin.ios에 대 한 예제 단계 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

원하는 Xamarin. 지원 NuGet 패키지를 다운로드 합니다 (예: NuGet 패키지 관리자로 설치).

를 `ildasm` 사용 하 여 NuGet 패키지에 필요한 android_m2repository 버전을 확인 **합니다** .

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr SourceUrl
```

예제 출력:

```cmd
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
```

**Ildasm.exe**에서 반환 된 URL을 사용 하 여 Google에서 **android\_m2repository** 를 다운로드 합니다. 또는 Android SDK Manager에 현재 설치 되어 있는 _Android 지원 리포지토리의_ 버전을 확인할 수 있습니다.

!["Android 지원 리포지토리 버전 32이 설치 된 Android SDK 관리자"](install-android-support-library-images/sdk-extras.png)

NuGet 패키지에 필요한 버전과 일치 하는 버전이 있으면 새로운 항목을 다운로드할 필요가 없습니다. 대신 _SDK 경로_ (Android SDK 관리자 창의 맨 위에 표시 된 것 처럼)의 추가 **\\android** 아래에 있는 기존 **m2repository** 디렉터리를 다시 압축할 수 있습니다.

**Ildasm**에서 반환 된 URL의 MD5 해시를 계산 합니다. 모든 대문자를 사용 하 고 공백을 사용 하지 않도록 결과 문자열의 형식을 지정 합니다. 예를 들어 필요에 `$url` 따라 변수를 조정 하 고 PowerShell에서 다음 두 줄 ( [xamarin.ios의 원래 C# 코드](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208)를 기반으로 함)을 실행 합니다.

```powershell
$url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"
(([System.Security.Cryptography.MD5]::Create()).ComputeHash([System.Text.Encoding]::UTF8.GetBytes($url)) | %{ $_.ToString("X02") }) -join ""
```

예제 출력:

```powershell
F16A3455987DBAE5783F058F19F7FCDF
```

**Android\_m2repository** 을 **% LOCALAPPDATA\\%\\Xamarin zips\\**  폴더에 복사 합니다. 이전 MD5 해시 계산 단계의 MD5 해시를 사용 하도록 파일의 이름을 바꿉니다. 예를 들어:

**%LOCALAPPDATA%\\Xamarin\\zips\\F16A3455987DBAE5783F058F19F7FCDF.zip**

필드 **\\% LOCALAPPDATA%\\ Xamarin\\xamarin.ios. 지원 v4\\23.4.0.0\\내용** 으로 파일의 압축을 풉니다 ( **콘텐츠\\m2repository** 를 만듭니다. 하위 디렉터리). 이 단계를 건너뛰면 라이브러리를 사용 하는 첫 번째 빌드에서이 단계를 완료 해야 하기 때문에 시간이 더 오래 걸립니다.
하위 디렉터리의 버전 번호 (이 예제에서는**23.4.0.0** )는 NuGet 패키지 버전과 동일 하지 않습니다. 를 사용 `ildasm` 하 여 올바른 버전 번호를 찾을 수 있습니다.

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr /C:"string 'Version'"
```

예제 출력:

```cmd
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
```

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

원하는 Xamarin. 지원 NuGet 패키지를 다운로드 합니다 (예: NuGet 패키지 관리자로 설치).

Mac용 Visual Studio에 있는 Android 프로젝트의 _참조_ 섹션에서 _xamarin.ios_ 어셈블리를 두 번 클릭 하 여 어셈블리 브라우저에서 어셈블리를 엽니다. _언어_ 드롭다운이로 _C#_ 설정 되어 있는지 확인 하 고 어셈블리 브라우저 탐색 트리에서 최상위 _Xamarin Android. 지원 v4_ 어셈블리를 선택 합니다. `IncludeAndroidResourcesFrom` `SourceUrl` 또는`JavaLibraryReference` 특성 중 하나에서 속성을 찾습니다.

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

**Ildasm.exe**에서 반환 된를 `SourceUrl` 사용 하 여 Google에서 **\_android m2repository** 를 다운로드 합니다. 또는 Android SDK Manager에 현재 설치 되어 있는 _Android 지원 리포지토리의_ 버전을 확인할 수 있습니다.

!["Android 지원 리포지토리 버전 32이 설치 된 Android SDK 관리자"](install-android-support-library-images/sdk-extras.png)

NuGet 패키지에 필요한 버전과 일치 하는 버전이 있으면 새로운 항목을 다운로드할 필요가 없습니다. 대신 _SDK 경로_ 에서 **스페셜/android** 아래에 있는 기존 **m2repository** 디렉터리를 다시 압축할 수 있습니다 (Android SDK Manager 창의 위쪽).

**Ildasm**에서 반환 된 URL의 MD5 해시를 계산 합니다. 모든 대문자를 사용 하 고 공백을 사용 하지 않도록 결과 문자열의 형식을 지정 합니다. 예를 들어 필요에 따라 URL 문자열을 조정 하 고 **터미널 앱** 명령 프롬프트에서 다음 명령을 실행 합니다.

```bash
echo -n "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip" | md5 | tr '[:lower:]' '[:upper:]'
```

다른 옵션은 `csharp` 인터프리터를 사용 하 여 [Xamarin에서 사용 하는 것과 동일한 C# 코드](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208)를 실행 하는 것입니다.
이렇게 하려면 필요에 따라 `url` 변수를 조정 하 고 **터미널 앱** 명령 프롬프트에서 다음 명령을 실행 합니다.

```bash
csharp -e 'var url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"; string.Concat((System.Security.Cryptography.MD5.Create().ComputeHash(System.Text.Encoding.UTF8.GetBytes(url))).Select(b => b.ToString("X02")))'
```

예제 출력:

```bash
F16A3455987DBAE5783F058F19F7FCDF
```

**Android\_m2repository** 을 **$HOME/.local/share/xamarin/zips/** 폴더에 복사 합니다. 이전 MD5 해시 계산 단계의 MD5 해시를 사용 하도록 파일의 이름을 바꿉니다. 예:

**$HOME/.local/share/Xamarin/zips/F16A3455987DBAE5783F058F19F7FCDF.zip**

필드 파일의 압축을 풉니다. 

**$HOME/.local/share/Xamarin/Xamarin.Android.Support.v4/23.4.0.0/content/**

( **content/m2repository** 하위 디렉터리 만들기). 이 단계를 건너뛰면 라이브러리를 사용 하는 첫 번째 빌드에서이 단계를 완료 해야 하기 때문에 시간이 더 오래 걸립니다.

하위 디렉터리의 버전 번호 (이 예제에서는**23.4.0.0** )는 NuGet 패키지 버전과 동일 하지 않습니다. 이전 **ildasm.exe** 단계와 마찬가지로 Mac용 Visual Studio의 어셈블리 브라우저를 사용 하 여 올바른 버전 번호를 찾을 수 있습니다. 또는 특성 중 `Version` 하나에서`JavaLibraryReference`속성을찾습니다. `IncludeAndroidResourcesFrom`

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

-----


## <a name="additional-references"></a>추가 참조

- [버그 43245](https://bugzilla.xamarin.com/show_bug.cgi?id=43245) – 부정확 한 "다운로드에 실패 했습니다. 다운로드 {0} 하 여 {1} 디렉터리에 저장 하세요. " 그리고 "패키지 설치:{0}SDK 설치 관리자에서 사용 가능" 오류 메시지에 대 한 자세한 내용은 Xamarin. 지원 패키지를 참조 하십시오.

### <a name="next-steps"></a>다음 단계

이 문서에서는 8 월 2016 현재 동작에 대해 설명 합니다. 이 문서에서 설명 하는 기술은 Xamarin 용 안정적인 테스트 제품군의 일부가 아니므로 나중에 중단 될 수 있습니다.

추가 지원이 필요 하면 microsoft에 문의 하거나, 위의 정보를 사용한 후에도이 문제가 계속 발생 하는 경우 [Xamarin에 사용할 수 있는 지원 옵션](~/cross-platform/troubleshooting/support-options.md) 을 참조 하세요. 연락처 옵션, 제안 사항 및 필요한 경우 새 버그를 제출 하는 방법에 대 한 자세한 내용은을 참조 하세요. .

