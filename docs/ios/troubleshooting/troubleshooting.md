---
title: 문제 해결
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: B50FE9BD-9E01-AE88-B178-10061E3986DA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/22/0201
ms.openlocfilehash: 6a179c1d63e9b5a7b8a42705d5c112a7b71a4906
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="troubleshooting"></a>문제 해결

## <a name="xamarinios-cannot-resolve-systemvaluetuple"></a>Xamarin.iOS cannot resolve System.ValueTuple

이 오류는 Visual Studio와의 비 호환성으로 인해 발생합니다.

- **Visual Studio 2017 업데이트 1** (15.1 또는 이전 버전) 에서만 호환 됩니다. **System.ValueTuple NuGet 4.3.0** (또는 이전 버전).

- **Visual Studio 2017 업데이트 2** (15.2 또는 최신 버전) 호환 되는 **System.ValueTuple NuGet 4.3.1** 이상.

Visual Studio 2017 설치와 일치 하는 올바른 System.ValueTuple NuGet을 선택 하십시오.


## <a name="receiving-error-retrieving-update-information-error-message"></a>오류 메시지 '오류 업데이트 정보를 검색 하는 데 사용'

표시 되는 소프트웨어 및이 오류 메시지를 업데이트 하는 동안 IDE에서 IDE 및 로그 아웃 및 계정에 대 한 다음 다시를 다시 시작 시도 하세요.

## <a name="how-do-i-create-outlets-or-actions-with-interface-builder"></a>어떻게 만드나요 콘센트 또는 동작 인터페이스 작성기?

Mac 및 Visual studio 용 Visual Studio에서 iOS 용 Xamarin 디자이너의 도입으로 Xamarin.iOS 개발자가 이제 활용할 수 스토리 보드와.xibs 통해 UI 만들기. 참조는 [Hello, iOS](~/ios/get-started/hello-ios/index.md) 디자이너를 사용 하는 방법은 대 한 가이드입니다.

또한 Apple의를 참조할 수 있습니다 [콘센트](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) 및 [작업](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingAction.html) 콘센트 및 작업을 사용 하 여 IB에 대 한 자세한 내용은 가이드입니다.

## <a name="systemtextencodinggetencoding-throws-notsupportedexception"></a>System.Text.Encoding.GetEncoding NotSupportedException를 발생 시킵니다.

기본적으로 추가 되지 않습니다는 인코딩을 사용 하 여 수 있습니다. 확인 된 [국제화](~/ios/app-fundamentals/localization/index.md) 자세한 인코딩에 대 한 지원을 추가 하는 방법에 알아보려면 페이지입니다.

## <a name="systemmissingmethodexception-anything-else"></a>(다른 항목이) System.MissingMethodException

가 링커에 의해 제거 될 가능성이 구성원과 따라서 런타임에 어셈블리에 존재 하지 않습니다.  여러 가지 솔루션을이 있습니다.

-  추가 [[유지]](http://www.go-mono.com/docs/index.aspx?link=T:MonoTouch.Foundation.PreserveAttribute) 멤버에 특성입니다.  이렇게 하면 링커에서에서 제거 되지 것입니다.
-  호출할 때 [mtouch](http://www.go-mono.com/docs/index.aspx?link=man:mtouch%281%29) 를 사용 하 여는 **-nolink** 또는 **-linksdkonly** 옵션입니다. -    **-nolink** 모든 연결을 사용 하지 않도록 설정 합니다.
-    **-linksdkonly** 가 연결 될 Xamarin.iOS 제공 어셈블리와 같은 *monotouch.dll* 또는 xamarin.ios.dll 합니다.

결과 실행 파일을 더 작은; 어셈블리에 연결 되어 있음을 note합니다 따라서 연결 사용 안 함은 바람직한 보다 더 큰 실행 파일에 발생할 수 있습니다.

## <a name="you-are-getting-a-modelnotimplementedexception"></a>ModelNotImplementedException 가져오는지

이 예외를 발생 하는 경우이 호출 하는 기본 의미 합니다. 모델을 재정의 하는 클래스에 메서드 (). (이러한 클래스 [모델] 특성으로 플래그가 지정 되는) 모델에 대 한 클래스에서 기본 메서드를 호출할 필요가 없습니다.

## <a name="this-class-is-not-key-value-coding-compliant-for-the-key-xxxx"></a>이 클래스 코딩 규격 XXXX 키에 대 한 키 값이 아닙니다.

NIB 파일을 로드할 때이 오류가 발생할 경우 XXXX 관리 되는 클래스에 없는 값 하는 것이 것입니다. 이 처럼 선언 없는 것을 의미 합니다.

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

위의 정의 자동으로 생성 됩니다 Visual Studio가 Mac에서 Mac 용 Visual Studio에 추가 하는 모든 XIB 파일에 대 한는 `NAME_OF_YOUR_XIB_FILE.designer.xib.cs` 파일입니다.

또한 위의 코드를 포함 하는 형식의 하위 클래스 여야 [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/)합니다.  또한 있어야 포함 하는 형식은 네임 스페이스 내에 있으면는 [[등록]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) (작성기 인터페이스 형식에 네임 스페이스를 지원 하지 않습니다) 처럼 네임 스페이스가 없는 형식 이름을 제공 하는 특성:

```csharp
namespace Samples.GLPaint {

    // The [Register] attribute overrides the type name registered
    // with the Objective-C runtime, in this case removing the namespace.
    [Register ("AppDelegate")]
    public class AppDelegate {/* ... */}
}
```

## <a name="unknown-class-xxxx-in-interface-builder-file"></a>XXXX 인터페이스 작성기 파일에 알 수 없는 클래스

이 오류는 인터페이스 작성기 파일에서 클래스를 정의 하지만 실제 구현에 대 한 C# 코드에서를 제공 하지 않는 경우에 생성 됩니다.

다음과 같은 코드를 추가 해야 합니다.

```csharp
public partial class MyImageView : UIView {
   public MyImageView (IntPtr handle) : base (handle {}
}
```
## <a name="systemmissingmethodexception-no-constructor-found-for-foobarctorsystemintptr"></a>: System.MissingMethodException 생성자 Foo.Bar::ctor(System.IntPtr)에 대 한

이 오류 코드는 인터페이스 작성기 파일에서 참조 하는 클래스의 인스턴스를 인스턴스화하고 하려고 할 때 런타임 시 생성 됩니다. 즉, 매개 변수로 단일 IntPtr을 사용 하는 생성자를 추가를 하지 않았습니다.

IntPtr 핸들을 사용 하 여 생성자는 관리 되지 않는 값 대신 사용 하 여 관리 되는 개체를 바인딩할 사용 됩니다.

이 문제를 해결 하려면 Foo.Bar 클래스에 다음 코드 줄을 추가 합니다.

```csharp
public Bar (IntPtr handle) : base (handle) { }
```
## <a name="type-foo--does-not-contain-a-definition-for-getnativefield-and-no-extension-method-getnativefield-of-type-foo-could-be-found"></a>{Foo} 형식에 대 한 정의 포함 하지 않는 `GetNativeField' and no extension method `GetNativeField' 형식의 {Foo을 (를) 찾을 수 없습니다

생성된 된 디자이너 파일에서이 오류가 발생할 경우 (*. xib.designer.cs), 다음 두 가지 중 하나를 의미 합니다.

 **1) 누락 된 partial 클래스 또는 기본 클래스**

디자이너에서 생성 된 partial 클래스의 일부 하위 클래스에서 상속 하는 사용자 코드의 해당 부분 클래스 있어야 `NSObject`종종 `UIViewController`합니다. 이러한 클래스는 오류를 제공 하는 형식에 대 한 가졌는지 확인 합니다.

 **2) 기본 네임 스페이스 변경**

디자이너 파일은 프로젝트의 기본 네임 스페이스 설정을 사용 하 여 생성 됩니다. 이러한 설정을 변경 하거나 프로젝트 이름을 바꿀 경우 생성된 된 partial 클래스는 사용자 코드 함수와 동일한 네임 스페이스에 더 이상 수 없습니다.

Namespace 설정은 프로젝트 옵션 대화 상자에서 찾을 수 있습니다. 기본 네임 스페이스에서 발견 되는 **일반 주 설정->** 섹션. 빈 경우 프로젝트 이름은 기본값으로 사용 됩니다. 더 많은 고급 네임 스페이스 설정을 찾을 수 있습니다는 **소스 코드에는.NET Naming p->** 섹션.

## <a name="warning-for-actions-the-private-method-foo-is-never-used-cs0169"></a>작업에 대 한 경고: private 메서드 'Foo'은 사용 되지 않습니다. (CS0169)

이 경고 사용할 수 있도록 런타임에 리플렉션에 의해 인터페이스 작성기 파일에 대 한 작업 위젯에에 연결 됩니다.

"#Pragma 경고 0169 사용 하지 않도록 설정 하는 데 사용"을 사용할 수 있습니다 "#pragma 경고 사용 0169" 작업을 수행할 때 주위를 이러한 방법에 대 한이 경고를 표시 하거나 전체 프로젝트 (not 사용 하지 않도록 설정 하려는 경우 0169 컴파일러 옵션의 "경고 무시" 필드에 추가 하려는 경우 권장).

## <a name="mtouch-failed-with-the-following-message-cannot-open-assembly-pathtoyourprojectexe"></a>mtouch 다음 메시지와 함께 실패: 어셈블리를 열 수 없습니다 ' / path/to/yourproject.exe'

이 오류 메시지를 표시 하는 경우 일반적으로 문제는 프로젝트에 절대 경로 공백이 포함. Xamarin.iOS의 이후 버전에서 수정 될 것이 있지만 프로젝트 공백 없이 폴더를 이동 하 여 문제를 해결할 수 있습니다.

## <a name="your-sqlite3-version-is-old---please-upgrade-to-at-least-v350"></a>Sqlite3 버전이 오래 되어-이상으로 업그레이드 하십시오 v3.5.0!

다음과 같은 작업을 수행할 때 발생 합니다.

1.  Mono.Data.Sqlite 사용
1.  Mac OS X Leopard (10.5)를 사용 하 여
1.  시뮬레이터에서 앱을 실행 합니다.


모노 OS X를 선택 하는 문제는 `libsqlite3.dylib`, 하지: iPhoneSimulator `libsqlite3.dylib` 파일입니다. 앱 *됩니다* 장치를 제외 하지 않을 사용자 시뮬레이터에 작동 합니다.

## <a name="deploy-to-device-fails-with-systemexception-amdeviceinstallapplication-returned-3892346901"></a>System.Exception 실패 하 고 장치에 배포: AMDeviceInstallApplication 3892346901 반환

이 오류 코드 서명 인증서/번들 id에 대 한 구성을 장치에 설치 하 고 프로 비전 프로필와 일치 하지 않음을 의미 합니다.  번들 서명 iPhone-> 프로젝트 옵션에서 선택 된 적절 한 인증서 있고 프로젝트 옵션에 지정 된 올바른 번들 id iPhone 응용 프로그램->를 확인 합니다.

## <a name="code-completion-is-not-working-in-visual-studio-for-mac"></a>Mac 용 코드 완성 Visual Studio에서 작동 하지 않습니다.

Xamarin.iOS 및 Mac에 대 한 최신 버전의 Visual Studio를 사용 하 고 있는지 확인 하십시오.

이 문제는 아직 있는 경우 [버그를 보고할](http://monodevelop.com/Developers#Reporting_Bugs), 연결 된 **~/Library/Logs/XamarinStudio-{VERSION}/Ide-{TIMESTAMP}.log**, **AndroidTools-{TIMESTAMP}.log**, 및 **구성 요소-{TIMESTAMP}.log** 로그 파일입니다.

모든 작업이 실패 하면 다시 생성 될 수 있도록 코드 완성 캐시 제거를 시도할 수 있습니다.

 `[rm -r ~/.config/XamarinStudio-{VERSION}/CodeCompletionData]`

이 명령을 제대로 입력 또는 실수로 중요 한 파일을 제거할 수 있습니다에 주의 해야 합니다.

## <a name="visual-studio-for-mac-crashes-when-you-copy-text"></a>텍스트를 복사 하는 경우 Mac 용 visual Studio가 충돌

인기 있는 Mac 유틸리티 퀵, Google 도구 모음과 실행 창 Mac의 메모리에 대 한 Visual Studio 손상 클립보드 기능이 있습니다. 옵션에서 방해 하지 않아야 하는 프로세스로 Mac 용 Visual Studio를 나열할 수 있습니다.

## <a name="visual-studio-for-mac-complains-about-mono-24-required"></a>필요한 모노 2.4에 대해 Mac 용 visual Studio 불평

만 하면 Mac에 대 한 최근 업데이트로 인해 Visual Studio를 업데이트 하는 경우 시작 하려고 할 때 다시 것에 대해 불평 현재 사용 되 고 있지 모노 2.4 [모노 2.4 설치 업그레이드](http://www.go-mono.com/mono-downloads/download.html)합니다.  

모노 2.4.2.3_6 시작 시 Mac에 대 한 응답이 없는 경우에 따라 Visual Studio를 안전 하 게 실행에서 Mac 용 Visual Studio 수 없습니다. 또는 생성 되지 코드 완성 데이터베이스 수 있는 몇 가지 중요 한 문제를 해결 합니다.

새 모노를 설치한 후 예상 대로 Mac 용 Visual Studio 시작 됩니다.

## <a name="assertion-at-monometadatageneric-sharingc704-condition-oti-not-met"></a>에 대 한 어설션을... /.. /.. /.. /mono/metadata/generic-sharing.c:704, 조건에 맞지 않음 ' oti'

다음 스택 추적 발생 하는 경우:

```csharp
 - Assertion at ../../../../mono/metadata/generic-sharing.c:704, condition `oti' not met
Stacktrace:
    at System.Collections.Generic.List`1<object>..cctor () <0xffffffff>
    at System.Collections.Generic.List`1<object>..cctor () <0x0001c>
    at (wrapper runtime-invoke) object.runtime_invoke_dynamic (intptr,intptr,intptr,intptr) <0xffffffff>`
```

프로젝트에 thumb 코드가로 컴파일된 정적 라이브러리를 연결 하 고 있는지 나타냅니다. IPhone SDK 버전 3.1 이상이이 문서 작성 당시의 Apple Thumb 코드 (정적 라이브러리)와 비 Thumb 코드 (Xamarin.iOS)를 연결할 때 해당 링커의 버그를 도입 합니다. 이 문제를 완화 하기 위해 정적 라이브러리의 Thumb 아닌 버전과 연결 해야 합니다.

## <a name="systemexecutionengineexception-attempting-to-jit-compile-method-wrapper-managed-to-managed-foosystemcollectionsgenericicollection1getcount-"></a>JIT 컴파일 메서드 (관리에서 관리로 래퍼) Foo[]:System.Collections.Generic.ICollection'1.get_Count ()를 시도: System.ExecutionEngineException

접미사는 클래스 라이브러리 또는 메서드 호출 하는 IList <> ienumerable<>, ICollection <> 등의 제네릭 컬렉션을 통해 배열에 나타냅니다. 문제를 해결 명시적으로 되어 있어야 force 메서드를 포함 하도록 이러한 메서드를 직접 호출 하 여 AOT 컴파일러를 하면 및 하는지 확인 하 여이 코드는 예외를 트리거한 호출 전에 실행 됩니다. 이 경우 작성할 수 있습니다.

```csharp
Foo [] array = null;
int count = ((ICollection<Foo>) array).Count;
```

Get_Count 메서드를 포함 하도록 AOT 컴파일러를 강제로입니다.

## <a name="visual-studio-for-mac-source-editor-is-extremely-slow"></a>Visual Studio Mac 소스 편집기에 대 한 매우 느립니다.

경우에 따라 Mac 소스 편집기에 대 한 Visual Studio 나타나는 문자를 입력 간의 몇 초간 지연으로 매우 느린 됩니다.

이 문제는 매우 드문 및 재현 하기 어려운 매우-것 일반적으로 재현할 수 없는 같은 컴퓨터에서 Mac.에 대 한 Visual Studio를 다시 시작한 후 이러한 이유로 해 주시면, Mac 용 Visual Studio를 다시 시작 하기 전에 몇 가지 디버깅 단계를 수행 하 고 결과를 보낼 수 없습니다.

1.  편집기 탭을 닫고 다시 열어 및 합니다. 약간의 편집 하거나 있는 속도가 느려지는 다시 발생 될 때까지 캐럿을 이동 시간이 걸리나요?
1.  "Quartz Debug" 개발자 도구를 사용 하 여 "빔 동기화" 사용 안 함 (찾을 수 있는 추천을 사용 하 여), 소스 편집기 성능 normal로 복원 되는지 여부를 확인 합니다.
1.  광선 동기화 여전히 사용할 수 없습니다 (1) 단계를 반복 하십시오.
1.  편집기를 몇 초 동안 중지 하는 경우 실행 하려고 "killall-종료 [Mac 용 Visual Studio]" 중단 된 동안 종료에서 합니다. 편집기 정지 이지만를 위해 필수 때문에 명령이 강제로 모노 XS 중단 된 동안에 스레드는 어떤 상태를 검색 하는 데 사용할 수 있는 म MD 로그에 모든 스레드의 스택 추적을 작성 하는 동안 발생 하는 kill 명령을 시간 하기 어려울 수 있습니다.



XS 로그 첨부 하십시오 **~/Library/Logs/XamarinStudio-{VERSION}/Ide-{TIMESTAMP}.log**, **AndroidTools-{TIMESTAMP}.log**, 및 **구성 요소-{TIMESTAMP}.log**(XS/MonoDevelop의 이전 버전에서는 보내기만 **~/Library/Logs/MonoDevelop-(3.0|2.8|2.6)/MonoDevelop.log**).

 **참고: 위의 문제가 XS 2.2 마지막에 수정 된**

## <a name="compiled-application-is-very-large"></a>컴파일된 응용 프로그램은 매우 큰

디버그 빌드는 디버깅을 지원 하려면 추가 코드가 포함 됩니다. 릴리스 모드에서 기본적으로 제공 하는 프로젝트는 크기의 비율입니다.

Xamarin.iOS 1.3 디버그를 기준으로 빌드 디버깅 모노 (프레임 워크의 모든 클래스의 모든 메서드에)의 모든 단일 구성 요소에 대 한 지원이 포함 되어 있습니다.  

Xamarin.iOS 1.4와 디버깅에 대 한 보다 세밀 하 게 세분화 된 메서드에 대해 소개 합니다, 기본 제공 되며 디버깅 계측 코드 및 라이브러리를 제공 하 고 모든에 대해이 수행 하지 됩니다는 [모노 어셈블리](~/cross-platform/internals/available-assemblies.md) (이 계속 축소할 수 있지만에 참여 하는 해당 어셈블리를 디버깅 하는).

## <a name="installation-hangs"></a>설치 중지

Xamarin.iOS 및 모노 설치 관리자가 실행 되는 iPhone 시뮬레이터에 있는 경우 중지 합니다. 이 문제를 모노 또는 Xamarin.iOS 제하지 없습니다 계속 문제가 발생 소프트웨어 MacOS Snow Leopard iPhone 시뮬레이터 실행 하는 경우 설치 시에 설치 하려고 하는 모든 소프트웨어입니다.

IPhone 시뮬레이터를 종료 하 고 다시 설치 해야 합니다.

<a name="trampolines" />

## <a name="ran-out-of-trampolines-of-type-0"></a>형식의 trampolines 부족

장치를 실행 하는 동안이 메시지를 받을 경우 형식을 만들 수 있습니다 더 많은 (특정 입력) 0 trampolines 프로젝트 옵션 "iPhone 빌드" 섹션을 수정 하 여 합니다.  불필요 한 인수는 장치에 대 한 빌드 대상을 추가 하려면:

 `-aot "ntrampolines=2048"`

Trampolines의 기본값은 1024입니다.  응용 프로그램에 대 한 충분 한 될 때까지이 수를 늘려 보십시오.

## <a name="ran-out-of-trampolines-of-type-1"></a>1 형식의 trampolines 부족

재귀 제네릭을 많이 사용 하는 경우 장치에서이 메시지를 발생할 수 있습니다.  프로젝트 옵션 "iPhone 빌드" 섹션을 수정 하 여 더 많은 형식 1 trampolines (RGCTX 유형)을 만들 수 있습니다.  불필요 한 인수는 장치에 대 한 빌드 대상을 추가 하려면:

 `-aot "nrgctx-trampolines=2048"`

Trampolines의 기본값은 1024입니다.  제네릭의 사용에 대 한 충분 한 될 때까지이 수를 늘려 보십시오.

## <a name="ran-out-of-trampolines-of-type-2"></a>유형 2 trampolines 부족

인터페이스 사용 하는 경우 장치에서이 메시지를 발생할 수 있습니다.
프로젝트 옵션 "iPhone 빌드" 섹션을 수정 하 여 더 많은 형식 2 trampolines (IMT 썽크 형식)를 만들 수 있습니다.  불필요 한 인수는 장치에 대 한 빌드 대상을 추가 하려면:

 `-aot "nimt-trampolines=512"`

IMT 썽크 trampolines의 기본값은 128입니다.  인터페이스의 사용에 대 한 충분 한 될 때까지이 수를 늘려 보십시오.

## <a name="debugger-is-unable-to-connect-with-the-device"></a>디버거는 장치에서 연결할 수 없습니다.

장치 구성을 디버깅을 시작 하면 응용 프로그램에 연결 하려고 하는 대화 상자가 표시 디버거를 표시 됩니다. (USB 또는 WiFi) 연결 하는 데 사용 하는 모드에 따라, 몇 가지 이유로 디버거 응용 프로그램에 연결할 수 없을 수 있습니다.

 **장치와는 디버거 호스트로 서로 다른 네트워크에 있는**, 방화벽 또는 개인 네트워크 하지 못하게 WiFi 모드에서 디버거 호스트에 연결 하지 못하도록 응용 프로그램입니다.

 **Mac 용 visual Studio를 호스트의 올바른 IP를 쿼리할 수**합니다. WiFi Mac 용 Visual Studio 모드 사용 하면 응용 프로그램이 모든 Ip 찾을 수 있는 호스트의 한 응용 프로그램을 시도 모든 확인 하도록 하는 경우 사용할 수 있습니다 그 중 어느 Mac.에 대 한 Visual Studio에 연결 하려면

 **다른 장치는 호스트의 USB 포트에 연결 됩니다.** USB에 연결 된 다른 장치는 몇 가지 경우에는 호스트의 포트가 어떻게 하 든 USB 모드에서 디버깅을 방해 알려져 있습니다.

WiFi 또는 USB 모드 작동 하지 않을 경우 쉽게 연습할 수 다른: Mac 용 Visual Studio에서 기본 설정 페이지로 이동 하 여 기본 설정/디버거/iPhone 디버거를 열고 "USB를 통해 대신 WiFi를 통해 iOS 장치를 Debug" 확인란 설정/해제 합니다.   세부 정보 표시 모드로 장치 콘솔에서 실패에 대 한 자세한 정보를 볼 수 둘 다가 작동 하는 경우 (추가 하 여 활성화 되어 "-v-v-v" 프로젝트의 옵션에서 추가 mtouch 인수에).

## <a name="error-134-mtouch-failed-with-the-following-message"></a>오류 134 mtouch 다음 메시지와 함께 실패 했습니다.:

이 오류는 릴리스 Xamarin.iOS 1.4 스타일-nolink로 빌드 하려는 경우 발생할 수 있습니다. Monodevelop 프로젝트 구성에 추가 인수를 지정 하 여이 오류를 해결할 수 있습니다.

인수를 추가 합니다.

 `-nosymbolstrip`

및이 문제를 해결 해야 합니다.

## <a name="distribution-identity-is-not-shown-in-visual-studio-for-mac-project-signing-options"></a>서명 옵션 Mac 프로젝트에 대 한 배포 id Visual Studio에 표시 되지 않습니다.

Mac 2.2에 대 한 visual Studio에는 쉼표를 포함 하는 배포 인증서를 검색 하지 않는 버그가 있습니다. 2.2.1 Mac 용 Visual Studio로 업데이트 하세요.

## <a name="error-afcfilerefwrite-returned-1-during-upload"></a>오류 "반환 된 AFCFileRefWrite: 1" 업로드 중

장치에 앱을 업로드 하는 동안 오류가 발생할 수 있습니다 "반환 된 AFCFileRefWrite: 1". 길이가 0 인 파일이 있는 경우 발생할 수 있습니다.

## <a name="error-mtouch-failed-with-no-output"></a>오류 "mtouch 출력 없이 실패 했습니다."

Mac 용 Xamarin.iOS 및 Visual Studio의 현재 릴리스가 실패할 때 프로젝트 이름을 솔루션이 나 프로젝트가 저장 된 디렉터리에 공백을 포함 합니다.
이 문제를 해결 합니다.


-  프로젝트 또는 저장 된 디렉터리를 모두 공백이 포함 되어 있음을 확인 합니다.
-  "Main 설정" 프로젝트에서에서 프로젝트 이름에 공백이 없는지 확인 합니다.

## <a name="error-the-binary-you-uploaded-was-invalid-a-pre-release-beta-version-of-the-sdk-was-used-to-build-the-application"></a>오류 "업로드 이진 올바르지 않습니다. 시험판 베타 버전의 SDK는 응용 프로그램을 작성 하는 데 사용한 "

Xamarin.iOS 2.0.0 출시 되기 전에 iPad 개발에서 시작 된 프로젝트와 함께이 오류는 주로 발생, 같은 프로그램 Info.plist에서 일부 키를 있을 가능성이 높습니다.

```xml
<key>UIDeviceFamily</key>
       <array>
               <string>1</string>
       </array>
```

Mac 용 Visual Studio에서이 처리 하면에 대 한 자동으로이 키 쌍을 제거 해야 합니다.

## <a name="error-a-pre-release-beta-version-of-the-sdk-was-used-to-build-the-app"></a>오류 "시험판 베타 버전의 SDK는 응용 프로그램을 빌드하려면 사용 했습니다"

(Ed Anuff가 제공)

아래 단계를 수행합니다.

-  SDK 버전 3.2 또는 iTunes iPhone 빌드에에서 연결 하는 변경은 버립니다 업로드 시 3.2 보다 작은 SDK 버전을 사용 하 여 빌드된 iPad 호환 앱 보고 하기 때문에
-  프로젝트에 대 한 사용자 지정 Info.plist 만들고 그 안에 3.0 MinimumOSVersion를 명시적으로 설정 합니다.   Xamarin.iOS 설정한 MinimumOSVersion 3.2 값이 재정의 됩니다.   이렇게 하지 않으면 응용 프로그램 iPhone에서 실행할 수 없게 됩니다.
-  다시 작성, zip 및 업로드 iTunes에 연결합니다.

## <a name="unhandled-exception-systemexception-failed-to-find-selector-someselector-on-type"></a>처리 되지 않은 예외: System.Exception: 선택기 someSelector를 찾지 못했습니다. {type}에

이 예외는 다음 세 가지 중 하나로 인해.


1.  Objective C 런타임으로 선택기 메서드에 해당 하는 [내보내기] 특성을 적용 하지 않고 제공
1.  전체 링크를 활성화 하 고 [Preserve] 특성 [내보내기] ed 메서드에 적용 되지 않습니다.
1.  상속 된 형식의 private 메서드를 [내보내기] 특성을 적용 했습니다.


## <a name="mainwindowxibdesignercs-file-is-not-updated"></a>MainWindow.xib.designer.cs 파일이 업데이트 되지 않습니다.

새 프로젝트에서 MainWindow.xib.designer 파일을 사용 하 여 MainWindow.xib 파일 그룹화 되어 Xamarin Studio 2.4 버그가 했습니다. 즉, 해당 특정 파일에 대 한 디자이너 코드를 업데이트 합니다.

해당 기본 제공 업데이트 프로그램에서 사용할 수 있는 Mac에 대 한 버전의 Visual Studio에서이 문제는 해결 되므로 새 버전을 사용 하 여 확인 하세요.

기존 프로젝트 (삭제 하지 않고) 제거 하 여 해결할 수 있습니다는 xib 및 디자이너 파일 백업 후 추가 합니다. 다시 파일을 올바르게 그룹화 해야이 있습니다.

## <a name="uialertview-or-uiactionsheet-vanish-after-being-created"></a>만든 후 UIAlertView 또는 UIActionSheet 사라집니다.

다음과 같은 일부 코드를 가정해 봅니다.

```csharp
var actionSheet = new UIActionSheet ("My ActionSheet", null, null, "OK", null){
   Style = UIActionSheetStyle.Default
};

actionSheet.Clicked += delegate (sender, args){
    Console.WriteLine ("Clicked on item {0}", args.ButtonIndex);
};
```

함수에서 임시 변수로 거주 하 고 "actionSheet" 개체 및 가비지 수집을 종료 하도록 개체가 가비지 수집에 대 한 대상이 되는 함수가 종료 되는 즉시 합니다.

이 문제를 해결 하려면 "actionSheet" 어딘가에 메서드 외부를 초과 하 여 메서드에 유지 됩니다에 대 한 참조를 유지 해야 합니다.

## <a name="project-always-runs-in-the-ipad-simulator"></a>프로젝트 항상 실행 iPad 시뮬레이터에서

IPhone SDK 4.0 installer 2 Sdk-iPad 으로만 이동 가능한 앱을 빌드하기 위한 3.2 SDK와 문서 iPhone 및 범용 앱 용 4.0 SDK를 설치 합니다. 또한만 iPad를 시뮬레이트하는 3.2 시뮬레이터에서 및 4.0 시뮬레이트하는 시뮬레이터를 iPhone 또는 iPhone 4도 설치 합니다. Sdk 및 시뮬레이터에 대 한 모든 이전 제거 됩니다.

Mac iPhone 프로젝트 빌드 옵션에 대 한 visual Studio 응용 프로그램 작성 사용 되는 SDK 버전에 대 한 설정이 포함 됩니다. 이 설정을 확인할 수 있습니다 **프로젝트 옵션에는 빌드-> iPhone 빌드->** 합니다.

Mac 용 Visual Studio에서 새 프로젝트의 기본 SDK 설정으로 가장 오래 된 설치 된 SDK를 사용 하 고 지정 하는 SDK가 없는 경우 Mac 용 Visual Studio 합니다 가장 가까운 응용 프로그램을 찾을 수 있습니다. 이 프로젝트는 항상 최신 SDK requre 있도록 수행 되었습니다. 그러나이 현재 발생 되 고 있는 3.2 SDK에서 사용 된-줄어들고 결과적 iPad 시뮬레이터를 사용 하 고 있습니다.

이 4.0 SDK를 사용 하 여 수정 하려면로 이동 **프로젝트 옵션에는 빌드-> iPhone 빌드->**> SDK 값 드롭다운 상자를 사용 하 여 "4.0"을로 변경 합니다. 각 구성 및 플랫폼 조합, 패널의 맨 위에 있는 드롭다운 목록 사용 하 여 액세스에 대해이 작업을 수행 해야 합니다.

SDK 버전 "최소 OS 버전" 설정과 일치 하지 않습니다.
이 값 SDK 버전 값과 일치 하지 않아도-이전 운영 체제에 없거나 런타임 OS 버전 확인 합니다.를 사용 하 여 새로운 기능의 사용을 보호 하는 Api만을 사용 하는 SDK 보다 오래 되었을 수 있는 앱을 설치 합니다 한 운영 체제의 최소 버전에 영향을 주므로 ks 합니다. 응용 프로그램을 테스트 하는 가장 오래 된 OS 버전으로 설정 해야 합니다.

또한는 **프로젝트 iPhone 시뮬레이터 대상->**> 메뉴 선택 시뮬레이터는 프로젝트를 실행/디버깅 때 기본적으로 사용 되는 데 사용할 수 있습니다. 또한는 **실행으로 실행->**> 메뉴에서 수행 하는 특정 시뮬레이터에서 앱을 선택 하는 데 사용 수

## <a name="ibtool-returns-error-133"></a>ibtool은 133 오류를 반환합니다.

즉, XCode 4가 설치 되어 있다고 합니다.   XCode 4에서 도구 ibtool 사라졌습니다, 그리고 독립 실행형 도구로 XIB 파일을 편집할 수는 없습니다.

인터페이스 작성기를 사용 하려는 경우 설치 [XCode 계열 3](http://connect.apple.com/cgi-bin/WebObjects/MemberSite.woa/wa/getSoftware?bundleID=20792), Apple의 웹 사이트에서 사용할 수 있습니다.

## <a name="cant-create-display-binding-for-mime-type-applicationvndapple-wbrinterface-builder"></a>"Mime 형식에 대 한 디스플레이 바인딩을 만들 수 없습니다: application/vnd.apple-<wbr/>인터페이스 작성기"

이 오류는 비 iPhone 프로젝트에서 iPhone UI를 만들려고 하는 경우에 발생 합니다. IPhone/iPad 솔루션으로 시작 하 여, 방금 iPhone/iPad 아닌 프로젝트에 iPhone UI 요소를 추가할 수 없으면 있는지 확인 합니다.

## <a name="startup-crash-when-executing-inside-the-ios-simulator"></a>IOS 시뮬레이터 내에서 실행 하는 경우 시작 충돌

런타임 크래시 (SIGSEGV)는 다음과 같은 스택 추적와 함께 시뮬레이터 내 발생 하는 경우:

```csharp
  at (wrapper managed-to-native) System.Reflection.Assembly.GetTypes (System.Reflection.Assembly,bool)
  at MonoTouch.ObjCRuntime.Runtime.RegisterAssembly (System.Reflection.Assembly)
  at (wrapper runtime-invoke) <Module>.runtime_invoke_void_object (object,intptr,intptr,intptr)
```
....then 시뮬레이터 응용 프로그램 디렉터리에 하나 이상의 유효 하지 않은 어셈블리를 있을 가능성이 큽니다. 이러한 어셈블리는 Apple iOS 시뮬레이터 추가 하 고 업데이트 파일에 있지만 삭제 되지 않아야 하므로 있습니다 수 있습니다. 이런 것이 간편한 해결책 다음 시뮬레이터 메뉴에서 "다시 설정 하 고 콘텐츠 및 설정..."를 선택 하는 것입니다.   

**경고:** 시뮬레이터에서 모든 파일, 응용 프로그램 및 데이터 제거 합니다.   다음 응용 프로그램을 실행할 때 Mac 용 Visual Studio 시뮬레이터에 배포 됩니다 고 충돌이 발생할 수 없는 오래 되 고 유효 하지 않은 어셈블리 됩니다.

## <a name="simulator-hangs-during-application-installation"></a>응용 프로그램을 설치 하는 동안 시뮬레이터 중지

이 응용 프로그램 이름 포함 하는 경우에 발생할 수는 '.' (점) 이름에 있습니다.
수 있습니다 (예: 장치의 경우) 다른 많은 경우에 작동 하는 경우에 CFBundleExecutable-에 실행 파일 이름으로 없습니다.

 * "값 이름의 모든 확장명 포함 되지 않습니다." - [http://developer.apple.com/library/mac/documentation/General/Reference/InfoPlistKeyReference/InfoPlistKeyReference.pdf](http://developer.apple.com/library/mac/documentation/General/Reference/InfoPlistKeyReference/InfoPlistKeyReference.pdf)

## <a name="error-custom-attribute-type-0x43-is-not-supported-when-double-clicking-xib-files"></a>오류: "사용자 지정 특성 유형 0x43 지원 되지 않습니다" 두 번.xib 파일을 클릭 하는 경우

이 환경 변수를 올바르게 설정 되어.xib 파일을 열려고 시도 하 여 발생 합니다. Mac/Xamarin.iOS에 대 한 Visual Studio의 일반적인 사용으로 발생 하지 않는 이벤트 및 /Applications에서 Mac 용 Visual Studio를 다시 열어 문제가 해결 됩니다.

업데이트 하려고 시도할 때의 소프트웨어 및이 오류 메시지가 나타나면 하십시오 전자 메일 *support@xamarin.com*

## <a name="application-runs-on-simulator-but-fails-on-device"></a>응용 프로그램 시뮬레이터에서 실행 되지만 장치에서 실패

이 문제는 여러 형식으로 나타날 수와 항상 일관 된 오류를 생성 하지 않습니다. 응용 프로그램에는.xib 있으면 있는지 확인은 **빌드 작업** 는.xib 설정 되어 **InterfaceDefinition**합니다. .Xibs에 대 한 기본 빌드 작업입니다.

빌드 작업을 확인 하려면.xib 파일을 마우스 오른쪽 단추로 클릭 하 고 선택 **빌드 작업**합니다.


## <a name="systemnotsupportedexception-no-data-is-available-for-encoding-437"></a>System.NotSupportedException: 데이터가 없는 ´ ü ± 437 인코딩

양식에서 오류가 발생할 수를 포함 하는 타사 라이브러리 Xamarin.iOS 앱에서 "System.NotSupportedException: 데이터가 없는 ´ ü ± 437 인코딩" 컴파일하고 응용 프로그램을 실행 하려고 할 때입니다. 예를 들어 라이브러리와 같은 `Ionic.Zip.ZipFile`, 작업 중이 예외를 throw 할 수 있습니다.

Xamarin.iOS 프로젝트에 대 한 옵션을 열어이 해결할 수 있습니다 **iOS 빌드** > **국제화** 및 검사는 **서쪽** 국제화 합니다.



<a name="Can't_upgrade_to_Indie/Business_from_Trial_Account" />


## <a name="cant-upgrade-to-indiebusiness-from-trial-account"></a>평가판 계정에서 Indie/비즈니스로 업그레이드할 수 없습니다.

최근에 Xamarin.iOS를 구입 하 고 이전에 Xamarin.iOS 평가판을 시작 하는 경우 Visual Studio가 Mac 또는 Visual Studio에 대 한 선택이 라이선스 변경 하려면 다음 단계를 완료 해야 합니다.

-  Mac/Visual Studio 용 Visual Studio를 닫습니다.
-  Windows에 대 한 모든 파일을 Mac에서 ~/Library/MonoTouch 또는 %PROGRAMDATA%\MonoTouch\License\에서 제거 합니다.
-  Mac/Visual Studio 용 Visual Studio를 다시 엽니다 및 Xamarin.iOS 프로젝트 빌드


## <a name="receiving-activation-incomplete-error-message"></a>'활성화에 불완전 한 경우' 오류 메시지

이 문제는 Visual Studio 용 Xamarin.iOS를 사용 하는 경우에 발생할 수 있습니다. 이 문제를 해결 하려면 보내 주십시오 로그를 다음 위치에서 [ contact@xamarin.com ](mailto:contact@xamarin.com)합니다.

-  로그 위치: %LocalAppData%/Xamarin/Logs
