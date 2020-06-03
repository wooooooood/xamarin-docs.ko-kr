---
title: Xamarin.Forms 모달 페이지
description: Xamarin.Forms는 모달 페이지를 지원합니다. 모달 페이지는 사용자가 작업이 완료되거나 취소될 때까지 다른 부분으로 이동할 수 없는 자체 포함된 작업을 완료하도록 권장합니다. 이 문서에서는 모달 페이지를 탐색하는 방법을 설명합니다.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4f6547049f2801e5d15115c0ae80af9a07034731
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137828"
---
# <a name="xamarinforms-modal-pages"></a>Xamarin.Forms 모달 페이지

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-modal)

_Xamarin.Forms는 모달 페이지를 지원합니다. 모달 페이지는 사용자가 작업이 완료되거나 취소될 때까지 다른 부분으로 이동할 수 없는 자체 포함된 작업을 완료하도록 권장합니다. 이 문서에서는 모달 페이지를 탐색하는 방법을 설명합니다._

이 문서에서는 다음 토픽을 설명합니다.

- [탐색 수행](#Performing_Navigation) – 모달 스택에 페이지를 푸시하고, 모달 스택에서 페이지를 팝하고, 뒤로 단추를 사용하지 않도록 설정하고, 페이지 전환에 애니메이션을 적용합니다.
- [탐색할 때 데이터를 전달](#Passing_Data_when_Navigating) – 페이지 생성자 및 `BindingContext`를 통해 데이터를 전달합니다.

## <a name="overview"></a>개요

모달 페이지는 Xamarin.Forms에서 지원하는 [페이지](~/xamarin-forms/user-interface/controls/pages.md) 형식이라면 어떤 것이든 될 수 있습니다. 모달 페이지를 표시하려면 애플리케이션은 다음 다이어그램에 나온 것처럼 새 페이지를 모달 스택으로 푸시하여 활성 페이지가 되게 합니다.

![](modal-images/pushing.png "Pushing a Page to the Modal Stack")

이전 페이지로 돌아가기 위해 애플리케이션은 다음 다이어그램에 나온 것처럼 모달 스택에서 현재 페이지를 팝하고 맨 위에 있는 새 페이지가 활성 페이지가 됩니다.

![](modal-images/popping.png "Popping a Page from the Modal Stack")

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>탐색 수행

모달 탐색 메서드는 모든 [`Page`](xref:Xamarin.Forms.Page) 파생 형식의 [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) 속성에 의해 노출됩니다. 이러한 메서드는 모달 스택으로 [모달 페이지를 푸시](#Pushing_Pages_to_the_Modal_Stack)하고, 모달 스택에서 [모달 페이지를 팝](#Popping_Pages_from_the_Modal_Stack)하는 기능을 제공합니다.

[`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) 속성은 또한 모달 스택의 모달 페이지를 얻을 수 있는 [`ModalStack`](xref:Xamarin.Forms.INavigation.ModalStack) 속성을 노출합니다. 그러나 모달 스택 조작을 수행하거나 모달 탐색에서 루트 페이지에 빼(pop)는 개념은 없습니다. 이러한 작업이 기본 플랫폼에서 보편적으로 지원되지 않기 때문입니다.

> [!NOTE]
> [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 인스턴스는 모달 페이지 탐색을 수행하는 데 필요하지 않습니다.

<a name="Pushing_Pages_to_the_Modal_Stack" />

### <a name="pushing-pages-to-the-modal-stack"></a>페이지를 모달 스택에 푸시

`ModalPage`로 이동하려면 다음 코드 예제에서 설명한 것처럼 현재 페이지의 [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) 속성에서 [`PushModalAsync`](xref:Xamarin.Forms.INavigation.PushModalAsync*) 메서드를 호출해야 합니다.

```csharp
async void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
{
  if (listView.SelectedItem != null) {
    var detailPage = new DetailPage ();
    ...
    await Navigation.PushModalAsync (detailPage);
  }
}
```

따라서 `MainPage` 인스턴스의 [`ListView`](xref:Xamarin.Forms.ListView)에서 항목이 선택된 경우 `ModalPage` 인스턴스가 활성 페이지가 되는 모달 스택으로 푸시됩니다. `ModalPage` 인스턴스는 다음 스크린샷과 같이 표시됩니다.

![](modal-images/modalpage.png "Modal Page Example")

[`PushModalAsync`](xref:Xamarin.Forms.INavigation.PushModalAsync*)가 호출되는 경우 다음 이벤트가 발생합니다.

- 기본 플랫폼이 Android가 아닌 경우 `PushModalAsync`를 호출하는 페이지에서 해당 [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) 재정의가 호출됩니다.
- 탐색 중인 페이지에서는 [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) 재정의가 호출됩니다.
- `PushAsync` 작업이 완료됩니다.

그러나 이러한 이벤트가 발생하는 정확한 순서는 플랫폼에 따라 다릅니다. 자세한 내용은 Charles Petzold의 Xamarin.Forms 책 [24장](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)을 참조하세요.

> [!NOTE]
> [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) 및 [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) 재정의에 대한 호출을 페이지 탐색의 보장된 표시로 다룰 수는 없습니다. 예를 들어 iOS에서 `OnDisappearing` 재정의는 애플리케이션이 종료될 때 활성 페이지에 호출됩니다.

<a name="Popping_Pages_from_the_Modal_Stack" />

### <a name="popping-pages-from-the-modal-stack"></a>모달 스택에서 페이지 팝

활성 페이지는 디바이스의 *뒤로* 단추(디바이스의 물리적 단추이든 화면상 단추이든 관계 없음)를 눌러 모달 스택에서 팝할 수 있습니다.

프로그래밍 방식으로 원래 페이지로 돌아가려면 `ModalPage` 개체가 다음 코드 예제에서 설명한 것처럼 [`PopModalAsync`](xref:Xamarin.Forms.INavigation.PopModalAsync) 메서드를 호출해야 합니다.

```csharp
async void OnDismissButtonClicked (object sender, EventArgs args)
{
  await Navigation.PopModalAsync ();
}
```

그러면 `ModalPage` 인스턴스가 모달 스택에서 제거되어 맨 위에 있는 새 페이지가 활성 페이지가 됩니다. [`PopModalAsync`](xref:Xamarin.Forms.INavigation.PopModalAsync)가 호출되는 경우 다음 이벤트가 발생합니다.

- `PopModalAsync`를 호출하는 페이지에서는 해당 [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) 재정의가 호출됩니다.
- 기본 플랫폼이 Android가 아닌 경우 반환되는 페이지에서는 해당 [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) 재정의가 호출됩니다.
- `PopModalAsync` 작업이 반환됩니다.

그러나 이러한 이벤트가 발생하는 정확한 순서는 플랫폼에 따라 다릅니다. 자세한 내용은 Charles Petzold의 Xamarin.Forms 책 [24장](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)을 참조하세요.

### <a name="disabling-the-back-button"></a>뒤로 단추 사용 안 함

Android에서 사용자는 항상 디바이스에서 표준 *뒤로* 단추를 눌러 이전 페이지를 반환할 수 있습니다. 모달 페이지에서 사용자가 페이지를 나가기 전에 자체 포함된 작업을 완료해야 하는 경우 애플리케이션은 *뒤로* 단추를 사용하지 않도록 설정해야 합니다. 이 작업은 모달 페이지에서 [`Page.OnBackButtonPressed`](xref:Xamarin.Forms.Page.OnBackButtonPressed) 메서드를 재정의하여 수행할 수 있습니다. 자세한 내용은 Charles Petzold의 Xamarin.Forms 책 [24장](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)을 참조하세요.

### <a name="animating-page-transitions"></a>페이지 전환에 애니메이션 적용

각 페이지의 [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) 속성은 또한 다음 코드 예제에 나온 것처럼 탐색하는 동안 페이지 애니메이션을 표시할지 여부를 제어하는 `boolean` 매개 변수가 포함된 재정의 푸시 및 팝 메서드를 제공합니다.

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PushModalAsync (new DetailPage (), false);
}

async void OnDismissButtonClicked (object sender, EventArgs args)
{
  // Page appearance not animated
  await Navigation.PopModalAsync (false);
}
```

기본 플랫폼에서 지원되는 경우 매개 변수를 `true`로 설정하면 페이지 전환 애니메이션을 사용하도록 설정되고, `boolean` 매개 변수를 `false`로 설정하면 페이지 전환 애니메이션을 사용하지 않도록 설정됩니다. 그러나 이 매개 변수가 없는 푸시 및 팝 메서드는 기본적으로 애니메이션을 사용하도록 설정합니다.

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>탐색 시 데이터 전달

경우에 따라 페이지에서 탐색하는 동안 다른 페이지로 데이터를 전달해야 합니다. 페이지 생성자를 통해 데이터를 전달하는 방법과 새 페이지의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)를 데이터로 설정하는 방법의 두 방법으로 이 작업을 수행할 수 있습니다. 각 방법을 이제 차례차례 설명하겠습니다.

### <a name="passing-data-through-a-page-constructor"></a>페이지 생성자를 통해 데이터 전달

탐색하는 동안 다른 페이지로 데이터를 전달하기 위한 가장 간단한 방법은 다음 코드 예제에 나와 있는 페이지 생성자 매개 변수를 통하는 것입니다.

```csharp
public App ()
{
  MainPage = new MainPage (DateTime.Now.ToString ("u")));
}
```

이 코드는 ISO8601 형식의 현재 날짜 및 시간으로 전달하는 `MainPage` 인스턴스를 생성합니다.

`MainPage` 인스턴스는 다음 코드 예제에 나온 것처럼 생성자 매개 변수를 통해 데이터를 수신합니다.

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

그런 다음, [`Label.Text`](xref:Xamarin.Forms.Label.Text) 속성을 설정하여 데이터를 페이지에 표시합니다.

### <a name="passing-data-through-a-bindingcontext"></a>BindingContext를 통해 데이터 전달

탐색하는 동안 다른 페이지로 데이터를 전달하는 또 다른 방법은 다음 코드 예제에 표시된 대로 새 페이지의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)를 데이터로 설정하는 것입니다.

```csharp
async void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
{
  if (listView.SelectedItem != null) {
    var detailPage = new DetailPage ();
    detailPage.BindingContext = e.SelectedItem as Contact;
    listView.SelectedItem = null;
    await Navigation.PushModalAsync (detailPage);
  }
}
```

이 코드는 `DetailPage` 인스턴스의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)를 `Contact` 인스턴스로 설정한 다음, `DetailPage`로 이동합니다.

그런 다음, 다음 XAML 코드 예제에 표시된 대로 `DetailPage`는 데이터 바인딩을 사용하여 `Contact` 인스턴스 데이터를 표시합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ModalNavigation.DetailPage">
    <ContentPage.Padding>
      <OnPlatform x:TypeArguments="Thickness">
        <On Platform="iOS" Value="0,40,0,0" />
      </OnPlatform>
    </ContentPage.Padding>
    <ContentPage.Content>
        <StackLayout HorizontalOptions="Center" VerticalOptions="Center">
            <StackLayout Orientation="Horizontal">
                <Label Text="Name:" FontSize="Medium" HorizontalOptions="FillAndExpand" />
                <Label Text="{Binding Name}" FontSize="Medium" FontAttributes="Bold" />
            </StackLayout>
              ...
            <Button x:Name="dismissButton" Text="Dismiss" Clicked="OnDismissButtonClicked" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

다음 코드 예제는 C#에서 데이터 바인딩이 어떻게 수행되는지를 보여줍니다.

```csharp
public class DetailPageCS : ContentPage
{
  public DetailPageCS ()
  {
    var nameLabel = new Label {
      FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
      FontAttributes = FontAttributes.Bold
    };
    nameLabel.SetBinding (Label.TextProperty, "Name");
    ...
    var dismissButton = new Button { Text = "Dismiss" };
    dismissButton.Clicked += OnDismissButtonClicked;

    Thickness padding;
    switch (Device.RuntimePlatform)
    {
        case Device.iOS:
            padding = new Thickness(0, 40, 0, 0);
            break;
        default:
            padding = new Thickness();
            break;
    }

    Padding = padding;
    Content = new StackLayout {
      HorizontalOptions = LayoutOptions.Center,
      VerticalOptions = LayoutOptions.Center,
      Children = {
        new StackLayout {
          Orientation = StackOrientation.Horizontal,
          Children = {
            new Label{ Text = "Name:", FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)), HorizontalOptions = LayoutOptions.FillAndExpand },
            nameLabel
          }
        },
        ...
        dismissButton
      }
    };
  }

  async void OnDismissButtonClicked (object sender, EventArgs args)
  {
    await Navigation.PopModalAsync ();
  }
}
```

그런 다음, 일련의 [`Label`](xref:Xamarin.Forms.Label) 컨트롤로 데이터를 페이지에 표시합니다.

데이터 바인딩에 대한 자세한 내용은 [데이터 바인딩 기본](~/xamarin-forms/xaml/xaml-basics/index.md)을 참조하세요.

## <a name="summary"></a>요약

이 문서에서는 모달 페이지를 탐색하는 방법을 설명했습니다. 모달 페이지는 사용자가 작업이 완료되거나 취소될 때까지 다른 부분으로 이동할 수 없는 자체 포함된 작업을 완료하도록 권장합니다.

## <a name="related-links"></a>관련 링크

- [페이지 탐색](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [모달(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-modal)
- [PassingData(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-passingdata)
