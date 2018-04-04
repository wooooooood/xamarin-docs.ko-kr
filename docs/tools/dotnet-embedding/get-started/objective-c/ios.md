---
title: IOS를 시작 하기
ms.prod: xamarin
ms.assetid: D5453695-69C9-44BC-B226-5B86950956E2
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 34afdd9e91ebfbe7ad57c7eec6ba7f05fff1a2aa
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="getting-started-with-ios"></a>IOS를 시작 하기


## <a name="requirements"></a>요구 사항

요구 사항 뿐 아니라 우리의 [Objective-c 시작](~/tools/dotnet-embedding/get-started/objective-c/index.md) 가이드도 필요 합니다.

* [Xamarin.iOS 10.11](https://www.visualstudio.com/xamarin/) 이상 버전

## <a name="hello-world"></a>Hello World

첫 번째 C#에서 간단한 hello world 예제를 제작 해 보겠습니다.

### <a name="create-c-sample"></a>C# 예제 만들기

Mac 용 Visual Studio를 열고, 새 iOS 클래스 라이브러리 프로젝트 만들기, 이름을 `hello-from-csharp`, 저장 하 고 `~/Projects/hello-from-csharp`합니다.

코드는 `MyClass.cs` 다음 조각을 파일:

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

프로젝트를 빌드, 결과 어셈블리 다음과 같이 저장 됨 `~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll`합니다.

### <a name="bind-the-managed-assembly"></a>관리 되는 어셈블리 바인딩

관리 되는 어셈블리에 대 한 네이티브 프레임 워크를 만들도록 embeddinator를 실행 합니다.

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=iOS --outdir=output -c --debug
```

프레임 워크에 들어갈 `~/Projects/hello-from-csharp/output/hello-from-csharp.framework`합니다.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Xcode 프로젝트에서 생성 된 출력을 사용 하 여

새 iOS 단일 보기 응용 프로그램을 만들를 이름을 지정 하 고 Xcode를 열고 `hello-from-csharp` 선택 하 고는 **Objective-c** 언어입니다.

열기는 `~/Projects/hello-from-csharp/output` 디렉터리 선택 검색기에서 `hello-from-csharp.framework`, Xcode 프로젝트에 끌어서 놓습니다 바로 위에 `hello-from-csharp` 프로젝트의 폴더입니다.

! [끌어서 놓기 프레임 워크] Images/hello-from-csharp-ios-drag-drop-framework.png)

확인 `Copy items if needed` , 팝업 되는 대화 상자에서 선택 되 고 클릭 `Finish`합니다.

![필요한 경우 항목을 복사 합니다.](ios-images/hello-from-csharp-ios-copy-items-if-needed.png)

선택 된 `hello-from-csharp` 프로젝트 및 탐색 하는 `hello-from-csharp` 대상의 **일반 탭**합니다. 에 **포함 된 이진** 섹션을 추가 `hello-from-csharp.framework`합니다.

![포함 된 이진 파일](ios-images/hello-from-csharp-ios-embedded-binaries.png)

ViewController.m를 열고 사용 하 여 콘텐츠를 바꿉니다.

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

마지막으로 Xcode 프로젝트를 실행 하 고 다음과 같이 표시 됩니다.

![C# 샘플 시뮬레이터에서 실행 하는 hello](ios-images/hello-from-csharp-ios.png)
