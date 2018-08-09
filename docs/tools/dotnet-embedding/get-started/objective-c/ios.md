---
title: IOS 시작
description: 이 문서에서는.NET 포함을 사용 하 여 iOS를 사용 하 여 시작 하는 방법을 설명 합니다. 요구 사항에 설명 하 고 관리 되는 어셈블리를 바인딩하고 Xcode 프로젝트에서 출력을 사용 하는 방법을 보여 주기 위해 샘플 앱을 제공 합니다.
ms.prod: xamarin
ms.assetid: D5453695-69C9-44BC-B226-5B86950956E2
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: d61eb8f1ad1def764c8552b2f047aa46cd712018
ms.sourcegitcommit: ef04a4ae1b19c1854a8e4e8315516d4030f4bbd6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39654830"
---
# <a name="getting-started-with-ios"></a>IOS 시작

## <a name="requirements"></a>요구 사항

요구 사항 외에도 우리 [Objective-c로 시작](~/tools/dotnet-embedding/get-started/objective-c/index.md) 도 필요 가이드:

* [Xamarin.iOS 10.11](https://visualstudio.microsoft.com/xamarin/) 이상

## <a name="hello-world"></a>Hello World

먼저, C#에서 간단한 hello world 예제를 빌드하십시오.

### <a name="create-c-sample"></a>C# 샘플 만들기

Mac 용 Visual Studio를 열고, 새 iOS 클래스 라이브러리 프로젝트를 만들고, 이름을 **csharp에서 hello**에 저장 하 고 **~/Projects/hello-from-csharp**합니다.

코드를 대체 합니다 **MyClass.cs** 다음 코드 조각 파일:

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

프로젝트를 빌드하고으로 결과 어셈블리를 저장할 **~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll**합니다.

### <a name="bind-the-managed-assembly"></a>관리 되는 어셈블리 바인딩

관리 되는 어셈블리를 만든 후.NET 포함을 호출 하 여 바인딩하십시오.

에 설명 된 대로 합니다 [설치](~/tools/dotnet-embedding/get-started/install/install.md) 가이드, 이렇게 하려면 프로젝트에서 빌드 후 단계로 사용자 지정 MSBuild 대상으로 또는 수동으로:

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=iOS --outdir=output -c --debug
```

프레임 워크에 들어갈 **~/Projects/hello-from-csharp/output/hello-from-csharp.framework**합니다.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Xcode 프로젝트에서 생성 된 출력을 사용 하 여

Xcode를 열고, 새 iOS 단일 뷰 응용 프로그램을 만들고, 이름을 **csharp에서 hello**를 선택 합니다 **Objective-c로** 언어입니다.

엽니다는 **~/Projects/hello-from-csharp/output** Finder 선택에서에서 디렉터리 **csharp.framework에서 hello**, Xcode 프로젝트에 끌어서 놓습니다 바로 위에 **csharp에서 hello**  프로젝트의 폴더입니다.

![끌어서 놓기 프레임 워크](ios-images/hello-from-csharp-ios-drag-drop-framework.png)

했는지 **필요한 경우 항목 복사** 팝업 대화 상자에서 체크 인 되 고 클릭 **마침**합니다.

![필요한 경우 항목 복사](ios-images/hello-from-csharp-ios-copy-items-if-needed.png)

선택는 **csharp에서 hello** 프로젝트를 이동 합니다 **csharp에서 hello** 대상의 **일반 탭**합니다. 에 **포함 된 이진** 섹션에서 추가할 **csharp.framework에서 hello**합니다.

![포함 된 이진 파일](ios-images/hello-from-csharp-ios-embedded-binaries.png)

열기 **ViewController.m**를 사용 하 여 내용을 바꿉니다.

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

.NET 포함 지원 하지 않습니다 현재 bitcode ios의 경우 일부 Xcode 프로젝트 템플릿을 사용 하도록 설정 됩니다. 

프로젝트 설정에서이 비활성화 합니다.

![Bitcode 옵션](../../images/ios-bitcode-option.png)

마지막으로 Xcode 프로젝트를 실행 하 고 다음과 같이 표시 됩니다.

![Hello C# 샘플 시뮬레이터에서 실행](ios-images/hello-from-csharp-ios.png)
