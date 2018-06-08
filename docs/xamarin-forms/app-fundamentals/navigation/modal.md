---
title: 모달 페이지
description: Xamarin.Forms는 모달 페이지를 지원합니다. 모달 페이지는 사용자가 작업이 완료되거나 취소될 때까지 다른 부분으로 이동할 수 없는 자체 포함된 작업을 완료하도록 권장합니다. 이 문서에 모달 페이지를 탐색 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 486CB7FD-2B9A-4DE3-94BD-C8D904E5D3C6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 30d0371e0eaa31673561ae12c7a46b7a7819a647
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847421"
---
# <a name="modal-pages"></a>모달 페이지

_Xamarin.Forms 모달 페이지에 대 한 지원을 제공합니다. 모달 페이지는 사용자가 작업이 완료되거나 취소될 때까지 다른 부분으로 이동할 수 없는 자체 포함된 작업을 완료하도록 권장합니다. 이 문서에 모달 페이지를 탐색 하는 방법을 보여 줍니다._

이 문서에서는 다음 내용을 다룹니다.

- [탐색을 수행](#Performing_Navigation) – 모달 스택의 모달 스택에서 팝 페이지에 페이지를 강제 설치, 뒤로 단추를 사용 하지 않도록 설정 하 고 페이지 전환 애니메이션을 적용 합니다.
- [데이터를 탐색할 때 전달](#Passing_Data_when_Navigating) – 데이터 및 페이지 생성자를 통해 전달는 `BindingContext`합니다.

## <a name="overview"></a>개요

모달 페이지 중 하나일 수 있습니다는 [페이지](~/xamarin-forms/user-interface/controls/pages.md) Xamarin.Forms에서 지 원하는 형식. 모달 페이지를 표시 하려면 응용 프로그램은 밀어 넣습니다 모달 스택에 있는 사이트용으로 활성 페이지는 다음 다이어그램에 나와 있는 것 처럼:

![](modal-images/pushing.png "모달 스택에 페이지를 강제 설치")

이전 페이지로 돌아가려면 응용 프로그램이 모달 스택에서 현재 페이지 팝업 매핑되며 새 맨 위에 있는 페이지 활성 페이지는 다음 다이어그램에 나와 있는 것 처럼:

![](modal-images/popping.png "모달 스택에서 페이지를 팝합니다.")

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>탐색을 수행합니다.

모달 탐색 메서드는 모든 [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) 파생 형식의 [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) 속성에 의해 노출됩니다. 이러한 메서드를 제공 하는 기능 [모달 페이지 푸시](#Pushing_Pages_to_the_Modal_Stack) 모달 스택으로 및 [모달 페이지 팝](#Popping_Pages_from_the_Modal_Stack) 모달 스택에서 합니다.

[ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) 속성도 노출 한 [ `ModalStack` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.ModalStack/) 모달 스택의 모달 페이지 생성 하는 데 사용 될 수 있는 속성입니다. 그러나 모달 스택 조작을 수행하거나 모달 탐색에서 루트 페이지에 팝하는 개념은 없습니다. 이러한 작업이 기본 플랫폼에서 보편적으로 지원되지 않기 때문입니다.

> [!NOTE]
> [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 인스턴스는 모달 페이지 탐색을 수행하는 데 필요하지 않습니다.

<a name="Pushing_Pages_to_the_Modal_Stack" />

### <a name="pushing-pages-to-the-modal-stack"></a>모달 스택에 푸시 페이지

으로 이동 하는 `ModalPage` 호출 해야 하는 [ `PushModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page)/) 에서 메서드는 [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) 현재 페이지에 있는 다음 코드 예제에서 설명한로 속성:

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

이 인해는 `ModalPage` 을 모달 스택에 밀어넣을 수 있는 활성 페이지를 되 면 인스턴스 항목에서 선택한 제공는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 에 `MainPage` 인스턴스. `ModalPage` 인스턴스 다음 스크린샷에 표시 됩니다.

![](modal-images/modalpage.png "모달 페이지 예제")

때 [ `PushModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page)/) 를 호출 하면 다음 이벤트가 발생 합니다.

- 페이지 호출 `PushModalAsync` 가 해당 [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) 재정의 호출 하는 기본 플랫폼에는 Android 되지 않습니다.
- 탐색 대상인 페이지에 해당 [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) 재정의 호출 합니다.
- `PushAsync` 작업을 완료 합니다.

그러나 이러한 이벤트가 발생 하는 정확한 순서는 플랫폼에 따라 다릅니다. 자세한 내용은 참조 [24 장](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold Xamarin.Forms 책의 합니다.

> [!NOTE]
> 에 대 한 호출이 [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) 및 [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) 재정의 보장 된 페이지 탐색으로 처리할 수 없습니다. 예를 들어 iOS의 경우에 `OnDisappearing` 재정의 응용 프로그램이 종료 될 때 현재 페이지에서 호출 됩니다.

<a name="Popping_Pages_from_the_Modal_Stack" />

### <a name="popping-pages-from-the-modal-stack"></a>모달 스택에서 팝 페이지

키를 눌러 활성 페이지 모달 스택에서 팝 될 수는 *다시* 에 관계 없이 장치에서 단추이 장치에 물리적 단추 인지 또는 화면에 나타나는 단추입니다.

프로그래밍 방식으로 원래 페이지로 돌아가려면 `ModalPage` 개체가 다음 코드 예제에서 설명한 것처럼 [`PopModalAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/) 메서드를 호출해야 합니다.

```csharp
async void OnDismissButtonClicked (object sender, EventArgs args)
{
  await Navigation.PopModalAsync ();
}
```

이 인해는 `ModalPage` 활성 페이지가 되 고 새 맨 위에 있는 페이지를 사용 하 여 모달 스택에서 제거할 인스턴스입니다. 때 [ `PopModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/) 를 호출 하면 다음 이벤트가 발생 합니다.

- 페이지 호출 `PopModalAsync` 가 해당 [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) 재정의 호출 합니다.
- 반환 되는 페이지에 해당 [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) 재정의 호출 하는 기본 플랫폼에는 Android 되지 않습니다.
- `PopModalAsync` 작업 반환 합니다.

그러나 이러한 이벤트가 발생 하는 정확한 순서는 플랫폼에 따라 다릅니다. 자세한 내용은 참조 [24 장](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold Xamarin.Forms 책의 합니다.

### <a name="disabling-the-back-button"></a>뒤로 단추를 사용 하지 않도록 설정

Android에서는 사용자 항상 페이지로 이동할 수는 이전 표준 키를 눌러 *다시* 장치에서 단추입니다. 응용 프로그램을 사용 하지 않도록 해야 모달 페이지는 페이지에서 이동 하기 전에 자체 포함 된 작업을 완료 하는 사용자가 필요한 경우는 *다시* 단추입니다. 재정의 하 여이 작업을 수행할 수 있습니다는 [ `Page.OnBackButtonPressed` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnBackButtonPressed/) 모달 페이지 메서드. 자세한 내용은 참조 [24 장](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Charles Petzold Xamarin.Forms 책의 합니다.

### <a name="animating-page-transitions"></a>이 페이지는 전환에 애니메이션 적용

[ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) 재정의 된 푸시 및 팝 메서드를 포함 하는 각 페이지의 속성 제공는 `boolean` 다음 코드에 나와 있는 것 처럼를 탐색 하는 동안 페이지 애니메이션을 표시할지 여부를 제어 하는 매개 변수 예:

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

설정의 `boolean` 매개 변수를 `false` 는 매개 변수를 설정 하는 동안 페이지 전환 애니메이션을 사용 하지 않도록 설정 `true` 은 기본 플랫폼에서 지원 제공 페이지 전환 애니메이션을 사용할 수 있습니다. 그러나이 매개 변수 없는 푸시 및 팝 메서드는 기본적으로 애니메이션을 사용 합니다.

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>탐색할 때 데이터 전달

경우에 따라 다른 페이지를 탐색 하는 동안 데이터를 전달 하는 페이지에 대 한 필요 합니다. 이 작업을 수행 하는 두 가지 기술이 되며 page 생성자를 통해 데이터를 전달 하 여 새 페이지를 설정 하 여 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) 데이터입니다. 다시 설명 이제 각 합니다.

### <a name="passing-data-through-a-page-constructor"></a>페이지 생성자를 통해 데이터 전달

다른 페이지를 탐색 하는 동안 데이터를 전달 하기 위한 가장 간단한 방법은 다음 코드 예제에 표시 되는 페이지 생성자 매개 변수를 통해:

```csharp
public App ()
{
  MainPage = new MainPage (DateTime.Now.ToString ("u")));
}
```

이 코드에서는 `MainPage` 인스턴스를 현재 날짜 및 시간 ISO8601 형식에 전달 합니다.

`MainPage` 인스턴스는 다음 코드 예제와 같이 생성자 매개 변수를 통해 데이터를 수신 합니다.

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

이후에 데이터가 설정 하 여 페이지에 표시 되는 [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) 속성입니다.

### <a name="passing-data-through-a-bindingcontext"></a>BindingContext 통해 데이터 전달

새 페이지를 설정 하 여 다른 페이지를 탐색 하는 동안 데이터를 전달 하기 위한 다른 방법은 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) 다음 코드 예제와 같이 데이터에:

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

설정 하는이 코드는 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) 의 `DetailPage` 인스턴스는 `Contact` 인스턴스를 선택한 다음로 이동는 `DetailPage`합니다.

`DetailPage` 데이터 바인딩을 사용 하 여 표시 된 `Contact` 다음 XAML 코드 예제와 같이 인스턴스 데이터:

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

다음 코드 예제에서는 C#에서 데이터 바인딩 수 수행 하는 방법을 보여 줍니다.

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

일련의 데이터 페이지에 표시 한 다음는 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 컨트롤입니다.

데이터 바인딩에 대한 자세한 내용은 [데이터 바인딩 기본](~/xamarin-forms/xaml/xaml-basics/index.md)을 참조하세요.

## <a name="summary"></a>요약

이 문서에 모달 페이지를 탐색 하는 방법을 보여 줍니다. 모달 페이지는 사용자가 작업이 완료되거나 취소될 때까지 다른 부분으로 이동할 수 없는 자체 포함된 작업을 완료하도록 권장합니다.


## <a name="related-links"></a>관련 링크

- [페이지 탐색](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [Modal (샘플)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Modal/)
- [PassingData (샘플)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
