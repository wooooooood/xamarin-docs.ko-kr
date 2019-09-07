---
title: 콘솔 앱에 대 한 Xamarin.ios 바인딩 사용
description: Xamarin.ios를 사용 하 여 헤드리스 콘솔 앱을 만들어 기본 macOS Api에 액세스 합니다.
ms.prod: xamarin
ms.assetid: 9E52B4CC-F530-4B3E-984A-7F5719A6D528
ms.technology: xamarin-mac
author: migueldeicaza
ms.author: miguel
ms.date: 07/27/2018
ms.openlocfilehash: a38543d575f655a3b1b70ff94eece7fef1bf2d40
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769899"
---
# <a name="xamarinmac-bindings-in-console-apps"></a>콘솔 앱의 xamarin.ios 바인딩

에서 C# Apple 네이티브 api 중 일부를 사용 하 여를 사용 하 &ndash; C#는 사용자 인터페이스가 &ndash; 없는 헤드리스 응용 프로그램을 빌드하는 몇 가지 시나리오가 있습니다.

Mac 응용 프로그램에 대 한 프로젝트 템플릿에는를 `NSApplication.Init()` 호출 하는 호출이 `NSApplication.Main(args)`포함 되며, 일반적으로 다음과 같이 표시 됩니다.

```csharp
static class MainClass {
    static void Main (string [] args)
    {
        NSApplication.Init ();
        NSApplication.Main (args);
    }
}
```

에 대 `Init` 한 호출은 xamarin.ios 런타임을 준비 하 고,에 대 `Main(args)` 한 호출은 cocoa 응용 프로그램 main 루프를 시작 하 여 키보드 및 마우스 이벤트를 수신 하 고 응용 프로그램의 주 창을 표시 하도록 응용 프로그램을 준비 합니다.   또한를 `Main` 호출 하면 cocoa 리소스를 찾고, toplevel 창을 준비 하 고, 프로그램이 응용 프로그램 번들의 일부가 될 것으로 예상 합니다 ( `.app` 확장명을 사용 하 여 디렉터리에 배포 되는 프로그램 및 매우 구체적인 레이아웃).

헤드리스 응용 프로그램은 사용자 인터페이스가 필요 하지 않으며 응용 프로그램 번들의 일부로 실행할 필요가 없습니다.

## <a name="creating-the-console-app"></a>콘솔 앱 만들기

따라서 일반 .NET 콘솔 프로젝트 형식으로 시작 하는 것이 좋습니다.

다음 몇 가지 작업을 수행 해야 합니다.

- 빈 프로젝트를 만듭니다.
- Xamarin.ios 라이브러리를 참조 합니다.
- 관리 되지 않는 종속성을 프로젝트에 가져옵니다.

이러한 단계는 아래에 자세히 설명 되어 있습니다.

### <a name="create-an-empty-console-project"></a>빈 콘솔 프로젝트 만들기

새 .NET 콘솔 프로젝트를 만들어 .net core가 아닌 .NET이 고 .net core .net이 아닌 .net core를 사용 하 여 .net core를 실행 합니다.

### <a name="reference-the-xamarinmac-library"></a>Xamarin.ios 라이브러리 참조

코드를 컴파일하려면이 디렉터리에서 `Xamarin.Mac.dll` 어셈블리를 참조 해야 합니다.`/Library/Frameworks/Xamarin.Mac.framework/Versions/Current//lib/x86_64/full`

이렇게 하려면 프로젝트 참조로 이동 하 여 **.Net 어셈블리** 탭을 선택 하 고 **찾아보기** 단추를 클릭 하 여 파일 시스템에서 파일을 찾습니다.  위의 경로로 이동한 다음 해당 디렉터리에서 **xamarin.ios** 를 선택 합니다.

이렇게 하면 컴파일 시 Cocoa Api에 대 한 액세스를 제공 합니다.   이 시점에서 파일의 맨 위에 `using AppKit` 를 추가 하 고 메서드를 `NSApplication.Init()` 호출할 수 있습니다.   응용 프로그램을 실행 하려면 하나 이상의 단계가 있습니다.

### <a name="bring-the-unmanaged-support-library-into-your-project"></a>관리 되지 않는 지원 라이브러리를 프로젝트로 가져오기

응용 프로그램을 실행 하기 전에 `Xamarin.Mac` 지원 라이브러리를 프로젝트로 가져와야 합니다.   이렇게 하려면 프로젝트에 새 파일을 추가 하 고 (프로젝트 옵션에서 **추가**를 선택 하 고 **기존 파일 추가**) 다음 디렉터리로 이동 합니다.

`/Library/Frameworks/Xamarin.Mac.framework/Versions/Current/lib`

여기에서 **libxammac. dylib**파일을 선택 합니다.   복사, 연결 또는 이동 중에서 선택할 수 있습니다.   개인적으로 연결 하는 것이 아니라 복사도 작동 합니다.    그런 다음, 파일을 선택 하 고 속성 패드에서 (속성 패드가 표시 되지 않는 경우 속성 **> 속성** 을 선택 하 여 보기 >) **빌드** 섹션으로 이동 하 고 **출력 디렉터리로 복사** 설정을 **최신으로 복사**로 설정 합니다.

이제 Xamarin.ios 응용 프로그램을 실행할 수 있습니다.

Bin 디렉터리의 결과는 다음과 같이 표시 됩니다.

```
Xamarin.Mac.dll
Xamarin.Mac.pdb
consoleapp.exe
consoleapp.pdb
libxammac.dylib
```

이 앱을 실행 하려면 동일한 디렉터리에 모든 파일이 필요 합니다.

## <a name="building-a-standalone-application-for-distribution"></a>배포를 위한 독립 실행형 응용 프로그램 빌드

단일 실행 파일을 사용자에 게 배포 하는 것이 좋습니다.  이렇게 하려면 `mkbundle` 도구를 사용 하 여 다양 한 파일을 자체 포함 된 실행 파일로 전환할 수 있습니다.

먼저 응용 프로그램이 컴파일되고 실행 되는지 확인 합니다.   결과가 만족 스 러 우면 명령줄에서 다음 명령을 실행할 수 있습니다.

```
$ mkbundle --simple -o /tmp/consoleapp consoleapp.exe --library libxammac.dylib --config /Library/Frameworks/Mono.framework/Versions/Current/etc/mono/config --machine-config /Library/Frameworks/Mono.framework/Versions/Current//etc/mono/4.5/machine.config
[Output from the bundling tool]
$ _
```

위의 명령줄 호출에서 옵션 `-o` 을 사용 하 여 생성 된 출력을 지정 합니다 .이 경우에는를 전달 `/tmp/consoleapp`했습니다.   이 응용 프로그램은 이제 배포할 수 있고 Mono 또는 Xamarin.ios에 대 한 외부 종속성이 없는 독립 실행형 응용 프로그램입니다. 완전히 자체 포함 된 실행 파일입니다.

명령줄에서 사용할 **machine.config** 파일 및 시스템 차원의 라이브러리 매핑 구성 파일을 수동으로 지정 했습니다.   모든 응용 프로그램에는 필요 하지 않지만 .NET의 추가 기능을 사용 하는 경우 사용 되므로 번들로 묶는 것이 편리 합니다.

## <a name="project-less-builds"></a>프로젝트 없는 빌드

자체 포함 된 Xamarin.ios 응용 프로그램을 만드는 데 전체 프로젝트가 필요 하지는 않지만 간단한 Unix 메이크파일을 사용 하 여 작업을 완료할 수도 있습니다.   다음 예제에서는 간단한 명령줄 응용 프로그램에 대해 메이크파일을 설정 하는 방법을 보여 줍니다.

```
XAMMAC_PATH=/Library/Frameworks/Xamarin.Mac.framework/Versions/Current//lib/x86_64/full/
DYLD=/Library/Frameworks/Xamarin.Mac.framework/Versions/Current//lib
MONODIR=/Library/Frameworks/Mono.framework/Versions/Current/etc/mono

all: consoleapp.exe

consoelapp.exe: consoleapp.cs Makefile
    mcs -g -r:$(XAMMAC_PATH)/Xamarin.Mac.dll consoleapp.cs
    
run: consoleapp.exe
    MONO_PATH=$(XAMMAC_PATH) DYLD_LIBRARY_PATH=$(DYLD) mono --debug consoleapp.exe $(COMMAND)

bundle: consoleapp.exe
    mkbundle --simple consoleapp.exe -o ncsharp -L $(XAMMAC_PATH) --library $(DYLD)/libxammac.dylib --config $(MONODIR)/config --machine-config $(MONODIR)/4.5/machine.config
```

위의 `Makefile` 에서는 세 가지 대상을 제공 합니다.

- `make`프로그램을 빌드합니다.
- `make run`현재 디렉터리에서 프로그램을 빌드하고 실행 합니다.
- `make bundle`자체 포함 된 실행 파일을 만듭니다.
