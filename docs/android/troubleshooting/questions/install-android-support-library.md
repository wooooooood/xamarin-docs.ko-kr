---
title: 수동으로 Xamarin.Android.Support 패키지에 필요한 Android 지원 라이브러리를 설치할 수는 방법
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A9CB8CA8-8A6D-405E-B84C-A16CE452C0F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: e760a87cbd1e0220ed5cf3a350d3539ffe29650e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30766002"
---
# <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packages"></a>수동으로 Xamarin.Android.Support 패키지에 필요한 Android 지원 라이브러리를 설치할 수는 방법

## <a name="example-steps-for-xamarinandroidsupportv4"></a>Xamarin.Android.Support.v4에 대 한 예제 단계 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

(예를 들어 설치 하 여 NuGet 패키지 관리자를 통해) 원하는 Xamarin.Android.Support NuGet 패키지를 다운로드 합니다.

사용 하 여 `ildasm` 의 어떤 버전을 확인 하려면 **android_m2repository.zip** NuGet 패키지 해야 합니다.

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr SourceUrl
```
예제 출력:

```cmd
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
```

다운로드 **android\_m2repository.zip** Google에서 URL을 사용 하 여에서 반환 된 **ildasm**합니다. 또는 버전을 확인할 수 있습니다는 _Android 지원 저장소_ 현재 Android SDK Manager를 설치 했습니다.

!["Android SDK Manager Android 지원 저장소 32 설치 된 버전을 보여 주는"](install-android-support-library-images/sdk-extras.png)

NuGet 패키지에 대 한 필요한 일치를 새 어떤 항목도 다운로드할 필요가 없습니다. 있습니다 수 대신 다시 압축 기존 **m2repository** 디렉터리 아래에 있는 **extras\\android** 에 _SDK 경로_ (Android의 맨 위에 나와 있는 것 처럼 SDK Manager 창)입니다.

반환 하는 URL의 MD5 해시를 계산할 **ildasm**합니다. 모든 대 문자와 공백 없이 사용 하는 결과 문자열의 서식을 지정 합니다. 예를 들어 조정는 `$url` 있는 것 처럼 변수 필요한 한 후 다음 두 줄을 실행 (기반 [Xamarin.Android에서 원래 C# 코드](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208)) PowerShell에서:

```powershell
$url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"
(([System.Security.Cryptography.MD5]::Create()).ComputeHash([System.Text.Encoding]::UTF8.GetBytes($url)) | %{ $_.ToString("X02") }) -join ""
```
예제 출력:

```powershell
F16A3455987DBAE5783F058F19F7FCDF
```

복사 **android\_m2repository.zip** 에 **% LOCALAPPDATA %\\Xamarin\\이동\\**  폴더입니다. 이전 MD5 해시 계산 단계에서에서 MD5 해시를 사용 하는 파일을 이름을 바꿉니다. 예를 들어:

**%LOCALAPPDATA%\\Xamarin\\zips\\F16A3455987DBAE5783F058F19F7FCDF.zip**

(선택 사항) 에 파일의 압축을 푸는 **% LOCALAPPDATA %\\Xamarin\\Xamarin.Android.Support.v4\\23.4.0.0\\콘텐츠\\**  (만들기는 **콘텐츠\\m2repository** 하위 디렉터리). 이 단계를 건너뛸 경우 다음 라이브러리를 사용 하는 첫 번째 빌드 약간 더 오래 걸립니다 하므로이 단계를 완료 해야 합니다.
하위 디렉터리에 대 한 버전 번호 (**23.4.0.0** 이 예에서) NuGet 패키지 버전 매우 동일 하지 않습니다. 사용할 수 있습니다 `ildasm` 올바른 버전 번호를 찾으려면:

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr /C:"string 'Version'"
```
예제 출력:

```cmd
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
```

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

(예를 들어 설치 하 여 NuGet 패키지 관리자를 통해) 원하는 Xamarin.Android.Support NuGet 패키지를 다운로드 합니다.

두 번 클릭은 _Xamarin.Android.Support.v4_ 으로 어셈블리는 _참조_ 어셈블리 브라우저에서 어셈블리를 열려는 Mac 용 Visual Studio에서 Android 프로젝트의 섹션입니다. 확인는 _언어_ 드롭 다운으로 설정 되어 _C#_ 최상위 선택 하 고 _Xamarin.Android.Support.v4_ 어셈블리 브라우저 탐색 트리에서에서 어셈블리. 찾을 `SourceUrl` 속성 중 하나는 `IncludeAndroidResourcesFrom` 또는 `JavaLibraryReference` 특성:

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

다운로드 **android\_m2repository.zip** 사용 하 여 Google의 `SourceUrl` 에서 반환 된 **ildasm**합니다. 또는 버전을 확인할 수 있습니다는 _Android 지원 저장소_ 현재 Android SDK Manager를 설치 했습니다.

!["Android SDK Manager Android 지원 저장소 32 설치 된 버전을 보여 주는"](install-android-support-library-images/sdk-extras.png)

NuGet 패키지에 대 한 필요한 일치를 새 어떤 항목도 다운로드할 필요가 없습니다. 있습니다 수 대신 다시 압축 기존 **m2repository** 디렉터리 아래에 있는 **extras/android** 에 _SDK 경로_ (같이 Android SDK Manager 창의 위쪽) .

반환 하는 URL의 MD5 해시를 계산할 **ildasm**합니다. 모든 대 문자와 공백 없이 사용 하는 결과 문자열의 서식을 지정 합니다. 예를 들어, 필요에 따라 URL 문자열을 조정 하 고 다음 명령을 실행 하는 **Terminal.app** 명령 프롬프트:

```bash
echo -n "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip" | md5 | tr '[:lower:]' '[:upper:]'
```

다른 옵션은 사용 하는 `csharp` 실행 인터프리터 [동일한 C# 코드를 사용 하 여 자체 Xamarin.Android](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208)합니다.
이렇게 하려면 조정는 `url` 으로 필요한 변수와 다음 명령을 실행 하는 **Terminal.app** 명령 프롬프트:

```bash
csharp -e 'var url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"; string.Concat((System.Security.Cryptography.MD5.Create().ComputeHash(System.Text.Encoding.UTF8.GetBytes(url))).Select(b => b.ToString("X02")))'
```
예제 출력:

```bash
F16A3455987DBAE5783F058F19F7FCDF
```

복사 **android\_m2repository.zip** 에 **$HOME/.local/share/Xamarin/zips/** 폴더입니다. 이전 MD5 해시 계산 단계에서에서 MD5 해시를 사용 하는 파일을 이름을 바꿉니다. 예를 들어:

**$HOME/.local/share/Xamarin/zips/F16A3455987DBAE5783F058F19F7FCDF.zip**

(선택 사항) 에 파일을 압축을 풉니다. 

**$HOME/.local/share/Xamarin/Xamarin.Android.Support.v4/23.4.0.0/content/**

(만들기는 **콘텐츠/m2repository** 하위 디렉터리). 이 단계를 건너뛸 경우 다음 라이브러리를 사용 하는 첫 번째 빌드 약간 더 오래 걸립니다 하므로이 단계를 완료 해야 합니다.

하위 디렉터리에 대 한 버전 번호 (**23.4.0.0** 이 예에서) NuGet 패키지 버전 매우 동일 하지 않습니다. 와 같이 **ildasm** 앞 단계, Mac 용 Visual Studio에서 어셈블리 브라우저를 사용 하 여 올바른 버전 번호를 찾을 수 있습니다. 에 대 한 확인은 `Version` 속성 중 하나는 `IncludeAndroidResourcesFrom` 또는 `JavaLibraryReference` 특성:

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

-----


## <a name="additional-references"></a>추가 참조

- [버그 43245](https://bugzilla.xamarin.com/show_bug.cgi?id=43245) – Inaccurate "다운로드 하지 못했습니다. {0}를 다운로드 하 고 하세요 {1 \} 디렉터리에 넣습니다. " 및 "패키지를 설치 하십시오: '{0}' SDK 설치 관리자에서 사용할 수 있는" Xamarin.Android.Support 패키지와 관련 된 오류 메시지

### <a name="next-steps"></a>다음 단계

이 문서에서는 2016 년 8 월을 기준으로 현재 동작에 설명 합니다. 이 문서에서 설명 하는 방법 앞으로 손상 될 수 있으므로, Xamarin에 대 한 안정적인 테스트 제품군의 일부가 아닙니다.

추가 지원을 문의 또는 위의 정보를 활용 한 후에이 문제가 여전히 발생 하는 경우를 참조 하십시오 [지원 옵션에 대해 Xamarin에 대 한 사용할 수 있는?](~/cross-platform/troubleshooting/support-options.md) 연락처 옵션, 제안 사항에 대 한 내용은 하는 방법 필요한 경우 새 버그를 파일입니다.

