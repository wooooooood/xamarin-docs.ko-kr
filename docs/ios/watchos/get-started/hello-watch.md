---
title: Hello, watchOS – 연습
description: 이 문서에서는 Xamarin을 사용 하 여 간단한 watchOS 응용 프로그램을 빌드하는 연습을 제공 합니다. Visual Studio와 Mac용 Visual Studio에서 작업 하 고 스토리 보드를 사용 하 여 작업 하 고 코드에서 이벤트에 응답 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: AD1DA488-51AB-420A-A0B7-3AE69A964A40
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 12/14/2016
ms.openlocfilehash: c5527db543a0b0d5218c37f0d75e22afcd59297a
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70293155"
---
# <a name="hello-watchos--walkthrough"></a>Hello, watchOS – 연습

[설정 및 설치](~/ios/watchos/get-started/installation.md)의 단계에 따라 솔루션을 만든 후에는 다음 3 개의 프로젝트가 있습니다.

- 설치 또는 기타 장치 관리 작업에 사용 되는 iOS 부모 앱입니다. (다른 유형의 iOS 확장을 사용 하면 "컨테이너" 앱이 라고도 합니다.) 시청 앱을 사용 하면 사용자가 부모 앱을 **실행 하지 않고도 시청 앱 실행을** 시작할 수 있습니다.
- Watch 앱에 대 한 프로그램 코드를 포함 하는 조사식 확장입니다. 하거나
- Watch에서 렌더링 되는 스토리 보드 및 이미지 리소스를 보유 하는 Watch 앱입니다.

부모 앱에 확장에 대 한 참조가 있고 확장에 조사식 앱 [에 대 한](~/ios/watchos/get-started/project-references.md)참조가 있는지 확인 합니다.

번들 식별자가 \*. watchkitextension \*. watchkitapp 규칙을 따르고 확장 프로그램의 info.plist 파일에 감시 앱의 번들 식별자로 설정 된 **WKApp 번들 ID** 값이 있는지 확인 합니다.

이제 Watch 앱을 실행할 수 있지만, Watch 앱 내에 있는 스토리 보드 파일이 비어 있으므로 알 수 없습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](hello-watch-images/projectstructure.png "솔루션 탐색기")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](hello-watch-images/vs-projectstructure.png "솔루션 탐색기")

-----

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Watch 앱에서 Xcode를 두 번 클릭 하 여 Xamarin iOS Designer를 시작 합니다. (Mac을 사용 하는 경우 마우스 오른쪽 단추를 클릭 하 고 **Interface Builder >를 사용 하 여 열**수도 있습니다.)


1. **도구 상자** 및 **속성** 패드가 표시 되는지 확인 합니다.
1. 인터페이스 컨트롤러를 클릭 하 여 선택 합니다.
1. 인터페이스 컨트롤러의 식별자와 제목을 **interfaceController** 및 **Hi**로 설정 합니다.
1. **클래스가** **InterfaceController** 로 설정 되었는지 확인 합니다.

    ![](hello-watch-images/interfacecontrollerattributes.png "인터페이스 컨트롤러의 식별자와 제목을 interfaceController 및 Hi로 설정 합니다.")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio에서 Xamarin iOS Designer를 사용 하 여 편집 하려면 Watch 앱에서 storyboard를 두 번 클릭 합니다.

1. 속성 창을 엽니다.
1. 클래스를 **InterfaceController**로 변경 합니다.
1. 인터페이스 컨트롤러를 클릭 합니다. 하거나
1. 인터페이스 컨트롤러의 식별자와 제목을 **interfaceController** 및 **Hi**로 설정 합니다.

    ![](hello-watch-images/vs-interfacecontrollerattributes.png "인터페이스 컨트롤러의 식별자와 제목을 interfaceController 및 Hi로 설정 합니다.")

-----


UI를 만듭니다.

1. **도구 상자** 패드에서
1. **단추** 와 **레이블을** 장면에 끌어서 놓습니다.
1. 다음과 같이 컨트롤의 텍스트 및 특성을 설정 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](hello-watch-images/draganddrop.png "다음과 같이 컨트롤의 텍스트 및 특성을 설정 합니다.")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](hello-watch-images/vs-draganddrop.png "다음과 같이 컨트롤의 텍스트 및 특성을 설정 합니다.")

-----

1. 각 컨트롤에 대 한 **속성** 패드에 **이름을** 설정 합니다. 이 예제에서는 및 `myButton` `myLabel`를 사용 했습니다.

1. 스토리 보드에서 단추를 선택 하 고 **속성** 패드의 **이벤트** 목록으로 이동한 다음

1. 를 입력 하 `OnButtonPress` 고 **enter**키를 눌러 새 작업을 만듭니다.
  작업은 목록에 표시 되 고 부분 메서드는에 C#자동으로 생성 됩니다.

![](hello-watch-images/buttonaction.png "단추에 추가 된 OnButtonPress 작업")

스토리 보드를 저장 하면 **InterfaceController.designer.cs** 가 컨트롤 이름 및 동작으로 업데이트 됩니다. 이 파일을 업데이트 한 후에이 파일을 열면가 컨트롤러에 `RegisterAttribute` 해당 하는 방법 및 UI 컨트롤이로 표시 된 C# 인스턴스 `OutletAttribute` 변수에 해당 하는 방법 및 작업이로 태그가 지정 `ActionAttribute`된부분메서드에매핑되는방법에대해알수있습니다.:

```csharp
// WARNING
//
// This file has been generated automatically by Visual Studio for Mac from the outlets and
// actions declared in your storyboard file.
// Manual changes to this file will not be maintained.
//
[Register ("InterfaceController")]
partial class InterfaceController
{
    [Outlet]
    [GeneratedCode ("iOS Designer", "1.0")]
    WatchKit.WKInterfaceButton myButton { get; set; }

    [Outlet]
    [GeneratedCode ("iOS Designer", "1.0")]
    WatchKit.WKInterfaceLabel myLabel { get; set; }

    [Action ("OnButtonPress:")]
    [GeneratedCode ("iOS Designer", "1.0")]
    partial void OnButtonPress (WatchKit.WKInterfaceButton sender);

    void ReleaseDesignerOutlets ()
    {
        if (myButton != null) {
            myButton.Dispose ();
            myButton = null;
        }
        if (myLabel != null) {
            myLabel.Dispose ();
            myLabel = null;
        }
    }
}
```

이제 **InterfaceController.cs** (InterfaceController.designer.cs*아님* )를 열고 다음 코드를 추가 합니다.

```csharp
int clickCount = 0;
partial void OnButtonPress (WatchKit.WKInterfaceButton sender)
{
  var msg = String.Format("Clicked {0} times", ++clickCount);
  myLabel.SetText(msg);
}
```

이 코드는 매우 투명 해야 합니다. 인스턴스 변수 `clickCount` 는 함수가 `OnButtonPress` 호출 될 때마다 증가 합니다. 의 `myLabel` 텍스트는이 횟수를 반영 하도록 변경 됩니다. `myLabel`물론은 XCode에서 만든 콘센트 중 하나의 이름입니다. 함수 `partial` 는 지정한 동작의 이름과 연결 된 함수의 구현입니다.

아직 시작 프로젝트가 아닌 경우

1. 조사식 확장 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **시작 프로젝트로 설정**을 선택 합니다.

1. 배포 대상을 감시 키트 호환 시뮬레이터 이미지 (예: iPhone 6 iOS 8.2)로 설정 합니다.

1. **디버그** 단추를 눌러 빌드 및 시뮬레이터 시작을 트리거합니다.

    [![](hello-watch-images/readytodebug-sml.png "Visual Studio 인터페이스 요소")](hello-watch-images/readytodebug.png#lightbox)

시뮬레이터가 시작 되 면 단추를 눌러 레이블을 늘립니다.
축 하 합니다. 이제 시청 앱을 만들었습니다.

![](hello-watch-images/running.png "시뮬레이터에서 실행 중인 앱")


## <a name="related-links"></a>관련 링크

- [시작 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchkit-gettingstarted)
- [설정 및 설치](~/ios/watchos/get-started/installation.md)
- [첫 번째 Watch 앱 비디오](https://blog.xamarin.com/your-first-watch-kit-app/)
