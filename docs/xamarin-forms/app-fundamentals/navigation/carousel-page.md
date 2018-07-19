---
title: Xamarin.Forms 회전식 페이지
description: Xamarin.Forms CarouselPage 페이지인 사용자 측면에서 살짝 밀 수 있는 갤러리와 같은 콘텐츠 페이지를 탐색 합니다. 이 문서에서는 페이지의 컬렉션을 탐색 하는 CarouselPage를 사용 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 2D14FC9D-DF5F-427E-9006-2AAE61ECF8DC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: bce3a60f3647a537906cfa11fc1dcfcc6f5cf365
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998611"
---
# <a name="xamarinforms-carousel-page"></a>Xamarin.Forms 회전식 페이지

_Xamarin.Forms CarouselPage 페이지인 사용자 측면에서 살짝 밀 수 있는 갤러리와 같은 콘텐츠 페이지를 탐색 합니다. 이 문서에서는 페이지의 컬렉션을 탐색 하는 CarouselPage를 사용 하는 방법을 보여 줍니다._

## <a name="overview"></a>개요

다음 스크린샷에서 표시 된 [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) 각 플랫폼에서:

![](carousel-page-images/thirdpage.png "CarouselPage Thid 항목")

레이아웃을 [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) 각 플랫폼에서 동일 합니다. 컬렉션을 통해 이동 하려면 오른쪽에서 왼쪽으로 살짝 밀어 및 이전 버전과 컬렉션 탐색을 왼쪽에서 오른쪽을 살짝 밀어를 통해 페이지를 탐색할 수 있습니다. 다음 스크린샷에서 표시의 첫 페이지에는 [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) 인스턴스:

![](carousel-page-images/firstpage.png "CarouselPage 첫 번째 항목")

다음 스크린샷에서 표시 된 것 처럼 오른쪽에서 왼쪽된 이동으로 두 번째 페이지 넘기기가:

![](carousel-page-images/secondpage.png "CarouselPage 두 번째 항목")

다시 오른쪽에서 왼쪽으로 살짝 왼쪽에서 오른쪽으로 살짝 이전 페이지를 반환 하는 동안 세 번째 페이지로 이동 합니다.

<!--
> [!NOTE]
> The [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) has been deprecated, and will be removed from Xamarin.Forms in a future release. Instead, the [`CarouselView`](xref:Xamarin.Forms.CarouselView) should be used to provide a gallery-like view, where users can swipe from side to side to move through a collection of items.
-->

## <a name="creating-a-carouselpage"></a>CarouselPage 만들기

두 가지 방법을 만드는 데 사용할 수는 [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage):

- [채울](#Populating_a_CarouselPage_with_a_Page_Collection) 는 `CarouselPage` 자식 컬렉션과 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 인스턴스.
- [할당](#Populating_a_CarouselPage_with_a_Template) 컬렉션을 합니다 [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) 속성 및 할당을 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 에 [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) 반환할속성[ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 컬렉션의 개체에 대 한 인스턴스.

두 방법으로는 `CarouselPage` 됩니다 표시 한 다음 각 페이지를 표시 하 여 다음 페이지로 이동 하는 스와이프 상호 작용을 사용 하 여 합니다.

> [!NOTE]
> A [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) 으로 채울 수 있습니다 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 인스턴스 또는 `ContentPage` 파생 합니다.

<a name="Populating_a_CarouselPage_with_a_Page_Collection" />

### <a name="populating-a-carouselpage-with-a-page-collection"></a>페이지 컬렉션으로 CarouselPage 채우기

다음 XAML 코드 예제는 [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) 표시 하는 세 가지 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 인스턴스:

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

다음 코드 예제에서는 C#에서 해당 UI를 보여 줍니다.

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

각 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 표시를 [ `Label` ](xref:Xamarin.Forms.Label) 특정 색 및 [ `BoxView` ](xref:Xamarin.Forms.BoxView) 색입니다.

> [!NOTE]
> 합니다 [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) UI 가상화를 지원 하지 않습니다. 경우 성능이 저하 될 수 있습니다 따라서는 `CarouselPage` 너무 많은 자식 요소가 포함 됩니다.

경우는 [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) 에 포함 되는 [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) 페이지를 [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage)의 [ `MasterDetailPage.IsGestureEnabled` ](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty) 속성에 설정할 `false` 간에 제스처 충돌을 방지 하는 `CarouselPage` 및 `MasterDetailPage`합니다.

에 대 한 자세한 내용은 합니다 [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage)를 참조 하세요 [25 장](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Charles Petzold의 Xamarin.Forms 책의 합니다.

<a name="Populating_a_CarouselPage_with_a_Template" />

### <a name="populating-a-carouselpage-with-a-template"></a>템플릿 사용 하 여 CarouselPage 채우기

다음 XAML 코드 예제에서는 [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) 할당 하 여 생성을 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) 에 [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) 에 대 한 페이지를 반환 하도록 속성 컬렉션의 개체:

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

합니다 [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) 설정 하 여 데이터를 채운 합니다 [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) 코드 숨김 파일에 대 한 생성자에서 속성:

```csharp
public MainPage ()
{
    ...
    ItemsSource = ColorsDataModel.All;
}
```

다음 코드 예제에서는 해당 [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) C#에서 만든:

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

각 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 표시를 [ `Label` ](xref:Xamarin.Forms.Label) 특정 색 및 [ `BoxView` ](xref:Xamarin.Forms.BoxView) 색입니다.

> [!NOTE]
> 합니다 [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) UI 가상화를 지원 하지 않습니다. 경우 성능이 저하 될 수 있습니다 따라서는 `CarouselPage` 너무 많은 자식 요소가 포함 됩니다.

경우는 [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) 에 포함 되는 [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) 페이지를 [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage)의 [ `MasterDetailPage.IsGestureEnabled` ](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty) 속성에 설정할 `false` 간에 제스처 충돌을 방지 하는 `CarouselPage` 및 `MasterDetailPage`합니다.

에 대 한 자세한 내용은 합니다 [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage)를 참조 하세요 [25 장](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Charles Petzold의 Xamarin.Forms 책의 합니다.

## <a name="summary"></a>요약

이 문서에 사용 하는 방법을 보여 줍니다는 [ `CarouselPage` ](xref:Xamarin.Forms.CarouselPage) 페이지의 컬렉션을 이동 합니다. `CarouselPage` 페이지인 사용자 측면에서 살짝 밀 수 있는 콘텐츠 갤러리 매우 유사 하 게 페이지를 탐색 합니다.


## <a name="related-links"></a>관련 링크

- [페이지 종류](~/xamarin-forms/user-interface/controls/pages.md)
- [CarouselPage (샘플)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/)
- [CarouselPageTemplate (샘플)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/)
- [CarouselPage](xref:Xamarin.Forms.CarouselPage)
