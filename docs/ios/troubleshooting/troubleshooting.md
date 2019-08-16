---
title: Xamarin.ios에 대 한 문제 해결 팁
description: 이 문서에서는 Xamarin.ios 응용 프로그램을 개발 하는 동안 문제를 해결 하는 데 유용한 다양 한 팁을 제공 합니다. 특정 오류 메시지 및 기타 잠재적인 문제를 설명 합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: B50FE9BD-9E01-AE88-B178-10061E3986DA
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/22/2018
ms.openlocfilehash: 37e5e68aff293910db4638c52f592e10fd60abfa
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69528129"
---
# <a name="troubleshooting-tips-for-xamarinios"></a>Xamarin.ios에 대 한 문제 해결 팁 

## <a name="xamarinios-cannot-resolve-systemvaluetuple"></a>Xamarin.ios가 System.valuetuple를 확인할 수 없습니다.

이 오류는 Visual Studio와의 비 호환성으로 인해 발생 합니다.

- **Visual Studio 2017 업데이트 1** (버전 15.1 이상)는 **System.valuetuple NuGet 4.3.0** (또는 이전 버전)과만 호환 됩니다.

- **Visual Studio 2017 업데이트 2** (버전 15.2 이상)은 **System.valuetuple NuGet 4.3.1** 이상과만 호환 됩니다.

Visual Studio 2017 설치에 해당 하는 올바른 System.valuetuple NuGet을 선택 하세요.


## <a name="receiving-error-retrieving-update-information-error-message"></a>' 업데이트 정보를 검색 하는 동안 오류 발생 ' 오류 메시지를 받고 있습니다.

소프트웨어를 업데이트 하려고 할 때이 오류 메시지가 표시 되 면 IDE를 다시 시작 하 고 로그 아웃 한 다음 IDE의 계정으로 다시 로그인 하세요.

## <a name="how-do-i-create-outlets-or-actions-with-interface-builder"></a>Interface Builder를 사용 하 여 콘센트가 나 작업을 만들 어떻게 할까요? 있나요?

Mac용 Visual Studio 및 Visual Studio에서 Xamarin Designer for iOS를 도입 하 여 Xamarin.ios 개발자는 이제 Storyboard 및. xib를 통해 UI를 만드는 데 활용할 수 있습니다. 디자이너 사용에 대 한 자세한 내용은 [Hello, iOS](~/ios/get-started/hello-ios/index.md) 가이드를 참조 하세요.

IB에서 콘센트 및 작업을 사용 하는 방법에 대 한 자세한 내용은 Apple의 [콘센트](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) 및 [작업](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingAction.html) 가이드를 참조 하세요.

## <a name="systemtextencodinggetencoding-throws-notsupportedexception"></a>Encoding.getencoding는 NotSupportedException을 throw 합니다.

기본적으로 추가 되지 않는 인코딩을 사용 하 고 있을 수 있습니다. 더 많은 인코딩에 대 한 지원을 추가 하는 방법을 알아보려면 [국제화](~/ios/app-fundamentals/localization/index.md) 페이지를 확인 하세요.

## <a name="systemmissingmethodexception-anything-else"></a>MissingMethodException (다른 모든 항목)

멤버가 링커가 제거 했을 수 있으므로 런타임에 어셈블리에 존재 하지 않습니다.  이에 대 한 몇 가지 솔루션이 있습니다.

- 멤버에 [`[Preserve]`](http://www.go-mono.com/docs/index.aspx?link=T:MonoTouch.Foundation.PreserveAttribute) 특성을 추가 합니다.  이렇게 하면 링커가 제거 되지 않습니다.
- [**Mtouch**](http://www.go-mono.com/docs/index.aspx?link=man:mtouch%281%29)를 호출할 때 **-nolink** 또는 **-linksdkonly** 옵션을 사용 합니다.
  - **-nolink** 는 모든 링크를 사용 하지 않습니다.
  - **-linksdkonly** 는 사용자가 만든 어셈블리의 모든 형식 (즉, 앱프로젝트)을 유지 하면서 xamarin.ios와 같이 xamarin.ios에서 제공 하는 어셈블리만 연결 합니다.

어셈블리가 연결 되어 결과 실행 파일의 크기가 작아집니다. 따라서 링크를 사용 하지 않도록 설정 하면 더 큰 실행 파일을 사용 하는 것이 바람직 할 수 있습니다.

## <a name="you-are-getting-a-modelnotimplementedexception"></a>ModelNotImplementedException를 받고 있습니다.

이 예외가 발생 하면 base를 호출 하는 것입니다. 모델을 재정의 하는 클래스의 메서드 () 모델에 대 한 클래스에서 기본 메서드를 호출할 필요가 없습니다 ([Model] 특성으로 플래그가 지정 된 클래스).

## <a name="this-class-is-not-key-value-coding-compliant-for-the-key-xxxx"></a>이 클래스는 키 XXXX의 키 값을 코딩 하는 데 호환 되지 않습니다.

NIB 파일을 로드할 때이 오류가 발생 하는 경우 관리 되는 클래스에서 값 XXXX를 찾을 수 없음을 의미 합니다. 즉, 다음과 같은 선언이 없습니다.

```csharp
[Connect]
TypeName XXXX {
   get {
       return (TypeName) GetNativeField ("XXXX");
   }
   set {
       SetNativeField ("XXXX", value);
   }
}
```

위의 정의는 `NAME_OF_YOUR_XIB_FILE.designer.xib.cs` 파일의 Mac용 Visual Studio에 추가 하는 XIB 파일에 대해 Mac용 Visual Studio에 의해 자동으로 생성 됩니다.

또한 위의 코드를 포함 하는 형식은 [Nsobject](xref:Foundation.NSObject)의 서브 클래스 여야 합니다.  포함 하는 형식이 네임 스페이스 내에 있는 경우에는 형식에서 네임 스페이스를 지원 하지 않으므로 네임 Interface Builder 스페이스 없이 형식 이름을 제공 하는 [[Register]](xref:Foundation.RegisterAttribute) 특성도 있어야 합니다.

```csharp
namespace Samples.GLPaint {

    // The [Register] attribute overrides the type name registered
    // with the Objective-C runtime, in this case removing the namespace.
    [Register ("AppDelegate")]
    public class AppDelegate {/* ... */}
}
```

## <a name="unknown-class-xxxx-in-interface-builder-file"></a>Interface Builder 파일에 알 수 없는 클래스 XXXX가 있습니다.

이 오류는 인터페이스 작성기 파일에 클래스를 정의 했지만 C# 코드에서 클래스의 실제 구현을 제공 하지 않는 경우에 발생 합니다.

다음과 같은 코드를 추가 해야 합니다.

```csharp
public partial class MyImageView : UIView {
   public MyImageView (IntPtr handle) : base (handle {}
}
```
## <a name="systemmissingmethodexception-no-constructor-found-for-foobarctorsystemintptr"></a>System.MissingMethodException: Foo. Bar:: ctor (System.web)에 대해 생성자를 찾을 수 없습니다.

이 오류는 코드가 Interface Builder 파일에서 참조 한 클래스의 인스턴스를 인스턴스화하려고 할 때 런타임에 생성 됩니다. 즉, 단일 IntPtr을 매개 변수로 사용 하는 생성자를 추가 하지 않습니다.

IntPtr 핸들을 사용 하는 생성자는 관리 되는 개체를 관리 되지 않는 표현에 바인딩하는 데 사용 됩니다.

이 문제를 해결 하려면 다음 코드 줄을 Foo. Bar 클래스에 추가 합니다.

```csharp
public Bar (IntPtr handle) : base (handle) { }
```
## <a name="type-foo--does-not-contain-a-definition-for-getnativefield-and-no-extension-method-getnativefield-of-type-foo-could-be-found"></a>{Foo} 형식에에 대 한 `GetNativeField` 정의가 포함 되어 있지 않으며 {foo} 형식의 확장 메서드 `GetNativeField` 를 찾을 수 없습니다.

디자이너에서 생성 한 파일 (*. xib.designer.cs)에서이 오류가 발생 하는 경우 다음 두 가지 중 하나를 의미 합니다.

 **1) partial 클래스 또는 기본 클래스가 없습니다.**

디자이너에서 생성 된 partial 클래스에는 종종 `NSObject` `UIViewController`의 일부 서브 클래스에서 상속 되는 사용자 코드에 해당 하는 partial 클래스가 있어야 합니다. 오류를 제공 하는 형식에 대 한 클래스가 있는지 확인 합니다.

 **2) 기본 네임 스페이스가 변경 되었습니다.**

디자이너 파일은 프로젝트의 기본 네임 스페이스 설정을 사용 하 여 생성 됩니다. 이러한 설정을 변경 하거나 프로젝트의 이름을 바꾼 경우 생성 된 partial 클래스는 사용자 코드와 동일한 네임 스페이스에 더 이상 포함 되지 않을 수 있습니다.

네임 스페이스 설정은 프로젝트 옵션 대화 상자에서 찾을 수 있습니다. 기본 네임 스페이스는 **일반-> 주 설정** 섹션에서 찾을 수 있습니다. 비어 있는 경우 프로젝트의 이름이 기본값으로 사용 됩니다. 고급 네임 스페이스 설정은 **소스 코드-> .Net 명명 정책** 섹션에서 찾을 수 있습니다.

## <a name="warning-for-actions-the-private-method-foo-is-never-used-cs0169"></a>동작에 대 한 경고: 전용 메서드 ' Foo '는 사용 되지 않습니다. (CS0169)

인터페이스 작성기 파일에 대 한 작업은 런타임에 리플렉션에 의해 위젯에 연결 되므로이 경고가 발생 합니다.

이러한 메서드에만이 경고를 표시 하지 않으려면 "#pragma 경고 사용 안 함 0169" "#pragma 경고 사용 0169"을 사용 하거나, 전체 프로젝트에 대해 사용 하지 않도록 설정 하려는 경우 컴파일러 옵션의 "경고 무시" 필드에 0169를 추가 합니다. 권장).

## <a name="mtouch-failed-with-the-following-message-cannot-open-assembly-pathtoyourprojectexe"></a>mtouch가 다음 메시지와 함께 실패 했습니다. '/Path/to/yourproject.exe ' 어셈블리를 열 수 없습니다.

이 오류 메시지가 표시 되는 경우 일반적으로이 문제는 프로젝트의 절대 경로에 공백이 포함 되어 있다는 것입니다. 이 문제는 Xamarin.ios의 이후 버전에서 수정 될 수 있지만 프로젝트를 공백 없이 폴더로 이동 하면 문제를 해결할 수 있습니다.

## <a name="your-sqlite3-version-is-old---please-upgrade-to-at-least-v350"></a>Sqlite3 버전이 오래 되었습니다. 최소 v 3.5.0로 업그레이드 하세요.

이는 다음 작업을 모두 수행할 때 발생 합니다.

1. Mono. Sqlite를 사용 합니다.
1. Mac OS X Leopard 사용 (10.5)
1. 시뮬레이터 내에서 앱을 실행 합니다.


문제는 Mono가 드롭다운에서 iphonesimulator 대상을의 `libsqlite3.dylib` `libsqlite3.dylib` 파일이 아니라 OS X를 선택 하는 것입니다. 앱 *이* 장치에서 작동 하지만 시뮬레이터는 작동 하지 않습니다.

## <a name="deploy-to-device-fails-with-systemexception-amdeviceinstallapplication-returned-3892346901"></a>시스템에서 장치에 배포 하지 못했습니다. 예외: AMDeviceInstallApplication에서 3892346901 반환

이 오류는 인증서/번들 id에 대 한 코드 서명 구성이 장치에 설치 된 프로 비전 프로필과 일치 하지 않음을 의미 합니다.  프로젝트 옵션-> iPhone 번들 서명에서 적절 한 인증서가 선택 되어 있는지 확인 하 고, 프로젝트 옵션-> iPhone 응용 프로그램에 지정 된 올바른 번들 id를 확인 합니다.

## <a name="code-completion-is-not-working-in-visual-studio-for-mac"></a>Mac용 Visual Studio에서 코드 완성이 작동 하지 않습니다.

최신 버전의 Mac용 Visual Studio 및 Xamarin.ios를 사용 하 고 있는지 확인 합니다.

문제가 계속 되 면 **~/Library/Logs/XamarinStudio-{VERSION}/Ide-{TIMESTAMP}.log**, **androidtools-{timestamp} .log**및 **Components-{timestamp}** .log 로그 파일을 첨부 하 여 [버그](http://monodevelop.com/Developers#Reporting_Bugs)를 제출 하세요.

다른 모든 작업이 실패 하는 경우 코드 완료 캐시를 제거 하 여 다시 생성할 수 있습니다.

 `[rm -r ~/.config/XamarinStudio-{VERSION}/CodeCompletionData]`

이 명령을 올바르게 입력 하거나 중요 한 파일을 실수로 제거할 수 있습니다.

## <a name="visual-studio-for-mac-crashes-when-you-copy-text"></a>텍스트를 복사할 때 충돌 Mac용 Visual Studio

인기 있는 Mac 유틸리티 QuickSilver, Google 도구 모음 및 도구 모음은 Mac용 Visual Studio의 메모리를 손상 시키는 클립보드 기능을 제공 합니다. 이러한 옵션을 사용 하면 Mac용 Visual Studio를 방해 하지 않아야 하는 프로세스로 나열할 수 있습니다.

## <a name="visual-studio-for-mac-complains-about-mono-24-required"></a>Mac용 Visual Studio 불만에 대 한 Mono 2.4 필요

최근 업데이트로 인해 Mac용 Visual Studio를 업데이트 했을 때이를 다시 시작 하려고 할 때 mono 2.4 설치를 불만 하는 것 [2.4](http://www.go-mono.com/mono-downloads/download.html)이 좋습니다.  

Mono 2.4.2.3 _6은 Mac용 Visual Studio를 안정적으로 실행 하지 못하게 하는 몇 가지 중요 한 문제를 해결 합니다. 때때로 시작 시 Mac용 Visual Studio 중단 되거나 코드 완성 데이터베이스를 생성할 수 없습니다.

새 Mono를 설치 하면 Mac용 Visual Studio 예상 대로 시작 됩니다.

## <a name="assertion-at-monometadatageneric-sharingc704-condition-oti-not-met"></a>어설션 위치.. /.. /.. /.. /mono/metadata/generic-sharing.c: 704, ' oti ' 조건이 충족 되지 않았습니다.

다음 스택 추적을 수신 하는 경우:

```csharp
 - Assertion at ../../../../mono/metadata/generic-sharing.c:704, condition `oti' not met
Stacktrace:
    at System.Collections.Generic.List`1<object>..cctor () <0xffffffff>
    at System.Collections.Generic.List`1<object>..cctor () <0x0001c>
    at (wrapper runtime-invoke) object.runtime_invoke_dynamic (intptr,intptr,intptr,intptr) <0xffffffff>`
```

Thumb 코드를 사용 하 여 컴파일된 정적 라이브러리를 프로젝트에 연결 하는 것을 의미 합니다. IPhone SDK 릴리스 3.1 이상 (이 문서를 작성할 당시 이상)에는 엄지 코드 (정적 라이브러리)가 아닌 코드 (Xamarin.ios)를 연결 하는 경우 Apple에서 버그가 도입 되었습니다. 이 문제를 완화 하기 위해 정적 라이브러리의 Thumb이 아닌 버전에 연결 해야 합니다.

## <a name="systemexecutionengineexception-attempting-to-jit-compile-method-wrapper-managed-to-managed-foosystemcollectionsgenericicollection1get_count-"></a>System.ExecutionEngineException: Attempting to JIT compile method (wrapper managed-to-managed) Foo[]:System.Collections.Generic.ICollection`1.get_Count ()

[] 접미사는 사용자 또는 클래스 라이브러리가 IEnumerable < >, ICollection < > 또는 IList < > 등의 제네릭 컬렉션을 통해 배열에서 메서드를 호출 하 고 있음을 나타냅니다. 해결 방법으로, 직접 메서드를 호출 하 고 예외를 트리거한 호출 전에이 코드가 실행 되는지 확인 하 여 AOT 컴파일러에 이러한 메서드를 포함 하도록 명시적으로 강제할 수 있습니다. 이 경우 다음을 작성할 수 있습니다.

```csharp
Foo [] array = null;
int count = ((ICollection<Foo>) array).Count;
```

그러면 AOT 컴파일러에 get_Count 메서드가 포함 됩니다.

## <a name="visual-studio-for-mac-source-editor-is-extremely-slow"></a>Mac용 Visual Studio 원본 편집기가 매우 느림

경우에 따라 문자 입력 사이에 몇 초 동안 중단 된 것 처럼 표시 되는 Mac용 Visual Studio 원본 편집기가 매우 느릴 수 있습니다.

이 문제는 거의 발생 하지 않으며 재현 하기가 매우 어렵습니다. 따라서 Mac용 Visual Studio를 다시 시작한 후에는 일반적으로 동일한 컴퓨터에서 재현할 수 없습니다. 이러한 이유로 Mac용 Visual Studio을 다시 시작 하기 전에 몇 가지 디버깅 단계를 수행 하 고 결과를 microsoft에 보낼 수 있는지 감사 합니다.

1. 편집기 탭을 닫고 다시 엽니다. 감속이 다시 발생할 때까지 캐럿을 편집 하거나 이동 하는 데 약간의 시간이 걸리지 않나요?
1. "Quartz.dll 디버그" 개발자 도구 (스포트라이트를 사용 하 여 찾을 수 있음)를 사용 하 여 "빔 동기화"를 사용 하지 않도록 설정 하 고 원본 편집기 성능이 정상으로 복원 되었는지 확인 합니다.
1. 빔 동기화를 사용 하지 않도록 설정 하 여 (1) 단계를 반복 해 보세요.
1. 몇 초 넘게 편집기가 중단 되 면 터미널에서 "killall [Mac용 Visual Studio]"를 실행 해 보십시오. 편집기가 정지 되는 동안 kill 명령이 실행 되는 것은 어려울 수 있지만 이렇게 하는 것이 중요 합니다. 명령을 사용 하면 Mono가 모든 스레드의 스택 추적을 MD 로그에 쓰도록 합니다 .이는를 사용 하 여 XS가 정지 된 상태에서 스레드가 있는 상태를 검색 하는 데 사용할 수 있기 때문입니다.



XS logs, **~/Library/Logs/XamarinStudio-{VERSION}/Ide-{TIMESTAMP}.log**, **androidtools-{timestamp} .log**및 **Components-{timestamp} .log** 를 연결 하세요. 이전 버전의 XS/MonoDevelop에서 ~/library/logs를 전송 합니다.  **/MonoDevelop-(3.0 | 2.8 | 2.6)/MonoDevelop.log**).

 **참고: 위의 문제가 XS 2.2 최종에서 수정 되었습니다.**

## <a name="compiled-application-is-very-large"></a>컴파일된 응용 프로그램이 매우 큼

디버깅을 지원 하기 위해 디버그 빌드는 추가 코드를 포함 합니다. 릴리스 모드에서 빌드된 프로젝트는 크기의 일부입니다.

Xamarin.ios 1.3 기반 디버그 빌드에는 Mono의 모든 단일 구성 요소 (프레임 워크의 모든 클래스에 있는 모든 메서드)에 대 한 디버깅 지원이 포함 되었습니다.  

Xamarin.ios 1.4를 사용 하 여 디버깅을 위한 보다 세분화 된 방법을 소개 합니다. 기본값은 코드 및 라이브러리에 대 한 디버깅 계측만 제공 하는 것 이며, 모든 [Mono 어셈블리](~/cross-platform/internals/available-assemblies.md) 에 대해이 작업을 수행 하지는 않습니다 .이 작업은 가능 하지만 는 이러한 어셈블리를 디버깅 하기 위해 옵트인 해야 합니다.

## <a name="installation-hangs"></a>설치 중단

IPhone 시뮬레이터를 실행 하는 경우 Mono 및 Xamarin.ios 설치 관리자가 모두 중지 됩니다. 이 문제는 Mono 또는 Xamarin.ios로 국한 되지 않습니다 .이 문제는 설치 시 iPhone 시뮬레이터가 실행 되는 경우 MacOS에서 소프트웨어를 설치 하려고 하는 모든 소프트웨어에서 일관 된 문제입니다.

IPhone 시뮬레이터를 종료 하 고 설치를 다시 시도 하세요.

<a name="trampolines" />

## <a name="ran-out-of-trampolines-of-type-0"></a>0 유형의 trampolines 중에서 실행 되었습니다.

장치를 실행 하는 동안이 메시지가 표시 되는 경우 프로젝트 옵션인 "iPhone Build" 섹션을 수정 하 여 추가 유형 0 trampolines (유형 별)을 만들 수 있습니다.  장치 빌드 대상에 대 한 추가 인수를 추가 하려고 합니다.

 `-aot "ntrampolines=2048"`

Trampolines의 기본 수는 1024입니다. 응용 프로그램에 충분 한 시간까지이 숫자를 늘립니다.

## <a name="ran-out-of-trampolines-of-type-1"></a>유형 1의 trampolines 중에서 실행 되었습니다.

재귀적 제네릭을 과도 하 게 사용 하는 경우 장치에서이 메시지가 표시 될 수 있습니다.  프로젝트 옵션인 "iPhone Build" 섹션을 수정 하 여 더 많은 type 1 trampolines (type RGCTX)를 만들 수 있습니다.  장치 빌드 대상에 대 한 추가 인수를 추가 하려고 합니다.

 `-aot "nrgctx-trampolines=2048"`

Trampolines의 기본 수는 1024입니다. 제네릭을 사용 하기에 충분 한 시간까지이 숫자를 늘립니다.

## <a name="ran-out-of-trampolines-of-type-2"></a>유형 2의 trampolines 중에서 실행 되었습니다.

많은 인터페이스를 사용 하는 경우 장치에서이 메시지가 표시 될 수 있습니다.
프로젝트 옵션인 "iPhone Build" 섹션을 수정 하 여 추가 유형 2 trampolines (IMT 썽크 유형)를 만들 수 있습니다.  장치 빌드 대상에 대 한 추가 인수를 추가 하려고 합니다.

 `-aot "nimt-trampolines=512"`

IMT 썽크 trampolines의 기본 수는 128입니다. 인터페이스를 사용 하기에 충분 한 시간까지이 숫자를 늘립니다.

## <a name="debugger-is-unable-to-connect-with-the-device"></a>디버거가 장치에 연결할 수 없습니다.

장치 구성 디버깅을 시작할 때 디버거가 응용 프로그램에 연결을 시도 하 고 있음을 나타내는 대화 상자를 표시 하는 것을 볼 수 있습니다. 연결 하는 데 사용 하는 모드 (USB 또는 WiFi)에 따라 디버거가 응용 프로그램에 연결 하지 못할 수 있는 몇 가지 이유가 있습니다.

 **장치와 디버거 호스트가 서로 다른 네트워크에 있는 경우**방화벽이 나 개인 네트워크로 인해 응용 프로그램이 WiFi 모드의 디버거 호스트에 연결 되지 않을 수 있습니다.

 **Mac용 Visual Studio 호스트의 올바른 IP를 쿼리하지 못할 수 있습니다**. WiFi 모드 Mac용 Visual Studio에서 호스트를 찾을 수 있는 모든 Ip를 응용 프로그램에 제공 하 고 응용 프로그램에서 모든 Ip를 사용 하 여 Mac용 Visual Studio에 연결할 수 있는지 여부를 확인 합니다.

 **다른 장치가 호스트의 USB 포트에 연결 되어 있습니다.** 일부 경우에는 호스트의 USB 포트에 연결 된 다른 장치가 USB 모드에서 디버깅을 방해 하는 것으로 알려져 있습니다.

WiFi 또는 USB 모드가 작동 하지 않는 경우 다른 작업을 쉽게 시도할 수 있습니다. Mac용 Visual Studio에서 기본 설정을 열고 기본 설정/디버거/iPhone 디버거 페이지로 이동 하 고 "USB를 통해 iOS 장치를 사용 하는 대신 WiFi를 통해 iOS 장치 디버그" 확인란을 선택/해제 합니다.   작동 하지 않는 경우 자세한 정보 표시 모드에서 장치 콘솔의 오류에 대 한 자세한 정보를 볼 수 있습니다 .이는 프로젝트 옵션의 추가 mtouch 인수에 "-v-v-v"를 추가 하 여 활성화 됩니다.

## <a name="error-134-mtouch-failed-with-the-following-message"></a>오류 134: 다음 메시지와 함께 mtouch가 실패 했습니다.

이 오류는 버전의 Xamarin.ios 1.4 스타일에서-nolink를 사용 하 여 빌드하려고 할 때 발생할 수 있습니다. Monodevelop 프로젝트 구성에서 추가 인수를 지정 하 여이 오류를 해결할 수 있습니다.

인수 추가

 `-nosymbolstrip`

문제를 해결 해야 합니다.

## <a name="distribution-identity-is-not-shown-in-visual-studio-for-mac-project-signing-options"></a>배포 id는 Mac용 Visual Studio 프로젝트 서명 옵션에 표시 되지 않습니다.

Mac용 Visual Studio 2.2에는 쉼표를 포함 하는 배포 인증서를 검색 하지 않는 버그가 있습니다. Mac용 Visual Studio 2.2.1로 업데이트 하세요.

## <a name="error-afcfilerefwrite-returned-1-during-upload"></a>오류 "AFCFileRefWrite 반환: 1 "업로드 중

장치에 앱을 업로드 하는 동안 "AFCFileRefWrite이 반환 되었습니다. 1". 길이가 0 인 파일이 있는 경우이 문제가 발생할 수 있습니다.

## <a name="error-mtouch-failed-with-no-output"></a>"Mtouch가 출력 없이 실패 했습니다." 오류

솔루션 또는 프로젝트를 저장 하는 프로젝트 이름이 나 디렉터리에 공백이 포함 되어 있으면 최신 버전의 Xamarin.ios 및 Mac용 Visual Studio가 실패 합니다.
이 문제를 해결하려면


- 프로젝트 또는 저장 된 디렉터리에 공간이 포함 되어 있지 않은지 확인 합니다.
- 프로젝트 "기본 설정"에서 프로젝트 이름에 공백이 포함 되어 있지 않은지 확인 합니다.

## <a name="error-the-binary-you-uploaded-was-invalid-a-pre-release-beta-version-of-the-sdk-was-used-to-build-the-application"></a>"업로드 한 이진 파일이 잘못 되었습니다. SDK의 시험판 베타 버전이 응용 프로그램을 빌드하는 데 사용 되었습니다. "

이 오류는 일반적으로 Xamarin. iOS 2.0.0을 출시 하기 전에 iPad 개발에서 시작 된 프로젝트에 발생 합니다. info.plist와 같은 몇 가지 키가 있을 가능성이 높습니다.

```xml
<key>UIDeviceFamily</key>
       <array>
               <string>1</string>
       </array>
```

이 키 쌍은 Mac용 Visual Studio 자동으로 처리할 수 있으므로 제거 해야 합니다.

## <a name="error-a-pre-release-beta-version-of-the-sdk-was-used-to-build-the-app"></a>"SDK의 시험판 베타 버전이 앱을 빌드하는 데 사용 되었습니다." 오류

(Ed Anuff에서 기여)

다음 단계를 수행하세요.

- IPhone 빌드에서 SDK 버전을 3.2 또는 iTunes connect로 변경 하면 3.2 보다 작은 SDK 버전을 사용 하 여 빌드된 iPad 호환 앱이 표시 되기 때문에 업로드가 거부 됩니다.
- 프로젝트에 대 한 사용자 지정 info.plist을 만들고이를 명시적으로 3.0에 설정 합니다.   이렇게 하면 Xamarin.ios로 설정 된 이상 값 Osversion 3.2 값이 재정의 됩니다.   이 작업을 수행 하지 않으면 iPhone에서 앱을 실행할 수 없게 됩니다.
- ITunes connect에 다시 빌드하고 zip을 업로드 하 고 업로드 합니다.

## <a name="unhandled-exception-systemexception-failed-to-find-selector-someselector-on-type"></a>처리되지 않은 예외: System.Exception: 선택기 someSelector: {type}에서 찾지 못했습니다.

이 예외는 다음 세 가지 중 하나로 인해 발생 합니다.


1. 해당 [Export] 특성을 메서드에 적용 하지 않고 목표-C 런타임에 대 한 선택기를 제공 했습니다.
1. 전체 링크를 사용 하도록 설정 하 고 [Export] 특성을 [Export] ed 메서드에 적용 하지 않았습니다.
1. [Export] 특성을 상속 된 형식의 private 메서드에 적용 했습니다.


## <a name="mainwindowxibdesignercs-file-is-not-updated"></a>MainWindow.xib.designer.cs 파일이 업데이트 되지 않았습니다.

Xamarin Studio 2.4에는 xib 파일을 새 프로젝트의 xib 파일로 그룹화 하지 않는 버그가 있었습니다. 이는 특정 파일에 대 한 디자이너 코드를 업데이트 하지 않는 것을 의미 합니다.

이 문제는 기본 제공 되는 업데이트 프로그램에서 사용할 수 있는 Mac용 Visual Studio 버전에서 해결 되었으므로 최신 버전을 사용 하 고 있는지 확인 하세요.

Xib 및 해당 디자이너 파일을 제거 (삭제 하지 않음) 한 후 다시 추가 하 여 기존 프로젝트를 수정할 수 있습니다. 이렇게 하면 파일을 올바르게 그룹화 해야 합니다.

## <a name="uialertview-or-uiactionsheet-vanish-after-being-created"></a>UIAlertView 또는 Uialertview를 만든 후에는 표시 되지 않습니다.

다음과 같은 코드를 사용할 수 있습니다.

```csharp
var actionSheet = new UIActionSheet ("My ActionSheet", null, null, "OK", null){
   Style = UIActionSheetStyle.Default
};

actionSheet.Clicked += delegate (sender, args){
    Console.WriteLine ("Clicked on item {0}", args.ButtonIndex);
};
```

"actionSheet" 개체는 함수에서 임시 변수로, 함수가 종료 되는 즉시 개체는 가비지 수집에 적합 하므로 가비지 수집 됩니다.

이 문제를 해결 하려면 메서드 외부에 있는 "actionSheet"에 대 한 참조를 메서드 밖으로 유지 해야 합니다.

## <a name="project-always-runs-in-the-ipad-simulator"></a>프로젝트는 항상 iPad 시뮬레이터에서 실행 됩니다.

IPhone SDK 4.0 설치 관리자는 iPad 전용 앱을 빌드하기 위한 3.2 SDK와 iPhone 및 유니버설 앱을 빌드하는 데 사용 되는 4.0 SDK의 2 개 Sdk를 설치 합니다. 또한 iPad만 시뮬레이트하는 3.2 시뮬레이터와 iPhone 또는 iPhone 4를 시뮬레이트하는 4.0 시뮬레이터를 설치 합니다. 모든 이전 Sdk 및 시뮬레이터 제거 됩니다.

Mac용 Visual Studio iPhone 프로젝트 빌드 옵션에는 앱을 빌드하는 데 사용 되는 SDK 버전에 대 한 설정이 포함 됩니다. 이 설정은 **프로젝트 옵션-> 빌드-> IPhone 빌드**에서 찾을 수 있습니다.

Mac용 Visual Studio의 새 프로젝트는 가장 오래 설치 된 sdk를 기본 SDK 설정으로 사용 합니다. 지정 된 SDK가 없는 경우에는 찾을 수 있는 가장 가까운를 사용 하 여 앱을 빌드합니다 Mac용 Visual Studio. 이렇게 하면 프로젝트에서 항상 최신 SDK가 필요 하지 않습니다. 그러나 현재이로 인해 3.2 SDK가 사용 되며,이로 인해 iPad 시뮬레이터가 사용 됩니다.

4\.0 SDK를 사용 하 여이 문제를 해결 하려면 **프로젝트 옵션-> 빌드-> IPhone 빌드**>로 이동 하 고 드롭다운 상자를 사용 하 여 SDK 값을 "4.0"로 변경 합니다. 패널 위쪽의 드롭다운을 사용 하 여 액세스 하는 각 구성 및 플랫폼 조합에 대해이 작업을 수행 해야 합니다.

SDK 버전은 "최소 OS 버전" 설정과 혼동 해서는 안 됩니다.
이 값은 SDK 버전 값과 일치 하지 않아도 됩니다. 앱이 설치 하는 OS의 최소 버전에 영향을 줍니다. 이전 OS에 있는 Api만 사용 하거나 런타임 OS 버전 백업의을 사용 하 여 새로운 기능을 사용 하는 경우 SDK 보다 오래 될 수 있습니다. k. 앱을 테스트 하는 가장 오래 된 OS 버전으로 설정 해야 합니다.

프로젝트를 실행 하거나 디버그할 때 기본적으로 사용 되는 시뮬레이터를 선택 하는 데에는 **프로젝트 > IPhone 시뮬레이터 대상**> 메뉴를 사용할 수 있습니다. 또한 > 메뉴에서 실행 **>** 실행을 사용 하 여 실행할 특정 시뮬레이터를 선택할 수 있습니다.

## <a name="ibtool-returns-error-133"></a>ibtool는 오류 133를 반환 합니다.

이는 XCode 4가 설치 되어 있음을 의미 합니다.   XCode 4에서는 ibtool 도구가 제거 되었으므로 독립 실행형 도구를 사용 하 여 XIB 파일을 더 이상 편집할 수 없습니다.

Interface Builder를 사용 하려면 Apple 웹 사이트에서 사용할 수 있는 [XCode 시리즈 3](http://connect.apple.com/cgi-bin/WebObjects/MemberSite.woa/wa/getSoftware?bundleID=20792)을 설치 합니다.

## <a name="cant-create-display-binding-for-mime-type-applicationvndapple-interface-builder"></a>"Mime 형식에 대 한 표시 바인딩을 만들 수 없음: application/vnd. apple-interface-builder"

이 오류는 iPhone 이외의 프로젝트에서 iPhone UI를 만들려고 할 때 발생 합니다. Iphone/iPad 솔루션으로 시작 해야 하며 iphone/iPad가 아닌 프로젝트에 iPhone UI 요소를 추가할 수는 없습니다.

## <a name="startup-crash-when-executing-inside-the-ios-simulator"></a>IOS 시뮬레이터 내에서 실행할 때 시작 충돌

다음과 같은 스택 추적과 함께 시뮬레이터 내에서 런타임 충돌 (SIGSEGV)을 가져옵니다.

```csharp
  at (wrapper managed-to-native) System.Reflection.Assembly.GetTypes (System.Reflection.Assembly,bool)
  at MonoTouch.ObjCRuntime.Runtime.RegisterAssembly (System.Reflection.Assembly)
  at (wrapper runtime-invoke) <Module>.runtime_invoke_void_object (object,intptr,intptr,intptr)
```
... 그러면 시뮬레이터 응용 프로그램 디렉터리에 하나 이상의 부실 어셈블리가 있을 것입니다. Apple iOS 시뮬레이터는 파일을 추가 하 고 업데이트 하지만 삭제 하지 않으므로 이러한 어셈블리가 있을 수 있습니다. 이 경우 가장 쉬운 솔루션은 "다시 설정 및 콘텐츠 및 설정 ..."을 선택 하는 것입니다. 시뮬레이터 메뉴에서.   

> [!WARNING]
> 그러면 시뮬레이터에서 모든 파일, 응용 프로그램 및 데이터가 제거 됩니다.   다음 번에 응용 프로그램을 실행할 때 Mac용 Visual Studio는 시뮬레이터에 배포 하 고 크래시를 발생 시킬 오래 된 오래 된 어셈블리가 없게 됩니다.

## <a name="simulator-hangs-during-application-installation"></a>응용 프로그램 설치 중에 시뮬레이터가 중단 됨

응용 프로그램 이름에 '. '가 포함 된 경우이 문제가 발생할 수 있습니다. 이름에 (점)입니다.
이는 다른 여러 사례 (예: 장치)에서 작동할 수 있는 경우에도 CFBundleExecutable의 실행 파일 이름으로 사용할 수 없습니다.

 \* "값은 이름에 확장명을 포함 하지 않아야 합니다." -[https://developer.apple.com/library/mac/documentation/General/Reference/InfoPlistKeyReference/InfoPlistKeyReference.pdf](https://developer.apple.com/library/mac/documentation/General/Reference/InfoPlistKeyReference/InfoPlistKeyReference.pdf)

## <a name="error-custom-attribute-type-0x43-is-not-supported-when-double-clicking-xib-files"></a>오류: Xib 파일을 두 번 클릭 하면 "사용자 지정 특성 유형 0x43이 지원 되지 않습니다."

이 문제는 환경 변수가 잘못 설정 된 경우 xib 파일을 열려고 할 때 발생 합니다. 이는 Mac용 Visual Studio/Xamarin.ios를 정상적으로 사용 하는 경우에는 발생 하지 않아야 하 고/응용 프로그램에서 Mac용 Visual Studio를 다시 여는 문제를 해결 해야 합니다.

소프트웨어를 업데이트 하려고 할 때이 오류 메시지가 표시 되 면 전자 메일을 입력 합니다. *support@xamarin.com*

## <a name="application-runs-on-simulator-but-fails-on-device"></a>응용 프로그램이 시뮬레이터에서 실행 되지만 장치에서 실패 함

이 문제는 여러 형식으로 매니페스트 될 수 있으며 항상 일관 된 오류를 생성 하지는 않습니다. 응용 프로그램에. xib이 포함 된 경우 xib에 대 한 **빌드 작업이** **정의**로 설정 되었는지 확인 합니다. Xib에 대 한 기본 빌드 작업입니다.

빌드 작업을 확인 하려면 xib 파일을 마우스 오른쪽 단추로 클릭 하 고 **빌드 작업**을 선택 합니다.


## <a name="systemnotsupportedexception-no-data-is-available-for-encoding-437"></a>System.NotSupportedException: 인코딩 437에 사용할 수 있는 데이터가 없습니다.

Xamarin.ios 앱에 타사 라이브러리를 포함 하는 경우 다음과 같은 오류 메시지가 표시 될 수 있습니다. "NotSupportedException: 응용 프로그램을 컴파일하고 실행 하려고 할 때 "encoding 437"에 사용할 수 있는 데이터가 없습니다. 예를 들어와 `Ionic.Zip.ZipFile`같은 라이브러리는 작업 중에이 예외를 throw 할 수 있습니다.

이는 ios 프로젝트에 대 한 옵션을 열고 **ios** > **국제화** 로 이동 하 여 국제화 된 국제화를 확인 하 여 해결할 수 있습니다.
