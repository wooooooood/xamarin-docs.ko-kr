---
title: 'title: “Xamarin.Forms 회전식 페이지” description: “Xamarin.Forms CarouselPage는 사용자가 옆으로 살짝 밀어서 갤러리와 같은 콘텐츠 페이지를 탐색할 수 있는 페이지입니다.'
description: '이 문서에서는 페이지의 컬렉션을 탐색하기 위해 CarouselPage를 사용하는 방법을 설명합니다.” ms.prod: xamarin ms.assetid: 2D14FC9D-DF5F-427E-9006-2AAE61ECF8DC ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 12/01/2017 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
ms.prod: xamarin
ms.assetid: 2D14FC9D-DF5F-427E-9006-2AAE61ECF8DC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 896f652d69bca0f186e53185926ee5c46d87fa7c
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84570676"
---
# <a name="xamarinforms-carousel-page"></a>Xamarin.Forms 회전식 페이지

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpage)

Xamarin.Forms CarouselPage는 사용자가 옆으로 살짝 밀어서 갤러리와 같은 콘텐츠 페이지를 탐색할 수 있는 페이지입니다. 이 문서에서는 페이지의 컬렉션을 탐색 하려면 CarouselPage를 사용하는 방법을 설명합니다.

> [!IMPORTANT]
> [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)는 [`CarouselView`](xref:Xamarin.Forms.CarouselView)로 대체되었습니다. CarouselView는 사용자가 스와이프하여 항목 컬렉션 간에 이동할 수 있는 스크롤 가능 레이아웃을 제공합니다. `CarouselView`에 대한 자세한 내용은 [Xamarin.Forms CarouselView](~/xamarin-forms/user-interface/carouselview/index.md)를 참조하세요.

다음 스크린샷은 각 플랫폼에서 [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)를 보여줍니다.

![](carousel-page-images/thirdpage.png "CarouselPage Third Item")

[`CarouselPage`](xref:Xamarin.Forms.CarouselPage)의 레이아웃은 각 플랫폼에서 동일합니다. 컬렉션을 통해 앞으로 이동하려면 오른쪽에서 왼쪽으로 살짝 밀고 컬렉션을 통해 뒤로 이동하려면 왼쪽에서 오른쪽을 살짝 밀어 페이지를 탐색할 수 있습니다. 다음 스크린샷에서는 [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) 인스턴스의 첫 번째 페이지를 보여줍니다.

![](carousel-page-images/firstpage.png "CarouselPage First Item")

다음 스크린샷에 표시된 것처럼 오른쪽에서 왼쪽으로 살짝 밀어 두 번째 페이지로 이동합니다.

![](carousel-page-images/secondpage.png "CarouselPage Second Item")

다시 오른쪽에서 왼쪽으로 살짝 밀면 세 번째 페이지로 이동하는 반면 왼쪽에서 오른쪽으로 살짝 밀면 이전 페이지로 되돌아갑니다.

> [!NOTE]
> [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)는 UI 가상화를 지원하지 않습니다. 따라서 `CarouselPage`에 너무 많은 자식 요소가 포함된 경우 성능에 영향을 미칠 수 있습니다.

[`CarouselPage`](xref:Xamarin.Forms.CarouselPage)가 [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)의 [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) 페이지에 포함된 경우 [`MasterDetailPage.IsGestureEnabled`](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty) 속성을 `false`로 설정하여 `CarouselPage` 및 `MasterDetailPage` 간에 제스처 충돌을 방지해야 합니다.

[`CarouselPage`](xref:Xamarin.Forms.CarouselPage)에 대한 자세한 내용은 Charles Petzold의 Xamarin.Forms 책의 [25장](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)을 참조하세요.

## <a name="create-a-carouselpage"></a>CarouselPage 만들기

두 방법을 [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)를 만드는 데 사용할 수 있습니다.

- [`ContentPage`](xref:Xamarin.Forms.ContentPage) 자식 인스턴스의 컬렉션으로 `CarouselPage`를 [채웁니다](#populate-a-carouselpage-with-a-page-collection).
- 컬렉션을 [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) 속성에 [할당](#populate-a-carouselpage-with-a-template)하고 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)을 [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) 속성에 할당하여 컬렉션 개체에 대해 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 인스턴스를 반환합니다.

두 방법 모두를 사용하면 `CarouselPage`에는 표시될 다음 페이지로 이동하는 살짝 밀기 상호 작용을 통해 각 페이지가 차례로 표시됩니다.

> [!NOTE]
> [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)는 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 인스턴스 또는 `ContentPage` 파생물로만 채울 수 있습니다.

### <a name="populate-a-carouselpage-with-a-page-collection"></a>페이지 컬렉션으로 CarouselPage 채우기

다음 XAML 코드 예제에서는 세 개 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 인스턴스가 표시되는 [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)를 보여줍니다.

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

다음 코드 예제에서는 C#에 해당하는 UI를 보여줍니다.

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

각 [`ContentPage`](xref:Xamarin.Forms.ContentPage)는 특정 색상에 대해 [`Label`](xref:Xamarin.Forms.Label)을, 해당 색상에 대해 [`BoxView`](xref:Xamarin.Forms.BoxView)를 표시합니다.

### <a name="populate-a-carouselpage-with-a-template"></a>템플릿으로 CarouselPage 채우기

다음 XAML 코드 예제에서는 컬렉션의 개체에 대해 페이지를 반환하도록 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)을 [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) 속성에 할당하여 생성된 [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)를 보여줍니다.

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

[`CarouselPage`](xref:Xamarin.Forms.CarouselPage)는 코드 숨김 파일에 대한 생성자에서 [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) 속성을 설정하여 데이터로 채웁니다.

```csharp
public MainPage ()
{
    ...
    ItemsSource = ColorsDataModel.All;
}
```

다음 코드 예제에서는 C#에서 만든 해당 [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)를 보여줍니다.

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

각 [`ContentPage`](xref:Xamarin.Forms.ContentPage)는 특정 색상에 대해 [`Label`](xref:Xamarin.Forms.Label)을, 해당 색상에 대해 [`BoxView`](xref:Xamarin.Forms.BoxView)를 표시합니다.

## <a name="related-links"></a>관련 링크

- [페이지 종류](~/xamarin-forms/user-interface/controls/pages.md)
- [CarouselPage(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpage)
- [CarouselPageTemplate(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpagetemplate)
- [CarouselPage](xref:Xamarin.Forms.CarouselPage)
