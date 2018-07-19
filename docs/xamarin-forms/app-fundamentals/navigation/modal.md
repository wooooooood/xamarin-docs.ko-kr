---
title: Xamarin.Forms 모달 페이지
description: Xamarin.Forms는 모달 페이지를 지원합니다. 모달 페이지는 사용자가 작업이 완료되거나 취소될 때까지 다른 부분으로 이동할 수 없는 자체 포함된 작업을 완료하도록 권장합니다. 이 문서에는 모달 페이지를 탐색 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 486CB7FD-2B9A-4DE3-94BD-C8D904E5D3C6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 44aee8500c7de2ae56b59049368d6025ec49cc5e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994830"
---
# <a name="xamarinforms-modal-pages"></a>Xamarin.Forms 모달 페이지

_Xamarin.Forms는 모달 페이지에 대 한 지원을 제공합니다. 모달 페이지는 사용자가 작업이 완료되거나 취소될 때까지 다른 부분으로 이동할 수 없는 자체 포함된 작업을 완료하도록 권장합니다. 이 문서에는 모달 페이지를 탐색 하는 방법을 보여 줍니다._

이 문서에서는 다음 내용을 다룹니다.

- [탐색을 수행](#Performing_Navigation) – 모달 스택의 모달 스택에서 팝업 페이지에 페이지를 푸시, 뒤로 단추를 사용 하지 않도록 설정 하 고 페이지 전환 애니메이션을 적용 합니다.
- [탐색할 때 데이터를 전달](#Passing_Data_when_Navigating) – 데이터 페이지 생성자를 통해 전달는 `BindingContext`합니다.

## <a name="overview"></a>개요

모달 페이지 중 하나일 수 있습니다 합니다 [페이지](~/xamarin-forms/user-interface/controls/pages.md) Xamarin.Forms에서 지 원하는 형식. 모달 페이지를 표시 하려면 응용 프로그램은 푸시하고 모달 스택으로 되 게 활성 페이지는 다음 다이어그램과에서 같이:

![](modal-images/pushing.png "페이지 모달 스택에 푸시")

반환할 이전 페이지에는 응용 프로그램은 모달 스택에서 현재 페이지를 팝 하 고 새 최상위 페이지로 페이지를 사용 하는 활성 페이지가 됩니다 다음 다이어그램에 나와 있는 것 처럼:

![](modal-images/popping.png "스택의 모달 페이지를 팝합니다.")

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>탐색을 수행합니다.

모달 탐색 메서드는 모든 [`Page`](xref:Xamarin.Forms.Page) 파생 형식의 [`Navigation`](xref:Xamarin.Forms.VisualElement.Navigation) 속성에 의해 노출됩니다. 이러한 메서드를 제공 하는 기능 [모달 페이지를 푸시](#Pushing_Pages_to_the_Modal_Stack) 모달 스택으로 및 [모달 페이지를 팝](#Popping_Pages_from_the_Modal_Stack) 모달 스택에서 합니다.

[ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) 속성 노출 된 [ `ModalStack` ](xref:Xamarin.Forms.INavigation.ModalStack) 모달 스택의 모달 페이지를 생성 하는 데 사용 될 수 있는 속성입니다. 그러나 모달 스택 조작을 수행하거나 모달 탐색에서 루트 페이지에 팝하는 개념은 없습니다. 이러한 작업이 기본 플랫폼에서 보편적으로 지원되지 않기 때문입니다.

> [!NOTE]
> [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 인스턴스는 모달 페이지 탐색을 수행하는 데 필요하지 않습니다.

<a name="Pushing_Pages_to_the_Modal_Stack" />

### <a name="pushing-pages-to-the-modal-stack"></a>모달 스택에 푸시 페이지

이동할를 `ModalPage` 호출 해야 하는 합니다 [ `PushModalAsync` ](xref:Xamarin.Forms.INavigation.PushModalAsync*) 메서드를 [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) 다음 코드 예제에서 설명한 것 처럼 현재 페이지의 속성:

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

이 인해 합니다 `ModalPage` 푸시되 어 모달 스택에 있는 활성 페이지에 인스턴스 제공에서 항목이 선택 되어 있는지를 [ `ListView` ](xref:Xamarin.Forms.ListView) 에 `MainPage` 인스턴스. `ModalPage` 인스턴스 다음 스크린샷과에서 같습니다.

![](modal-images/modalpage.png "모달 페이지 예제")

때 [ `PushModalAsync` ](xref:Xamarin.Forms.INavigation.PushModalAsync*) 호출을 다음 이벤트가 발생 합니다.

- 페이지 호출 `PushModalAsync` 에 해당 [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) 재정의 기본 플랫폼 되지 Android 호출 합니다.
- 탐색 중인 페이지에 해당 [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) 재정의 호출 합니다.
- `PushAsync` 작업 완료 합니다.

그러나 이러한 이벤트가 발생 하는 정확한 순서는 플랫폼에 따라 다릅니다. 자세한 내용은 [24 장](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold의 Xamarin.Forms 책입니다.

> [!NOTE]
> 에 대 한 호출을 [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) 하 고 [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) 재정의 페이지 탐색의 보장 된 표시로 처리할 수 없습니다. 예를 들어 iOS에는 `OnDisappearing` 응용 프로그램이 종료 될 때 재정의 활성 페이지 라고 합니다.

<a name="Popping_Pages_from_the_Modal_Stack" />

### <a name="popping-pages-from-the-modal-stack"></a>스택에서 모달 팝업 페이지

키를 눌러 모달 스택 으로부터 현재 페이지를 팝 할 수 있습니다는 *다시* 에 관계 없이 장치에서 단추 장치의 물리적 단추 인지 또는 화면상 단추.

프로그래밍 방식으로 원래 페이지로 돌아가려면 `ModalPage` 개체가 다음 코드 예제에서 설명한 것처럼 [`PopModalAsync`](xref:Xamarin.Forms.INavigation.PopModalAsync) 메서드를 호출해야 합니다.

```csharp
async void OnDismissButtonClicked (object sender, EventArgs args)
{
  await Navigation.PopModalAsync ();
}
```

이 인해는 `ModalPage` 활성 페이지가 새 최상위 페이지를 사용 하 여 모달 스택에서 제거할 인스턴스. 때 [ `PopModalAsync` ](xref:Xamarin.Forms.INavigation.PopModalAsync) 호출을 다음 이벤트가 발생 합니다.

- 페이지 호출 `PopModalAsync` 에 해당 [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) 재정의 호출 합니다.
- 페이지에 반환 되는 해당 [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) 재정의 기본 플랫폼 되지 Android 호출 합니다.
- `PopModalAsync` 작업 반환 합니다.

그러나 이러한 이벤트가 발생 하는 정확한 순서는 플랫폼에 따라 다릅니다. 자세한 내용은 [24 장](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold의 Xamarin.Forms 책입니다.

### <a name="disabling-the-back-button"></a>뒤로 단추를 사용 하지 않도록 설정

Android에서 사용자 수 항상 이전 페이지로 돌아가려면 표준 키를 눌러 *다시* 장치에서 단추입니다. 모달 페이지에 페이지를 나가기 전에 자체 포함 된 작업을 완료 하는 사용자가 필요한 경우 응용 프로그램의 사용 하지 않아야 합니다 *다시* 단추입니다. 재정의 하 여이 작업을 수행할 수 있습니다 합니다 [ `Page.OnBackButtonPressed` ](xref:Xamarin.Forms.Page.OnBackButtonPressed) 모달 페이지 상의 메서드. 자세한 내용은 참조 [24 장](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold의 Xamarin.Forms 책입니다.

### <a name="animating-page-transitions"></a>이 페이지는 전환에 애니메이션 적용

[ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) 각 페이지의 속성은 또한 재정의 푸시 및 팝 메서드를 포함 하는 제공 된 `boolean` 다음 코드 에서처럼를 탐색 하는 동안 페이지 애니메이션을 표시할지 여부를 제어 하는 매개 변수 예:

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

설정 된 `boolean` 매개 변수를 `false` 는 매개 변수를 설정 하는 동안 페이지 전환 애니메이션을 사용 하지 않도록 설정 `true` 내부 플랫폼에서 지원 되는 페이지 전환 애니메이션을 사용 하도록 설정 합니다. 그러나이 매개 변수 없는 push와 pop 메서드는 기본적으로 애니메이션을 사용 합니다.

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>탐색할 때 데이터 전달

경우에 따라 다른 페이지로 탐색 하는 동안 데이터를 전달 하는 페이지에 대 한 필요 합니다. 두 방법으로이 작업을 수행 하는 새 페이지의 설정 및 페이지 생성자를 통해 데이터를 전달 하 여 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 데이터입니다. 다시 설명 이제 각 합니다.

### <a name="passing-data-through-a-page-constructor"></a>페이지 생성자를 통해서만 데이터 전달

다른 페이지로 탐색 하는 동안 데이터를 전달 하기 위한 가장 간단한 방법은 다음과 같습니다. 다음 코드 예제에 나와 있는 페이지 생성자 매개 변수를 통해

```csharp
public App ()
{
  MainPage = new MainPage (DateTime.Now.ToString ("u")));
}
```

이 코드에서는 `MainPage` 인스턴스를 현재 날짜 및 시간 ISO8601 형식으로 전달 합니다.

`MainPage` 인스턴스는 다음 코드 예제와 같이 생성자 매개 변수를 통해 데이터를 받습니다.

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

데이터는 다음을 설정 하 여 페이지에 표시 됩니다는 [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) 속성입니다.

### <a name="passing-data-through-a-bindingcontext"></a>BindingContext 통해 데이터 전달

새 페이지의 설정 된 경우 다른 페이지로 탐색 하는 동안 데이터를 전달 하는 또 다른 방법은 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 다음 코드 예제에 표시 된 대로 데이터에:

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

이 코드를 설정 합니다 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 의 `DetailPage` 인스턴스를 `Contact` 인스턴스로 이동한 다음 이동할는 `DetailPage`합니다.

`DetailPage` 데이터 바인딩을 사용 하 여 표시 된 `Contact` 인스턴스 데이터를 다음 XAML 코드 예제에 표시 된 대로:

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

다음 코드 예제에서는 C#에서 데이터 바인딩을 수행할 수 있습니다 하는 방법을 보여 줍니다.

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

데이터는 다음 일련의 페이지에 표시 됩니다 [ `Label` ](xref:Xamarin.Forms.Label) 컨트롤입니다.

데이터 바인딩에 대한 자세한 내용은 [데이터 바인딩 기본](~/xamarin-forms/xaml/xaml-basics/index.md)을 참조하세요.

## <a name="summary"></a>요약

이 문서에는 모달 페이지를 탐색 하는 방법을 보여 줍니다. 모달 페이지는 사용자가 작업이 완료되거나 취소될 때까지 다른 부분으로 이동할 수 없는 자체 포함된 작업을 완료하도록 권장합니다.


## <a name="related-links"></a>관련 링크

- [페이지 탐색](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [모달 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Modal/)
- [PassingData (샘플)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
