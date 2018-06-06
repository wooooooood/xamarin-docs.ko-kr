---
title: Xamarin.Mac 발생 한 경고
description: 이 문서에서는 Xamarin.Mac 응용 프로그램에서 알림이 있는 작업을 설명합니다. 만들기 및 C# 코드에서 발생 한 경고를 표시 하 고, 사용자 상호 작용에 응답을 설명 합니다.
ms.prod: xamarin
ms.assetid: F1DB93A1-7549-4540-AD5E-D7605CCD8435
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 1eb781fe02213de6a994f56e321316b93a128b60
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792644"
---
# <a name="alerts-in-xamarinmac"></a>Xamarin.Mac 발생 한 경고

_이 문서에서는 Xamarin.Mac 응용 프로그램에서 알림이 있는 작업을 설명합니다. 만들기 및 C# 코드에서 발생 한 경고를 표시 하 고, 사용자 상호 작용에 응답을 설명 합니다._

를 사용할 때 C# 및.NET Xamarin.Mac 응용 프로그램에서 액세스할 수 있는 동일 하 게 경고 하는 개발자의 작업 *Objective-c* 및 *Xcode* 않습니다. 

경고는 특수 한 유형의 (예: 오류)는 심각한 문제가 발생 하면 나타나는 대화 상자 또는 (예: 파일을 삭제 하려면 준비) 경고로 표시 합니다. 경고 대화 상자 이기 때문에 문제가 닫을 수 전에 사용자 응답도 필요 합니다.

[![](alert-images/alert06.png "예제에서는 경고")](alert-images/alert06.png#lightbox)

이 문서는 기본적인 Xamarin.Mac 응용 프로그램에서 경고를 사용 하겠습니다. 

<a name="Introduction_to_Alerts" />

## <a name="introduction-to-alerts"></a>경고에 대 한 소개

경고는 특수 한 유형의 (예: 오류)는 심각한 문제가 발생 하면 나타나는 대화 상자 또는 (예: 파일을 삭제 하려면 준비) 경고로 표시 합니다. 사용자 작업을 계속할 수 전에 해제할 수 해야 하므로 사용자를 방해 하는 경고, 때문에 반드시 필요 하지 않은 경고를 표시 하지 마십시오.

Apple 다음 지침을 제안 합니다.

- 단지 사용자 정보를 제공 하는 경고를 사용 하지 마십시오.
- 일반적으로 실행 취소할 수 있는 작업에 대 한 경고를 표시 하지 않습니다. 이 경우 데이터가 손실 될 수 있습니다 경우에.
- 경우에 경고의 가치를 다른 UI 요소 또는 메서드를 사용 하 여 표시 하지 마십시오.
- 주의 아이콘은 자주 사용 됩니다.
- 경고 상황이 명확 하 고 간결 하 게 경고 메시지에 설명 합니다.
- 경고 메시지에 설명 하는 동작 기본값 단추 이름이 일치 해야 합니다.

자세한 내용은 참조는 [경고](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAlerts.html#//apple_ref/doc/uid/20000957-CH44-SW1) Apple의 섹션 [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Anatomy_of_an_Alert" />

## <a name="anatomy-of-an-alert"></a>경고의 구조

위에서 언급 한 것 처럼 (예: 저장 되지 않은 파일 닫기) 잠재적인 데이터 손실 경고 또는 심각한 문제가 발생 한 경우 응용 프로그램의 사용자에 게 알림 표시 되어야 합니다. Xamarin.Mac에서 경고가 만들어지고 C# 코드에서는 예:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Critical,
    InformativeText = "We need to save the document here...",
    MessageText = "Save Document",
};
alert.RunModal ();
```

위의 코드는 단일 경고 아이콘, 제목, 경고 메시지 등에 겹쳐 응용 프로그램 아이콘으로 경고를 표시 **확인** 단추:

[![](alert-images/alert01.png "확인 단추를 사용 하는 경고")](alert-images/alert01.png#lightbox)

Apple 경고 사용자 지정 하는 데 사용할 수 있는 몇 가지 속성을 제공 합니다.

- **AlertStyle** 다음 중 하나로 경고의 유형을 정의 합니다.
    - **경고** -중요 하지 않은 현재 또는 임박 했음을 알리는 이벤트를 사용자에 게 경고를 사용 합니다. 기본 스타일입니다.
    - **정보 제공 용 이므로** -현재 또는 임박한 이벤트에 대 한 사용자에 게 경고를 사용 합니다. 현재 사이 차이점이 없고 볼 수 있습니다. 한 **경고** 및 **정보**
    - **중요 한** -심각한 문제가 발생할 이벤트 (예: 파일을 삭제)에 대 한 사용자에 게 경고를 사용 합니다. 이 유형의 경고를 제한적으로 사용 해야 합니다.
- **MessageText** -주 메시지 또는 경고의 제목 이므로 입력 해야 사용자에 게 상황을 신속 하 게 정의 해야 합니다.
- **InformativeText** -상황을 명확 하 게 정의 하 고 사용자에 게 처리 가능한 여러 옵션을 제공 해야 하는 경고의 본문입니다.
- **아이콘** -사용자 지정 아이콘에는 사용자에 게 표시 될 수 있습니다.
- **HelpAnchor** & **ShowsHelp** -HelpBook 응용 프로그램에 연결 하 고 경고에 대 한 도움말을 표시 하는 경고를 허용 합니다.
- **단추** -기본적으로 경고에만 **확인** 단추 하지만 **단추** 컬렉션을 사용 하면 필요에 따라 추가 선택 사항을 추가할 수 있습니다.
- **ShowsSuppressionButton** -경우 `true` 사용자는 자신을 트리거한 이벤트의 후속 발생에 대 한 경고가 표시 되지 않도록 하는 데 사용할 수 있는 확인란을 표시 합니다.
- **AccessoryView** -다른 하위 뷰 추가 하는 등의 추가 정보를 제공 하는 경고를 연결할 수는 **텍스트 필드** 데이터를 입력 합니다. 새 설정 하면 **AccessoryView** 기존 구성을 수정 하거나를 호출 해야는 `Layout()` 메서드를 표시 되는 경고의 레이아웃을 조정 합니다.

<a name="Displaying_an_Alert" />

## <a name="displaying-an-alert"></a>경고를 표시합니다.

두 가지가 서로 다른 경고를 표시, 자유 부동 수 있는지 또는 시트로 합니다. 다음 코드는 자유 부동으로 경고를 표시합니다.

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.RunModal ();
```
이 코드를 실행 하는 경우 다음 내용이 표시 됩니다.

[![](alert-images/alert02.png "간단한 경고")](alert-images/alert02.png#lightbox)

다음 코드를 시트로 동일한 경고를 표시합니다.

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.BeginSheet (this);
```

이 코드를 실행 하는 경우 다음 표시 됩니다.

[![](alert-images/alert03.png "시트로 표시 되는 경고")](alert-images/alert03.png#lightbox)


<a name="Working_with_Alert_Buttons" />

## <a name="working-with-alert-buttons"></a>경고 단추 사용

기본적으로 경고 표시만 **확인** 단추입니다. 그러나 제한이 없습니다를,에 추가 하 여 추가 단추를 만들 수 있습니다는 **단추** 컬렉션입니다. 다음 코드와 자유롭게 이동할 경고를 생성 한 **확인**, **취소** 및 **미정** 단추:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
var result = alert.RunModal ();
```

추가 된 첫 번째 단추 수는 _기본값 단추_ 사용자가 Enter 키를 누를 경우 활성화 되는 합니다. 반환 된 값에는 사용자가 어느 단추를 나타내는 정수 됩니다. 이 경우에는 다음 값 반환 됩니다.

- **정상** -1000입니다.
- **취소** 1001-합니다.
- **아마** 1002-합니다.

코드를 실행 하는 경우 다음 표시 됩니다.

[![](alert-images/alert04.png "세 개의 단추 옵션과 함께 경고")](alert-images/alert04.png#lightbox)

시트로 동일한 경고에 대 한 코드는 다음과 같습니다.

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.BeginSheetForResponse (this, (result) => {
    Console.WriteLine ("Alert Result: {0}", result);
});
```
이 코드를 실행 하는 경우 다음 표시 됩니다.

[![](alert-images/alert05.png "시트로 표시 되는 세 개의 단추 경고")](alert-images/alert05.png#lightbox)

> [!IMPORTANT]
> 경고에 세 개 이상의 단추를 추가 해서는 안 됩니다.

<a name="Showing_the_Suppress_Button" />

## <a name="showing-the-suppress-button"></a>표시 안 함 단추 표시

하는 경우 경고의 `ShowSuppressButton` 속성은 `true`, 경고에는 사용자가을 트리거한 이벤트의 후속 발생에 대 한 경고가 표시 되지 않도록 하는 데 사용할 수 있는 확인란을 표시 합니다. 다음 코드에 자유 부동 경고를 표시 안 함 단추를 표시합니다.

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
var result = alert.RunModal ();
Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
```

하는 경우의 값은 `alert.SuppressionButton.State` 은 `NSCellStateValue.On`, 사용자가 표시 안 함 확인란을 체크 아웃, 되지 않은 다른 합니다.

코드를 실행 하는 경우 다음 표시 됩니다.

[![](alert-images/alert06.png "경고 표시 안 함 단추")](alert-images/alert06.png#lightbox)

시트로 동일한 경고에 대 한 코드는 다음과 같습니다.

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.BeginSheetForResponse (this, (result) => {
    Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
});
```

이 코드를 실행 하는 경우 다음 표시 됩니다.

[![](alert-images/alert07.png "경고 표시 안 함 단추와 시트로 표시")](alert-images/alert07.png#lightbox)

<a name="Adding_a_Custom_SubView" />

## <a name="adding-a-custom-subview"></a>사용자 지정 하위 뷰 추가

경고는 `AccessoryView` 추가 경고를 사용자 지정과 같은 추가 하는 데 사용할 수 있는 속성 한 **텍스트 필드** 사용자 입력에 대 한 합니다. 다음 코드는 추가 된 텍스트 입력된 필드와 부동성 경고를 만듭니다.

```csharp
var input = new NSTextField (new CGRect (0, 0, 300, 20));

var alert = new NSAlert () {
AlertStyle = NSAlertStyle.Informational,
InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.AccessoryView = input;
alert.Layout ();
var result = alert.RunModal ();
Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
```

여기에 키 줄은 `var input = new NSTextField (new CGRect (0, 0, 300, 20));` 하에서는 새 **텍스트 필드** 경고를 추가할 예정 하 합니다. `alert.AccessoryView = input;` 연결 합니다. 어떤는 **텍스트 필드** 경고에 대 한 호출을는 `Layout()` 경고 새 하위 보기에 맞게 크기를 조정 하는 데 필요한 메서드.

코드를 실행 하는 경우 다음 표시 됩니다.

[![](alert-images/alert08.png "코드를 실행 하는 경우 다음 나타납니다.")](alert-images/alert08.png#lightbox)

시트로 동일한 경고는 다음과 같습니다.

```csharp
var input = new NSTextField (new CGRect (0, 0, 300, 20));

var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.AccessoryView = input;
alert.Layout ();
alert.BeginSheetForResponse (this, (result) => {
    Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
});
```

이 코드를 실행 하는 경우 다음 표시 됩니다.

[![](alert-images/alert09.png "사용자 지정 보기와 경고")](alert-images/alert09.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>요약

이 문서는 Xamarin.Mac 응용 프로그램에서 경고 작업에 대해 자세히를 수행 했습니다. 다양 한 유형 및 경고, 만들기 및 경고를 사용자 지정 하는 방법 및 C# 코드에서 경고를 사용 하는 방법의 사용에 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [MacWindows (샘플)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [창 작업](~/mac/user-interface/window.md)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [NSAlert](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSAlert_Class/index.html#//apple_ref/doc/uid/TP40004001)
