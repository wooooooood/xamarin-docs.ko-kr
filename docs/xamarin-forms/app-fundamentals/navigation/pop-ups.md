---
title: 팝업 표시
description: Xamarin.Forms는 팝업과 유사한 두 가지 사용자 인터페이스 요소인 경고 및 작업 시트를 제공합니다. 이 문서에서는 사용자에게 간단한 질문을 하고 작업을 안내하는 경고 및 작업 시트 API를 사용하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 46AB0D5E-0025-4A8A-9D00-3E66C3D0BA2E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 36a253a46b76e4c883639ca683a8eab64700b064
ms.sourcegitcommit: 5d4e6677224971e2bc0268f405d192d0358c74b8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58329314"
---
# <a name="displaying-pop-ups"></a>팝업 표시

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Pop-ups/)

_Xamarin.Forms는 팝업과 유사한 두 가지 사용자 인터페이스 요소인 경고와 작업 시트를 제공합니다. 이 문서에서는 사용자에게 간단한 질문을 하고 작업을 안내하는 경고 및 작업 시트 API를 사용하는 방법을 설명합니다._

경고를 표시하거나 사용자가 선택하도록 하는 것은 일반적인 UI 작업입니다. Xamarin.Forms에는 팝업을 통해 사용자와 상호 작용하기 위한 [`Page`](xref:Xamarin.Forms.Page) 클래스에 두 메서드인 [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) 및 [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*)가 있습니다. 이 두 가지 항목은 각 플랫폼에 적절한 네이티브 컨트롤을 사용하여 렌더링됩니다.

## <a name="displaying-an-alert"></a>경고 표시

Xamarin.Forms에서 지원하는 모든 플랫폼에는 사용자에게 경고를 표시하고 간단한 질문을 하는 모달 팝업이 있습니다. Xamarin.Forms에서 이러한 경고를 표시하려면 [`Page`](xref:Xamarin.Forms.Page)에서 [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) 메서드를 사용합니다. 다음 코드 줄은 사용자에게 표시되는 간단한 메시지입니다.

```csharp
DisplayAlert ("Alert", "You have been alerted", "OK");
```

![](pop-ups-images/alert.png "단추 하나가 포함된 경고 대화 상자")

이 예제에서는 사용자로부터 정보를 수집하지 않습니다. 경고는 모달 형식으로 표시되고 해제되면 사용자는 애플리케이션과 상호 작용을 계속합니다.

[`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) 메서드는 또한 두 개의 단추를 표시하고 `boolean`을 반환하여 사용자의 응답을 캡처하는 데 사용할 수 있습니다. 경고에서 응답을 가져오려면 두 단추 모두에 텍스트를 적용하고 메서드를 `await`합니다. 사용자가 옵션 중 하나를 선택한 후 대답이 사용자 코드로 반환됩니다. 아래 샘플 코드의 `async` 및 `await` 키워드를 참고하세요.

```csharp
async void OnAlertYesNoClicked (object sender, EventArgs e)
{
  bool answer = await DisplayAlert ("Question?", "Would you like to play a game", "Yes", "No");
  Debug.WriteLine ("Answer: " + answer);
}
```

[![DisplayAlert](pop-ups-images/alert2-sml.png "두 개의 단추가 포함된 경고 대화 상자")](pop-ups-images/alert2.png#lightbox "두 개의 단추가 포함된 경고 대화 상자")

## <a name="guiding-users-through-tasks"></a>사용자에게 작업 안내

[UIActionSheet](https://developer.apple.com/library/ios/documentation/uikit/reference/uiactionsheet_class/Reference/Reference.html)는 iOS의 일반적인 UI 요소입니다. Xamarin.Forms [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*) 메서드를 통해 Android 및 UWP에서 기본 대체 항목을 렌더링하는 플랫폼 간 앱에 이 컨트롤을 포함시킬 수 있습니다.

작업 시트를 표시하려면 [`Page`](xref:Xamarin.Forms.Page)에서 [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*)를 `await`하여 메시지 및 단추 레이블을 문자열로 전달합니다. 메서드는 사용자가 클릭한 단추의 문자열 레이블을 반환합니다. 다음은 간단한 예제입니다.

```csharp
async void OnActionSheetSimpleClicked (object sender, EventArgs e)
{
  string action = await DisplayActionSheet ("ActionSheet: Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
  Debug.WriteLine ("Action: " + action);
}
```

![](pop-ups-images/action.png "ActionSheet 대화 상자")

`destroy` 단추가 다른 항목과 다르게 렌더링되고, `null`로 남겨두거나 세 번째 문자열 매개 변수로 지정할 수 있습니다. 다음 예제에서는 `destroy` 단추를 사용합니다.

```csharp
async void OnActionSheetCancelDeleteClicked (object sender, EventArgs e)
{
  string action = await DisplayActionSheet ("ActionSheet: SavePhoto?", "Cancel", "Delete", "Photo Roll", "Email");
  Debug.WriteLine ("Action: " + action);
}
```

[![DisplayActionSheet](pop-ups-images/action2-sml.png "삭제 단추가 포함된 작업 시트 대화 상자")](pop-ups-images/action2.png#lightbox "삭제 단추가 포함된 작업 시트 대화 상자")

## <a name="summary"></a>요약

이 문서에서는 사용자에게 간단한 질문을 하고 작업을 안내하는 경고 및 작업 시트 API를 사용하는 방법을 설명합니다. Xamarin.Forms에는 [`Page`](xref:Xamarin.Forms.Page) 클래스에 팝업을 통해 사용자와 상호 작용하기 위한 두 가지 메서드, [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert*) 및 [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet*)가 있습니다. 모두 각 플랫폼에서 적절한 네이티브 컨트롤을 사용하여 렌더링됩니다.



## <a name="related-links"></a>관련 링크

- [PopupsSample](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Pop-ups/)
