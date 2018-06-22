---
title: Xamarin.Forms FlexLayout
description: 누적 또는 자식 뷰의 컬렉션을 배치에 대 한 FlexLayout를 사용 합니다.
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
ms.custom: xamu-video
author: charlespetzold
ms.author: chape
ms.date: 05/07/2018
ms.openlocfilehash: 7585138cd6c33c2a5dc537ba28101a84e1c4b7ae
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
ms.locfileid: "33921838"
---
# <a name="the-xamarinforms-flexlayout"></a>Xamarin.Forms FlexLayout

_누적 또는 자식 뷰의 컬렉션을 배치에 대 한 FlexLayout를 사용 합니다._

Xamarin.Forms는 [ `FlexLayout` ](xref:Xamarin.Forms.FlexLayout) Xamarin.Forms 버전 3.0의에서 새로운 기능입니다. CSS에 기반 하는 [유연한 상자 레이아웃 모듈](http://www.w3.org/TR/css-flexbox-1/)으로 일반적으로 알려진, _레이아웃 flex_ 또는 _flex 상자_, 자식을 정렬 하는 많은 유연성 있는 옵션을 포함 하기 때문에 이렇게 부름 내 레이아웃입니다.

`FlexLayout` Xamarin.Forms는 유사한 [ `StackLayout` ](~/xamarin-forms/user-interface/layouts/stack-layout.md) 것 수 자식을 가로 또는 세로로 정렬할 스택으로 한다는 점에서 합니다. 그러나는 `FlexLayout` 는 또한 수 있는 단일 행 이나 열에 맞게 너무 많은 경우 자식 래핑 있으며 방향, 맞춤 및 다양 한 화면 크기에 맞게 조정에 대 한 많은 옵션도 있습니다.

`FlexLayout` 파생 [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) 상속는 [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) 형식의 속성이 `IList<View>`합니다.

`FlexLayout` 6 개의 바인딩 가능 속성을 공개 및 크기, 방향 및 해당 자식 요소의 맞춤에 영향을 주는 다섯 개의 연결 된 바인딩 가능한 속성을 정의 합니다. (연결 된 바인딩 가능한 속성 모르는 경우 문서를 참조  **[연결 된 속성](~/xamarin-forms/xaml/attached-properties.md)**.) 이러한 속성에 아래 섹션에서 자세히 설명 되어 **[자세히 바인딩 가능 속성](#bindable-properties)** 및  **[세부정보에서연결된된바인딩가능한속성](#attached-properties)**. 이 문서에서 일부 섹션으로 시작 하는 반면 **[일반적인 사용 시나리오](#common-scenarios)** 의 `FlexLayout` 를 설명 하는 이러한 속성 중 상당수 비공식적으로 더 합니다. 문서의 끝으로 결합 하는 방법을 확인할 `FlexLayout` 와 [CSS 스타일 시트](~/xamarin-forms/user-interface/styles/css/index.md)합니다.

<a name="common-scenarios" />

## <a name="common-usage-scenarios"></a>일반적인 사용 시나리오

**[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** 샘플 프로그램에 여러 페이지의 몇 가지 일반적인 사용 하 여 해당 demonstate `FlexLayout` 해당 속성을 시험해 볼 수 있습니다.

### <a name="using-flexlayout-for-a-simple-stack"></a>FlexLayout 간단한 스택을 사용 하 여

**간단한 스택** 방법을 보여 줍니다 페이지 `FlexLayout` 대체할 수 있는 한 `StackLayout` 있지만 간단한 태그와 함께 합니다. 이 샘플의 모든 항목은 XAML 페이지에 정의 됩니다. `FlexLayout` 자식 항목 네 개 포함 되어 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:FlexLayoutDemos"
             x:Class="FlexLayoutDemos.SimpleStackPage"
             Title="Simple Stack">

    <FlexLayout Direction="Column"
                AlignItems="Center"
                JustifyContent="SpaceEvenly">

        <Label Text="FlexLayout in Action"
               FontSize="Large" />

        <Image Source="{local:ImageResource FlexLayoutDemos.Images.SeatedMonkey.jpg}" />

        <Button Text="Do-Nothing Button" />

        <Label Text="Another Label" />
    </FlexLayout>
</ContentPage>
```

IOS, Android 및 유니버설 Windows 플랫폼에서 실행 되 고 해당 페이지는 다음과 같습니다.

[![간단한 스택 페이지](flex-layout-images/SimpleStack.png "간단한 스택 페이지")](flex-layout-images/SimpleStack-Large.png#lightbox)

세 가지 속성 `FlexLayout` 에 표시 되는 **SimpleStackPage.xaml** 파일:

- [ `Direction` ](xref:Xamarin.Forms.FlexLayout.Direction) 속성의 값으로 설정 되는 [ `FlexDirection` ](xref:Xamarin.Forms.FlexDirection) 열거 합니다. 기본값은 `Row`입니다. 이 속성을 설정 `Column` 하면의 하위는 `FlexLayout` 를 항목의 단일 열에 정렬 합니다.

    때 항목에 `FlexLayout` 열에 정렬 되는 `FlexLayout` 세로 했다고 _주 축_ 및 가로 _축 교차_합니다.

- [ `AlignItems` ](xref:Xamarin.Forms.FlexLayout.AlignItems) 형식의 속성은 [ `FlexAlignItems` ](xref:Xamarin.Forms.FlexAlignItems) 교차 축에 항목을 정렬 하는 방법을 지정 합니다. `Center` 옵션을 사용 하면 각 항목을 가로로 가운데에 맞춰야 합니다.

    사용 하는 경우는 `StackLayout` 아닌 `FlexLayout` 이 작업을 할당 하 여 모든 항목 센터는 `HorizontalOptions` 각 항목의 속성 `Center`합니다. `HorizontalOptions` 속성의 자식에 대 한 작동 하지 않습니다는 `FlexLayout`, 하지만 단일 `AlignItems` 속성 같은 목표를 수행 합니다. 를 해야 하는 경우 사용할 수 있습니다는 `AlignSelf` 연결 된 바인딩 가능한 속성을 재정의 하 여 `AlignItems` 개별 항목에 대 한 속성:

    ```xaml
    <Label Text="FlexLayout in Action"
           FontSize="Large"
           FlexLayout.AlignSelf="Start" />
    ```

    이 변경으로 인해 `Label` 의 왼쪽된 가장자리에 배치 되는 `FlexLayout` 때 읽기 순서는 왼쪽에서 오른쪽입니다.

- [ `JustifyContent` ](xref:Xamarin.Forms.FlexLayout.JustifyContent) 형식의 속성은 [ `FlexJustify` ](xref:Xamarin.Forms.FlexJustify), 기본 축에 항목이 정렬 되는 방법을 지정 합니다. `SpaceEvenly` 모든 잔여 세로 공간 모든 항목 간에 균등 하 게 하 고 첫 번째 항목을 위와 아래의 마지막 항목을 할당 하는 옵션입니다.

    사용 하는 경우는 `StackLayout`를 할당 해야는 `VerticalOptions` 각 항목의 속성 `CenterAndExpand` 비슷한 효과 달성 하기 위해. 하지만 `CenterAndExpand` 옵션 각 항목 사이의 보다 첫째 항목 앞과 마지막 항목 뒤 두 배 많은 공간을 할당 합니다. 모방할 수 있습니다는 `CenterAndExpand` 옵션의 `VerticalOptions` 설정 하 여는 `JustifyContent` 속성 `FlexLayout` 를 `SpaceAround`합니다.

이러한 `FlexLayout` 속성 섹션에서 자세하게에서 설명 **[자세히 바인딩 가능 속성](#bindable-properties)** 아래 합니다.

### <a name="using-flexlayout-for-wrapping-items"></a>FlexLayout를 사용 하 여 항목을 배치에 대 한

**사진 래핑** 의 페이지는 **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** 샘플 방법을 `FlexLayout` 자식을 추가 행 이나 열으로 줄 바꿈될 수입니다. XAML 파일에서 인스턴스화하는 `FlexLayout` 의 두 가지 속성을 할당 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FlexLayoutDemos.PhotoWrappingPage"
             Title="Photo Wrapping">
    <Grid>
        <ScrollView>
            <FlexLayout x:Name="flexLayout"
                        Wrap="Wrap"
                        JustifyContent="SpaceAround" />
        </ScrollView>

        <ActivityIndicator x:Name="activityIndicator"
                           IsRunning="True"
                           VerticalOptions="Center" />
    </Grid>
</ContentPage>
```

`Direction` 속성 `FlexLayout` 설정 되지 않은 경우 기본 설정인 않았으므로 `Row`, 즉 자식 행에 정렬 되 고 기본 축이 가로입니다.

[ `Wrap` ](xref:Xamarin.Forms.FlexLayout.Wrap) 열거형 형식의 속성은 [ `FlexWrap` ](xref:Xamarin.Forms.FlexWrap)합니다. 행에 너무 많은 항목이 있으면이 속성 설정 하면 항목을 다음 행으로 래핑합니다.

에 `FlexLayout` 의 자식인는 `ScrollView`합니다. 페이지에 맞게 행이 너무 많은 경우 하면 `ScrollView` 기본 `Orientation` 속성 `Vertical` 세로 스크롤할 수 있도록 합니다.

`JustifyContent` 속성 각 항목 주위에 같은 크기의 빈 공간을 주 축 (가로 축)에 남은 공간을 할당 합니다.

코드 숨김 파일 액세스 하는 샘플 사진 컬렉션에 추가 합니다.는 `Children` 의 컬렉션은 `FlexLayout`:

```csharp
public partial class PhotoWrappingPage : ContentPage
{
    // Class for deserializing JSON list of sample bitmaps
    [DataContract]
    class ImageList
    {
        [DataMember(Name = "photos")]
        public List<string> Photos = null;
    }

    public PhotoWrappingPage ()
    {
        InitializeComponent ();

        LoadBitmapCollection();
    }

    async void LoadBitmapCollection()
    {
        int imageDimension = Device.RuntimePlatform == Device.iOS ||
                             Device.RuntimePlatform == Device.Android ? 240 : 120;

        string urlSuffix = String.Format("?width={0}&height={0}&mode=max", imageDimension);

        using (WebClient webClient = new WebClient())
        {
            try
            {
                // Download the list of stock photos
                Uri uri = new Uri("http://docs.xamarin.com/demo/stock.json");
                byte[] data = await webClient.DownloadDataTaskAsync(uri);

                // Convert to a Stream object
                using (Stream stream = new MemoryStream(data))
                {
                    // Deserialize the JSON into an ImageList object
                    var jsonSerializer = new DataContractJsonSerializer(typeof(ImageList));
                    ImageList imageList = (ImageList)jsonSerializer.ReadObject(stream);

                    // Create an Image object for each bitmap
                    foreach (string filepath in imageList.Photos)
                    {
                        Image image = new Image
                        {
                            Source = ImageSource.FromUri(new Uri(filepath + urlSuffix))
                        };
                        flexLayout.Children.Add(image);
                    }
                }
            }
            catch
            {
                flexLayout.Children.Add(new Label
                {
                    Text = "Cannot access list of bitmap files"
                });
            }
        }

        activityIndicator.IsRunning = false;
        activityIndicator.IsVisible = false;
    }
}
```

다음은 점진적으로 아래로 스크롤할 위쪽에서 세 가지 플랫폼에서 실행 중인 프로그램입니다.

[![사진 래핑 페이지](flex-layout-images/PhotoWrapping.png "사진 래핑 페이지")](flex-layout-images/PhotoWrapping-Large.png#lightbox)

### <a name="page-layout-with-flexlayout"></a>FlexLayout 포함 된 페이지 레이아웃

웹 디자인 호출 중인 표준 레이아웃의 [ _신성 grail_ ](https://en.wikipedia.org/wiki/Holy_grail_(web_design)) 매우 효과적 이지만 자주 하드 완벽 하 게 구워진을 나타내려고 하 되는 레이아웃 형식 이므로 합니다. 레이아웃은 페이지의 위쪽에는 머리글 및 바닥글이 페이지의 전체 너비로 확장 맨 아래에 구성 됩니다. 콘텐츠 및 보충 정보의 왼쪽에 열 메뉴와 않지만 종종 주 콘텐츠는 사용 하 여 페이지의 가운데 (라고도 _따로_ 영역) 오른쪽에 있습니다. [CSS 유연한 상자 레이아웃 사양의 섹션 5.4.1](http://www.w3.org/TR/css-flexbox-1/#order-accessibility) flex 상자가 신성 grail 레이아웃 수 구현 하는 방법을 설명 합니다.

**신성 Grail 레이아웃** 의 페이지는 **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** 샘플 하나를 사용 하 여이 레이아웃의 간단한 구현을 보여 줍니다 `FlexLayout` 또 다른 중첩 합니다. 이 페이지는 설계 되었으므로 세로 모드로 휴대폰에 대해, 콘텐츠 영역 왼쪽 및 오른쪽 영역 에서만 50 픽셀입니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FlexLayoutDemos.HolyGrailLayoutPage"
             Title="Holy Grail Layout">

    <FlexLayout Direction="Column">

        <!-- Header -->
        <Label Text="HEADER"
               FontSize="Large"
               BackgroundColor="Aqua"
               HorizontalTextAlignment="Center" />

        <!-- Body -->
        <FlexLayout FlexLayout.Grow="1">

            <!-- Content -->
            <Label Text="CONTENT"
                   FontSize="Large"
                   BackgroundColor="Gray"
                   HorizontalTextAlignment="Center"
                   VerticalTextAlignment="Center"
                   FlexLayout.Grow="1" />

            <!-- Navigation items-->
            <BoxView FlexLayout.Basis="50"
                     FlexLayout.Order="-1"
                     Color="Blue" />

            <!-- Aside items -->
            <BoxView FlexLayout.Basis="50"
                     Color="Green" />

        </FlexLayout>

        <!-- Footer -->
        <Label Text="FOOTER"
               FontSize="Large"
               BackgroundColor="Pink"
               HorizontalTextAlignment="Center" />
    </FlexLayout>
</ContentPage>
```

다음 세 가지 플랫폼에서 실행 중인:

[![신성 Grail 레이아웃 페이지](flex-layout-images/HolyGrailLayout.png "신성 Grail 레이아웃 페이지")](flex-layout-images/HolyGrailLayout-Large.png#lightbox)

탐색 및 aside 영역이 렌더링 되는 `BoxView` 왼쪽 및 오른쪽에 있습니다.

첫 번째 `FlexLayout` xaml에서 파일에 주 세로 축이 및 열으로 정렬 하는 세 명의 자식을 포함 합니다. 이들은, 머리글, 페이지 및 바닥글의 본문입니다. 중첩 된 `FlexLayout` 에 가로 주요 축 세 명의 자식이 행으로 정렬 된 합니다.

이 프로그램에 3 개의 연결 된 바인딩 가능한 속성을 보여 줍니다.

- `Order` 연결 된 바인딩 가능한 속성이 설정 된 첫 번째 `BoxView`합니다. 이 속성은 기본값인 0으로 정수입니다. 레이아웃 순서를 변경 하려면이 속성을 사용할 수 있습니다. 일반적으로 개발자가 태그 탐색 항목 이전에 표시 되도록 페이지의 콘텐츠를 선호 하 고 예약할 항목. 설정의 `Order` 첫 번째 속성 `BoxView` 값으로 다른 형제 미만으로 인해 행의 첫 번째 항목으로 표시 합니다. 마찬가지로, 확신할 수 있는 항목을 설정 하 여 마지막 나타나는지는 `Order` 형제 보다 큰 값으로 속성입니다.

- `Basis` 연결 된 바인딩 가능한 속성은 두에 설정 `BoxView` 에 50 픽셀의 너비를 지정 하는 항목입니다. 이 속성은 형식의 `FlexBasis`, 형식의 정적 속성을 정의 하는 구조 `FlexBasis` 라는 `Auto`, 기본값입니다. 사용할 수 있습니다 `Basis` 공간 크기를 나타내는 백분율 또는 픽셀 크기를 지정 하는 항목 주 축에서 차지 합니다. 호출 되는 _별로_ 이후의 모든 레이아웃의 기준이 되는 항목 크기를 지정 하기 때문에 있습니다.

- `Grow` 속성이 설정 된 중첩에 `Layout` 및는 `Label` 자식 콘텐츠를 나타내는입니다. 이 속성은 형식이 `float` 하며 0의 기본값입니다. 양수 값으로 설정 된 경우 기본 축 따라 나머지 공간을 모두 할당 해당 항목을의 양수 값을 사용 하 여 형제를 `Grow`합니다. 별표 사양에 처럼 값에는 공간이 비례적으로 할당 한 `Grid`합니다.

    첫 번째 `Grow` 연결 된 속성을 설정에 중첩 된 `FlexLayout`한다는 표시 이므로이 `FlexLayout` 내 외부에서 사용 되지 않는 모든 세로 공간을 차지 하는 것 `FlexLayout`합니다. 두 번째 `Grow` 연결 된 속성에 설정 되어는 `Label` 내부 내에서 사용 되지 않는 모든 가로 공간을 차지 하도록이 콘텐츠 임을 나타내는 내용을 나타내는 `FlexLayout`합니다.

    이기도 비슷한 `Shrink` 연결 된 자식 항목의 크기의 크기를 초과 하는 경우 사용할 수 있는 바인딩 가능한 속성은 `FlexLayout` 래핑이 적절 하지 않지만 합니다.

### <a name="catalog-items-with-flexlayout"></a>FlexLayout 된 카탈로그 항목

**카탈로그 항목** 페이지에 **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** 샘플은 유사한 [CSSFlex레이아웃상자사양의섹션1.1예1](http://www.w3.org/TR/css-flexbox-1/#overview)제외 하 고 가로로 스크롤할 수 있는 일련의 사진 및 세 가지 원숭이 대 한 설명을 표시 합니다.

[![카탈로그 항목 페이지](flex-layout-images/CatalogItems.png "카탈로그 항목 페이지")](flex-layout-images/CatalogItems-Large.png#lightbox)

각각 세 개의 원숭이 `FlexLayout` 에 포함 된 한 `Frame` 명시적 높이 너비를 지정 하 고 더 큰 규모의 자식 이기도 `FlexLayout`합니다. 이 XAML 파일의 속성 대부분에는 `FlexLayout` 자식 스타일은 제품군 이며 암시적 스타일은 하나만 남기고 모두에 지정 된:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:FlexLayoutDemos"
             x:Class="FlexLayoutDemos.CatalogItemsPage"
             Title="Catalog Items">
    <ContentPage.Resources>
        <Style TargetType="Frame">
            <Setter Property="BackgroundColor" Value="LightYellow" />
            <Setter Property="BorderColor" Value="Blue" />
            <Setter Property="Margin" Value="10" />
            <Setter Property="CornerRadius" Value="15" />
        </Style>

        <Style TargetType="Label">
            <Setter Property="Margin" Value="0, 4" />
        </Style>

        <Style x:Key="headerLabel" TargetType="Label">
            <Setter Property="Margin" Value="0, 8" />
            <Setter Property="FontSize" Value="Large" />
            <Setter Property="TextColor" Value="Blue" />
        </Style>

        <Style TargetType="Image">
            <Setter Property="FlexLayout.Order" Value="-1" />
            <Setter Property="FlexLayout.AlignSelf" Value="Center" />
        </Style>

        <Style TargetType="Button">
            <Setter Property="Text" Value="LEARN MORE" />
            <Setter Property="FontSize" Value="Large" />
            <Setter Property="TextColor" Value="White" />
            <Setter Property="BackgroundColor" Value="Green" />
            <Setter Property="BorderRadius" Value="20" />
        </Style>
    </ContentPage.Resources>

    <ScrollView Orientation="Both">
        <FlexLayout>
            <Frame WidthRequest="300"
                   HeightRequest="480">

                <FlexLayout Direction="Column">
                    <Label Text="Seated Monkey"
                           Style="{StaticResource headerLabel}" />
                    <Label Text="This monkey is laid back and relaxed, and likes to watch the world go by." />
                    <Label Text="  &#x2022; Doesn't make a lot of noise" />
                    <Label Text="  &#x2022; Often smiles mysteriously" />
                    <Label Text="  &#x2022; Sleeps sitting up" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.SeatedMonkey.jpg}"
                           WidthRequest="180"
                           HeightRequest="180" />
                    <Label FlexLayout.Grow="1" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame WidthRequest="300"
                   HeightRequest="480">

                <FlexLayout Direction="Column">
                    <Label Text="Banana Monkey"
                           Style="{StaticResource headerLabel}" />
                    <Label Text="Watch this monkey eat a giant banana." />
                    <Label Text="  &#x2022; More fun than a barrel of monkeys" />
                    <Label Text="  &#x2022; Banana not included" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.Banana.jpg}"
                           WidthRequest="240"
                           HeightRequest="180" />
                    <Label FlexLayout.Grow="1" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame WidthRequest="300"
                   HeightRequest="480">

                <FlexLayout Direction="Column">
                    <Label Text="Face-Palm Monkey"
                           Style="{StaticResource headerLabel}" />
                    <Label Text="This monkey reacts appropriately to ridiculous assertions and actions." />
                    <Label Text="  &#x2022; Cynical but not unfriendly" />
                    <Label Text="  &#x2022; Seven varieties of grimaces" />
                    <Label Text="  &#x2022; Doesn't laugh at your jokes" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.FacePalm.jpg}"
                           WidthRequest="180"
                           HeightRequest="180" />
                    <Label FlexLayout.Grow="1" />
                    <Button />
                </FlexLayout>
            </Frame>
        </FlexLayout>
    </ScrollView>
</ContentPage>
```

에 대 한 암시적 스타일의 `Image` 의 두 개의 연결 된 바인딩 가능한 속성의 설정이 포함 되어 `Flexlayout`:

```xaml
<Style TargetType="Image">
    <Setter Property="FlexLayout.Order" Value="-1" />
    <Setter Property="FlexLayout.AlignSelf" Value="Center" />
</Style>
```

`Order` 설정인 &ndash;1 원인은 `Image` 요소는 중첩 된 각 처음에 표시를 `FlexLayout` children 컬렉션 내에서 위치에 관계 없이 뷰. `AlignSelf` 속성 `Center` 하면는 `Image` 내에서 가운데 맞춤 될는 `FlexLayout`합니다. 이 옵션의 설정을 재정의 `AlignItems` 에 기본값이 있는 속성의 `Stretch`즉, 하는 `Label` 및 `Button` 자식을 확장 하 여의 전체 너비는 `FlexLayout`합니다.

세 개의 각 내 `FlexLayout` 는 빈 값을 볼 `Label` 앞에 `Button`, 했으나는 `Grow` 1을 설정 합니다. 모든 여분의 세로 공간을이 공백으로 할당은 의미 `Label`는 효과적으로 푸시는 `Button` 맨 아래에 있습니다.

<a name="bindable-properties" />

## <a name="the-bindable-properties-in-detail"></a>세부 정보에 바인딩 가능한 속성

일반적인 몇 가지를 살펴 보았으며 했으므로 `FlexLayout`, 속성을 `FlexLayout` 자세히 탐색할 수 있습니다. 
`FlexLayout` 에 설정한 여섯 개의 바인딩 가능한 속성 정의 `FlexLayout` 코드 또는 XAML 컨트롤 orientatin 및 맞춤을 자체입니다. (이러한 속성 중 하나 [ `Position` ](xref:Xamarin.Forms.FlexLayout.Position),이 문서에서는 다루지 않습니다.)

실험할 수 바인딩 가능 속성을 사용 하 여 남은 5는 **실험** 의 페이지는 **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** 샘플. 이 페이지에서는 추가 또는 자식을에서 제거할 수 있습니다는 `FlexLayout` 의 5 개 바인딩 가능한 속성 조합을 설정할 수 있습니다. 모든 자식은 `FlexLayout` 는 `Label` 다양 한 색 및 크기의 보기와는 `Text` 속성이 숫자에 해당 하는에서 위치를 설정의 `Children` 컬렉션입니다.

프로그램이 시작 될 때, 포함 된 5 `Picker` 이러한 5의 기본 값을 표시 하는 뷰 `FlexLayout` 속성입니다. `FlexLayout` 세 명의 자식을 포함 하는 화면 아래쪽으로 이동 합니다.

[![실험 페이지: 기본](flex-layout-images/ExperimentDefault.png "실험 페이지-기본")](flex-layout-images/ExperimentDefault-Large.png#lightbox)

각각의 `Label` 보기에는 할당 된 공간을 보여 주는 회색 배경이 `Label` 내에서 `FlexLayout`합니다. 배경을 `FlexLayout` Alice 파랑은 자체입니다. 왼쪽 및 오른쪽에서 작은 여백을 제외 하 고 페이지의 전체 아래쪽 영역을 차지합니다.

<a name="direction" />

### <a name="the-direction-property"></a>Direction 속성

[ `Direction` ](xref:Xamarin.Forms.FlexLayout.Direction) 형식의 속성은 [ `FlexDirection` ](xref:Xamarin.Forms.FlexDirection), 4 개 멤버가 포함 된 열거:

- `Column`
- `ColumnReverse` (또는 "열 역방향" XAML에서)
- `Row`기본값
- `RowReverse` (또는 "행 반복" XAML에서)

Xaml에서는 열거형 멤버 이름은 소문자, 대문자, 사용 하 여이 속성의 값을 지정할 수 있습니다 또는 혼합된 대/소문자 또는 있습니다 CSS 표시기로 동일 하 게 괄호 안에 표시 하는 두 개의 추가 문자열을 사용할 수 있습니다. ("열 반복" 및 "행 반복" 문자열에 정의 된는 [ `FlexDirectionTypeConverter` ](xref:Xamarin.Forms.FlexDirectionTypeConverter) XAML 파서에서 사용 되는 클래스입니다.)

다음은 **실험** (왼쪽에서 오른쪽)를 보여 주는 페이지는 `Row` 방향, `Column` 방향 및 `ColumnReverse` 방향:

[![실험 페이지: 방향](flex-layout-images/ExperimentDirection.png "실험 페이지-방향")](flex-layout-images/ExperimentDirection-Large.png#lightbox)

에 대 한 다음에 유의 `Reverse` 옵션 항목의 오른쪽 또는 아래쪽에서 시작 합니다.

<a name="wrap" />

### <a name="the-wrap-property"></a>줄 바꿈 속성

[ `Wrap` ](xref:Xamarin.Forms.FlexLayout.Wrap) 형식의 속성은 [ `FlexWrap` ](xref:Xamarin.Forms.FlexWrap), 세 멤버가 포함 된 열거:

- `NoWrap`기본값
- `Wrap`
- `Reverse` (또는 "래핑 역방향" XAML에서)

이 화면 왼쪽에서 오른쪽으로 표시 된 `NoWrap`, `Wrap` 및 `Reverse` 12 자식에 대 한 옵션:

[![실험 페이지: 래핑할](flex-layout-images/ExperimentWrap.png "실험 페이지-줄 바꿈")](flex-layout-images/ExperimentWrap-Large.png#lightbox)

때는 `Wrap` 속성이 `NoWrap` 주 축 메모리가 제한 (예:이 프로그램) 및 기본 축이 전각 또는 모든 자식에 맞게 충분히 크게 만듭니다는 `FlexLayout` iOS 스크린 샷으로 더 작은 항목을 확인 하려고 합니다. 보여 줍니다. 사용 하 여 항목 shrinkness 제어할 수 있습니다는 [ `Shrink` ](#shrink) 연결 된 바인딩 가능한 속성입니다.

<a name="justify-content" />

### <a name="the-justifycontent-property"></a>JustifyContent 속성

[ `JustifyContent` ](xref:Xamarin.Forms.FlexLayout.JustifyContent) 형식의 속성은 [ `FlexJustify` ](xref:Xamarin.Forms.FlexJustify), 6 개 멤버가 포함 된 열거:

- `Start` (또는 "flex start" XAML에서) 기본값
- `Center`
- `End` (또는 "flex end" XAML에서)
- `SpaceBetween` (또는 "공간-" XAML에서)
- `SpaceAround` (또는 "공간-주위" XAML에서)
- `SpaceEvenly`

이 속성을이 예제에서 가로 축의 주 축에 항목의 간격이 방법을 지정 합니다.

[![실험 페이지: 콘텐츠를 정당화](flex-layout-images/ExperimentJustifyContent.png "실험 페이지-콘텐츠 양쪽 맞춤")](flex-layout-images/ExperimentJustifyContent-Large.png#lightbox)

모든 세 스크린샷에서 `Wrap` 속성이 `Wrap`합니다. `Start` 기본 Android 이전 스크린 샷에 표시 됩니다. IOS 여기 스크린샷에서 `Center` 옵션: 센터에 모든 항목은 이동 합니다. 세 가지 다른 옵션으로 시작 `Space` 항목에 의해 차지 하지 않는 추가 공간을 할당 합니다. `SpaceBetween` 항목; 간에 균등 하 게 공간 할당 `SpaceAround` 넣습니다 같은 각 항목 주위의 공간 동안 `SpaceEvenly` 넣습니다 같은 공간 각 항목 사이 및 첫 번째 항목 전과 후의 행에서 마지막 항목입니다.

<a name="align-items" />

### <a name="the-alignitems-property"></a>AlignItems 속성

[ `AlignItems` ](xref:Xamarin.Forms.FlexLayout.AlignItems) 형식의 속성은 [ `FlexAlignItems` ](xref:Xamarin.Forms.FlexAlignItems), 4 개 멤버가 포함 된 열거:

- `Stretch`기본값
- `Center`
- `Start` (또는 "flex start" XAML에서)
- `End` (또는 "flex end" XAML에서)

이 두 속성 중 하나입니다 (다른 되 고 [ `AlignContent` ](#align-content)) 자식 항목은 교차 축에 정렬 되는 방법을 나타내는입니다. 각 행 내에서 자식 (같이 이전 스크린샷에서), 늘어나거나 다음 세 가지 스크린샷에서 같이 시작, 가운데 또는 각 항목의 끝에 정렬:

[![실험 페이지: 항목을 정렬 하](flex-layout-images/ExperimentAlignItems.png "실험 페이지-항목 정렬")](flex-layout-images/ExperimentAlignItems-Large.png#lightbox)

IOS 스크린 샷에 모든 자식 항목의 위쪽을 맞춥니다. Android 스크린샷에서 항목 세로로 가운데 맞춤 됩니다 가장 높은 자식에 따라. UWP 화면에 있는 모든 항목의 아래쪽에 따라 정렬 됩니다.

모든 개별 항목에 대 한는 `AlignItems` 설정으로 재정의 될 수는 [ `AlignSelf` ](#align-self) 연결 된 바인딩 가능한 속성입니다.

<a name="align-content" />

### <a name="the-aligncontent-property"></a>AlignContent 속성

[ `AlignContent` ](xref:Xamarin.Forms.FlexLayout.AlignContent) 형식의 속성은 [ `FlexAlignContent` ](xref:Xamarin.Forms.FlexAlignContent), 7 개 멤버가 포함 된 열거:

- `Stretch`기본값
- `Center`
- `Start` (또는 "flex start" XAML에서)
- `End` (또는 "flex end" XAML에서)
- `SpaceBetween` (또는 "공간-" XAML에서)
- `SpaceAround` (또는 "공간-주위" XAML에서)
- `SpaceEvenly`

마찬가지로 `AlignItems`, `AlignContent` 속성은 교차 축에 자식을 정렬 하지만 전체 행 이나 열에 영향을 줍니다.

[![실험 페이지: 콘텐츠를 정렬 하](flex-layout-images/ExperimentAlignContent.png "실험 페이지-콘텐츠를 정렬 합니다.")](flex-layout-images/ExperimentAlignContent-Large.png#lightbox)

IOS screnshot에서 두 행 맨 위에 있는; Android 스크린 샷 있을 때에 센터; 그리고 UWP 스크린 샷에 그들은 맨 아래에 있습니다. 행은 다양 한 방법으로 간격이 수 있습니다.:

[![실험 페이지: 맞춤 콘텐츠 2](flex-layout-images/ExperimentAlignContent2.png "실험 페이지-맞춤 콘텐츠 2")](flex-layout-images/ExperimentAlignContent2-Large.png#lightbox)

`AlignContent` 하나만 행 또는 열이 있으면 효과가 없습니다.

<a name="attached-properties" />

## <a name="the-attached-bindable-properties-in-detail"></a>세부 정보에서 연결된 된 바인딩 가능한 속성

`FlexLayout` 5 개의 연결 된 바인딩 가능한 속성을 정의합니다. 자식에이 속성이 설정 된는 `FlexLayout` 및 특정 자식에만 해당 합니다.

<a name="align-self" />

### <a name="the-alignself-property"></a>AlignSelf 속성

[ `AlignSelf` ](xref:Xamarin.Forms.FlexLayout.AlignSelfProperty) 형식의 연결 된 바인딩 가능한 속성은 [ `FlexAlignSelf` ](xref:Xamarin.Forms.FlexAlignContent), 5 개 멤버가 포함 된 열거:

- `Auto`기본값
- `Stretch`
- `Center`
- `Start` (또는 "flex start" XAML에서)
- `End` (또는 "flex end" XAML에서)

개별 자식에 대 한는 `FlexLayout`, 재정의 설정 하는이 속성은 [ `AlignItems` ](#align-items) 에 속성 집합에 `FlexLayout` 자체입니다. 기본 설정은 `Auto` 사용할 의미는 `AlignItems` 설정 합니다.

에 대 한는 `Label` 라는 요소 `label` (또는 예제)을 설정할 수 있습니다는 `AlignSelf` 다음과 같은 코드에서 속성:

```csharp
FlexAlign.SetAlignSelf(label, FlexAlignSelf.Center);
```

에 대 한 참조는는 `FlexLayout` 의 부모는 `Label`합니다. Xaml에서는 다음과 같은 속성을 설정 하면:

```xaml
<Label ... FlexAlign.AlignSelf="Center" ... />
```

### <a name="the-order-property"></a>Order 속성

[ `Order` ](xref:Xamarin.Forms.FlexLayout.OrderProperty) 속성은 형식이 `int`합니다. 기본값은 0입니다.

`Order` 속성을 사용 하면 순서를 변경할 수 있습니다 하의 자식을 `FlexLayout` 정렬 됩니다. 자식에 일반적으로 `FlexLayout` 정렬 되는에 나타나는 순서와 동일는 `Children` 컬렉션입니다. 이 순서를 설정 하 여 재정의할 수 있습니다는 `Order` 하나 이상의 자식에서 0이 아닌 정수 값으로 연결 된 바인딩 가능한 속성입니다. `FlexLayout` 다음의 설정에 따라 자식을 정렬는 `Order` 각 자식에 있지만 동일한 자식 속성 `Order` 설정에 표시 된 순서 대로 정렬 되는 `Children` 컬렉션입니다.

### <a name="the-basis-property"></a>기본 속성

[ `Basis` ](xref:Xamarin.Forms.FlexLayout.BasisProperty) 바인딩할 수 있는 연결 된 속성의 자식에 할당 된 공간의 양을 나타냅니다는 `FlexLayout` 주 축에 있습니다. 지정한 크기는 `Basis` 속성은 부모의 주요 축 크기 `FlexLayout`합니다. 따라서 `Basis` 자식 열에 정렬 되는 경우 행 또는 높이의 자식 항목을 정렬할 때 자식의 너비를 나타냅니다.

`Basis` 형식의 속성은 [ `FlexBasis` ](xref:Xamarin.Forms.FlexBasis)는 구조입니다. 장치 독립적 단위에서 또는 크기의 백분율로 크기를 지정할 수는 `FlexLayout`합니다. 기본값은 `Basis` 속성은 정적 속성 `FlexBasis.Auto`, 자식의 너비 또는 높이 데 사용 됩니다 요청 한다는 의미입니다.

코드에서 설정할 수는 `Basis` 속성에 대 한는 `Label` 라는 `label` 를 다음과 같이 40 장치 독립적 단위:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40, false));
```

두 번째 인수는 `FlexBasis` 생성자 라는 `isRelative` 상대 크기 인지 여부를 나타냅니다 (`true`) 또는 절대 (`false`). 인수에는 기본값인 `false`이므로 다음 코드를 사용할 수도 있습니다.

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40));
```

암시적 변환이 `float` 를 `FlexBasis` 정의 되어 않으므로 훨씬 간소화할 수 있습니다.

```csharp
FlexLayout.SetBasis(label, 40);
```

크기의 25%로 설정할 수 있습니다는 `FlexLayout` 다음과 같이 부모:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(0.25f, true));
```

소수 자릿수 값이 0 ~ 1 범위에 있어야 합니다.

Xaml에서는 장치 독립적 단위에서 크기에 대 한 숫자를 사용할 수 있습니다.

```xaml
<Label ... FlexLayout.Basis="40" ... />
```

또는 0%에서 100%의 범위에는 백분율을 지정할 수 있습니다.

```xaml
<Label ... FlexLayout.Basis="25%" ... />
```

**별로 실험** 의 페이지는 **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** 샘플에서는 시험해 볼 수는 `Basis` 속성입니다. 페이지에는 5 개의 래핑된 열 표시 `Label` 배경색 및 전경색 교대로 반복 되는 요소입니다. 두 개의 `Slider` 요소를 통해 지정할 수 `Basis` 두 번째 및 네 번째에 대 한 값 `Label`:

[![기본 페이지 실험](flex-layout-images/BasisExperiment.png "기준 실험 페이지")](flex-layout-images/BasisExperiment-Large.png#lightbox)

왼쪽에 iOS 스크린샷은 두 `Label` 장치 독립적 단위에서 높이 지정 된 요소입니다. 전체 높이의 비율을는 높이 지정 된 Android 화면 표시는 `FlexLayout`합니다. 경우는 `Basis` 자식은의 높이 100%로 설정 되는 `FlexLayout`, 인과 다음 열으로 줄 바꿈 하 고 UWP 스크린 샷은 설명 된 것 처럼 해당 열의 전체 높이 차지: 다섯 명의 자식 행 정렬 되는 것 처럼 표시 하지만 실제로 5 개의 열에 정렬 합니다.

### <a name="the-grow-property"></a>확장 속성

[ `Grow` ](xref:Xamarin.Forms.FlexLayout.GrowProperty) 형식의 연결 된 바인딩 가능한 속성은 `int`합니다. 기본값은 0, 및 값 보다 크거나 0 이어야 합니다.

`Grow` 때 경우에 역할을 수행 하는 속성의 `Wrap` 속성이로 설정 되어 `NoWrap` 고 자식 행의 너비 보다 작은 총 너비가 `FlexLayout`, 자식 열에 보다 짧은 높이 또는 `FlexLayout`합니다. `Grow` 속성에는 자식 항목 간에 빈 공간을 할당 하는 방법을 나타냅니다.

에 **증가 실험** 5 페이지 `Label` 다른 색의 요소는 열 및 두 개의 정렬 되 `Slider` 요소를 사용 하면 조정할 수는 `Grow` 속성의 두 번째 및 네 번째 `Label`합니다. 맨 왼쪽 iOS 스크린샷은 기본 `Grow` 0의 속성:

[![실험 페이지 성장](flex-layout-images/GrowExperiment.png "실험 페이지 성장")](flex-layout-images/GrowExperiment-Large.png#lightbox)

하나의 자식 주어 지 면는 양수 `Grow` Android 스크린 샷은 설명 된 것 처럼 해당 자식 모든 나머지 공간을 차지 값입니다. 또한 두 개 이상의 자식 항목 간에이 공간을 할당할 수 있습니다. UWP 스크린 샷에 `Grow` 두 번째 속성 `Label` 0.5로 설정 하는 동안는 `Grow` 속성 네 번째 `Label` 는 네 번째 주는 1.5 `Label` 3 배의 빈 공간을 두 번째 의`Label`.

자식 뷰에 해당 공간을 사용 하는 방법 특정 형식의 자식에 따라 달라 집니다. 에 대 한는 `Label`, 텍스트의 총 공간 내에 배치 될 수 있습니다는 `Label` 속성을 사용 하 여 `HorizontalTextAlignment` 및 `VerticalTextAlignment`합니다.

<a name="shrink" />

### <a name="the-shrink-property"></a>축소 속성

[ `Shrink` ](xref:Xamarin.Forms.FlexLayout.ShrinkProperty) 형식의 연결 된 바인딩 가능한 속성은 `int`합니다. 기본값은 1, 및 값 보다 크거나 0 이어야 합니다.

`Shrink` 속성 역할 때는 `Wrap` 속성이 `NoWrap` 자식 행의 집계 너비의 너비 보다 크면는 `FlexLayout`, 자식의 단일 열의 집계 높이 보다 큽니다. 또는 높이 `FlexLayout`합니다. 일반적으로 `FlexLayout` 크기로 constricting 하 여이 자식 컨트롤에 표시 됩니다. `Shrink` 속성이 있는 자식에 전체 크기에 표시 되 고 우선 순위가 지정 됩니다 나타낼 수 있습니다.

**축소 실험** 페이지가 플러시되는 `FlexLayout` 5 개의 행과 `Label` 보다 더 많은 공간을 필요로 하는 자식은 `FlexLayout` 너비입니다. 왼쪽에 iOS 스크린샷은 모든는 `Label` 1의 기본값을 사용 하 여 요소:

[![축소 실험 페이지](flex-layout-images/ShrinkExperiment.png "축소 실험 페이지")](flex-layout-images/ShrinkExperiment-Large.png#lightbox)

Android 스크린 샷에 `Shrink` 두 번째에 대 한 값 `Label` 0이 고, 있는으로 설정 되어 `Label` 전체 너비가에 표시 됩니다. 또한, 네 번째 `Label` 지정는 `Shrink` 에 축소 하 고, 1 보다 큰 값입니다. UWP 스크린샷은 모두 `Label` 부여 되는 요소는 `Shrink` 해당 경우에 전체 크기로 표시 될 수 있도록 허용 하려면 값 0은 가능 합니다.

둘 다 설정할 수 있습니다는 `Grow` 및 `Shrink` 값 집계 자식 크기 때로는 수 있는 보다 작거나의 크기 보다 큰 경우에 따라 상황에 맞게는 `FlexLayout`합니다.

## <a name="css-styling-with-flexlayout"></a>FlexLayout 된 CSS 스타일 지정

사용할 수는 [CSS 스타일이](~/xamarin-forms/user-interface/styles/css/index.md) 연결 하 여 Xamarin.Forms 3.0에 도입 된 기능 `FlexLayout`합니다. **CSS 카탈로그 항목** 의 페이지는 **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** 의 레이아웃을 복제 하는 예제는 **카탈로그 항목** 페이지 하지만 CSS 스타일 시트 스타일에 대 한:

[![CSS 카탈로그 항목 페이지](flex-layout-images/CssCatalogItems.png "CSS 카탈로그 항목 페이지")](flex-layout-images/CssCatalogItems-Large.png#lightbox)

원래 **CatalogItemsPage.xaml** 파일에 포함 된 5 `Style` 정의에서 해당 `Resources` 15 사용 하 여 섹션 `Setter` 개체입니다. 에 **CssCatalogItemsPage.xaml** 파일을 2로 축소 되었습니다을 `Style` 방금 4와 함께 정의 `Setter` 개체입니다. 이러한 스타일 Xamarin.Forms CSS 스타일 지정 기능은 현재 지원 하지 않는 속성에 대 한 CSS 스타일 시트를 보완 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:FlexLayoutDemos"
             x:Class="FlexLayoutDemos.CssCatalogItemsPage"
             Title="CSS Catalog Items">
    <ContentPage.Resources>
        <StyleSheet Source="CatalogItemsStyles.css" />

        <Style TargetType="Frame">
            <Setter Property="BorderColor" Value="Blue" />
            <Setter Property="CornerRadius" Value="15" />
        </Style>

        <Style TargetType="Button">
            <Setter Property="Text" Value="LEARN MORE" />
            <Setter Property="BorderRadius" Value="20" />
        </Style>
    </ContentPage.Resources>

    <ScrollView Orientation="Both">
        <FlexLayout>
            <Frame>
                <FlexLayout Direction="Column">
                    <Label Text="Seated Monkey" StyleClass="header" />
                    <Label Text="This monkey is laid back and relaxed, and likes to watch the world go by." />
                    <Label Text="  &#x2022; Doesn't make a lot of noise" />
                    <Label Text="  &#x2022; Often smiles mysteriously" />
                    <Label Text="  &#x2022; Sleeps sitting up" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.SeatedMonkey.jpg}" />
                    <Label StyleClass="empty" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame>
                <FlexLayout Direction="Column">
                    <Label Text="Banana Monkey" StyleClass="header" />
                    <Label Text="Watch this monkey eat a giant banana." />
                    <Label Text="  &#x2022; More fun than a barrel of monkeys" />
                    <Label Text="  &#x2022; Banana not included" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.Banana.jpg}" />
                    <Label StyleClass="empty" />
                    <Button />
                </FlexLayout>
            </Frame>

            <Frame>
                <FlexLayout Direction="Column">
                    <Label Text="Face-Palm Monkey" StyleClass="header" />
                    <Label Text="This monkey reacts appropriately to ridiculous assertions and actions." />
                    <Label Text="  &#x2022; Cynical but not unfriendly" />
                    <Label Text="  &#x2022; Seven varieties of grimaces" />
                    <Label Text="  &#x2022; Doesn't laugh at your jokes" />
                    <Image Source="{local:ImageResource FlexLayoutDemos.Images.FacePalm.jpg}" />
                    <Label StyleClass="empty" />
                    <Button />
                </FlexLayout>
            </Frame>
        </FlexLayout>
    </ScrollView>
</ContentPage>
```

CSS 스타일 시트의 첫 번째 줄에서 참조 되는 `Resources` 섹션:

```xaml
<StyleSheet Source="CatalogItemsStyles.css" />
```

세 가지 항목에는 각각에 두 개의 요소가 포함 되어도 있을 `StyleClass` 설정:

```xaml
<Label Text="Seated Monkey" StyleClass="header" />
···
<Label StyleClass="empty" />
```

이러한 참조에 포함 된 선택기는 **CatalogItemsStyles.css** 스타일 시트:

```css
frame {
    width: 300;
    height: 480;
    background-color: lightyellow;
    margin: 10;
}

label {
    margin: 4 0;
}

label.header {
    margin: 8 0;
    font-size: large;
    color: blue;
}

label.empty {
    flex-grow: 1;
}

image {
    height: 180;
    order: -1;
    align-self: center;
}

button {
    font-size: large;
    color: white;
    background-color: green;
}
```

여러 `FlexLayout` 연결 된 바인딩 가능한 속성은 여기 참조 됩니다. 에 `label.empty` 선택기를 표시는 `flex-grow` 빈 스타일을 특성 `Label` 위의 빈 공간을 제공 하는 `Button`합니다. `image` 선택기 포함는 `order` 특성 및 `align-self` 는 모두에 해당 특성 `FlexLayout` 바인딩 가능 속성을 연결 합니다.

지금까지 살펴본에서 직접 속성을 설정할 수는 `FlexLayout` 의 자식에 대해 연결 된 바인딩 가능한 속성을 설정할 수 있습니다는 `FlexLayout`합니다. 또는 이러한 속성을 사용 하 여 직접 하지 XAML 기반 전통적인 스타일 또는 CSS 스타일을 설정할 수 있습니다. 중요 한 것은을 알고 있어야 하며 이러한 속성을 이해 합니다. 이러한 속성은 어떤 원리에 의해는 `FlexLayout` 기능이 더 유연 합니다. 

## <a name="flexlayout-with-xamarinuniversity"></a>FlexLayout Xamarin.University와

> [!VIDEO https://youtube.com/embed/Ng3sel_5D_0]

**Xamarin.Forms 3.0 Flex 레이아웃으로 [Xamarin 대학](https://university.xamarin.com/)**

## <a name="related-links"></a>관련된 링크

- [FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)
