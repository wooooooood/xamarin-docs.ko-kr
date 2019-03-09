---
title: Hello, watchOS – 연습
description: 이 문서는 Xamarin을 사용 하 여 간단한 watchOS 응용 프로그램을 구축 하는 연습을 제공 합니다. Mac 용 Visual Studio 및 Visual Studio에서 작업, 스토리 보드를 사용 하 여 작업, 코드의 이벤트에 응답 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: AD1DA488-51AB-420A-A0B7-3AE69A964A40
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 12/14/2016
ms.openlocfilehash: de04a2d7f42ec36c464c75ced73bf8f8029ec1da
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57672614"
---
# <a name="hello-watchos--walkthrough"></a>Hello, watchOS – 연습

단계를 수행 하는 솔루션을 만든 후 [설정 및 설치](~/ios/watchos/get-started/installation.md), 3 프로젝트를 해야 합니다.

- 설치 프로그램 또는 다른 장치 관리 작업에 사용 되는 iOS 부모 앱입니다. (다른 유형의 iOS 확장을 사용 하 여이 라고 "컨테이너" 앱입니다.) Watch 앱으로 실행 하지 않고 Watch 앱을 시작 하는 사용자에 대 한 가능한 됩니다 **적이** 부모 응용 프로그램 실행
- Watch 앱;에 대 한 프로그램 코드를 포함 하는 조사식 확장 및
- Watch 앱을 시계에서 렌더링 되는 스토리 보드 및 이미지 리소스를 보유 합니다.

확인 프로그램 [참조가](~/ios/watchos/get-started/project-references.md): 부모 앱 확장에 대 한 참조에 있고 확장 Watch 앱에 대 한 참조입니다.

번들 식별자에 따르는지 확인 합니다 \*.watchkitextension \*.watchkitapp 규칙 확장의 Info.plist 파일 하에 있고 **WKApp 번들 ID** 의 번들 식별자로 값 설정 Watch 앱입니다.

이제 Watch 앱을 실행할 수 있어야 있지만 Watch 앱 내에서 스토리 보드 파일 비어 있으므로 알 수 없습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](hello-watch-images/projectstructure.png "솔루션 탐색기")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](hello-watch-images/vs-projectstructure.png "솔루션 탐색기")

-----

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)
    
Xamarin iOS 디자이너를 시작 하려면 Watch 앱에서 Interface.storyboard 두 번 클릭 (Mac을 사용 하는 경우 또한 단추로 클릭 하 고 **연결 > Xcode Interface Builder**)


1.  확인 합니다 **도구 상자** 하 고 **속성** 패드 표시 됩니다.
1.  인터페이스 컨트롤러를 선택 하려면 클릭
1.  식별자 및 인터페이스 컨트롤러의 제목 설정 **interfaceController** 하 고 **Hi 조사식**,
1.  확인 합니다 **클래스** 로 설정 된 **InterfaceController**

    ![](hello-watch-images/interfacecontrollerattributes.png "식별자 및 인터페이스 컨트롤러의 제목 interfaceController 및 Hi 조사식 설정")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Xamarin iOS Visual Studio의 디자이너를 사용 하 여 편집할 Watch 앱에서 Interface.storyboard 두 번 클릭 합니다.

1.  속성 창, 열기
1.  클래스를 변경 **InterfaceController**;
1.  인터페이스 컨트롤러;를 클릭 합니다. 및
1.  식별자 및 인터페이스 컨트롤러의 제목 설정 **interfaceController** 하 고 **Hi 조사식**합니다.

    ![](hello-watch-images/vs-interfacecontrollerattributes.png "식별자 및 인터페이스 컨트롤러의 제목 interfaceController 및 Hi 조사식 설정")

-----


UI를 만듭니다.

1. **도구 상자** 패드
1. 끌어서 놓기는 **단추** 와 **레이블을** 장면에 및
1. 텍스트와 같이 컨트롤의 속성을 설정 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](hello-watch-images/draganddrop.png "표시 된 것 처럼 컨트롤의 속성을 확인 하 고 텍스트를 설정 합니다.")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](hello-watch-images/vs-draganddrop.png "표시 된 것 처럼 컨트롤의 속성을 확인 하 고 텍스트를 설정 합니다.")

-----

1. 설정 합니다 **이름** 에 **속성** 패드 각 컨트롤에 대 한 합니다. 이 예제에서 사용 했습니다 `myButton` 고 `myLabel`입니다.

1. 스토리 보드에 있는 단추를 선택 하 고 이동 합니다 **속성** 방향 패드 **이벤트** 다음 목록

1. 새 **동작** 입력 하 여 `OnButtonPress` 키를 눌러 **Enter**합니다.
  작업 목록에 표시 됩니다 및 부분 메서드에 자동으로 생성 됩니다 C#입니다.

![](hello-watch-images/buttonaction.png "단추에 OnButtonPress 작업 추가")

스토리 보드를 저장 합니다 **InterfaceController.designer.cs** 컨트롤 이름 및 작업을 사용 하 여 업데이트... 업데이트 한 후이 파일을 열면 볼 수 있습니다 하는 방법을 `RegisterAttribute` 컨트롤러 및 UI 컨트롤에 해당 하는 방법에 해당 C# 인스턴스 변수 표시는 `OutletAttribute` 동작 합니다 태그로지정부분메서드에매핑되는방식및`ActionAttribute`:

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

이제 열 **InterfaceController.cs** (*하지* InterfaceController.designer.cs) 다음 코드를 추가 합니다.

```csharp
int clickCount = 0;
partial void OnButtonPress (WatchKit.WKInterfaceButton sender)
{
  var msg = String.Format("Clicked {0} times", ++clickCount);
  myLabel.SetText(msg);
}
```

이 코드는 투명 하 게 유지 해야 합니다.: 인스턴스 변수에 `clickCount` 가 증가 될 때마다 함수 `OnButtonPress` 라고 합니다. 텍스트 `myLabel` 이 count;을 반영 하도록 변경 됩니다 `myLabel`은 물론 XCode에서 만든 출 선 중 하나의 이름입니다. `partial` 함수는 지정한 작업의 이름과 연결 된 함수를 구현 합니다.

없으면 이미 시작 프로젝트

1. 조사식 확장 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **시작 프로젝트로 설정**,

1. 배포 대상 집합을 조사 키트 호환 시뮬레이터 이미지로 (예: iPhone 6 iOS 8.2)

1. 키를 눌러 합니다 **디버그** 를 빌드하고 시뮬레이터 시작을 트리거하는 단추를 합니다.

    [![](hello-watch-images/readytodebug-sml.png "Visual Studio 인터페이스 요소")](hello-watch-images/readytodebug.png#lightbox)

시뮬레이터 시작 되 면 레이블을 증가 단추를 누릅니다.
축, 갖다 Watch 앱!

![](hello-watch-images/running.png "시뮬레이터에서 실행 중인 앱")


## <a name="related-links"></a>관련 링크

- [Getting Started (샘플)](https://developer.xamarin.com/samples/monotouch/WatchKit/GettingStarted/)
- [설정 및 설치](~/ios/watchos/get-started/installation.md)
- [첫 번째 Watch 앱 비디오](https://blog.xamarin.com/your-first-watch-kit-app/)
