---
title: 팝업 표시
description: Xamarin.Forms에는 두 팝업 등록와 같은 사용자 인터페이스 요소 – 경고 및 작업 시트를 제공합니다. 이 문서에서는 사용자에 게 간단한 질문을 요청 하는 데 사용자가 작업을 통해 경고 및 작업 시트 Api를 사용 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 46AB0D5E-0025-4A8A-9D00-3E66C3D0BA2E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 156c2f9dca47a7755d4f810d7921a05662388ded
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996716"
---
# <a name="displaying-pop-ups"></a>팝업 표시

_Xamarin.Forms에는 두 팝업 등록와 같은 사용자 인터페이스 요소 – 경고 및 작업 시트를 제공합니다. 이 문서에서는 사용자에 게 간단한 질문을 요청 하는 데 사용자가 작업을 통해 경고 및 작업 시트 Api를 사용 하는 방법을 보여 줍니다._

경고를 표시 하거나 선택 묻는 일반적인 UI 작업입니다. Xamarin.Forms 두 메서드가 합니다 [ `Page` ](xref:Xamarin.Forms.Page) 팝업을 통해 사용자 상호 작용 하기 위한 클래스: [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*) 하 고 [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet*)합니다. 각 플랫폼에 적절 한 네이티브 컨트롤을 사용 하 여 렌더링 됩니다.

## <a name="displaying-an-alert"></a>경고를 표시합니다.

Xamarin.Forms에서 지 원하는 모든 플랫폼의 다음과 같은 경고의 간단한 질문에 모달 팝업 경우 Xamarin.Forms에서 이러한 경고를 표시 하려면 사용 합니다 [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*) 에서 메서드 [ `Page` ](xref:Xamarin.Forms.Page)합니다. 다음 코드 줄에는 사용자에 게 간단한 메시지를 보여 줍니다.

```csharp
DisplayAlert ("Alert", "You have been alerted", "OK");
```

![](pop-ups-images/alert.png "단추 하나를 사용 하 여 경고 대화 상자")

이 예제에서는 사용자 로부터 정보를 수집 하지 않습니다. 경고 모달 형식으로 표시 하 고 사용자를 해제 되 면 응용 프로그램 상호 작용을 계속 합니다.

합니다 [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*) 메서드를 두 개의 단추를 표시 하 고 반환 하 여 사용자의 응답을 캡처할 사용할 수도 있습니다는 `boolean`합니다. 경고에서 응답을 가져오려면 두 단추에 대 한 텍스트를 입력 하 고 `await` 메서드. 사용자가 옵션 중 하나를 선택한 후의 응답 코드에 반환 됩니다. 참고 합니다 `async` 고 `await` 아래 샘플 코드에서 키워드:

```csharp
async void OnAlertYesNoClicked (object sender, EventArgs e)
{
  var answer = await DisplayAlert ("Question?", "Would you like to play a game", "Yes", "No");
  Debug.WriteLine ("Answer: " + answer);
}
```

[![DisplayAlert](pop-ups-images/alert2-sml.png "경고 두 개의 단추를 사용 하 여 대화 상자")](pop-ups-images/alert2.png#lightbox "경고 두 개의 단추를 사용 하 여 대화 상자")

## <a name="guiding-users-through-tasks"></a>작업을 통해 기본 사용자

합니다 [UIActionSheet](https://developer.apple.com/library/ios/documentation/uikit/reference/uiactionsheet_class/Reference/Reference.html) iOS는 일반적인 UI 요소입니다. Xamarin.Forms [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet*) 메서드를 사용 하면 Android 및 UWP에서 기본 대체 렌더링 되는 플랫폼 간 앱에서이 컨트롤을 포함 합니다.

작업 시트를 표시할 `await` [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet*) 에서 [ `Page` ](xref:Xamarin.Forms.Page), 메시지를 전달 하 고 문자열 레이블의 단추입니다. 메서드는 사용자가 클릭 한 단추의 문자열 레이블을 반환 합니다. 간단한 예는 다음과 같습니다.

```csharp
async void OnActionSheetSimpleClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: Send to?", "Cancel", null, "Email", "Twitter", "Facebook");
  Debug.WriteLine ("Action: " + action);
}
```

![](pop-ups-images/action.png "ActionSheet 대화 상자")

합니다 `destroy` 단추가 다른 다르게 렌더링 되 고 유지할 수 있습니다 `null` 또는 세 번째 문자열 매개 변수로 지정 합니다. 다음 예제에서는 `destroy` 단추:

```csharp
async void OnActionSheetCancelDeleteClicked (object sender, EventArgs e)
{
  var action = await DisplayActionSheet ("ActionSheet: SavePhoto?", "Cancel", "Delete", "Photo Roll", "Email");
  Debug.WriteLine ("Action: " + action);
}
```

[![DisplayActionSheet](pop-ups-images/action2-sml.png "Destroy 단추를 사용 하 여 작업 시트 대화 상자")](pop-ups-images/action2.png#lightbox "Destroy 단추를 사용 하 여 작업 시트 대화 상자")

## <a name="summary"></a>요약

이 문서에서는 사용자에 게 간단한 질문을 요청 하 고 작업을 통해 사용자를 안내 하는 경고 및 작업 시트 Api를 사용 하 여 보여 줍니다. Xamarin.Forms 두 메서드가 합니다 [ `Page` ](xref:Xamarin.Forms.Page) 팝업을 통해 사용자 상호 작용 하기 위한 클래스: [ `DisplayAlert` ](xref:Xamarin.Forms.Page.DisplayAlert*) 및 [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet*), 것 이며 모두 각 플랫폼에 적절 한 네이티브 컨트롤을 사용 하 여 렌더링 합니다.



## <a name="related-links"></a>관련 링크

- [PopupsSample](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Pop-ups/)
