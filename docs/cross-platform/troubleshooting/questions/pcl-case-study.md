---
title: 'PCL 사례 연구: System.Diagnostics.Tracing Microsoft TPL 데이터 흐름 NuGet 패키지에 대 한 관련 된 문제는 어떻게 해결할 수 있습니까?'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7986A556-382D-4D00-ACCF-3589B4029DE8
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 04814b78fd035005aabd8b9229d36bbda17ba140
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-package"></a>PCL 사례 연구: System.Diagnostics.Tracing Microsoft TPL 데이터 흐름 NuGet 패키지에 대 한 관련 된 문제는 어떻게 해결할 수 있습니까?

## <a name="summary"></a>요약

Xamarin.iOS 및 Xamarin.Android 참조 이름으로 허용 하는 모든 PCL 프로필의 100%를 구현 하지 않습니다. Xamarin 프로젝트만 있는 여러 프로필을 사용할 수 있도록 Mac, Visual Studio 및 NuGet 패키지 관리자 Visual Studio에서 실용적인 편의 위해 _불완전 한_ 구현 합니다. 예를 들어, Xamarin.iOS 아니고 Xamarin.Android에서는 현재 "System.Diagnostics.Tracing" PCL에에서 있는 형식의 완전 한 구현 네임 스페이스입니다. 기본 사용 하려고 할 때이 제한 사항은 3 계층의 오류 까지로 `portable-net45+win8+wpa81` Microsoft TPL 데이터 흐름 NuGet 패키지의 버전입니다.


## <a name="workaround-switch-the-app-project-to-reference-the-portable-net45win8wp8wpa81-version-of-the-tpl-dataflow-library"></a>해결 방법: 앱 프로젝트 참조를 전환 하 여 `portable-net45+win8+wp8+wpa81` TPL 데이터 흐름 라이브러리의 버전

(모든 3 계층 오류 및 모든 최신 버전의 Xamarin에 대해 작동을 방지합니다.)

1. 응용 프로그램 프로젝트를 열고 `.csproj` 파일 텍스트 편집기에서.

2. 다음과 비슷한 줄을 찾습니다.

    ```xml
    <HintPath>..\packages\Microsoft.Tpl.Dataflow.4.5.24\lib\portable-net45+win8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

3. `portable-net45+win8+wpa81`를 `portable-net45+win8+**wp8**+wpa81`로 변경합니다.

    ```xml
    <HintPath>..\packages\System.Threading.Tasks.Dataflow.4.5.25\lib\portable-net45+win8+wp8+wpa81\System.Threading.Tasks.Dataflow.dll</HintPath>
    ```

### <a name="explanation"></a>설명

`portable-net45+win8+wp8+wpa81` 라이브러리의 버전 "System.Diagnostics.Tracing.dll"를 참조 하지 않으면 _전혀_완전히 모든 3 계층의 문제를 방지 하기 때문입니다.

### <a name="limitations"></a>제한 사항

- `portable-net45+win8+wp8+wpa81` 라이브러리의 버전의 기능을 100%를 포함 되지 않을 수는 `portable-net45+win8+wpa81` 버전입니다.

- NuGet 패키지 관리자 설치에서 `portable-net45+win8+wpa81` 기본적으로 대 한 참조를 수동으로 조정 해야 합니다는 PCL NuGet 패키지의 버전입니다.




## <a name="details-about-the-3-layers-of-errors"></a>3 계층의 오류에 대 한 세부 정보

1. `System.Diagnostics.Tracing.dll` 외관 어셈블리는 현재 고 Xamarin.Android (public이 아닌 버그 34888)의 모든 Mac 버전 둘이 없는 모든 Xamarin.iOS 버전 9.0 보다 낮은 (또는 Windows에서 3.11.1443 XamarinVS 보다 낮은) (고정 [32388버그](https://bugzilla.xamarin.com/show_bug.cgi?id=32388)). 이 문제는 배포 대상 및 링커에 따라 다음 오류 중 하나가 설정 발생 합니다.

    - > Xamarin.Android.Common.targets: 오류: 어셈블리를 로드 하는 동안 예외: System.IO.FileNotFoundException: 어셈블리를 로드할 수 없습니다 ' System.Diagnostics.Tracing, Version = 4.0.0.0, Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a'. 아마도 Android 프로필에 대 한 모노에 존재 하지 않는?

    - > 파일이 나 어셈블리 'System.Diagnostics.Tracing' 또는 해당 종속성 중 하나를 로드할 수 없습니다. 지정한 파일을 찾을 수 없습니다. (System.IO.FileNotFoundException)

    - > MTOUCH: MT3001 오류: 어셈블리 AOT 하지 못했습니다 ' / Users/macuser/Projects/TPLDataflow/UnifiedSingleViewIphone1/obj/iPhone/Debug/mtouch-cache/64/Build/System.Threading.Tasks.Dataflow.dll '

    - > MTOUCH: MT2002 오류: 어셈블리를 확인 하지 못했습니다: ' System.Diagnostics.Tracing, Version = 4.0.0.0, Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a'



2. 현재 ["System.Diagnostics.Tracing"의 형식이의 모노 구현을](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) 일부 메서드 오버 로드가 없습니다 ([버그 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)). Xamarin 앱을 빌드할 때이 문제는 다음 링커 오류 중 하나가 하면:

    - > / Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: 오류: LinkAssemblies 작업 오류 실행: XA2006 오류: 메타 데이터 항목에 대 한 참조 ' System.Void System.Diagnostics.Tracing.EventSource:: WriteEvent(System.Int32,System.Object[])' (에 정의 된 ' System.Threading.Tasks.Dataflow, 버전 4.5.24.0, Culture = neutral, PublicKeyToken = = b03f5f7f11d50a3a')에서 ' System.Threading.Tasks.Dataflow, Version = 4.5.24.0, Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a' 확인할 수 없습니다.

    - > MTOUCH: MT2002 오류: "System.Void System.Diagnostics.Tracing.EventSource::WriteEvent(System.Int32,System.Object[])" 참조를 확인 하지 못했습니다. "System.Diagnostics.Tracing, Version = 4.0.0.0, Culture = neutral, PublicKeyToken = b03f5f7f11d50a3a "


3. 현재 ["System.Diagnostics.Tracing"의 형식이의 모노 구현을](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) 현재 이기도 한 _빈_ "dummy" 구현 ([버그 34890](https://bugzilla.xamarin.com/show_bug.cgi?id=34890)). 런타임 시 이러한 메서드를 사용 하려는 모든 시도가 예기치 않은 결과 생성할 따라서 수 있습니다. 에 대 한는 _특정_ 대/소문자에 대 한 호출의 Microsoft TPL 데이터 흐름 라이브러리는 같습니다 `WriteEvent(System.Int32,System.Object[])` 대부분 라이브러리의 동작에 대 한 중요 하지 않은 하므로 "레이어 2"에 대 한 수정 ([버그 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337), 빈 추가 구현)은 likey 대부분 Microsoft TPL 데이터 흐름 사용 사례에 적합 합니다.




## <a name="questions--answers"></a>질문 및 답변


###<a name="i-was-able-to-leave-linking-enabled-with-the-codeportable-net45win8wpa81code-version-of-the-library-on-older-versions-of-xamarinios-or-on-xamarinandroid-how-did-that-work"></a>"연결을 사용 하도록 설정 하는 상태로 유지 하는 할 수 있습니다는 <code>portable-net45+win8+wpa81</code> Xamarin.iOS의 이전 버전 또는 Xamarin.Android 라이브러리의 버전입니다. 어떻게 진행 되었습니까? "

#### <a name="answer"></a>응답

_가능한_ 를 가져오려면 빌드를 완료 "성공적 으로" (링크 사용 활성화) 또는 Mac에서 Xamarin.Android Xamarin.iOS의 이전 버전에서에 대 한 참조를 포함 하는 경우는 `System.Diagnostics.Tracing.dll` _어셈블리참조_ \[1\] 보다는 _외관 어셈블리_ \[2], 있지만 하지만이 "올바름" 문제를 해결 합니다. 참조 어셈블리를 빌드할 때 사용 하는 것만 _이식 가능한 라이브러리_, 응용 프로그램 같은 플랫폼별으로 코드입니다. 하려고 _실행_ 참조 어셈블리 (정당한 빌드에 대해 아님)에 포함 된 코드는 예기치 않은 결과를 생성 합니다. 누락 된 추가할 모노 팀에 대 한 올바른 수정 될 수 `WriteEvent(System.Int32,System.Object[])` 를 오버 로드는 [ `EventSource` ](https://github.com/mono/mono/blob/master/mcs/class/corlib/System.Diagnostics.Tracing/EventSource.cs) 유형 ([버그 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337)). 가장 적합 한 옵션으로 전환 하는 것 이제는 `portable-net45+win8+wp8+wpa81` 위 해결 방법 섹션에 설명 된 대로 Microsoft TPL 데이터 흐름 라이브러리의 버전입니다.

(StackOverflow에서 관련된 이전, 길면 대답을 확인 한 후이 문서를 읽을 수는 사람에 대해 (<http://stackoverflow.com/a/23591322/2561894>), 참조 어셈블리 및 외관 어셈블리 구별 있었던 노트 _하지_ 나와 있습니다.)


**\[1\] 위치 "어셈블리 참조"**

Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\System.Diagnostics.Tracing.dll`

Mac (모노): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/xbuild-frameworks/.NETPortable/v4.5/System.Diagnostics.Tracing.dll`

**\[2\] "외관 어셈블리" 위치**

Windows: <code>C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\<strong>Facades</strong>\System.Diagnostics.Tracing.dll</code>

Mac (모노): <code>/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/<strong>Facades</strong>/System.Diagnostics.Tracing.dll</code>


###<a name="will-it-help-if-i-manually-add-a-reference-to-the-systemdiagnosticstracing-facade-assembly"></a>"도움이"System.Diagnostics.Tracing"외관 어셈블리에 대 한 참조를 수동으로 추가 합니까"?

다음 2 단계를 사용 하 여 문제를 해결할 수는 특히

1. 복사는 `System.Diagnostics.Tracing.dll` 외관 어셈블리를 다음 위치 중 하나에서 응용 프로그램 프로젝트 폴더에 있습니다.

    Windows: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.5\Facades\System.Diagnostics.Tracing.dll`

    Mac (모노): `/Library/Frameworks/Mono.framework/Versions/Current/lib/mono/4.5/Facades/System.Diagnostics.Tracing.dll`

2. Xamarin.iOS 또는 Xamarin.Android 응용 프로그램 프로젝트에는 외관 어셈블리에 대 한 참조를 추가 합니다.

#### <a name="answer"></a>응답

아니요,이 도움이 되지 않습니다.

- Xamarin.iOS 9.0 또는 모든 최신 버전 Windows에서 Xamarin.Android의 경우이 해결 방법은 엄격 하 게 되며 "는 동일한 id 사용 하 여 'System.Diagnostics.Tracing' 어셈블리는 이미 가져왔습니다."와 같은 컴파일 오류가 발생할 수 있습니다.

- 이 해결 방법은 할 Xamarin.iOS 8.10 이하로 또는 Mac에서 Xamarin.Android 있지만 _만_ "레이어 1" 누락 된 어셈블리 문제에 대 한 합니다. 됩니다 _하지_ 완벽 한 솔루션 되기 때문에 "레이어 2" 링커 오류를 해결 합니다.


###"사용할 수는 <a href="https://www.nuget.org/packages/System.Diagnostics.Tracing/">System.Diagnostics.Tracing NuGet 패키지</a> 문제 해결을 위해?"

#### <a name="answer"></a>응답

아니요, NuGet 3.0 "System.Diagnostics.Tracing" 패키지에는 "DNXCore50" 및 "netcore50"에 대 한 플랫폼별 구현을 포함 됩니다. 이 명시적으로 _생략_ ("MonoTouch" 및 "xamarinios") Xamarin.iOS 및 Xamarin.Android ("MonoAndroid")에 대 한 구현 합니다. 즉, 패키지를 설치 해야는 _아무런 영향을 주지_ Xamarin.iOS 및 Xamarin.Android 프로젝트에 대 한 합니다. NuGet 패키지는 해당 플랫폼의 둘 다 제공 하는 것으로 가정 자신의 _자체_ 형식 구현입니다. 이 가정은 "올바른" 모노가 있다는 점에서 _는_ 네임 스페이스에서 있지만 아래에서 설명된 하는 지점으로 구현 \#2 및 \#"세부 정보" 3 계층의 오류에 대 한 구현 위에 3 현재 완전 하지 않습니다. 해결 하려면 모노 팀에 대 한 적절 한 수정 될 수 있도록 [버그 27337](https://bugzilla.xamarin.com/show_bug.cgi?id=27337) 및 [버그 34890](https://bugzilla.xamarin.com/show_bug.cgi?id=34890)합니다.


## <a name="next-steps"></a>다음 단계

추가 지원을 문의 또는 위의 정보를 활용 한 후에이 문제가 여전히 발생 하는 경우를 참조 하십시오 [지원 옵션에 대해 Xamarin에 대 한 사용할 수 있는?](~/cross-platform/troubleshooting/support-options.md) 연락처 옵션, 제안 사항에 대 한 내용은 하는 방법 필요한 경우 새 버그를 파일입니다.
