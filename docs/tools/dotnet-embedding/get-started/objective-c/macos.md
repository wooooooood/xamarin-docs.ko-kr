---
title: MacOS를 사용 하 여 시작
description: 이 문서에서는 설명 하는 방법을 포함 하는.NET을 사용 하 여 macOS를 사용 하 여 시작 합니다. 요구 사항에 설명 하 고 관리 되는 어셈블리를 바인딩하고 Xcode 프로젝트에서 생성 된 출력을 사용 하는 방법을 보여주는 샘플 응용 프로그램을 제공 합니다.
ms.prod: xamarin
ms.assetid: AE51F523-74F4-4EC0-B531-30B71C4D36DF
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: 3de47aa57df29f52f71508977ae017dff009c282
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121570"
---
# <a name="getting-started-with-macos"></a>MacOS를 사용 하 여 시작

## <a name="what-you-will-need"></a>요구 사항

* 지침에 따라 합니다 [Objective-c 시작](~/tools/dotnet-embedding/get-started/objective-c/index.md) 가이드입니다.

## <a name="hello-world"></a>Hello World

먼저, C#에서 간단한 hello world 예제를 빌드하십시오.

### <a name="create-c-sample"></a>C# 샘플 만들기

Mac 용 Visual Studio를 열고, 라는 새 Mac 클래스 라이브러리 프로젝트를 만듭니다 **csharp에서 hello**에 저장 하 고 **~/Projects/hello-from-csharp**합니다.

코드를 대체 합니다 **MyClass.cs** 다음 코드 조각 파일:

```csharp
using AppKit;
public class MyNSView : NSTextView
{
    public MyNSView ()
    {
        Value = "Hello from C#";
    }
}
```

프로젝트를 빌드합니다. 으로 결과 어셈블리를 저장할 **~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll**합니다.

### <a name="bind-the-managed-assembly"></a>관리 되는 어셈블리 바인딩

관리 되는 어셈블리를 만든 후.NET 포함을 호출 하 여 바인딩하십시오.

에 설명 된 대로 합니다 [설치](~/tools/dotnet-embedding/get-started/install/install.md) 가이드, 이렇게 하려면 프로젝트에서 빌드 후 단계로 사용자 지정 MSBuild 대상으로 또는 수동으로:

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=macOS-modern --abi=x86_64 --outdir=output -c --debug
```

프레임 워크에 들어갈 **~/Projects/hello-from-csharp/output/hello-from-csharp.framework**합니다.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Xcode 프로젝트에서 생성 된 출력을 사용 하 여

Xcode를 열고 새 Cocoa 응용 프로그램을 만듭니다. 이름을 **csharp에서 hello** 선택 합니다 **Objective C** 언어입니다.

엽니다는 **~/Projects/hello-from-csharp/output** Finder 선택에서에서 디렉터리 **csharp.framework에서 hello**, Xcode 프로젝트에 끌어서 놓습니다 바로 위에 **csharp에서 hello**  프로젝트의 폴더입니다.

![끌어서 놓기 프레임 워크](macos-images/hello-from-csharp-mac-drag-drop-framework.png)

했는지 **필요한 경우 항목 복사** 팝업 대화 상자에서 체크 인 되 고 클릭 **마침**합니다.

![필요한 경우 항목 복사](macos-images/hello-from-csharp-mac-copy-items-if-needed.png)

선택는 **csharp에서 hello** 프로젝트를 이동 합니다 **csharp에서 hello** 대상의 **일반** 탭 합니다. 에 **포함 된 이진** 섹션에서 추가할 **csharp.framework에서 hello**합니다.

![포함 된 이진 파일](macos-images/hello-from-csharp-mac-embedded-binaries.png)

열기 **ViewController.m**를 사용 하 여 내용을 바꿉니다.

```objc
#import "ViewController.h"

#include "hello-from-csharp/hello-from-csharp.h"

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
    MyNSView *view = [[MyNSView alloc] init];
    view.frame = CGRectMake(0, 200, 200, 200);
    [self.view addSubview: view];
}

@end
```

마지막으로 Xcode 프로젝트를 실행 하 고 다음과 같이 표시 됩니다.

![Hello C# 샘플 시뮬레이터에서 실행](macos-images/hello-from-csharp-mac.png)

보다 완전 하 고 시각적 효과가 적용 된 샘플 [는](https://github.com/mono/Embeddinator-4000/tree/objc/samples/mac/weather)합니다.
