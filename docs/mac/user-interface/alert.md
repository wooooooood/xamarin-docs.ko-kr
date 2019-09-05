---
title: Xamarin.ios의 경고
description: 이 문서에서는 Xamarin.ios 응용 프로그램에서 경고를 사용 하는 방법을 설명 합니다. 코드에서 C# 경고를 만들고 표시 하 고 사용자 상호 작용에 응답 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: F1DB93A1-7549-4540-AD5E-D7605CCD8435
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: 1e2ad12e7dc52b44bda079340638298b87ac5f65
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291209"
---
# <a name="alerts-in-xamarinmac"></a>Xamarin.ios의 경고

_이 문서에서는 Xamarin.ios 응용 프로그램에서 경고를 사용 하는 방법을 설명 합니다. 코드에서 C# 경고를 만들고 표시 하 고 사용자 상호 작용에 응답 하는 방법을 설명 합니다._

Xamarin.ios 응용 프로그램 C# 에서 및 .net을 사용 하는 경우 *목표-C* 및 *Xcode* 에서 작업 하는 개발자와 동일한 경고에 액세스할 수 있습니다. 

경고는 심각한 문제 (예: 오류) 나 경고 (예: 파일 삭제 준비)가 발생 한 경우 표시 되는 특별 한 유형의 대화 상자입니다. 경고는 대화 상자 이므로 닫아야 하기 전에 사용자 응답이 필요 합니다.

[![](alert-images/alert06.png "예제 경고")](alert-images/alert06.png#lightbox)

이 문서에서는 Xamarin.ios 응용 프로그램에서 경고로 작업 하는 기본 사항을 설명 합니다. 

<a name="Introduction_to_Alerts" />

## <a name="introduction-to-alerts"></a>경고 소개

경고는 심각한 문제 (예: 오류) 나 경고 (예: 파일 삭제 준비)가 발생 한 경우 표시 되는 특별 한 유형의 대화 상자입니다. 사용자가 작업을 계속 진행 하기 전에 사용자가 해제 해야 하므로 경고는 사용자가 중단 되므로 반드시 필요한 경우에만 경고를 표시 하지 않습니다.

Apple에서 다음 지침을 제안 합니다.

- 사용자 정보를 제공 하기 위해 경고만 사용 하지 마세요.
- 일반적이 고 실행 취소할 수 있는 작업에 대해서는 경고를 표시 하지 않습니다. 이로 인해 데이터가 손실 될 수 있는 경우에도 마찬가지입니다.
- 상황에 따라 경고가 발생 하는 경우 다른 UI 요소나 메서드를 사용 하 여 표시 하지 마십시오.
- 주의 아이콘은 매우 자주 사용 해야 합니다.
- 경고 메시지를 명확 하 고 간략하게 경고 상황을 설명 합니다.
- 기본 단추 이름은 경고 메시지에 설명 된 작업과 일치 해야 합니다.

자세한 내용은 Apple [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) 의 [경고](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAlerts.html#//apple_ref/doc/uid/20000957-CH44-SW1) 섹션을 참조 하세요.

<a name="Anatomy_of_an_Alert" />

## <a name="anatomy-of-an-alert"></a>경고 분석

위에서 설명한 것 처럼 심각한 문제가 발생 하거나 데이터 손실 (예: 저장 되지 않은 파일 닫기)에 대 한 경고로 인해 응용 프로그램 사용자에 게 경고를 표시 해야 합니다. Xamarin.ios에서 코드에 C# 경고가 생성 됩니다. 예를 들면 다음과 같습니다.

```csharp
var alert = new NSAlert () {
  AlertStyle = NSAlertStyle.Critical,
  InformativeText = "We need to save the document here...",
  MessageText = "Save Document",
};
alert.RunModal ();
```

위의 코드는 경고 아이콘, 제목, 경고 메시지 및 단일 **확인** 단추를 사용 하 여 응용 프로그램 아이콘을 표시 하는 경고를 표시 합니다.

[![](alert-images/alert01.png "확인 단추가 있는 경고")](alert-images/alert01.png#lightbox)

Apple은 경고를 사용자 지정 하는 데 사용할 수 있는 몇 가지 속성을 제공 합니다.

- **Alertstyle** 은 경고의 유형을 다음 중 하나로 정의 합니다.
  - **경고** -중요 하지 않은 현재 또는 임박 이벤트를 사용자에 게 경고 하는 데 사용 됩니다. 기본 스타일입니다.
  - **정보** -현재 또는 임박 이벤트에 대해 사용자에 게 경고 하는 데 사용 됩니다. 현재 **경고** 와 **정보** 간에 표시 되는 차이가 없습니다.
  - **중요** -발생 한 이벤트의 심각한 결과 (예: 파일 삭제)에 대해 사용자에 게 경고 하는 데 사용 됩니다. 이 유형의 경고는 자주 사용 해야 합니다.
- **MessageText** -경고의 기본 메시지 또는 제목 이며 사용자에 게 상황을 신속 하 게 정의 해야 합니다.
- **InformativeText** -상황을 명확 하 게 정의 하 고 사용자에 게 작동 가능한 옵션을 제공 해야 하는 경고의 본문입니다.
- **아이콘** -사용자 지정 아이콘을 사용자에 게 표시할 수 있습니다.
- ShowsHelp-경고를 응용 프로그램 helpbook에 연결 하 고 경고에 대 한 도움말을 표시할 수 있습니다.  & 
- **단추** -기본적으로 경고에는 **확인** 단추만 있지만 **Buttons** 컬렉션을 사용 하면 필요에 따라 선택 항목을 더 추가할 수 있습니다.
- **ShowsSuppressionButton** - `true` 사용자가 경고를 트리거한 이벤트의 후속 발생에 대 한 경고를 표시 하지 않는 데 사용할 수 있는 확인란을 표시 합니다.
- **AccessoryView** -다른 하위 뷰를 경고에 연결 하 여 데이터 입력에 대 한 **텍스트 필드** 를 추가 하는 등의 추가 정보를 제공할 수 있습니다. 새 **AccessoryView** 를 설정 하거나 기존 항목을 수정 하는 경우 `Layout()` 메서드를 호출 하 여 경고의 표시 레이아웃을 조정 해야 합니다.

<a name="Displaying_an_Alert" />

## <a name="displaying-an-alert"></a>경고 표시

경고는 두 가지 방법으로 사용할 수 있습니다. 즉, 두 가지 방법으로 사용할 수 있습니다. 다음 코드는 경고를 자유 부동으로 표시 합니다.

```csharp
var alert = new NSAlert () {
  AlertStyle = NSAlertStyle.Informational,
  InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
  MessageText = "Alert Title",
};
alert.RunModal ();
```

이 코드가 실행 되 면 다음이 표시 됩니다.

[![](alert-images/alert02.png "간단한 경고")](alert-images/alert02.png#lightbox)

다음 코드는 시트와 동일한 경고를 표시 합니다.

```csharp
var alert = new NSAlert () {
  AlertStyle = NSAlertStyle.Informational,
  InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
  MessageText = "Alert Title",
};
alert.BeginSheet (this);
```

이 코드가 실행 되 면 다음이 표시 됩니다.

[![](alert-images/alert03.png "시트로 표시 되는 경고")](alert-images/alert03.png#lightbox)


<a name="Working_with_Alert_Buttons" />

## <a name="working-with-alert-buttons"></a>경고 단추 사용

기본적으로 경고는 **확인** 단추만 표시 합니다. 그러나 **단추** 컬렉션에 추가 하 여 추가 단추를 만들 수 있습니다. 다음 코드에서는 **확인**, **취소** **및 다음을 수행 하 여** 자유 부동 경고를 만듭니다.

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

처음 추가 된 단추는 사용자가 Enter 키를 누를 때 활성화 되는 _기본 단추가_ 됩니다. 반환 된 값은 사용자가 누른 단추를 나타내는 정수입니다. 이 경우 다음 값이 반환 됩니다.

- **확인** -1000.
- **취소** -1001.
- 1002 일 **수도** 있습니다.

코드를 실행 하는 경우 다음이 표시 됩니다.

[![](alert-images/alert04.png "세 가지 단추 옵션이 있는 경고")](alert-images/alert04.png#lightbox)

시트와 동일한 경고에 대 한 코드는 다음과 같습니다.

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

이 코드가 실행 되 면 다음이 표시 됩니다.

[![](alert-images/alert05.png "시트로 표시 되는 세 개의 단추 경고")](alert-images/alert05.png#lightbox)

> [!IMPORTANT]
> 세 개 이상의 단추를 경고에 추가 하면 안 됩니다.

<a name="Showing_the_Suppress_Button" />

## <a name="showing-the-suppress-button"></a>표시 안 함 단추 표시

경고 `ShowSuppressButton` 속성이 인 `true`경우 경고는 사용자가 경고를 트리거한 이벤트의 후속 발생에 대 한 경고를 표시 하지 않는 데 사용할 수 있는 확인란을 표시 합니다. 다음 코드는 표시 안 함 단추가 있는 자유 부동 경고를 표시 합니다.

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

의 `alert.SuppressionButton.State` 값이 `NSCellStateValue.On`이면 사용자는 표시 안 함 확인란을 선택 하 고 그렇지 않은 경우에는 그렇지 않습니다.

코드가 실행 되 면 다음이 표시 됩니다.

[![](alert-images/alert06.png "표시 안 함 단추가 있는 경고")](alert-images/alert06.png#lightbox)

시트와 동일한 경고에 대 한 코드는 다음과 같습니다.

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

이 코드가 실행 되 면 다음이 표시 됩니다.

[![](alert-images/alert07.png "표시 안 함 단추가 시트로 표시 되는 경고")](alert-images/alert07.png#lightbox)

<a name="Adding_a_Custom_SubView" />

## <a name="adding-a-custom-subview"></a>사용자 지정 하위 뷰 추가

경고에는 `AccessoryView` 경고를 추가로 사용자 지정 하 고 사용자 입력에 대 한 **텍스트 필드** 와 같은 항목을 추가 하는 데 사용할 수 있는 속성이 있습니다. 다음 코드는 추가 된 텍스트 입력 필드를 사용 하 여 자유 부동 경고를 만듭니다.

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

여기 `var input = new NSTextField (new CGRect (0, 0, 300, 20));` 에서 키 줄은 경고를 추가할 새 **텍스트 필드** 를 만듭니다. `alert.AccessoryView = input;`이는 **텍스트 필드** 를 경고에 연결 하 고 `Layout()` 메서드를 호출 하 여 새 하위 뷰의 크기를 조정 하는 데 필요 합니다.

코드를 실행 하는 경우 다음이 표시 됩니다.

[![](alert-images/alert08.png "코드를 실행 하는 경우 다음이 표시 됩니다.")](alert-images/alert08.png#lightbox)

시트와 동일한 경고는 다음과 같습니다.

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

이 코드를 실행 하는 경우 다음이 표시 됩니다.

[![](alert-images/alert09.png "사용자 지정 보기를 사용 하는 경고")](alert-images/alert09.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Xamarin.ios 응용 프로그램에서 경고를 사용 하는 방법을 자세히 살펴봅니다. 경고의 다양 한 유형 및 사용, 경고를 만들고 사용자 지정 하는 방법 및 코드에서 C# 경고를 사용 하는 방법을 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [MacWindows (샘플)](https://docs.microsoft.com/samples/xamarin/mac-samples/macwindows)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Windows 작업](~/mac/user-interface/window.md)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [NSAlert](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSAlert_Class/index.html#//apple_ref/doc/uid/TP40004001)
