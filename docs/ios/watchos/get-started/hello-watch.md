---
title: "Hello, 조사식"
description: "Xamarin 및 watchOS 시작"
ms.topic: article
ms.prod: xamarin
ms.assetid: AD1DA488-51AB-420A-A0B7-3AE69A964A40
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/14/2016
ms.openlocfilehash: 88f9a86173756738d44f099b13177489226fa0e1
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="hello-watch"></a>Hello, 조사식

단계에 따라 솔루션을 만든 후 [설정 및 설치](~/ios/watchos/get-started/installation.md), 프로젝트 3 해야 합니다.

- 설치 프로그램 또는 다른 장치에서 관리 작업에 사용 되는 iOS 부모 앱입니다. (다른 형식의 확장 iOS와이 종종 라고 "Container" 응용 프로그램입니다.) Watch 앱을 실행 하지 않고 Watch 앱을 시작 하는 사용자에 대 한 가능한 됩니다 **적이** 부모 응용 프로그램 실행
- 조사식 응용 프로그램에 대 한 프로그램 코드를 포함 하는 조사식 확장 및
- Watch 앱을 시계에 렌더링 하는 스토리 보드 및 이미지 리소스를 보유 합니다.

있는지 여부를 확인 하면 [참조가](~/ios/watchos/get-started/project-references.md): 부모 응용 프로그램에는 확장에 대 한 참조가 있는지 그리고 확장에 Watch 앱에 대 한 참조입니다.

번들 식별자 따르는지 확인는 \*.watchkitextension \*.watchkitapp 규칙 확장의 Info.plist 파일 것에 있고 **WKApp 번들 ID** 값의 번들 식별자로 설정 Watch 앱입니다.

이제 Watch 앱을 실행할 수 있어야 하지만 Watch 앱 내에서 스토리 보드 파일 비어 있으므로 확인할 수 없게 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-watch-images/projectstructure.png "솔루션 탐색기")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-watch-images/vs-projectstructure.png "솔루션 탐색기")

-----

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
    
Xamarin iOS 디자이너를 시작 하려면 Watch 앱에서 Interface.storyboard을 두 번 클릭 (Mac을 사용 하는 경우 스키마 및 **프로그램 > Xcode 인터페이스 작성기**)


1.  확인 된 **도구 상자** 및 **속성** 패드 표시 되는
1.  인터페이스 컨트롤러를 선택 하려면 클릭
1.  식별자 및 인터페이스 컨트롤러의 제목을 설정 **interfaceController** 및 **Hi 조사식**,
1.  확인 된 **클래스** 로 설정 된 **InterfaceController**

    ![](hello-watch-images/interfacecontrollerattributes.png "식별자 및 인터페이스 컨트롤러의 제목을 interfaceController 및 Hi 조사식을로 설정")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin ios Visual Studio의 디자이너 편집 하려면 Watch 앱에서 Interface.storyboard을 두 번 클릭:

1.  속성 창을; 열으십시오
1.  클래스를 변경 **InterfaceController**;
1.  인터페이스 컨트롤러;를 클릭 합니다. 및
1.  식별자 및 인터페이스 컨트롤러의 제목을 설정 **interfaceController** 및 **Hi 조사식**합니다.

    ![](hello-watch-images/vs-interfacecontrollerattributes.png "식별자 및 인터페이스 컨트롤러의 제목을 interfaceController 및 Hi 조사식을로 설정")

-----


UI를 만듭니다.

1. **도구 상자** 패드,
1. 끌어서 놓기는 **단추** 및 **레이블** 장면에 끌어다 및
1. 텍스트와 같이 컨트롤의 속성을 설정 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](hello-watch-images/draganddrop.png "텍스트와 같이 컨트롤의 속성을 설정 합니다.")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](hello-watch-images/vs-draganddrop.png "텍스트와 같이 컨트롤의 속성을 설정 합니다.")

-----

1. 설정의 **이름** 에 **속성** 패드 각 컨트롤에 대 한 합니다. 이 예에서는 사용 했습니다 `myButton` 및 `myLabel`합니다.

1. 스토리 보드에 있는 단추를 선택 하 고에 이동 된 **속성** 패드의 **이벤트** 목록 다음

1. 새 **동작** 입력 하 여 `OnButtonPress` 한 키를 눌러 **Enter**합니다.
  작업 목록에 나타나고 부분 메서드는 C#에서 자동으로 생성 됩니다.

![](hello-watch-images/buttonaction.png "OnButtonPress 동작이 단추에 추가")

스토리 보드를 저장 한 후에 **InterfaceController.designer.cs** 는 컨트롤 이름 및 동작으로 업데이트... 업데이트 한 후이 파일을 열 경우 확인할 수 있습니다 어떻게는 `RegisterAttribute` 컨트롤러 및 UI 컨트롤 C# 인스턴스 변수로 표시에 해당 하는 방법에 해당 하는 `OutletAttribute` 어떻게 매핑되는지 작업 부분 메서드를 태그로 지정 하 고는 `ActionAttribute`:

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

이제 열 **InterfaceController.cs** (*하지* InterfaceController.designer.cs) 하 고 다음 코드를 추가 합니다.

```csharp
int clickCount = 0;

partial void OnButtonPress (WatchKit.WKInterfaceButton sender)
{
  var msg = String.Format("Clicked {0} times", ++clickCount);
  myLabel.SetText(msg);
}

```

이 코드는 투명 하 게 유지 해야 합니다.: 인스턴스 변수 `clickCount` 은 될 때마다 증가 하는 함수 `OnButtonPress` 호출 됩니다. 텍스트 `myLabel` ;이 수를 반영 하도록 변경 `myLabel`은 물론, XCode에서 만든 콘센트 중 하나의 이름입니다. `partial` 함수는 지정한 작업의 이름과 연결 된 함수를 구현 합니다.

없으면 이미 시작 프로젝트

1. 조사식 확장 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **시작 프로젝트로 설정**,

1. (예: iPhone 6 iOS 8.2), 조사식 키트 호환 시뮬레이터 이미지를 배포 대상을 설정합니다

1. 키를 눌러는 **디버그** 를 빌드 및 시뮬레이터 시작을 트리거하는 단추입니다.

    [ ![](hello-watch-images/readytodebug-sml.png "Visual Studio 인터페이스 요소")](hello-watch-images/readytodebug.png)

시뮬레이터가 시작 되 면 단추 레이블이 증가를 키를 누릅니다.
축 하 합니다, Watch 앱 구현할 수!

![](hello-watch-images/running.png "시뮬레이터에서 실행 중인 앱")


## <a name="related-links"></a>관련 링크

- [시작 하기 (샘플)](https://developer.xamarin.com/samples/monotouch/WatchKit/GettingStarted/)
- [설정 및 설치](~/ios/watchos/get-started/installation.md)
- [첫 번째 Watch 앱 비디오](http://blog.xamarin.com/your-first-watch-kit-app/)
