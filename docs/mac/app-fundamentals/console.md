---
title: Xamarin.Mac 바인딩을 사용 하 여 콘솔 앱을 위한
description: Xamarin.Mac을 사용 하 여 네이티브 macOS Api에 액세스 하려면 헤드리스 콘솔 앱을 만듭니다.
ms.prod: xamarin
ms.assetid: 9E52B4CC-F530-4B3E-984A-7F5719A6D528
ms.technology: xamarin-mac
author: migueldeicaza
ms.author: miguel
ms.date: 07/27/2018
ms.openlocfilehash: 135ef06cd044235023c3acc970c8e7a33f144b47
ms.sourcegitcommit: d0da5ce4158239abd2b36f67550e9b475055766a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39320832"
---
# <a name="xamarinmac-bindings-in-console-apps"></a>콘솔 응용 프로그램에서 Xamarin.Mac 바인딩

일부의 시나리오가 헤드리스 응용 프로그램을 만드는 C#의 Apple 네이티브 Api 중 일부를 사용 하려는 &ndash; 사용자 인터페이스가 없는 것 &ndash; C#을 사용 합니다.

Mac 응용 프로그램에 대 한 프로젝트 템플릿 호출을 포함할 `NSApplication.Init()` 에 대 한 호출 뒤에 `NSApplication.Main(args)`, 일반적으로 다음과 같습니다.

```csharp
static class MainClass {
    static void Main (string [] args)
    {
        NSApplication.Init ();
        NSApplication.Main (args);
    }
}
```

에 대 한 호출 `Init` Xamarin.Mac 런타임에 대 한 호출을 준비 `Main(args)` Cocoa 응용 프로그램 기본 루프 키보드 및 마우스 이벤트를 수신 하 고 응용 프로그램의 주 창을 표시 하려면 응용 프로그램 준비를 시작 합니다.   에 대 한 호출 `Main` Cocoa 리소스를 찾고, 최상위 창 준비 하려고도 하며 응용 프로그램 번들의 일부로 프로그램 (디렉터리에 배포 하는 프로그램을 `.app` 확장 하 고 매우 구체적인 레이아웃).

헤드리스 응용 프로그램에 사용자 않아도 인터페이스 및 응용 프로그램 번들의 일부로 실행할 필요가 없습니다.

## <a name="creating-the-console-app"></a>콘솔 앱 만들기

따라서 일반.NET 콘솔 프로젝트 형식으로 시작 하는 것이 좋습니다.

다음을 수행 해야 합니다.

- 빈 프로젝트를 만듭니다.
- Xamarin.Mac.dll 라이브러리를 참조 합니다.
- 프로젝트에 관리 되지 않는 종속성을 제공 합니다.

이러한 단계는 아래에서 자세히 설명 되어 있습니다.

### <a name="create-an-empty-console-project"></a>빈 콘솔 프로젝트 만들기

새.NET 콘솔 프로젝트를 만드는.NET 및.NET Core가 아니라, Xamarin.Mac.dll으로 제대로 작동 하지 않으면.NET Core 런타임만 Mono 런타임과 함께 실행 되는 선택 되어 있는지 확인 합니다.

### <a name="reference-the-xamarinmac-library"></a>Xamarin.Mac 라이브러리 참조

코드를 컴파일하려면 하려는 참조를 `Xamarin.Mac.dll` 이 디렉터리에서 어셈블리: `/Library/Frameworks/Xamarin.Mac.framework/Versions/Current//lib/x86_64/full`

이 위해 선택 된 프로젝트 참조로 이동 합니다 **.NET 어셈블리** 탭을 클릭 합니다 **찾아보기** 를 파일 시스템에서 파일을 찾습니다.  위 경로로 이동 하 고 다음을 선택 합니다 **Xamarin.Mac.dll** 해당 디렉터리에서.

컴파일 타임에 이렇게에 액세스할 수 Cocoa Api로 제공 됩니다.   이 시점에서 추가할 수 있습니다 `using AppKit` 호출 프로그램과 파일의 맨 위에 `NSApplication.Init()` 메서드.   응용 프로그램을 실행 하기 전에 자세한 단계가 하나만 있습니다.

### <a name="bring-the-unmanaged-support-library-into-your-project"></a>관리 되지 않는 지원 라이브러리 프로젝트로 가져오기

상태로 전환 해야 하는 응용 프로그램을 실행 하기 전에 `Xamarin.Mac` 지원 라이브러리를 프로젝트에 있습니다.   이렇게 하려면 새 파일을 프로젝트에 추가 합니다 (프로젝트 옵션에서 선택 **추가**를 차례로 **기존 파일 추가**)이 디렉터리로 이동:

`/Library/Frameworks/Xamarin.Mac.framework/Versions/Current/lib`

여기에서 파일을 선택 **libxammac.dylib**합니다.   다양 한 복사, 연결 또는 이동할 제공 됩니다.   개인적으로 필자는 연결 하지만 복사도 작동 합니다.    파일을 선택 해야 합니다 및 속성 패드 (선택 **보기 > 패드 > 속성** 속성 패드 표시 되지 않으면)로 이동 합니다 **빌드** 섹션 및 설정의 **출력 디렉터리로 복사 디렉터리** 로 설정 **변경 된 내용만 복사**합니다.

이제 Xamarin.Mac 응용 프로그램을 실행할 수 있습니다.

Bin 디렉터리의 결과 다음과 같습니다.

```
Xamarin.Mac.dll
Xamarin.Mac.pdb
consoleapp.exe
consoleapp.pdb
libxammac.dylib
```

이 앱을 실행 하려면 같은 디렉터리에 해당 하는 모든 파일을 해야 합니다.

## <a name="building-a-standalone-application-for-distribution"></a>배포에 대 한 독립 실행형 응용 프로그램 빌드

단일 실행 파일의 사용자에 게 배포할 수도 있습니다.  이 위해 사용할 수는 `mkbundle` 도구를 다양 한 파일을 자체 포함 된 실행 파일을 변환 합니다.

우선, 응용 프로그램 컴파일 및 실행 되는지 확인 합니다.   결과 만족 했으면 다음 명령을 명령줄에서 실행할 수 있습니다.

```
$ mkbundle --simple -o /tmp/consoleapp consoleapp.exe --library libxammac.dylib --config /Library/Frameworks/Mono.framework/Versions/Current/etc/mono/config --machine-config /Library/Frameworks/Mono.framework/Versions/Current//etc/mono/4.5/machine.config
[Output from the bundling tool]
$ _
```

위의 명령줄 호출 옵션에에서 `-o` 는 생성 된 출력을 지정 하는 데,이 경우 전달 `/tmp/consoleapp`합니다.   이 지금 배포할 수 있는 독립 실행형 응용 프로그램 및 Mono에서 Xamarin.Mac 외부 종속성이 없는는 완전히 자체 포함 된 실행 파일입니다.

수동으로 지정한 명령줄 합니다 **machine.config** 사용할 파일 및 시스템 라이브러리 매핑 구성 파일입니다.   모든 응용 프로그램의 경우 필요 하지 않습니다 이지만, 번들로 묶는 편리한.NET의 많은 기능을 사용 하는 경우 사용 되는

## <a name="project-less-builds"></a>프로젝트를 사용 하지 않는 빌드

자체 포함 된 Xamarin.Mac 응용 프로그램을 만드는 전체 프로젝트 않아도, 작업을 수행 하려면 간단한 Unix 메이크파일을 사용할 수도 있습니다.   다음 예제에서는 간단한 명령줄 응용 프로그램에 대 한 메이크파일의 설정할 수 있습니다 하는 방법을 보여 줍니다.

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

위의 `Makefile` 세 가지 목표를 제공 합니다.

- `make` 프로그램을 빌드하는
- `make run` 빌드를 현재 디렉터리에 프로그램을 실행 합니다.
- `make bundle` 자체 포함 된 실행 파일을 만듭니다.
