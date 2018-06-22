---
title: 팝업 표시
description: Xamarin.Forms는 두 pop up와 같은 사용자 인터페이스 요소 – 경고 및 작업 시트를 제공 합니다. 이 문서에서는 사용자에 게 간단한 질문을 요청 하 고 작업을 통해 사용자가 경고 및 작업 시트 Api를 사용 하 여 보여줍니다.
ms.prod: xamarin
ms.assetid: 46AB0D5E-0025-4A8A-9D00-3E66C3D0BA2E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 97f0917e4e8670ab379aae1b2707ae08cb29bb70
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/27/2018
ms.locfileid: "32018881"
---
# <a name="displaying-pop-ups"></a>팝업 표시

_Xamarin.Forms는 두 pop up와 같은 사용자 인터페이스 요소 – 경고 및 작업 시트를 제공 합니다. 이 문서에서는 사용자에 게 간단한 질문을 요청 하 고 작업을 통해 사용자가 경고 및 작업 시트 Api를 사용 하 여 보여줍니다._

경고를 표시 하거나 선택 기준이 사용자 공통 UI 작업입니다. Xamarin.Forms에 두 가지 방법에는 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 팝업을 통해 사용자와 상호 작용에 대 한 클래스: [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)/) 및 [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])/)합니다. 각 플랫폼에서 적절 한 기본 컨트롤을 렌더링 됩니다.

## <a name="displaying-an-alert"></a>경고를 표시합니다.

모든 Xamarin.Forms 지원 플랫폼에는 다음과 같은 경고 또는 둘 중 간단한 질문에 모달 팝업은. Xamarin.Forms을 이러한 경고를 표시 하려면 사용 된 [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)/) 에서 메서드 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)합니다. 다음 코드 줄에서는 사용자에 게 간단한 메시지를 보여 줍니다.

```csharp
DisplayAlert ("Alert", "You have been alerted", "OK");
```

![](pop-ups-images/alert.png "하나의 단추와 경고 대화 상자")

이 예제에서는 사용자 로부터 정보를 수집 하지 않습니다. 경고 모달 형식으로 표시 하 고 사용자 해제 되 면 응용 프로그램 상호 작용을 계속 합니다.

[ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)/) 메서드를 반환 하 여 두 개의 단추를 제공 합니다. 사용자의 응답을 캡처할 사용할 수도 있습니다는 `boolean`합니다. 경고에서 응답을 받을 두 단추에 대 한 텍스트를 입력 하 고 `await` 메서드. 사용자가 옵션 중 하나를 선택한 후에 대답 코드에 반환 됩니다. 참고는 `async` 및 `await` 아래 샘플 코드에서 키워드:

```csharp
async void OnAlertYesNoClicked (object sender, EventArgs e)
{
  var answer = await DisplayAlert ("Question?", "Would you like to play a game", "Yes", "No");
  Debug.WriteLine ("Answer: " + answer);
}
```

[![DisplayAlert](pop-ups-images/alert2-sml.png "경고 개의 단추가 있는 대화 상자")](pop-ups-images/alert2.png#lightbox "경고 개의 단추가 있는 대화 상자")

## <a name="guiding-users-through-tasks"></a>작업을 통해 기본 사용자

[UIActionSheet](https://developer.apple.com/library/ios/documentation/uikit/reference/uiactionsheet_class/Reference/Reference.html) iOS에는 일반적인 UI 요소입니다. Xamarin.Forms는 [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])/) 메서드를 사용 하면 플랫폼 간 앱을 UWP 및 Android에서 native 대체 렌더링에이 컨트롤을 포함 합니다.

작업 시트를 표시 하려면 `await` [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])/) 모든 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), 메시지 전달 및 레이블을 문자열로 단추입니다. 메서드는 사용자가 클릭 된 단추의 문자열 레이블을 반환 합니다. 간단한 예는 다음과 같습니다.

```csharp
async void OnActionSheetSimpleClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
  Debug.WriteLine ("Action: " + action);
}
```

![](pop-ups-images/action.png "ActionSheet 대화 상자")

`destroy` 단추 다른 다르게 렌더링 되 고 남아 있을 수 있습니다 `null` 또는 세 번째 문자열 매개 변수로 지정 합니다. 다음 예제에서는 `destroy` 단추:

```csharp
async void OnActionSheetCancelDeleteClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: SavePhoto?", "Cancel", "Delete", "Photo Roll", "Email");
  Debug.WriteLine ("Action: " + action);
}
```

[![DisplayActionSheet](pop-ups-images/action2-sml.png "Destroy 단추와 작업 시트 대화")](pop-ups-images/action2.png#lightbox "Destroy 단추와 작업 시트 대화 상자")

## <a name="summary"></a>요약

이 문서에 사용자에 게 간단한 질문을 요청 하 고 작업을 통해 사용자가 경고 및 작업 시트 Api를 사용 하 여 명시 합니다. Xamarin.Forms에 두 가지 방법에는 [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 팝업을 통해 사용자와 상호 작용에 대 한 클래스: [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)/) 및 [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])/), 며, 각 플랫폼에 적절 한 기본 컨트롤을 사용 하 여 렌더링 둘 다 합니다.



## <a name="related-links"></a>관련 링크

- [PopupsSample](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Pop-ups/)
