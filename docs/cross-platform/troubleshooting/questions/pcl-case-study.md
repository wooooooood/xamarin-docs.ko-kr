---
title: 진단과 관련 된 문제 해결-추적 및 TPL 데이터 흐름
description: 'PCL 사례 연구: Microsoft TPL 데이터 흐름 NuGet 패키지용 System.Diagnostics.Tracing과 관련된 문제를 해결하려면 어떻게 할까요?'
ms.prod: xamarin
ms.assetid: 7986A556-382D-4D00-ACCF-3589B4029DE8
ms.date: 04/17/2018
author: conceptdev
ms.author: crdun
ms.openlocfilehash: e3ced3c989a88c6e759a5bc497147128b0a79868
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70288202"
---
# <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-package"></a>PCL 사례 연구: Microsoft TPL 데이터 흐름 NuGet 패키지용 System.Diagnostics.Tracing과 관련된 문제를 해결하려면 어떻게 할까요?

> [!IMPORTANT]
> 의 `System.Diagnostic.Tracing` 이 특정 예제에서는 최신 버전의 Xamarin에서 기본적으로 오류를 더 이상 생성 하지 않습니다. 제안 된 해결 방법이 계속 작동 하지만 "오류 계층" 단원에 언급 된 일부 버그가 수정 되었습니다.
> 또한 .NET Standard은 이제 플랫폼 간 .NET Api를 구현 하는 데 선호 되는 방법입니다.

## <a name="summary"></a>요약

Xamarin.ios 및 Xamarin은 참조로 허용 하는 모든 PCL 프로필의 100%를 구현 하지 않습니다. Mac용 Visual Studio, Visual Studio 및 NuGet 패키지 관리자의 실용적인 편의를 위해 Xamarin 프로젝트를 사용 하면 _불완전_ 한 구현만 있는 여러 프로필을 사용할 수 있습니다. 예를 들어 Xamarin.ios와 Xamarin.ios에는 현재 "System.web" PCL 네임 스페이스에 있는 형식의 완전 한 구현이 포함 되어 있지 않습니다. 이러한 제한으로 인해 Microsoft TPL 데이터 흐름 NuGet 패키지의 기본 `portable-net45+win8+wpa81` 버전을 사용 하려고 할 때 세 가지 오류 계층이 발생 합니다.

## <a name="workaround-switch-the-app-project-to-reference-the-portable-net45win8wp8wpa81-version-of-the-tpl-dataflow-library"></a>해결 방법: 응용 프로그램 프로젝트를 전환 하 여 `portable-net45+win8+wp8+wpa81` TPL 데이터 흐름 라이브러리의 버전을 참조 합니다.

이 경우 세 가지 오류 계층이 모두 방지 되 고 모든 최신 버전의 Xamarin에서 작동 합니다.

1. 텍스트 편집기에서 응용 프로그램 프로젝트 **.csproj** 파일을 엽니다.

2. 다음과 같은 줄을 찾습니다.

    ```xml
    <HintPath>..\packages\Microsoft.Tpl.Dataflow.4.5.24\lib\portable-net45+win8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

3. 변경 `portable-net45+win8+wpa81` (추가`+wp8` 됨) `portable-net45+win8+wp8+wpa81` :

    ```xml
    <HintPath>..\packages\System.Threading.Tasks.Dataflow.4.5.25\lib\portable-net45+win8+wp8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

### <a name="explanation"></a>설명

라이브러리 `portable-net45+win8+wp8+wpa81` 의 버전은 _모든_문제를 해결 하는 데 문제가 없도록 완전히 **방지 합니다.**

### <a name="limitations"></a>제한 사항

- 버전 `portable-net45+win8+wp8+wpa81` 의 라이브러리에는 `portable-net45+win8+wpa81` 버전의 기능 중 100%가 포함 되지 않을 수 있습니다.

- NuGet 패키지 관리자는 기본적으로 `portable-net45+win8+wpa81` PCL nuget 패키지 버전을 설치 하므로 참조를 직접 조정 해야 합니다.

## <a name="details-about-the-three-layers-of-errors"></a>세 가지 오류 계층에 대 한 세부 정보

1. XamarinVS **외관 어셈블리는 현재** xamarin의 모든 Mac 버전 (public이 아닌 버그 34888)에 없으며 9.0 보다 낮은 모든 xamarin.ios 버전 (또는 Windows의 3.11.1443 보다 낮음)에서 제외 됩니다 (다음에서 수정 됨). [버그 32388](https://bugzilla.xamarin.com/show_bug.cgi?id=32388)). 이 문제가 발생 하면 배포 대상 및 링커 설정에 따라 다음 오류 중 하나가 발생 합니다.

    - Xamarin.Android.Common.targets: 오류: 어셈블리를 로드 하는 동안 예외가 발생 했습니다. System.IO.FileNotFoundException: ' 4.0.0.0, Version =, Culture = 중립, PublicKeyToken = b03f5f7f11d50a3a ' 어셈블리를 로드할 수 없습니다. Android 용 Mono 프로필에 존재 하지 않는 것 같습니다.

    - 파일이 나 어셈블리 ' System.object ' 또는 해당 종속성 중 하나를 로드할 수 없습니다. 시스템에서 지정한 파일을 찾을 수 없습니다. (System.IO.FileNotFoundException)

    - MTOUCH: error MT3001: '/Users/macuser/Projects/TPLDataflow/UnifiedSingleViewIphone1/obj/iPhone/Debug/mtouch-cache/64/Build/System.Threading.Tasks.Dataflow.dll ' 어셈블리를 AOT 수 없습니다.

    - MTOUCH: error MT2002: 어셈블리를 확인 하지 못했습니다. ' 4.0.0.0, Version =, Culture = 중립, PublicKeyToken = b03f5f7f11d50a3a '

2. ["System.web. 추적"에 있는 형식의 현재 Mono 구현](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) 에 일부 메서드 오버 로드가 없습니다 ([버그 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)). 이 문제가 발생 하면 Xamarin 앱을 빌드할 때 다음 링커 오류 중 하나가 발생 합니다.

    - /Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: 오류: 작업 LinkAssemblies를 실행 하는 동안 오류 발생: 오류 XA2006: 메타 데이터 항목 ' WriteEvent:: (System.web, System.object []) '에 대 한 참조입니다 (' 4.5.24.0, Version =, Culture = 중립, PublicKeyToken = b03f5f7f11d50a3a '). ' 4.5.24.0, Version =, Culture = 중립, PublicKeyToken = b03f5f7f11d50a3a '에서 확인할 수 없습니다.

    - MTOUCH: error MT2002: "WriteEvent, Version = 4.0.0.0, Culture = 중립, PublicKeyToken = b03f5f7f11d50a3a"에서 ",:: (System.web, System.object [])" 참조를 확인 하지 못했습니다.

3. ["System.web. 추적"에 있는 형식의 현재 Mono 구현](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) 도 현재 _빈_ "더미" 구현 ([버그 34890](https://bugzilla.xamarin.com/show_bug.cgi?id=34890))입니다. 따라서 런타임에 이러한 메서드를 사용 하려고 하면 예기치 않은 결과가 발생할 수 있습니다. Microsoft TPL 데이터 흐름 라이브러리 _의 경우에는 대부분_ 의 라이브러리 동작에 대 한 `WriteEvent(System.Int32,System.Object[])` 호출이 필수적이 지 않으므로 "계층 2" ([버그 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337), 빈 구현 추가)에 대 한 수정 사항이 충분할 수 있습니다. 대부분의 Microsoft TPL 데이터 흐름 사용 사례

## <a name="questions--answers"></a>질문 & 대답

### <a name="i-was-able-to-leave-linking-enabled-with-the-portable-net45win8wpa81-version-of-the-library-on-older-versions-of-xamarinios-or-on-xamarinandroid-how-did-that-work"></a>이전 버전의 xamarin.ios 또는 xamarin.ios에서 라이브러리의 `portable-net45+win8+wpa81` 버전에 대 한 링크를 사용할 수 있습니다. 어떻게 작동 하나요?

#### <a name="answer"></a>대답할

_참조 어셈블리_ `System.Diagnostics.Tracing.dll` 1\[ 에대한참조를포함하는경우에는이전버전의xamarin.ios또는Mac의xamarin에서빌드를완료하는"성공"(연결사용)을가져올수있습니다.\]그러나이는 "올바른" 해결 방법이 아니라, \[ 참조 어셈블리는 앱과 같은 플랫폼 특정 코드가 아닌 _이식 가능한 라이브러리_를 빌드할 때만 사용 됩니다. 참조 어셈블리에 포함 된 코드를 _실행_ 하려고 시도 하는 경우 (이에 대해서만 빌드하는 대신) 예기치 않은 결과가 발생할 수 있습니다. Mono 팀이 `WriteEvent(System.Int32,System.Object[])` [`EventSource`](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) 형식에 누락 된 오버 로드를 추가 하는 것이 올바른 수정입니다 ([버그 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)). 지금은 위의 해결 방법 섹션에 설명 된 대로 Microsoft `portable-net45+win8+wp8+wpa81` TPL 데이터 흐름 라이브러리의 버전으로 전환 하는 것이 가장 좋습니다.

(관련 된 이전 항목을 확인 한 후에이 문서를 읽을 수 있는 모든 사용자에 게는<https://stackoverflow.com/a/23591322/2561894>stackoverflow ()에서 답변을 보다 간단 참조 어셈블리와 외관 어셈블리 간의 차이점을 언급 _하지_ 않았습니다.)

**\[1\] "참조 어셈블리" 위치**

Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\System.Diagnostics.Tracing.dll`

Mac (Mono):`/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/xbuild-frameworks/.NETPortable/v4.5/System.Diagnostics.Tracing.dll`

**\[2\] "외관 어셈블리" 위치**

Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

Mac (Mono):`/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`


### <a name="will-it-help-if-i-manually-add-a-reference-to-the-systemdiagnosticstracing-facade-assembly"></a>"시스템 진단" 외관 어셈블리에 대 한 참조를 수동으로 추가 하는 경우 도움이 되나요?

_특히 이러한 2 단계를 사용 하 여 문제를 해결할 수 있나요?_

1. _다음 위치 `System.Diagnostics.Tracing.dll` 중 하나에서 응용 프로그램 프로젝트 폴더로 외관 어셈블리를 복사 합니다._

    Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

    Mac (Mono):`/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`

2. _Xamarin.ios 또는 Xamarin Android 응용 프로그램 프로젝트에서 외관 어셈블리에 대 한 참조를 추가 합니다._

#### <a name="answer"></a>대답할

아니요,이는 도움이 되지 않습니다.

- Windows에서 Xamarin.ios 9.0 또는 최신 버전의 Xamarin.ios의 경우이 해결 방법은 완전히 중복 되며, 동일한 id를 사용 하는 "어셈블리 ' 시스템에 대 한 컴파일 오류를 이미 가져왔습니다." 라는 오류가 발생할 수 있습니다.

- Mac에서 xamarin.ios 8.10이 하 또는 하위 Android의 경우이 해결 방법은 도움이 되지만 "계층 1" _에는 어셈블리_ 문제가 없습니다. "계층 2" 링커 오류는 해결 _되지_ 않으므로 완전 한 솔루션은 아닙니다.

### <a name="can-i-use-the-systemdiagnosticstracing-nuget-packagehttpswwwnugetorgpackagessystemdiagnosticstracing-to-solve-the-problem"></a>[진단 NuGet 패키지](https://www.nuget.org/packages/System.Diagnostics.Tracing/) 를 사용 하 여 문제를 해결할 수 있나요?

#### <a name="answer"></a>대답할

아니요, NuGet 3.0 "DNXCore50" 패키지는 "" 및 "netcore50"에 대 한 플랫폼별 구현만 포함 합니다. Xamarin Android ("MonoAndroid") 및 Xamarin.ios ("Monotouch.dialog" 및 "xamarinios")에 대 한 구현은 명시적으로 _생략_ 됩니다. 즉, 패키지를 설치 해도 Xamarin.ios 및 Xamarin.ios 프로젝트에는 _영향을 주지_ 않습니다. NuGet 패키지는 두 플랫폼 모두 형식의 _고유한_ 구현을 제공 한다고 가정 합니다. Mono _에는 네임 스페이스의 구현이 있지만_ , 위의 "세 가지 오류 계층 정보에 대 한 세부 정보"의 요소 \#2와 \#3에 설명 된 것 처럼 이러한 가정이 "올바른" 것으로 간주 됩니다. 안함. 따라서 Mono 팀이 [버그 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337) 및 [버그 34890](https://bugzilla.xamarin.com/show_bug.cgi?id=34890)을 해결 하기 위한 적절 한 해결 방법이 됩니다.

## <a name="next-steps"></a>다음 단계

추가 지원이 필요 하면 microsoft에 문의 하거나, 위의 정보를 사용한 후에도이 문제가 계속 발생 하는 경우 [Xamarin에 사용할 수 있는 지원 옵션](~/cross-platform/troubleshooting/support-options.md) 을 참조 하세요. 연락처 옵션, 제안 사항 및 필요한 경우 새 버그를 제출 하는 방법에 대 한 자세한 내용은을 참조 하세요. .
