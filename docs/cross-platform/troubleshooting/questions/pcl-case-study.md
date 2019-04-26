---
title: System.Diagnostics.Tracing 및 TPL 데이터 흐름에 관련 된 문제 해결
description: 'PCL 사례 연구: Microsoft TPL 데이터 흐름 NuGet 패키지용 System.Diagnostics.Tracing과 관련된 문제를 해결하려면 어떻게 할까요?'
ms.prod: xamarin
ms.assetid: 7986A556-382D-4D00-ACCF-3589B4029DE8
ms.date: 04/17/2018
author: asb3993
ms.author: amburns
ms.openlocfilehash: d9aa85b946f20addb7d69c559bff68c6b1f75429
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61342276"
---
# <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-package"></a>PCL 사례 연구: Microsoft TPL 데이터 흐름 NuGet 패키지용 System.Diagnostics.Tracing과 관련된 문제를 해결하려면 어떻게 할까요?

> [!IMPORTANT]
> 이 특정 예 `System.Diagnostic.Tracing` 더 이상 최신 버전의 Xamarin에는 기본적으로 모든 오류를 생성 합니다. 제안 된 해결 방법을 계속 작동 하는 동안 "계층의 오류" 섹션에 언급 된 버그 중 일부 고정 note 합니다.
> 또한.NET Standard는 이제 플랫폼 간.NET Api를 구현 하는 기본 방법은 점에 유의 해야 합니다.

## <a name="summary"></a>요약

Xamarin.iOS 및 Xamarin.Android에는 참조로 허용 되는 모든 PCL 프로필의 100%를 구현 하지 않습니다. Visual Studio for Mac을 Visual Studio 및 NuGet 패키지 관리자에서에서 실제 편의 위해 Xamarin 프로젝트만 있는 몇 가지 프로필 사용을 허용 _불완전 한_ 구현 합니다. 예를 들어, Xamarin.iOS도 아니고 Xamarin.Android 현재 포함 "System.Diagnostics.Tracing" PCL에서에서 형식의 완전히 구현 하는 네임 스페이스입니다. 이 제한은 기본값을 사용 하려고 할 때 세 가지 계층의 오류 발생 시키는 `portable-net45+win8+wpa81` Microsoft TPL 데이터 흐름 NuGet 패키지의 버전입니다.

## <a name="workaround-switch-the-app-project-to-reference-the-portable-net45win8wp8wpa81-version-of-the-tpl-dataflow-library"></a>해결 방법: 앱 프로젝트 참조를 전환 합니다 `portable-net45+win8+wp8+wpa81` TPL 데이터 흐름 라이브러리의 버전

(모든 최신 버전의 Xamarin에 대 한 오류와의 모든 세 가지 계층 방지합니다.)

1. 응용 프로그램 프로젝트를 엽니다 **.csproj** 텍스트 편집기에서 파일입니다.

2. 다음과 유사한 줄을 찾습니다.

    ```xml
    <HintPath>..\packages\Microsoft.Tpl.Dataflow.4.5.24\lib\portable-net45+win8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

3. 변경 `portable-net45+win8+wpa81` 하 `portable-net45+win8+wp8+wpa81` (`+wp8` 추가):

    ```xml
    <HintPath>..\packages\System.Threading.Tasks.Dataflow.4.5.25\lib\portable-net45+win8+wp8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

### <a name="explanation"></a>설명

`portable-net45+win8+wp8+wpa81` 라이브러리의 버전을 참조 하지 않습니다 **System.Diagnostics.Tracing.dll** _전혀_이므로 문제의 세 계층을 모두 완전히 방지 합니다.

### <a name="limitations"></a>제한 사항

- 합니다 `portable-net45+win8+wp8+wpa81` 버전의 라이브러리 기능의 100%를 포함 하지 않을 수 있습니다는 `portable-net45+win8+wpa81` 버전입니다.

- NuGet 패키지 관리자 설치를 `portable-net45+win8+wpa81` 참조를 수동으로 조정 해야 하므로 기본적으로 PCL NuGet 패키지의 버전입니다.

## <a name="details-about-the-three-layers-of-errors"></a>세 가지 계층의 오류에 대 한 세부 정보

1. 합니다 **System.Diagnostics.Tracing.dll** 외관 어셈블리는 현재 모든 Mac 버전의 Xamarin.Android (public이 아닌 버그 34888)에서 없기 및 없기 모든 Xamarin.iOS 버전 9.0 미만 (또는 미만 XamarinVS 3.11.1443 Windows)에서 (고정 [버그 32388](https://bugzilla.xamarin.com/show_bug.cgi?id=32388)). 이 문제가 발생 하면 배포 대상과 링커에 따라 다음 오류 중 하나가 설정:

    - Xamarin.Android.Common.targets: 오류: 어셈블리를 로드 하는 동안 예외가 발생 합니다. System.IO.FileNotFoundException: 어셈블리를 로드할 수 없습니다 ' System.Diagnostics.Tracing, 버전 4.0.0.0, Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a ='. 아마도 Android 프로필에 대 한 Mono에서 존재 하지 않는?

    - 파일이 나 어셈블리 'System.Diagnostics.Tracing' 또는 해당 종속성 중 하나를 로드할 수 없습니다. 지정한 파일을 찾을 수 없습니다. (System.IO.FileNotFoundException)

    - MTOUCH: 오류 MT3001: AOT 어셈블리 하지 못했습니다 ' / Users/macuser/Projects/TPLDataflow/UnifiedSingleViewIphone1/obj/iPhone/Debug/mtouch-cache/64/Build/System.Threading.Tasks.Dataflow.dll '

    - MTOUCH: 오류 MT2002: 어셈블리를 확인 하지 못했습니다. 'System.Diagnostics.Tracing, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a'

2. 현재 ["System.Diagnostics.Tracing"에 있는 형식의 모노 구현을](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) 메서드를 오버 로드를 누락 되었습니다 ([버그 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)). Xamarin 앱을 빌드할 때이 문제는 다음 링커 오류 중 하나가 하면:

    - / Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: 오류: LinkAssemblies 작업 실행 오류: XA2006 오류: 메타 데이터에 대 한 참조는 ' System.Void System.Diagnostics.Tracing.EventSource::WriteEvent(System.Int32,System.Object[])' 항목 (에 정의 된 ' System.Threading.Tasks.Dataflow, 버전 4.5.24.0, Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a =') ' System.Threading.Tasks.Dataflow, 버전 4.5.24.0, Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a =' 확인할 수 없습니다.

    - MTOUCH: 오류 MT2002: "System.Void System.Diagnostics.Tracing.EventSource::WriteEvent(System.Int32,System.Object[])" 참조를 확인 하지 못했습니다 "System.Diagnostics.Tracing, 버전 4.0.0.0, Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a ="

3. 현재 ["System.Diagnostics.Tracing"에 있는 형식의 모노 구현을](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) 이기도 현재는 _빈_ "더미" 구현 ([버그 34890](https://bugzilla.xamarin.com/show_bug.cgi?id=34890)). 런타임 시 이러한 메서드를 사용 하려고 예기치 않은 결과 생성할 따라서 수 있습니다. 에 대 한는 _특정_ 사례 것에 대 한 호출의 Microsoft TPL 데이터 흐름 라이브러리 `WriteEvent(System.Int32,System.Object[])` 대부분의 라이브러리의 동작에 대 한 중요 하지 않은 하므로 "레이어 2"에 대 한 수정 ([버그 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337), 빈 추가 구현) 대부분의 Microsoft TPL 데이터 흐름 사용 사례에 대 한 충분할 것입니다.

## <a name="questions--answers"></a>질문 및 답변

### <a name="i-was-able-to-leave-linking-enabled-with-the-codeportable-net45win8wpa81code-version-of-the-library-on-older-versions-of-xamarinios-or-on-xamarinandroid-how-did-that-work"></a>사용 하도록 설정 된 연결 유지 있었습니다는 <code>portable-net45+win8+wpa81</code> 이전 버전의 Xamarin.iOS 또는 Xamarin.Android 라이브러리의 버전입니다. 어떻게 작동 했나요?

#### <a name="answer"></a>응답

것 _가능한_ 시작 하기 빌드가 완료 "성공적 으로" (활성화 링크 사용) Mac에서 Xamarin.Android 또는 Xamarin.iOS의 이전 버전에서에 대 한 참조를 포함 하는 경우를 `System.Diagnostics.Tracing.dll` _어셈블리참조_ \[1\] 것이 아니라 _외관 어셈블리_ \[2], 하지만 아쉽게도이 아닙니다 "올바른" 문제를 해결 합니다. 참조 어셈블리는 빌드 시 사용할 으로만 _이식 가능한 라이브러리_, 앱과 같은 플랫폼별으로 코드입니다. 하려고 _실행_ 참조 어셈블리 (만 빌드에 대해 아님)에 포함 된 코드는 예기치 않은 결과가 발생할 수 있습니다. 누락 된 추가할 Mono 팀에 대 한 올바른 수정 됩니다 `WriteEvent(System.Int32,System.Object[])` 를 오버 로드는 [ `EventSource` ](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) 형식 ([버그 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)). 전환 하려면 가장 적합 한 옵션은 이제는 `portable-net45+win8+wp8+wpa81` 위 해결 방법 섹션에 설명 된 대로 Microsoft TPL 데이터 흐름 라이브러리의 버전입니다.

(StackOverflow에서 관련된 오래 되 고 보다 대답을 확인 한 후이 문서를 읽을 수 있는 사람이 라면 (<https://stackoverflow.com/a/23591322/2561894>), 참조 어셈블리 및 외관 어셈블리 구분 했습니다 보면 _하지_ 나와 있습니다.)

**\[1\] "어셈블리 참조" 위치**

Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\System.Diagnostics.Tracing.dll`

Mac (Mono): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/xbuild-frameworks/.NETPortable/v4.5/System.Diagnostics.Tracing.dll`

**\[2\] "외관 어셈블리" 위치**

Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

Mac (Mono): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`


### <a name="will-it-help-if-i-manually-add-a-reference-to-the-systemdiagnosticstracing-facade-assembly"></a>"System.Diagnostics.Tracing" 외관 어셈블리에 대 한 참조를 수동으로 추가 하면 도움말은?

_2 단계를 사용 하 여 문제를 해결할 수는 특히_

1. _복사를 `System.Diagnostics.Tracing.dll` 외관 어셈블리를 다음 위치 중 하나에서 응용 프로그램 프로젝트 폴더에 있습니다._

    Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

    Mac (Mono): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`

2. _Xamarin.iOS 또는 Xamarin.Android 응용 프로그램 프로젝트에는 외관 어셈블리에 대 한 참조를 추가 합니다._

#### <a name="answer"></a>응답

아니요,이 도움이 되지 않습니다.

- Xamarin.iOS 9.0 또는 모든 최신 버전의 Windows에서 Xamarin.Android의 경우이 해결 방법은 엄격 하 게 중복 되므로 "동일한 id 사용 하 여 어셈블리 'System.Diagnostics.Tracing' 이미 가져왔습니다." 유사한 컴파일 오류가 발생할 수 있습니다.

- 이 해결 방법에서는 Xamarin.iOS 8.10 이하로 또는 Mac에서 Xamarin.Android를 하지만 _만_ "레이어 1" 누락 된 어셈블리 문제에 대 한 합니다. 됩니다 _되지_ 완벽 한 솔루션을 되지 않도록 "레이어 2" 링커 오류를 해결 합니다.

### <a name="can-i-use-the-systemdiagnosticstracing-nuget-packagehttpswwwnugetorgpackagessystemdiagnosticstracing-to-solve-the-problem"></a>사용할 수는 [System.Diagnostics.Tracing NuGet 패키지](https://www.nuget.org/packages/System.Diagnostics.Tracing/) 문제를 해결 하 시겠습니까?

#### <a name="answer"></a>응답

아니요, NuGet 3.0 "System.Diagnostics.Tracing" 패키지 "DNXCore50" 및 "netcore50"에 대 한 플랫폼별 구현을 포함합니다. 이 명시적으로 _생략_ ("MonoTouch" 및 "xamarinios") Xamarin.iOS 및 Xamarin.Android ("MonoAndroid")에 대 한 구현 합니다. 즉, 패키지를 설치 합니다 _효과가_ Xamarin.iOS 및 Xamarin.Android 프로젝트에 대 한 합니다. NuGet 패키지를 해당 플랫폼의 둘 다가 제공 하는 것으로 가정 자신의 _고유의_ 형식의 구현 합니다. 이 가정은 "올바른" Mono가 된다는 점에서 _는_ 구현에서 설명한 지점 아니라 네임 스페이스 \#2 및 \#위의 "세 계층의 오류에 대 한 자세한 정보" 3는 구현은 현재 완전 하지 않습니다. 해결 하려면 Mono 팀에 대 한 적절 한 수정 되도록 [버그 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337) 하 고 [버그 34890](https://bugzilla.xamarin.com/show_bug.cgi?id=34890)합니다.

## <a name="next-steps"></a>다음 단계

추가 지원이 필요한 경우, 우리에 게 문의 하 여 위의 정보를 활용 하 여 한 후에이 문제가 여전히 발생 하는 경우를 참조 하세요 [Xamarin에 대해 사용할 수 있는 지원 옵션?](~/cross-platform/troubleshooting/support-options.md) 연락처 옵션을 제안에 대 한 정보에 대 한 방법 뿐만 아니라 필요한 경우 새 버그를 파일입니다.
