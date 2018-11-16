---
title: Xamarin.iOS에 대 한 문제 해결 팁
description: 이 문서에서는 Xamarin.iOS 응용 프로그램을 개발 하는 동안 문제 해결에 유용한 다양 한 팁을 제공 합니다. 다른 잠재적 문제가 뿐만 아니라 특정 오류 메시지에 설명 합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: B50FE9BD-9E01-AE88-B178-10061E3986DA
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/22/2018
ms.openlocfilehash: 4ab6b217190ea633611a9c869ec7e93befcc3c56
ms.sourcegitcommit: ae34d048aeb23a99678ae768cdeef0c92ca36b51
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51681568"
---
# <a name="troubleshooting-tips-for-xamarinios"></a>Xamarin.iOS에 대 한 문제 해결 팁 

## <a name="xamarinios-cannot-resolve-systemvaluetuple"></a>Xamarin.iOS System.ValueTuple를 확인할 수 없습니다.

Visual Studio를 사용 하 여 비 호환성으로 인해이 오류가 발생합니다.

- **Visual Studio 2017 업데이트 1** (버전 15.1 이상)만 호환 됩니다 **System.ValueTuple NuGet 4.3.0** (또는 이전 버전).

- **Visual Studio 2017 업데이트 2** (버전 15.2 이상)만 호환 됩니다. 합니다 **System.ValueTuple NuGet 4.3.1** 이상.

Visual Studio 2017 설치를 사용 하 여 해당 하는 올바른 System.ValueTuple NuGet을 선택 하세요.


## <a name="receiving-error-retrieving-update-information-error-message"></a>오류 메시지 '오류 업데이트 정보를 검색 하는 데 사용'

소프트웨어 및이 오류 메시지를 업데이트 하는 동안 표시 되 면 IDE에서 IDE 및 로그 아웃 했다가 계정에 다시 다시 시도 하세요.

## <a name="how-do-i-create-outlets-or-actions-with-interface-builder"></a>어떻게 만드나요 출 선 또는 작업 Interface Builder를 사용 하 여?

Mac 및 Visual studio 용 Visual Studio에서 iOS 용 Xamarin 디자이너의 도입으로 Xamarin.iOS 개발자 스토리 보드 및.xibs 통해 UI를 만드는 경우의 이점은 이제 사용할 수 있습니다. 참조 된 [Hello, iOS](~/ios/get-started/hello-ios/index.md) 디자이너를 사용 하 여 자세한 정보에 대 한 가이드입니다.

Apple의 참조할 수도 있습니다 [출 선](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) 하 고 [작업](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingAction.html) 출 선 및 작업을 사용 하 여 IB에 대 한 자세한 내용은 가이드입니다.

## <a name="systemtextencodinggetencoding-throws-notsupportedexception"></a>System.Text.Encoding.GetEncoding NotSupportedException을 throw

기본적으로 추가 되지 않습니다는 인코딩을 사용할 수 있습니다. 확인 합니다 [국제화](~/ios/app-fundamentals/localization/index.md) 자세한 인코딩에 대 한 지원을 추가 하는 방법을 알아보려면 페이지입니다.

## <a name="systemmissingmethodexception-anything-else"></a>System.MissingMethodException (무엇)

멤버는 링커로 가능성이 없어집니다 및 따라서 런타임에 어셈블리에 존재 하지 않습니다.  가지이 몇 가지 솔루션이 있습니다.

- 추가 된 [ `[Preserve]` ](http://www.go-mono.com/docs/index.aspx?link=T:MonoTouch.Foundation.PreserveAttribute) 멤버에 특성입니다.  이렇게 하면 링커에서 제거 되지 것입니다.
- 호출할 때 [ **mtouch**](http://www.go-mono.com/docs/index.aspx?link=man:mtouch%281%29)를 사용 합니다 **-nolink** 또는 **-linksdkonly** 옵션:
  - **-nolink** 모든 연결을 사용 하지 않도록 설정 합니다.
  - **-linksdkonly** 연결 Xamarin.iOS가 제공한 어셈블리와 같은 **xamarin.ios.dll**, 사용자가 만든 어셈블리의 모든 형식을 유지 하면서 (ie. 앱 프로젝트).

Note는 어셈블리는 연결 되어 있으므로 결과 실행 파일은 작은; 따라서 것이 바람직한 것 보다 더 큰 실행 파일에서 연결을 사용 하지 않도록 설정 될 수 있습니다.

## <a name="you-are-getting-a-modelnotimplementedexception"></a>ModelNotImplementedException 얻게 됩니다.

이 예외를 발생 하는 경우이 호출 하는 기본 의미 합니다. 모델을 재정의 하는 클래스에 메서드 (). (이 경우 [모델] 특성으로 플래그가 지정 되는 클래스) 모델에 대 한 클래스에서 기본 메서드를 호출할 필요가 없습니다.

## <a name="this-class-is-not-key-value-coding-compliant-for-the-key-xxxx"></a>이 클래스 코딩 규격 XXXX 키에 대 한 키 값이 아닙니다.

NIB 파일을 로드할 때이 오류가 발생할 경우 해당 값을 의미 하며는 XXXX 관리 되는 클래스에서 찾을 수 없습니다. 이 다음과 같은 선언이 없는 것을 의미 합니다.

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

위의 정의 자동으로 생성 Visual Studio for Mac에서 Mac 용 Visual Studio에 추가한 모든 XIB 파일에 대 한는 `NAME_OF_YOUR_XIB_FILE.designer.xib.cs` 파일입니다.

또한 위의 코드를 포함 하는 형식의 하위 클래스 여야 [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/)합니다.  또한 있어야 포함 하는 형식 네임 스페이스 내에 있으면 한 [[등록]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) (Interface Builder 형식의 네임 스페이스를 지원 하지 않습니다) 처럼 네임 스페이스가 없는 형식 이름을 제공 하는 특성:

```csharp
namespace Samples.GLPaint {

    // The [Register] attribute overrides the type name registered
    // with the Objective-C runtime, in this case removing the namespace.
    [Register ("AppDelegate")]
    public class AppDelegate {/* ... */}
}
```

## <a name="unknown-class-xxxx-in-interface-builder-file"></a>알 수 없는 클래스 XXXX Interface Builder 파일

인터페이스 작성기 파일에서 클래스를 정의 하지만 실제 구현에서이 제공 하지 않는 경우이 오류가 발생 하면 C# 코드입니다.

다음과 같은 몇 가지 코드를 추가 해야 합니다.

```csharp
public partial class MyImageView : UIView {
   public MyImageView (IntPtr handle) : base (handle {}
}
```
## <a name="systemmissingmethodexception-no-constructor-found-for-foobarctorsystemintptr"></a>System.MissingMethodException: 생성자 Foo.Bar::ctor(System.IntPtr)에

이 오류는 코드에서 Interface Builder 파일에서 참조 하는 클래스의 인스턴스를 인스턴스화하고 하려고 할 때 런타임 시 생성 됩니다. 즉, 매개 변수로 단일 IntPtr을 사용 하는 생성자를 추가 하지 않았습니다.

관리 되지 않는 해당 표현으로 관리 되는 개체를 바인딩할 IntPtr 핸들을 사용 하 여 생성자 사용 됩니다.

이 해결 하려면 Foo.Bar 클래스에 코드의 다음 줄을 추가 합니다.

```csharp
public Bar (IntPtr handle) : base (handle) { }
```
## <a name="type-foo--does-not-contain-a-definition-for-getnativefield-and-no-extension-method-getnativefield-of-type-foo-could-be-found"></a>형식 {Foo}에 대 한 정의 포함 되지 않습니다 `GetNativeField' and no extension method `GetNativeField' 형식의 {Foo을 (를) 찾을 수 없습니다

생성된 된 디자이너 파일에서이 오류가 발생할 경우 (*. xib.designer.cs), 두 가지 중 하나를 의미 합니다.

 **1) 누락 된 partial 클래스 또는 기본 클래스**

디자이너에서 생성 된 partial 클래스의 일부 하위 클래스에서 상속 되는 사용자 코드에서 해당 partial 클래스로 있어야 `NSObject`종종 `UIViewController`입니다. 이러한 클래스는 오류를 제공 하는 형식에 대 한 했는지 확인 합니다.

 **2) 기본 네임 스페이스 변경**

디자이너 파일은 프로젝트의 기본 네임 스페이스 설정을 사용 하 여 생성 됩니다. 이러한 설정을 변경 또는 프로젝트를 변경 하는 경우 생성된 된 partial 클래스는 사용자 코드 함수와 동일한 네임 스페이스에서 더 이상 수 없습니다.

Namespace 설정은 프로젝트 옵션 대화 상자에서 찾을 수 있습니다. 기본 네임 스페이스에서 발견 되는 **일반-> 기본 설정** 섹션입니다. 비어 있는 경우 프로젝트의 이름은 기본값으로 사용 됩니다. 고급 네임 스페이스 설정에서 찾을 수 있습니다 합니다 **소스 코드에는.NET 명명 정책->** 섹션입니다.

## <a name="warning-for-actions-the-private-method-foo-is-never-used-cs0169"></a>작업에 대 한 경고: 'Foo' 사용 되지 않는 전용 메서드. (CS0169)

인터페이스 작성기 파일에 대 한 작업이이 경고가 예상 되는지 하므로 런타임에 리플렉션에 의해 위젯에 연결 됩니다.

"#Pragma 경고 0169 사용 하지 않도록 설정 하는 데 사용"을 사용할 수 있습니다 "#pragma 경고 사용 0169" 작업 관련 이러한 방법에 대해서만이 경고를 표시 하지 않거나 (not 전체 프로젝트에 대해 사용 하지 않도록 하려는 경우 0169 컴파일러 옵션에서 "무시 경고" 필드를 추가 하려는 경우 권장).

## <a name="mtouch-failed-with-the-following-message-cannot-open-assembly-pathtoyourprojectexe"></a>mtouch 다음 메시지와 함께 실패 했습니다: 어셈블리를 열 수 없습니다 ' / path/to/yourproject.exe'

이 오류 메시지를 표시 하는 경우 일반적으로 문제는 프로젝트에 절대 경로 공백이 포함. Xamarin.iOS와의 이후 버전에서 수정 될 예정이 있지만 프로젝트 공백 없이 폴더로 이동 하 여 문제를 해결할 수 있습니다.

## <a name="your-sqlite3-version-is-old---please-upgrade-to-at-least-v350"></a>Sqlite3 버전이 이전-이상으로 업그레이드 하십시오 v3.5.0!

다음을 모두 수행할 때이 상황이 발생 합니다.

1.  사용 하 여 Mono.Data.Sqlite
1.  Mac OS X 표범 무늬 (10.5)를 사용 합니다.
1.  시뮬레이터에서 앱을 실행 합니다.


문제가 되는 Mono가 OS X을 넘겨 `libsqlite3.dylib`, 없습니다: iPhoneSimulator `libsqlite3.dylib` 파일. 앱 *는* 장치 있지만 아니라에 시뮬레이터에서 작동 합니다.

## <a name="deploy-to-device-fails-with-systemexception-amdeviceinstallapplication-returned-3892346901"></a>System.Exception 장치 실패 하도록 하려면 배포: AMDeviceInstallApplication 3892346901 반환

이 오류 코드 서명 인증서/번들 id 구성을 장치에 설치 하 고 프로 비전 프로필 일치 하지 않음을 의미 합니다.  IPhone 번들 서명-> 프로젝트 옵션에서 선택 된 적절 한 인증서가 올바른 번들 id 프로젝트 옵션에 지정 된 iPhone 응용 프로그램-> 확인

## <a name="code-completion-is-not-working-in-visual-studio-for-mac"></a>Mac 용 Visual Studio에서 코드 완성 작동 하지 않습니다.

Mac에서 Xamarin.iOS를 최신 버전의 Visual Studio를 사용 하는 확인

문제가 여전히 있는 경우 [버그](http://monodevelop.com/Developers#Reporting_Bugs), 연결 된 **~/Library/Logs/XamarinStudio-{VERSION}/Ide-{TIMESTAMP}.log**, **AndroidTools-{TIMESTAMP}.log**, 및 **구성 요소-{TIMESTAMP}.log** 로그 파일입니다.

모든 작업이 실패 하면 다시 생성할 수 있도록 코드 완성 캐시 제거를 시도할 수 있습니다.

 `[rm -r ~/.config/XamarinStudio-{VERSION}/CodeCompletionData]`

이 명령은 올바르게 입력 또는 실수로 중요 한 파일을 제거할 수는 주의 해야 합니다.

## <a name="visual-studio-for-mac-crashes-when-you-copy-text"></a>텍스트를 복사 하는 경우 Mac 용 visual Studio 작동이 중단

인기 있는 Mac 유틸리티 퀵, Google 툴바 및 출발점이 Visual Studio를 Mac의 메모리 손상 클립보드 기능이 있습니다. 해당 옵션을 사용 하 여 방해 하지 않아야 하는 프로세스로 Mac 용 Visual Studio를 나열할 수 있습니다.

## <a name="visual-studio-for-mac-complains-about-mono-24-required"></a>Mac 용 visual Studio에서 필요한 Mono 2.4에 대 한

최근 업데이트로 인해 Mac 용 Visual Studio를 업데이트 하 고 시작 하려고 할 때 다시 것에 대해 불평 하는 Mono 2.4를 수행 해야 됩니다 [Mono 2.4 설치 업그레이드](http://www.go-mono.com/mono-downloads/download.html)합니다.  

Mono 2.4.2.3_6 안정적으로 시작할 때 Mac 용 Visual Studio 응답이 없는 경우에 따라 실행에서 Mac 용 Visual Studio를 방지 또는 생성 되 고 코드 완성 데이터베이스를 방지 하는 몇 가지 중요 한 문제를 해결 합니다.

새 Mono를 설치한 후 예상 대로 Mac 용 Visual Studio 시작 됩니다.

## <a name="assertion-at-monometadatageneric-sharingc704-condition-oti-not-met"></a>에 대 한 어설션을... /.. /.. /.. /mono/metadata/generic-sharing.c:704, 조건이 충족 되지 않으면 ' oti'

다음 스택 추적 수신 하는 경우:

```csharp
 - Assertion at ../../../../mono/metadata/generic-sharing.c:704, condition `oti' not met
Stacktrace:
    at System.Collections.Generic.List`1<object>..cctor () <0xffffffff>
    at System.Collections.Generic.List`1<object>..cctor () <0x0001c>
    at (wrapper runtime-invoke) object.runtime_invoke_dynamic (intptr,intptr,intptr,intptr) <0xffffffff>`
```

즉, 프로젝트에 thumb 코드를 사용 하 여 컴파일된 정적 라이브러리를 링크 하는 있습니다. IPhone SDK 버전 3.1 이상이 문서 작성 당시부터 Apple Thumb 코드 (정적 라이브러리)를 사용 하 여 Thumb이 아닌 코드 (Xamarin.iOS)를 연결 하는 경우 해당 링커의 버그를 도입 했습니다. 이 문제를 완화 하려면 정적 라이브러리의 Thumb이 아닌 버전과 연결 해야 합니다.

## <a name="systemexecutionengineexception-attempting-to-jit-compile-method-wrapper-managed-to-managed-foosystemcollectionsgenericicollection1getcount-"></a>JIT 컴파일 메서드 (관리 되는--관리 되는 래퍼) Foo[]:System.Collections.Generic.ICollection'1.get_Count ()를 시도: System.ExecutionEngineException

접미사 또는 클래스 라이브러리 ienumerable<>, ICollection <> IList <> 등의 제네릭 컬렉션을 배열에서 메서드를 호출 되는 것을 나타냅니다. 대 안으로 AOT 컴파일러를 직접 메서드를 호출 하 여 이러한 메서드를 포함 하도록 명시적으로 할 수 있습니다 및 하는지 확인 하 여이 코드는 예외를 트리거한 호출 전에 실행 됩니다. 이 경우에 작성할 수 있습니다.

```csharp
Foo [] array = null;
int count = ((ICollection<Foo>) array).Count;
```

AOT 컴파일러 get_Count 메서드를 포함 하도록 강제로입니다.

## <a name="visual-studio-for-mac-source-editor-is-extremely-slow"></a>소스 편집기 Mac 용 visual Studio 매우 느립니다.

경우에 따라 소스 편집기 Mac 용 Visual Studio 나타나는 문자 입력 간에 몇 초 정도 중단으로 매우 느린 됩니다.

이 문제는 매우 드물게 발생 하며 재현 하기 어려운 매우-이 일반적으로 재현할 수 없는 동일한 컴퓨터에서 mac 용 Visual Studio를 다시 시작한 후 이러한 이유로 주시면이 Mac 용 Visual Studio를 다시 시작 하기 전에 몇 가지 디버깅 단계를 수행 하 고 우리에 게 결과 보낼 수 있습니다.

1.  편집기 탭을 닫고 닫았다가 다시 열면 됩니다. 약간의 편집 또는 속도 저하 다시 발생 될 때까지 캐럿을 이동 시간이?
1.  "Quartz Debug" 개발자 도구를 사용 하 여 "보 동기화" 사용 안 함 (찾을 수 있는 스포트라이트를 사용 하 여), 소스 편집기 성능 정상으로 복원 되 고 있는지 여부를 확인 합니다.
1.  보 동기화도 사용 하지 않도록 설정 (1) 단계를 반복 하십시오.
1.  편집기에 몇 초 동안 중지 되는 경우 실행 하려고 "killall-종료 [Mac 용 Visual Studio]" 중단 된 동안 터미널에서. 시간을 편집기 중지 되었습니다. 하지만 반드시 이렇게 명령을 강제로 MD 기록할 스레드 XS 중단 된 동안에 어떤 상태 검색을 사용 하 여 모든 스레드의 스택 추적을 작성 하는 Mono 때문에 발생 하는 kill 명령을 어려울 수 있습니다.



XS 로그를 첨부 해 주십시오 **~/Library/Logs/XamarinStudio-{VERSION}/Ide-{TIMESTAMP}.log**하십시오 **AndroidTools-{TIMESTAMP}.log**, 및 **구성 요소-{TIMESTAMP}.log**(XS/MonoDevelop의 이전 버전에서는 보내기만 **~/Library/Logs/MonoDevelop-(3.0|2.8|2.6)/MonoDevelop.log**).

 **참고: 위의 문제는 XS 2.2 최종에서 수정 되었습니다.**

## <a name="compiled-application-is-very-large"></a>컴파일된 응용 프로그램 매우 큽니다.

디버깅을 지원 하려면 디버그 빌드 추가 코드를 포함 합니다. 릴리스 모드에서 작성 하는 프로젝트는 크기의 비율입니다.

Xamarin.iOS 1.3 디버그 기준으로 빌드 디버깅 Mono (프레임 워크의 모든 클래스의 모든 메서드)의 모든 단일 구성 요소에 대 한 지원이 포함 되어 있습니다.  

디버깅에 대 한 더 세부적인된 메서드 소개 하겠습니다 Xamarin.iOS 1.4를 사용 하 여, 기본 디버깅 계측 코드 및 라이브러리를 제공 하며 모든이 수행 하지 해야 합니다는 [모노 어셈블리](~/cross-platform/internals/available-assemblies.md) (이 여전히 수는 있지만 해당 어셈블리를 디버깅 하도록 옵트인 하려면 해야) 합니다.

## <a name="installation-hangs"></a>설치 중단

Mono 및 Xamarin.iOS 설치 관리자의 iPhone 시뮬레이터 실행 중인 경우 중지 합니다. 이 문제가 Mono 또는 Xamarin.iOS로 제한 되지 않습니다., iPhone 시뮬레이터 설치 시에 실행 중인 경우 소프트웨어를 MacOS Snow Leopard에 설치 하려고 하는 모든 소프트웨어가는 문제가 발생 합니다.

IPhone 시뮬레이터를 종료 하 고 설치를 다시 시도 하는지 확인 합니다.

<a name="trampolines" />

## <a name="ran-out-of-trampolines-of-type-0"></a>유형 0의 trampolines 부족

장치를 실행 하는 동안이 메시지를 받게 되 면 만들면 자세한 형식 (특정 형식) 0 trampolines 프로젝트 옵션 "iPhone 빌드" 섹션을 수정 하 여 됩니다.  빌드 대상 장치에 대 한 추가 인수를 추가 하려면:

 `-aot "ntrampolines=2048"`

Trampolines의 기본값은 1024입니다.  응용 프로그램에 대 한 충분 한 수 있을 때까지이 수를 늘려 보세요.

## <a name="ran-out-of-trampolines-of-type-1"></a>유형 1 인 trampolines 부족

재귀 제네릭을 많이 사용 한다면 장치에서이 메시지를 발생할 수 있습니다.  프로젝트 옵션 "iPhone 빌드" 섹션을 수정 하 여 자세한 형식 1 trampolines (RGCTX 형식)를 만들 수 있습니다.  빌드 대상 장치에 대 한 추가 인수를 추가 하려면:

 `-aot "nrgctx-trampolines=2048"`

Trampolines의 기본값은 1024입니다.  제네릭을 사용 하기 위한 충분 한 수 있을 때까지이 수를 늘려 보세요.

## <a name="ran-out-of-trampolines-of-type-2"></a>유형 2 인 trampolines 부족

인터페이스 사용을 하는 경우 장치에서이 메시지를 발생할 수 있습니다.
프로젝트 옵션 "iPhone 빌드" 섹션을 수정 하 여 자세한 형식 2 trampolines (형식 IMT 썽크)를 만들 수 있습니다.  빌드 대상 장치에 대 한 추가 인수를 추가 하려면:

 `-aot "nimt-trampolines=512"`

IMT 썽크 trampolines의 기본 수는 128 개입니다.  인터페이스의 사용량에 대 한 충분 한 수 있을 때까지이 수를 늘려 보세요.

## <a name="debugger-is-unable-to-connect-with-the-device"></a>디버거는 장치에 연결할 수 없습니다.

장치 구성을 디버깅을 시작 하면 응용 프로그램에 연결 하려는 것을 나타내기 위해 대화 상자를 표시 하는 디버거를 표시 됩니다. (USB 또는 WiFi) 연결을 사용 하는 모드에 따라, 디버거를 응용 프로그램에 연결할 수 있습니다 하는 몇 가지 이유가 있습니다.

 **장치 및 디버거 호스트 서로 다른 네트워크에 있는 경우**, 방화벽 또는 개인 네트워크 응용 프로그램 WiFi 모드에서 디버거 호스트에 연결을 방지할 수 있습니다.

 **Mac 용 visual Studio를 호스트의 올바른 IP를 쿼리할 수**입니다. WiFi Mac 용 Visual Studio 모드 사용 하면 응용 프로그램이 모든 Ip를 찾을 수 있는 호스트의 시도 응용 프로그램이 이러한 모든 경우 사용할 수 있습니다 하의 모든 mac 용 Visual Studio에 연결 하려면를 참조 하세요.

 **다른 장치는 호스트의 USB 포트에 연결 됩니다.** 경우에 따라 다른 장치가 USB를 연결할 호스트의 포트가 어떤 이유로 든 USB 모드에서 디버깅에 방해가으로 알려져 있습니다.

USB 또는 WiFi 모드에서 작동 하지 않는 경우 쉽게 할 수 다른: Mac 용 Visual Studio에서 기본 설정을 엽니다 iPhone/기본/디버거 디버거 페이지로 이동 하 고 "대신 WiFi를 통해 USB를 통해 iOS 장치를 Debug" 확인란을 설정/해제 합니다.   모두 작동 세부 정보 표시 모드로 장치 콘솔에서 실패에 대 한 자세한 정보를 볼 수 있습니다 (추가 하 여 설정 되는 "-v-v-v" 프로젝트의 옵션에서 추가 mtouch 인수에).

## <a name="error-134-mtouch-failed-with-the-following-message"></a>오류 134 mtouch 다음과 같은 메시지가 실패 했습니다.:

버전의 Xamarin.iOS 1.4 스타일에서-nolink 구축 하려는 경우이 오류가 발생할 수 있습니다. Monodevelop 프로젝트 구성에 추가 인수를 지정 하 여이 오류를 해결할 수 있습니다.

인수 추가

 `-nosymbolstrip`

및 문제를 해결 해야 합니다.

## <a name="distribution-identity-is-not-shown-in-visual-studio-for-mac-project-signing-options"></a>서명 옵션 Mac 프로젝트에 대 한 배포 id Visual Studio에 표시 되지 않습니다.

Visual Studio for Mac 2.2에 쉼표를 포함 하는 배포 인증서를 검색 하 여 되지 않는 버그가 있습니다. 2.2.1 Mac 용 Visual Studio를 업데이트 하세요.

## <a name="error-afcfilerefwrite-returned-1-during-upload"></a>오류 "AFCFileRefWrite 반환: 1" 업로드 중

장치에 앱을 업로드 하는 동안 오류가 나타날 수 있습니다 "AFCFileRefWrite 반환: 1". 이 길이가 0 인 파일이 있는 경우에 발생할 수 있습니다.

## <a name="error-mtouch-failed-with-no-output"></a>오류 "mtouch 출력 없이 실패 했습니다."

Mac 용 Xamarin.iOS 및 Visual Studio의 현재 릴리스가 실패할 때 프로젝트 이름을 솔루션 또는 프로젝트를 저장 된 디렉터리에 공백을 포함 합니다.
이 문제를 해결하려면


-  프로젝트 또는 저장 된 디렉터리 모두에 공백을 포함 해야 합니다.
-  "기본 설정을" 프로젝트에서는 프로젝트 이름에 공백이 없는지 확인 합니다.

## <a name="error-the-binary-you-uploaded-was-invalid-a-pre-release-beta-version-of-the-sdk-was-used-to-build-the-application"></a>오류 "업로드 한 이진이 잘못 되었습니다. 응용 프로그램을 빌드하려면 시험판 베타 버전의 SDK 사용한 "

이 오류는 일반적으로 Xamarin.iOS 2.0.0 출시 되기 전에 iPad 개발에서 시작 된 프로젝트를 사용 하 여 발생, 같은 Info.plist에 일부 키를 있을 가능성이 높습니다.

```xml
<key>UIDeviceFamily</key>
       <array>
               <string>1</string>
       </array>
```

Mac 용 Visual Studio 처리를 자동으로이 키 쌍을 제거 해야 합니다.

## <a name="error-a-pre-release-beta-version-of-the-sdk-was-used-to-build-the-app"></a>오류 "시험판 베타 버전의 SDK 사용한 앱 빌드"

(Ed Anuff 제공한)

아래 단계를 수행합니다.

-  SDK 버전 3.2 또는 iTunes iPhone 빌드에에서 연결 하는 변경 거부 합니다. 업로드 3.2 미만의 SDK 버전을 사용 하 여 빌드된 iPad 호환 앱이 보는 것 때문에
-  프로젝트에 대 한 사용자 지정 Info.plist 만들고 MinimumOSVersion 3.0에 명시적으로 설정 합니다.   Xamarin.iOS 설정한 MinimumOSVersion 3.2 값이 재정의 됩니다.   이렇게 하지 않으면 앱을 iPhone에서 실행 되지 않습니다.
-  다시 작성, zip 및 업로드 iTunes에 연결합니다.

## <a name="unhandled-exception-systemexception-failed-to-find-selector-someselector-on-type"></a>처리 되지 않은 예외: System.Exception: 선택기 someSelector를 찾지 못했습니다. {type}에서

다음 세 가지 중 하나에서이 예외가 발생 했습니다.


1.  제공한 메서드에 해당 [내보내기] 특성을 적용 하지 않고 Objective-c 런타임에 선택기
1.  전체 연결을 활성화 하 고 [Preserve] 특성 [내보내기] ed 메서드에 적용 되지 않습니다.
1.  상속된 된 형식에서 개인 메서드에 [내보내기] 특성을 적용 합니다.


## <a name="mainwindowxibdesignercs-file-is-not-updated"></a>MainWindow.xib.designer.cs 파일이 업데이트 되지 않습니다.

새 프로젝트에서 MainWindow.xib.designer 파일을 사용 하 여 MainWindow.xib 파일 그룹 경우 Xamarin Studio 2.4에서 버그가 있었습니다. 이 해당 특정 파일에 대 한 디자이너 코드를 업데이트 하지 것을 의미 합니다.

이 문제는 해당 기본 제공 업데이트에서 사용할 수 있는 Mac 용 Visual Studio 버전에서 해결 하도록 최신 버전을 사용 하 여 확인 하세요.

기존 프로젝트 (삭제 되지 않습니다)를 제거 하 여 해결할 수 있습니다 xib 및 디자이너 파일을 다시 추가 합니다. 다시 파일을 제대로 그룹화 해야이 합니다.

## <a name="uialertview-or-uiactionsheet-vanish-after-being-created"></a>만든 후 UIAlertView 또는 UIActionSheet 사라집니다.

있는 경우 다음과 같은 몇 가지 코드:

```csharp
var actionSheet = new UIActionSheet ("My ActionSheet", null, null, "OK", null){
   Style = UIActionSheetStyle.Default
};

actionSheet.Clicked += delegate (sender, args){
    Console.WriteLine ("Clicked on item {0}", args.ButtonIndex);
};
```

"actionSheet" 개체 함수에서 임시 변수로 있으며 함수가 종료 되는 즉시 개체 이므로 가비지 수집 대상이 가비지 수집을 종료 합니다.

이 문제를 해결 하려면 "actionSheet" 어딘가에 메서드 외부 되 메서드 외에 대 한 참조를 유지 해야 합니다.

## <a name="project-always-runs-in-the-ipad-simulator"></a>프로젝트 항상 실행 iPad 시뮬레이터

IPhone SDK 4.0 설치 관리자는 2 Sdk-iPad 전용 앱을 빌드하기 위한 3.2 SDK와 iPhone 및 유니버설 앱을 빌드하는 데 4.0 SDK를 설치 합니다. 또한 iPad만 시뮬레이션을 3.2 시뮬레이터를 및 iPhone 또는 iPhone 4 시뮬레이트하는 4.0 시뮬레이터를 설치 합니다. Sdk 및 시뮬레이터에 대 한 모든 이전 제거 됩니다.

IPhone 프로젝트 빌드 옵션 Mac 용 visual Studio는 앱 빌드에 사용 되는 SDK 버전에 대 한 설정의 포함 합니다. 이 설정에서 찾을 수 있습니다 **프로젝트 옵션에는 빌드-> iPhone 빌드->** 합니다.

Mac 용 Visual Studio에서 새 프로젝트 가장 오래 된 설치 된 SDK를 사용 하 여 해당 기본 SDK 설정이로 하 고 Mac 용 Visual Studio는 가장 가까운 응용 프로그램을 찾을 수 있습니다 사용 하 여 지정 된 SDK 존재 하지 않는 경우. 이 프로젝트는 항상 최신 SDK를 요구 하지 않도록 작업을 수행 했습니다. 그러나 현재 그러면 되 고 있는 3.2 SDK 사용-그러면 사용 중인 iPad 시뮬레이터입니다.

4.0 SDK를 사용 하 여이 문제를 해결로 이동 **프로젝트 옵션에는 빌드-> iPhone 빌드->**> SDK 값 드롭다운 상자를 사용 하 여 "4.0"을 변경 합니다. 각 구성 및 액세스 패널의 맨 위에 있는 드롭다운을 사용 하 여 플랫폼 조합에 대 한이 작업을 수행 해야 합니다.

SDK 버전을 "최소 OS 버전" 설정을 혼동 해서는 안됩니다.
이 값은 SDK 버전 값과 일치 하지 않아도-으로 이전 OS에 없거나 런타임 OS 버전 검사를 사용 하 여 새 기능의 사용을 보호 하는 Api만 사용 하 여 SDK 보다 이전 일 수 있는 앱 설치는 OS의 최소 버전에 영향을 주므로 ks 합니다. 응용 프로그램을 테스트 하는 가장 오래 된 OS 버전으로 설정 해야 합니다.

또한 합니다 **프로젝트 iPhone 시뮬레이터 대상->**> 메뉴를 사용 하 여 프로젝트 실행/디버깅 하는 경우 기본적으로 사용 되는 시뮬레이터를 선택할 수 있습니다. 또한 합니다 **사용 하 여 실행-> 실행**> 메뉴를 사용 하 여 실행 하는 특정 시뮬레이터를 선택할 수 있습니다

## <a name="ibtool-returns-error-133"></a>ibtool 오류 133를 반환합니다.

즉, XCode 4가 설치 되어 있다고 합니다.   XCode 4에서는 도구 ibtool 제거 되었습니다, 그리고 독립 실행형 도구를 사용 하 여 XIB 파일을 편집할 수는 없습니다.

Interface Builder를 사용 하려는 경우 설치 [XCode 시리즈 3](http://connect.apple.com/cgi-bin/WebObjects/MemberSite.woa/wa/getSoftware?bundleID=20792), Apple의 웹 사이트에서 사용할 수 있습니다.

## <a name="cant-create-display-binding-for-mime-type-applicationvndapple-wbrinterface-builder"></a>"Mime 형식에 대 한 표시 바인딩을 만들 수 없습니다: application/vnd.apple-<wbr/>인터페이스 작성기"

이 오류는 iPhone이 아닌 프로젝트에서 iPhone UI를 만들려고 할 경우에 발생 합니다. IPhone/iPad 솔루션을 사용 하 여 시작을 iPhone/iPad 아닌 프로젝트에 iPhone UI 요소를 바로 추가할 수 없는 있는지 확인 합니다.

## <a name="startup-crash-when-executing-inside-the-ios-simulator"></a>IOS 시뮬레이터 내에서 실행 하는 경우 시작 충돌

다음과 같은 스택 추적을와 함께 시뮬레이터 내에서 (SIGSEGV) 런타임 크래시가 발생 하면:

```csharp
  at (wrapper managed-to-native) System.Reflection.Assembly.GetTypes (System.Reflection.Assembly,bool)
  at MonoTouch.ObjCRuntime.Runtime.RegisterAssembly (System.Reflection.Assembly)
  at (wrapper runtime-invoke) <Module>.runtime_invoke_void_object (object,intptr,intptr,intptr)
```
... 다음 시뮬레이터 응용 프로그램 디렉터리에 하나의 (또는 그 이상) 유효 하지 않은 어셈블리를 있을 가능성이 큽니다. 이러한 어셈블리는 Apple iOS 시뮬레이터 추가 하 고 업데이트 파일 있지만 삭제 하지 있습니다 수 있습니다. 이 가장 쉬운 해결 방법은 다음 경우 시뮬레이터 메뉴에서 "다시 설정 하 고 콘텐츠 및 설정..."를 선택 하는 것입니다.   

**경고:** 모든 파일 및 응용 프로그램 데이터 시뮬레이터에서 제거 합니다.   응용 프로그램을 실행 하는 다음에 Mac 용 Visual Studio 시뮬레이터에 배포 됩니다 하 고 크래시를 오래 되 고 오래 된 어셈블리가 됩니다.

## <a name="simulator-hangs-during-application-installation"></a>응용 프로그램을 설치 하는 동안 시뮬레이터 중지

이 응용 프로그램 이름을 포함 하는 경우에 발생할 수는 '.' (점) 이름에 있습니다.
이 값은 많은 경우 (예: 장치)에서 작동 하는 가능한 경우에 CFBundleExecutable-실행 파일 이름으로 금지 되어 있습니다.

 * "값을 모든 확장 프로그램 이름을 포함 해야 합니다." - [http://developer.apple.com/library/mac/documentation/General/Reference/InfoPlistKeyReference/InfoPlistKeyReference.pdf](http://developer.apple.com/library/mac/documentation/General/Reference/InfoPlistKeyReference/InfoPlistKeyReference.pdf)

## <a name="error-custom-attribute-type-0x43-is-not-supported-when-double-clicking-xib-files"></a>오류: "사용자 지정 특성 유형 0x43 지원 되지 않습니다" double.xib 파일을 클릭할 때

이 환경 변수를 올바르게 설정 된 경우.xib 파일을 열려고 시도 하 여 발생 합니다. Mac/Xamarin.iOS에 대 한 일반적인 사용의 Visual Studio를 사용 하 여 발생 되지 않아야 하 고 /Applications에서 Mac 용 Visual Studio를 다시 열어 문제를 해결 해야 합니다.

업데이트 하려고 할 때 소프트웨어 및이 오류 메시지가 나타나면 하세요 전자 메일 *support@xamarin.com*

## <a name="application-runs-on-simulator-but-fails-on-device"></a>응용 프로그램은 시뮬레이터에서 실행 되지만 장치에서 실패

이 문제는 여러 가지 형태로 나타날 수 있습니다 이며 항상 일관 된 오류를 생성 하지 않습니다. 응용 프로그램을.xib 있으면 있는지 확인 합니다 **빌드 작업** .xib을로 설정 됩니다 **InterfaceDefinition**합니다. 이것이.xibs에 대 한 기본 빌드 작업입니다.

빌드 작업을 확인 하려면.xib 파일을 마우스 오른쪽 단추로 클릭 하 고 선택 **빌드 작업**합니다.


## <a name="systemnotsupportedexception-no-data-is-available-for-encoding-437"></a>System.NotSupportedException: 없습니다 데이터가 437 인코딩에 사용할 수 있습니다.

Xamarin.iOS 앱에서 타사 라이브러리를 포함 하는 경우 오류가 발생할 수 있습니다는 형태로 "System.NotSupportedException: 데이터가 437 인코딩에 사용할 수 없습니다" 컴파일 및 앱을 실행 하려고 할 때입니다. 예를 들어 라이브러리와 같은 `Ionic.Zip.ZipFile`를 작업 하는 동안이 예외를 throw 할 수 있습니다.

Xamarin.iOS 프로젝트에 대 한 옵션을 열어이 해결할 수 있습니다 **iOS 빌드** > **국제화** 확인 합니다 **서쪽** 국제화 합니다.



<a name="Can't_upgrade_to_Indie/Business_from_Trial_Account" />


## <a name="cant-upgrade-to-indiebusiness-from-trial-account"></a>평가판 계정에서 인디/비즈니스로 업그레이드할 수 없습니다.

최근에 Xamarin.iOS를 구입 하 고 이전에 Xamarin.iOS 평가판을 시작 하는 경우 Visual Studio가 Mac 또는 Visual Studio에 대 한 선택이 라이선스 변경 하려면 다음이 단계를 완료 해야 합니다.

-  Mac/Visual Studio 용 Visual Studio를 닫습니다.
-  Windows에 대 한 모든 파일을 Mac에서 ~/Library/MonoTouch 또는 %PROGRAMDATA%\MonoTouch\License\에서 제거 합니다.
-  다시 Visual Studio for Mac/Visual Studio를 열고 Xamarin.iOS 프로젝트를 빌드


## <a name="receiving-activation-incomplete-error-message"></a>'활성화에 불완전 한 경우' 오류 메시지를 수신합니다.

이 문제는 Visual Studio 용 Xamarin.iOS를 사용 하는 경우에 발생할 수 있습니다. 이 문제를 해결 하려면 로그를 보내세요를 하기 위해 다음 위치에서 [ contact@xamarin.com ](mailto:contact@xamarin.com)합니다.

-  로그 위치: %LocalAppData%/Xamarin/Logs
