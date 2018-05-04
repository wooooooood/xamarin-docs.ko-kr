---
title: MacOS 시작
ms.prod: xamarin
ms.assetid: AE51F523-74F4-4EC0-B531-30B71C4D36DF
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 3620312ff3fbf9d7aa879ae6d318f0b39eec386a
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/03/2018
---
# <a name="getting-started-with-macos"></a>MacOS 시작

## <a name="what-you-will-need"></a>요구 사항

* 지침에 따라는 [Objective-c 시작](~/tools/dotnet-embedding/get-started/objective-c/index.md) 가이드입니다.

## <a name="hello-world"></a>Hello World

첫째, C#에서 간단한 hello world 예제를 빌드하십시오.

### <a name="create-c-sample"></a>C# 예제 만들기

Mac 용 Visual Studio를 열고, 명명 된 새 Mac 클래스 라이브러리 프로젝트 만들기 **csharp에서 hello**, 저장 하 고 **~/Projects/hello-from-csharp**합니다.

코드는 **MyClass.cs** 다음 조각을 파일:

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

프로젝트를 빌드합니다. 결과 어셈블리 다음과 같이 저장 됨 **~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll**합니다.

### <a name="bind-the-managed-assembly"></a>관리 되는 어셈블리 바인딩

관리 되는 어셈블리를 만든 후를 포함 하는.NET 호출 하 여 바인딩하십시오.

에 설명 된 대로 [설치](~/tools/dotnet-embedding/get-started/install/install.md) 가이드, 이렇게 하려면 프로젝트의 빌드 후 단계 또는 수동으로 사용자 지정 MSBuild 대상을 사용 하 여:

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=macOS-modern --abi=x86_64 --outdir=output -c --debug
```

프레임 워크에 배치 됩니다 **~/Projects/hello-from-csharp/output/hello-from-csharp.framework**합니다.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Xcode 프로젝트에서 생성 된 출력을 사용 하 여

Xcode를 열고 새 Cocoa 응용 프로그램을 만듭니다. 이름을 **csharp에서 hello** 선택 하 고는 **Objective-c** 언어입니다.

열기는 **~/Projects/hello-from-csharp/output** 디렉터리 선택 검색기에서 **csharp.framework에서 hello**, Xcode 프로젝트에 끌어서 놓습니다 바로 위에 **hello에서 csharp**  프로젝트의 폴더입니다.

![끌어서 놓기 프레임 워크](macos-images/hello-from-csharp-mac-drag-drop-framework.png)

있는지 확인 **필요한 경우 항목을 복사할** , 팝업 되는 대화 상자에서 선택 되 고 클릭 **마침**합니다.

![필요한 경우 항목을 복사 합니다.](macos-images/hello-from-csharp-mac-copy-items-if-needed.png)

선택 된 **csharp에서 hello** 프로젝트 및 탐색 하는 **csharp에서 hello** 대상의 **일반** 탭 합니다. 에 **포함 된 이진** 섹션을 추가 **hello에서 csharp.framework**합니다.

![포함 된 이진 파일](macos-images/hello-from-csharp-mac-embedded-binaries.png)

열기 **ViewController.m**와 내용을 바꿉니다.

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

![C# 샘플 시뮬레이터에서 실행 하는 hello](macos-images/hello-from-csharp-mac.png)

보다 완벽 하 고 시각적 효과가 적용 된 샘플 [여기](https://github.com/mono/Embeddinator-4000/tree/objc/samples/mac/weather)합니다.
