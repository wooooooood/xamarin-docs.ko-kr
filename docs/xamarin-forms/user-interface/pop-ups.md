---
title: 팝업 표시
description: Xamarin.ios는 경고, 작업 시트 및 프롬프트와 같은 세 가지 팝업 사용자 인터페이스 요소를 제공 합니다. 이 문서에서는 경고, 작업 시트 및 프롬프트 Api를 사용 하 여 사용자에 게 간단한 질문을 하 고, 작업을 안내 하 고, 프롬프트를 표시 하는 대화 상자를 표시 하는 방법을 보여줍니다.
ms.prod: xamarin
ms.assetid: 46AB0D5E-0025-4A8A-9D00-3E66C3D0BA2E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/17/2020
ms.openlocfilehash: c71153cdaa94a7983b89968abc828011a648f2b1
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306628"
---
# <a name="display-pop-ups"></a>팝업 표시

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-pop-ups)

경고를 표시 하 여 사용자에 게 선택 하거나 프롬프트를 표시 하는 것은 일반적인 UI 작업입니다. Xamarin.ios에는 [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*), [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*)및 `DisplayPromptAsync`을 통해 사용자와 상호 작용 하기 위한 [`Page`](xref:Xamarin.Forms.Page) 클래스에 대 한 세 가지 메서드가 있습니다. 이 두 가지 항목은 각 플랫폼에 적절한 네이티브 컨트롤을 사용하여 렌더링됩니다.

## <a name="display-an-alert"></a>경고 표시

Xamarin.Forms에서 지원하는 모든 플랫폼에는 사용자에게 경고를 표시하고 간단한 질문을 하는 모달 팝업이 있습니다. Xamarin.Forms에서 이러한 경고를 표시하려면 [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*)에서 [`Page`](xref:Xamarin.Forms.Page) 메서드를 사용합니다. 다음 코드 줄은 사용자에게 표시되는 간단한 메시지입니다.

```csharp
await DisplayAlert ("Alert", "You have been alerted", "OK");
```

![](pop-ups-images/alert.png "Alert Dialog with One Button")

이 예제에서는 사용자로부터 정보를 수집하지 않습니다. 경고는 모달 형식으로 표시되고 해제되면 사용자는 애플리케이션과 상호 작용을 계속합니다.

[`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) 메서드는 또한 두 개의 단추를 표시하고 `boolean`을 반환하여 사용자의 응답을 캡처하는 데 사용할 수 있습니다. 경고에서 응답을 가져오려면 두 단추 모두에 텍스트를 적용하고 메서드를 `await`합니다. 사용자가 옵션 중 하나를 선택한 후 대답이 사용자 코드로 반환됩니다. 아래 샘플 코드의 `async` 및 `await` 키워드를 참고하세요.

```csharp
async void OnAlertYesNoClicked (object sender, EventArgs e)
{
  bool answer = await DisplayAlert ("Question?", "Would you like to play a game", "Yes", "No");
  Debug.WriteLine ("Answer: " + answer);
}
```

[![DisplayAlert](pop-ups-images/alert2-sml.png "두 단추가 있는 경고 대화 상자")](pop-ups-images/alert2.png#lightbox "두 단추가 있는 경고 대화 상자")

## <a name="guide-users-through-tasks"></a>사용자에 게 작업 안내

[UIActionSheet](https://developer.apple.com/library/ios/documentation/uikit/reference/uiactionsheet_class/Reference/Reference.html)는 iOS의 일반적인 UI 요소입니다. Xamarin.Forms [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) 메서드를 통해 Android 및 UWP에서 기본 대체 항목을 렌더링하는 플랫폼 간 앱에 이 컨트롤을 포함시킬 수 있습니다.

작업 시트를 표시 하려면 모든 [`Page`](xref:Xamarin.Forms.Page)에서 [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) 를 `await` 하 여 메시지 및 단추 레이블을 문자열로 전달 합니다. 메서드는 사용자가 클릭한 단추의 문자열 레이블을 반환합니다. 다음은 간단한 예제입니다.

```csharp
async void OnActionSheetSimpleClicked (object sender, EventArgs e)
{
  string action = await DisplayActionSheet ("ActionSheet: Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
  Debug.WriteLine ("Action: " + action);
}
```

![](pop-ups-images/action.png "ActionSheet Dialog")

`destroy` 단추가 다른 항목과 다르게 렌더링되고, `null`로 남겨두거나 세 번째 문자열 매개 변수로 지정할 수 있습니다. 다음 예제에서는 `destroy` 단추를 사용합니다.

```csharp
async void OnActionSheetCancelDeleteClicked (object sender, EventArgs e)
{
  string action = await DisplayActionSheet ("ActionSheet: SavePhoto?", "Cancel", "Delete", "Photo Roll", "Email");
  Debug.WriteLine ("Action: " + action);
}
```

[![DisplayActionSheet](pop-ups-images/action2-sml.png "소멸 단추가 있는 작업 시트 대화 상자")](pop-ups-images/action2.png#lightbox "소멸 단추가 있는 작업 시트 대화 상자")

## <a name="display-a-prompt"></a>프롬프트 표시

프롬프트를 표시 하려면 모든 [`Page`](xref:Xamarin.Forms.Page)에서 `DisplayPromptAsync`를 호출 하 여 제목 및 메시지를 `string` 인수로 전달 합니다.

```csharp
string result = await DisplayPromptAsync("Question 1", "What's your name?");
```

프롬프트는 다음과 같이 모달 형식으로 표시 됩니다.

[![IOS 및 Android의 모달 프롬프트 스크린샷](pop-ups-images/simple-prompt.png "모달 프롬프트")](pop-ups-images/simple-prompt-large.png#lightbox "모달 프롬프트")

확인 단추를 탭 하면 입력 한 응답이 `string`반환 됩니다. 취소 단추를 탭 하면 `null` 반환 됩니다.

`DisplayPromptAsync` 메서드의 전체 인수 목록은 다음과 같습니다.

- `string`형식의 `title`는 프롬프트에 표시할 제목입니다.
- `string`형식의 `message`는 프롬프트에 표시할 메시지입니다.
- `string`형식의 `accept`은 수락 단추의 텍스트입니다. 이 인수는 선택적 인수 이며 기본값은 OK입니다.
- `string`형식의 `cancel`는 취소 단추의 텍스트입니다. 기본값은 Cancel 인 선택적 인수입니다.
- `string`형식의 `placeholder`는 프롬프트에 표시할 자리 표시자 텍스트입니다. 이 인수는 선택적 인수 이며 기본값은 `null`입니다.
- `int`형식의 `maxLength`은 사용자 응답의 최대 길이입니다. 이 인수는 선택적 인수 이며 기본값은-1입니다.
- `Keyboard`형식의 `keyboard`은 사용자 응답에 사용할 키보드 유형입니다. 이 인수는 선택적 인수 이며 기본값은 `Keyboard.Default`입니다.
- `string`형식의 `initialValue`은 표시 되 고 편집할 수 있는 미리 정의 된 응답입니다. 이 인수는 선택적 인수 이며 기본값은 비어 있는 `string`입니다.

다음 예에서는 일부 선택적 인수를 설정 하는 방법을 보여 줍니다.

```csharp
string result = await DisplayPromptAsync("Question 2", "What's 5 + 5?", initialValue: "10", maxLength: 2, keyboard: Keyboard.Numeric);
```

이 코드는 미리 정의 된 응답 10 개를 표시 하 고, 입력 될 수 있는 문자 수를 2로 제한 하 고, 사용자 입력에 대 한 숫자 키보드를 표시 합니다.

[![IOS 및 Android의 모달 프롬프트 스크린샷](pop-ups-images/keyboard-prompt.png "모달 프롬프트")](pop-ups-images/keyboard-prompt-large.png#lightbox "모달 프롬프트")

> [!NOTE]
> `DisplayPromptAsync` 메서드는 현재 iOS 및 Android 에서만 구현 됩니다.

## <a name="related-links"></a>관련 링크

- [PopupsSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-pop-ups)
