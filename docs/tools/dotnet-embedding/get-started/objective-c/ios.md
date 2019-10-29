---
title: IOS 시작
description: 이 문서에서는 iOS를 사용 하 여 .NET 포함을 시작 하는 방법을 설명 합니다. 이 샘플에서는 요구 사항에 대해 설명 하 고 관리 되는 어셈블리를 바인딩하고 Xcode 프로젝트에서 출력을 사용 하는 방법을 보여 주는 샘플 앱을 제공 합니다.
ms.prod: xamarin
ms.assetid: D5453695-69C9-44BC-B226-5B86950956E2
author: davidortinau
ms.author: daortin
ms.date: 11/14/2017
ms.openlocfilehash: 5697d20077b746d9d33db985111c2d04908d5d01
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029754"
---
# <a name="getting-started-with-ios"></a>IOS 시작

## <a name="requirements"></a>요구 사항

[목표 시작-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) 가이드의 요구 사항 외에도 다음이 필요 합니다.

* [Xamarin.ios 10.11](https://visualstudio.microsoft.com/xamarin/) 이상

## <a name="hello-world"></a>Hello World

먼저에서 C#간단한 hello 세계 예제를 빌드합니다.

### <a name="create-c-sample"></a>샘플 C# 만들기

Mac용 Visual Studio를 열고 새 iOS 클래스 라이브러리 프로젝트를 만든 다음 이름을 **hello-csharp**로 하 고 **~/Projects/hello-from-csharp**에 저장 합니다.

**MyClass.cs** 파일의 코드를 다음 코드 조각으로 바꿉니다.

```csharp
using UIKit;
public class MyUIView : UITextView
{
    public MyUIView ()
    {
        Text = "Hello from C#";
    }
}
```

프로젝트를 빌드하고 결과 어셈블리가 **~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll**로 저장 됩니다.

### <a name="bind-the-managed-assembly"></a>관리 되는 어셈블리 바인딩

관리 되는 어셈블리가 있으면 .NET 포함을 호출 하 여 바인딩합니다.

[설치](~/tools/dotnet-embedding/get-started/install/install.md) 가이드에 설명 된 대로이 작업은 프로젝트의 빌드 후 단계, 사용자 지정 MSBuild 대상 또는 수동으로 수행할 수 있습니다.

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=iOS --outdir=output -c --debug
```

프레임 워크는 **~/Projects/hello-from-csharp/output/hello-from-csharp.framework**에 배치 됩니다.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Xcode 프로젝트에 생성 된 출력 사용

Xcode를 열고, 새 iOS 단일 뷰 응용 프로그램을 만들고, **-csharp에서 이름을 hello**로, **목표-C** 언어를 선택 합니다.

Finder에서 **~/Projects/hello-from-csharp/output** 디렉터리를 열고, **hello-csharp 프레임 워크**를 선택 하 고, Xcode 프로젝트로 끌어서 프로젝트의 **hello--csharp** 폴더 바로 위에 놓습니다.

![프레임 워크 끌어서 놓기](ios-images/hello-from-csharp-ios-drag-drop-framework.png)

표시 되는 대화 상자에서 **필요한 경우 항목 복사** 가 선택 되어 있는지 확인 하 고 **마침**을 클릭 합니다.

![필요한 경우 항목 복사](ios-images/hello-from-csharp-ios-copy-items-if-needed.png)

**-Csharp** 프로젝트를 선택 하 고 **hello-Csharp** 대상의 **일반 탭**으로 이동 합니다. 포함 된 **이진** 섹션에서 **hello--csharp 프레임 워크**를 추가 합니다.

![포함 된 이진 파일](ios-images/hello-from-csharp-ios-embedded-binaries.png)

**Viewcontroller. m**을 열고 내용을 다음으로 바꿉니다.

```objective-c
#import "ViewController.h"
#include "hello-from-csharp/hello-from-csharp.h"

@interface ViewController ()
@end

@implementation ViewController
- (void)viewDidLoad {
    [super viewDidLoad];

    MyUIView *view = [[MyUIView alloc] init];
    view.frame = CGRectMake(0, 200, 200, 200);
    [self.view addSubview: view];
}
@end
```

.NET 포함은 현재 일부 Xcode 프로젝트 템플릿에 대해 사용 하도록 설정 된 iOS에서 bitcode를 지원 하지 않습니다. 

프로젝트 설정에서 사용 하지 않도록 설정 합니다.

![Bitcode 옵션](../../images/ios-bitcode-option.png)

마지막으로 Xcode 프로젝트를 실행 하 고 다음과 같은 항목이 표시 됩니다.

![시뮬레이터에서 C# 실행 되는 샘플의 Hello](ios-images/hello-from-csharp-ios.png)
