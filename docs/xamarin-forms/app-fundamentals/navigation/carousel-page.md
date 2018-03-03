---
title: "회전식 페이지"
description: "Xamarin.Forms CarouselPage 페이지인 사용자가 왼쪽에서 오른쪽으로 살짝 수 있는 갤러리와 같은 콘텐츠 페이지를 탐색 합니다. 이 문서를 사용 하는 CarouselPage 페이지의 컬렉션을 탐색 하는 방법을 보여줍니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 2D14FC9D-DF5F-427E-9006-2AAE61ECF8DC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: adb1042b8f44d3f414e2161e20b7eb3dc0e940a2
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="carousel-page"></a>회전식 페이지

_Xamarin.Forms CarouselPage 페이지인 사용자가 왼쪽에서 오른쪽으로 살짝 수 있는 갤러리와 같은 콘텐츠 페이지를 탐색 합니다. 이 문서를 사용 하는 CarouselPage 페이지의 컬렉션을 탐색 하는 방법을 보여줍니다._

## <a name="overview"></a>개요

다음 스크린 샷 표시는 [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) 각 플랫폼에서:

![](carousel-page-images/thirdpage.png "CarouselPage Thid 항목")

레이아웃을 [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) 는 각 플랫폼에서 동일 합니다. 컬렉션을 이동 합니다 오른쪽에서 왼쪽으로 살짝 밀면 및 이전 버전과 컬렉션 탐색을 왼쪽에서 오른쪽을 살짝 밀어를 통해 페이지를 탐색할 수 있습니다. 첫 번째 페이지를 표시 하는 다음 스크린샷에서 [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) 인스턴스:

![](carousel-page-images/firstpage.png "CarouselPage 첫 번째 항목")

다음 스크린샷에서 같이 오른쪽에서 왼쪽된 이동으로 두 번째 페이지로 넘기기가:

![](carousel-page-images/secondpage.png "두 번째 CarouselPage 항목")

오른쪽에서 왼쪽으로 다시 넘기기가 왼쪽에서 오른쪽으로 넘기기가 이전 페이지로 반환 하는 동안 세 번째 페이지를 이동 합니다.

<!--
> [!NOTE]
> **Note**: The [`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) has been deprecated, and will be removed from Xamarin.Forms in a future release. Instead, the [`CarouselView`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselView/) should be used to provide a gallery-like view, where users can swipe from side to side to move through a collection of items.
-->

## <a name="creating-a-carouselpage"></a>CarouselPage 만들기

두 가지 접근 방법을 만드는 데 사용할 수는 [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/):

- [채울](#Populating_a_CarouselPage_with_a_Page_Collection) 는 `CarouselPage` 자식 요소의 컬렉션으로 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 인스턴스.
- [할당](#Populating_a_CarouselPage_with_a_Template) 컬렉션에는 [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/) 속성 및 할당은 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 에 [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) 반환할속성[ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 개체 컬렉션의 인스턴스.

두 방법으로는 `CarouselPage` 를 표시 한 다음 각 페이지 차례로 표시 될 다음 페이지로 이동 하 여 살짝 밀어 상호 작용 합니다. 이 탐색 환경을 자연스럽 고 Windows Phone 사용자에 게 익숙한 느낄 됩니다.

> [!NOTE]
> **참고**: A [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) 을 채울 수 있습니다 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 인스턴스 또는 `ContentPage` 파생 항목입니다.

<a name="Populating_a_CarouselPage_with_a_Page_Collection" />

### <a name="populating-a-carouselpage-with-a-page-collection"></a>페이지 컬렉션으로 CarouselPage 채우기

다음 XAML 코드 예제는 [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) 세 개를 표시 하는 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 인스턴스:

```xaml
<CarouselPage xmlns="http://xamarin.com/schemas/2014/forms"
              xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
              x:Class="CarouselPageNavigation.MainPage">
    <ContentPage>
        <ContentPage.Padding>
          <OnPlatform x:TypeArguments="Thickness">
              <On Platform="iOS, Android" Value="0,40,0,0" />
          </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
            <Label Text="Red" FontSize="Medium" HorizontalOptions="Center" />
            <BoxView Color="Red" WidthRequest="200" HeightRequest="200" HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
        </StackLayout>
    </ContentPage>
    <ContentPage>
        ...
    </ContentPage>
    <ContentPage>
        ...
    </ContentPage>
</CarouselPage>
```

다음 코드 예제에서는 C#의 동등한 UI를 보여 줍니다.

```csharp
public class MainPageCS : CarouselPage
{
    public MainPageCS ()
    {
        Thickness padding;
        switch (Device.RuntimePlatform)
        {
            case Device.iOS:
            case Device.Android:
                padding = new Thickness(0, 40, 0, 0);
                break;
            default:
                padding = new Thickness();
                break;
        }

        var redContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                Children = {
                    new Label {
                        Text = "Red",
                        FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
                        HorizontalOptions = LayoutOptions.Center
                    },
                    new BoxView {
                        Color = Color.Red,
                        WidthRequest = 200,
                        HeightRequest = 200,
                        HorizontalOptions = LayoutOptions.Center,
                        VerticalOptions = LayoutOptions.CenterAndExpand
                    }
                }
            }
        };
        var greenContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                ...
            }
        };
        var blueContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                ...
            }
        };

        Children.Add (redContentPage);
        Children.Add (greenContentPage);
        Children.Add (blueContentPage);
    }
}
```

각 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 단순히 표시는 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 특정 색에 대 한 및 [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) 해당 색상의 합니다.

> [!NOTE]
> **참고**:는 [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) UI 가상화를 지원 하지 않습니다. 따라서 성능 영향을 받을 수는 경우는 `CarouselPage` 자식 요소가 너무 많습니다.

경우는 [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) 에 포함 되는 [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) 의 페이지는 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/), [ `MasterDetailPage.IsGestureEnabled` ](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty/) 속성 설정 해야 `false` 간에 제스처 충돌을 방지 하는 `CarouselPage` 및 `MasterDetailPage`합니다.

에 대 한 자세한 내용은 [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/), 참조 [25 장](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Charles Petzold Xamarin.Forms 책의 합니다.

<a name="Populating_a_CarouselPage_with_a_Template" />

### <a name="populating-a-carouselpage-with-a-template"></a>템플릿 사용 하 여 CarouselPage 채우기

다음 XAML 코드 예제에서는 [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) 할당 하 여 생성 한 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) 에 [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) 에 대 한 페이지를 반환 하는 속성 컬렉션의 개체:

```xaml
<CarouselPage xmlns="http://xamarin.com/schemas/2014/forms"
              xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
              x:Class="CarouselPageNavigation.MainPage">
    <CarouselPage.ItemTemplate>
        <DataTemplate>
            <ContentPage>
                <ContentPage.Padding>
                  <OnPlatform x:TypeArguments="Thickness">
                    <On Platform="iOS, Android" Value="0,40,0,0" />
                  </OnPlatform>
                </ContentPage.Padding>
                <StackLayout>
                    <Label Text="{Binding Name}" FontSize="Medium" HorizontalOptions="Center" />
                    <BoxView Color="{Binding Color}" WidthRequest="200" HeightRequest="200" HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
                </StackLayout>
            </ContentPage>
        </DataTemplate>
    </CarouselPage.ItemTemplate>
</CarouselPage>
```

[ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) 설정 하 여 데이터를 채운는 [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/) 코드 숨김 파일에 대 한 생성자에서 속성:

```csharp
public MainPage ()
{
    ...
    ItemsSource = ColorsDataModel.All;
}
```

다음 코드 예제에 해당 하는 [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) C#에서 만들어진:

```csharp
public class MainPageCS : CarouselPage
{
    public MainPageCS ()
    {
        Thickness padding;
        switch (Device.RuntimePlatform)
        {
            case Device.iOS:
            case Device.Android:
                padding = new Thickness(0, 40, 0, 0);
                break;
            default:
                padding = new Thickness();
                break;
        }

        ItemTemplate = new DataTemplate (() => {
            var nameLabel = new Label {
                FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
                HorizontalOptions = LayoutOptions.Center
            };
            nameLabel.SetBinding (Label.TextProperty, "Name");

            var colorBoxView = new BoxView {
                WidthRequest = 200,
                HeightRequest = 200,
                HorizontalOptions = LayoutOptions.Center,
                VerticalOptions = LayoutOptions.CenterAndExpand
            };
            colorBoxView.SetBinding (BoxView.ColorProperty, "Color");

            return new ContentPage {
                Padding = padding,
                Content = new StackLayout {
                    Children = {
                        nameLabel,
                        colorBoxView
                    }
                }
            };
        });

        ItemsSource = ColorsDataModel.All;
    }
}
```

각 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 단순히 표시는 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 특정 색에 대 한 및 [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) 해당 색상의 합니다.

> [!NOTE]
> **참고**:는 [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) UI 가상화를 지원 하지 않습니다. 따라서 성능 영향을 받을 수는 경우는 `CarouselPage` 자식 요소가 너무 많습니다.

경우는 [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) 에 포함 되는 [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) 의 페이지는 [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/), [ `MasterDetailPage.IsGestureEnabled` ](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty/) 속성 설정 해야 `false` 간에 제스처 충돌을 방지 하는 `CarouselPage` 및 `MasterDetailPage`합니다.

에 대 한 자세한 내용은 [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/), 참조 [25 장](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Charles Petzold Xamarin.Forms 책의 합니다.

## <a name="summary"></a>요약

이 문서에 사용 하는 방법을 보여 주는 [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) 페이지의 컬렉션을 탐색할 수 있습니다. `CarouselPage` 는 갤러리와 같은 콘텐츠, 페이지 탐색 하려면 사용자가 왼쪽에서 오른쪽으로 살짝 수 있는 페이지가 고 자연스럽 고 Windows Phone 사용자에 게 익숙한 느낌 탐색 환경을 제공 합니다.


## <a name="related-links"></a>관련 링크

- [페이지 변형](~/xamarin-forms/user-interface/controls/pages.md)
- [CarouselPage (샘플)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/)
- [CarouselPageTemplate (샘플)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/)
- [CarouselPage](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)
