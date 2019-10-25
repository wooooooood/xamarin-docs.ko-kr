---
title: Xamarin.ios의 레이아웃
description: 자식 보기의 컬렉션을 누적 하거나 래핑하는 데는는 안 됩니다.
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 187befd88c115133a92aa90a711438e7754518d5
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "68648800"
---
# <a name="the-xamarinforms-flexlayout"></a>Xamarin.ios의 레이아웃

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)

_자식 보기의 컬렉션을 누적 하거나 래핑하는 데는는 안 됩니다._

Xamarin.ios [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) 는 xamarin.ios 버전 3.0에 새로 있습니다. 이는 레이아웃 내에서 자식을 정렬 하는 여러 유연한 _옵션을 포함_하기 때문에 _일반적으로 사용_ 되는 CSS [유연한 상자 레이아웃 모듈](http://www.w3.org/TR/css-flexbox-1/)을 기반으로 합니다.

`FlexLayout`는 해당 자식을 스택에서 가로 및 세로로 배열할 수 있다는 점에서 Xamarin.ios [`StackLayout`](~/xamarin-forms/user-interface/layouts/stack-layout.md) 와 비슷합니다. 그러나이 `FlexLayout` 단일 행 이나 열에 맞지 않는 경우에는 자식을 래핑할 수 있으며, 다양 한 화면 크기에 대 한 방향, 맞춤 및 조정에 대 한 많은 옵션도 있습니다.

`FlexLayout` [`Layout<View>`](xref:Xamarin.Forms.Layout`1) 에서 파생 되며 `IList<View>` 형식의 [`Children`](xref:Xamarin.Forms.Layout`1.Children) 속성을 상속 합니다.

`FlexLayout`은 6 개의 공용 바인딩 가능 속성 및 자식 요소의 크기, 방향 및 맞춤에 영향을 주는 바인딩 가능한 5 개의 바인딩 가능 속성을 정의 합니다. 연결 된 바인딩 가능한 속성에 대해 잘 모르는 경우 **[연결 된 속성](~/xamarin-forms/xaml/attached-properties.md)** 문서를 참조 하세요. 이러한 속성은 **[바인딩 가능한 속성](#bindable-properties)** 에 대 한 자세한 내용 및 **[연결 된 바인딩 가능한 속성](#attached-properties)** 에 자세히 설명 되어 있습니다. 그러나이 문서에서는 이러한 많은 속성을 보다 비공식적으로 설명 하는 `FlexLayout`의 **[일반적인 사용 시나리오](#common-scenarios)** 에 대 한 섹션으로 시작 합니다. 문서의 끝 부분에 `FlexLayout` [CSS 스타일 시트](~/xamarin-forms/user-interface/styles/css/index.md)와 결합 하는 방법을 확인할 수 있습니다.

<a name="common-scenarios" />

## <a name="common-usage-scenarios"></a>일반적인 사용 시나리오

이 **[샘플 프로그램에는](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** `FlexLayout`의 몇 가지 일반적인 사용을 보여 주는 여러 페이지가 포함 되어 있으며 해당 속성을 사용 하 여 실험해 볼 수 있습니다.

### <a name="using-flexlayout-for-a-simple-stack"></a>단순 스택에 대해가는 레이아웃 사용

**단순 스택** 페이지는 `FlexLayout` `StackLayout`를 대체할 수 있지만 더 간단한 태그를 사용 하는 방법을 보여 줍니다. 이 샘플의 모든 항목은 XAML 페이지에서 정의 됩니다. @No__t_0는 다음과 같은 네 개의 자식을 포함 합니다.

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

IOS, Android 및 유니버설 Windows 플랫폼에서 실행 되는 페이지는 다음과 같습니다.

[![단순 스택 페이지](flex-layout-images/SimpleStack.png "단순 스택 페이지")](flex-layout-images/SimpleStack-Large.png#lightbox)

@No__t_0의 세 가지 속성은 **Simplestackpage** 파일에 표시 됩니다.

- [@No__t_1](xref:Xamarin.Forms.FlexLayout.Direction) 속성은 [`FlexDirection`](xref:Xamarin.Forms.FlexDirection) 열거형의 값으로 설정 됩니다. 기본값은 `Row`입니다. 속성을 `Column`로 설정 하면 `FlexLayout`의 자식이 항목의 단일 열에 정렬 됩니다.

    @No__t_0의 항목이 열에 정렬 되는 경우 `FlexLayout` 세로 _주 축_ 과 가로 _교차 축_이 있는 것으로 간주 됩니다.

- [@No__t_1](xref:Xamarin.Forms.FlexLayout.AlignItems) 속성은 [`FlexAlignItems`](xref:Xamarin.Forms.FlexAlignItems) 형식이 며, 교차 축에 항목이 정렬 되는 방법을 지정 합니다. @No__t_0 옵션은 각 항목의 가로 가운데 맞춤을 설정 합니다.

    이 작업에 대 한 `FlexLayout` 대신 `StackLayout`를 사용 하는 경우 각 항목의 `HorizontalOptions` 속성을 `Center`에 할당 하 여 모든 항목을 가운데에 맞춥니다. @No__t_0 속성은 `FlexLayout`의 자식에 대해 작동 하지 않지만 단일 `AlignItems` 속성은 동일한 목표를 달성 합니다. 필요한 경우 `AlignSelf` 연결 된 바인딩 가능한 속성을 사용 하 여 개별 항목에 대 한 `AlignItems` 속성을 재정의할 수 있습니다.

    ```xaml
    <Label Text="FlexLayout in Action"
           FontSize="Large"
           FlexLayout.AlignSelf="Start" />
    ```

    이와 같이 변경 하면 읽기 순서가 왼쪽에서 오른쪽 인 경우이 `Label`는 `FlexLayout` 왼쪽 가장자리에 배치 됩니다.

- [@No__t_1](xref:Xamarin.Forms.FlexLayout.JustifyContent) 속성은 [`FlexJustify`](xref:Xamarin.Forms.FlexJustify)형식 이며 주 축에 항목을 정렬 하는 방법을 지정 합니다. @No__t_0 옵션은 모든 항목, 첫 번째 항목 위 및 마지막 항목 아래에 있는 모든 잔여 세로 공간을 동일 하 게 할당 합니다.

    @No__t_0 사용 하는 경우 비슷한 효과를 얻으려면 각 항목의 `VerticalOptions` 속성을 `CenterAndExpand`에 할당 해야 합니다. 그러나 `CenterAndExpand` 옵션은 첫 번째 항목 앞과 마지막 항목 후에 각 항목 사이에 공백을 두 번 할당 합니다. @No__t_3의 `JustifyContent` 속성을 `SpaceAround`로 설정 하 여 `VerticalOptions`의 `CenterAndExpand` 옵션을 모방할 수 있습니다.

이러한 `FlexLayout` 속성은 아래에서 **[자세히 설명 하는 바인딩 가능한 속성](#bindable-properties)** 섹션에 자세히 설명 되어 있습니다.

### <a name="using-flexlayout-for-wrapping-items"></a>항목 래핑에 대해가는 레이아웃 사용

이 샘플의 **사진 줄 바꿈** **[페이지는 `FlexLayout`](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** 자식 항목을 추가 행 또는 열로 래핑할 수 있는 방법을 보여 줍니다. XAML 파일은 `FlexLayout`를 인스턴스화하고 두 가지 속성을 할당 합니다.

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

이 `FlexLayout`의 `Direction` 속성은 설정 되어 있지 않으므로 기본 설정 `Row` 있습니다. 즉, 자식이 행으로 정렬 되 고 주 축은 가로입니다.

[@No__t_1](xref:Xamarin.Forms.FlexLayout.Wrap) 속성은 [`FlexWrap`](xref:Xamarin.Forms.FlexWrap)열거형 형식입니다. 행에 포함할 항목이 너무 많은 경우이 속성 설정으로 인해 항목이 다음 행으로 래핑됩니다.

@No__t_0은 `ScrollView`의 자식입니다. 페이지에 포함할 행이 너무 많은 경우 `ScrollView`에는 `Vertical`의 기본 `Orientation` 속성이 있으며 세로로 스크롤할 수 있습니다.

@No__t_0 속성은 주 축 (가로 축)에 남아 있는 공간을 할당 하 여 각 항목에 동일한 양의 빈 공간을 둘러쌉니다.

코드 숨김이 샘플 사진 컬렉션에 액세스 하 여 `FlexLayout`의 `Children` 컬렉션에 추가 합니다.

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
        using (WebClient webClient = new WebClient())
        {
            try
            {
                // Download the list of stock photos
                Uri uri = new Uri("https://raw.githubusercontent.com/xamarin/docs-archive/master/Images/stock/small/stock.json");
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
                            Source = ImageSource.FromUri(new Uri(filepath))
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

다음은 위에서 아래로 점진적으로 스크롤 되는 프로그램을 실행 하는 프로그램입니다.

[![사진 줄 바꿈 페이지](flex-layout-images/PhotoWrapping.png "사진 줄 바꿈 페이지")](flex-layout-images/PhotoWrapping-Large.png#lightbox)

### <a name="page-layout-with-flexlayout"></a>가는 레이아웃을 사용 하는 페이지 레이아웃

웹 디자인의 표준 레이아웃은 매우 바람직한 레이아웃 형식이 기는 하지만 한가요를 사용 하는 것이 [_grail_](https://en.wikipedia.org/wiki/Holy_grail_(web_design)) . 레이아웃은 페이지의 위쪽에 있는 머리글과 아래쪽의 바닥글로 구성 되며 모두 페이지의 전체 너비로 확장 됩니다. 페이지의 중심을 차지 하는 것은 주요 내용 이지만 콘텐츠 왼쪽에 컬럼 형식의 메뉴와 오른쪽의 보조 정보 (또는 _옆_ 에 있는 영역 이라고 함)가 있습니다. [CSS 유연한 상자 레이아웃 사양 5.4.1](http://www.w3.org/TR/css-flexbox-1/#order-accessibility) 에는 flex 상자를 사용 하 여 성 grail 레이아웃을 실현 하는 방법이 설명 되어 있습니다.

Grail **[Layout데모가](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** 샘플의 **레이아웃** 페이지는 다른 `FlexLayout` 하나를 중첩 하 여이 레이아웃의 간단한 구현을 보여 줍니다. 이 페이지는 세로 모드의 휴대폰으로 설계 되었기 때문에 콘텐츠 영역의 왼쪽과 오른쪽에 있는 영역은 50 픽셀 너비입니다.

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

실행 되 고 있습니다.

[![성 Grail 레이아웃 페이지](flex-layout-images/HolyGrailLayout.png "성 Grail 레이아웃 페이지")](flex-layout-images/HolyGrailLayout-Large.png#lightbox)

탐색과 옆의 영역은 왼쪽 및 오른쪽에 `BoxView` 렌더링 됩니다.

XAML 파일의 첫 번째 `FlexLayout`에는 세로 주 축이 있고 열에 정렬 된 세 개의 자식이 포함 되어 있습니다. 이는 머리글, 페이지의 본문 및 바닥글입니다. 중첩 된 `FlexLayout`에는 행에 세 개의 자식이 정렬 된 가로 주 축이 있습니다.

이 프로그램에는 세 개의 연결 된 바인딩 가능 속성이 나와 있습니다.

- @No__t_0 연결 된 바인딩 가능 속성은 첫 번째 `BoxView`에 설정 됩니다. 이 속성은 정수 이며 기본값은 0입니다. 이 속성을 사용 하 여 레이아웃 순서를 변경할 수 있습니다. 일반적으로 개발자는 탐색 항목 이전에 페이지 콘텐츠를 태그에 표시 하는 것을 선호 합니다. 첫 번째 `BoxView`의 `Order` 속성을 다른 형제 보다 작은 값으로 설정 하면 해당 값이 행의 첫 번째 항목으로 표시 됩니다. 마찬가지로 `Order` 속성을 형제 보다 큰 값으로 설정 하 여 항목이 마지막에 표시 되도록 할 수 있습니다.

- @No__t_0 연결 된 바인딩 가능 속성은 너비 50 픽셀의 너비를 제공 하기 위해 두 개의 `BoxView` 항목에 대해 설정 됩니다. 이 속성은 `FlexBasis` 형식 이며 기본값 인 `Auto` `FlexBasis` 형식의 정적 속성을 정의 하는 구조체입니다. @No__t_0를 사용 하 여 주 축에서 항목이 차지 하는 공간의 크기를 나타내는 백분율 또는 픽셀 크기를 지정할 수 있습니다. 모든 후속 레이아웃의 기반이 되는 항목 크기를 지정 하기 때문에이를 _기준_ 이라고 합니다.

- @No__t_0 속성은 중첩 된 `Layout`와 콘텐츠를 나타내는 `Label` 자식에 대해 설정 됩니다. 이 속성은 `float` 형식 이며 기본값은 0입니다. 양수 값으로 설정 된 경우 주 축을 따라 나머지 모든 공간이 해당 항목에 할당 되 고 양수 값이 `Grow` 있는 형제 항목에 할당 됩니다. 공백은 `Grid`의 별모양 사양과 약간 유사 하 게 값에 비례 하 게 할당 됩니다.

    첫 번째 `Grow` 연결 된 속성은 중첩 된 `FlexLayout`에 설정 되며,이 `FlexLayout` 외부 `FlexLayout` 내에서 사용 되지 않는 모든 세로 공간을 차지 하 고 있음을 나타냅니다. 두 번째 `Grow` 연결 된 속성은 콘텐츠를 나타내는 `Label`에 설정 되어 있으며이 내용이 내부 `FlexLayout` 내에서 사용 되지 않는 모든 가로 공간을 차지 함을 나타냅니다.

    또한 자식의 크기가 `FlexLayout` 크기를 초과 하지만 줄 바꿈이 필요 하지 않을 때 사용할 수 있는 유사한 `Shrink` 연결 된 바인딩 가능 속성도 있습니다.

### <a name="catalog-items-with-flexlayout"></a>가는 레이아웃을 사용 하는 카탈로그 항목

Monkeys **[layoutsample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** 샘플의 **카탈로그 항목** 페이지는 가로 스크롤 가능한 일련의 그림 및 3 개의에 대 한 설명을 표시 한다는 점을 제외 하 고는 [CSS Flex 레이아웃 상자 사양의 섹션 1.1에 있는 예제 1](http://www.w3.org/TR/css-flexbox-1/#overview) 과 비슷합니다. :

[![카탈로그 항목 페이지](flex-layout-images/CatalogItems.png "카탈로그 항목 페이지")](flex-layout-images/CatalogItems-Large.png#lightbox)

3 개의 monkeys는 각각 명시적 높이와 너비를 지정 하는 `Frame`에 포함 된 `FlexLayout` 이며, 더 큰 `FlexLayout`의 자식 이기도 합니다. 이 XAML 파일에서 `FlexLayout` 자식의 속성 대부분은 스타일로 지정 됩니다 .이 중 하나는 모두 암시적 스타일입니다.

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

@No__t_0에 대 한 암시적 스타일에는 `Flexlayout`의 두 연결 된 바인딩 가능 속성 설정이 포함 됩니다.

```xaml
<Style TargetType="Image">
    <Setter Property="FlexLayout.Order" Value="-1" />
    <Setter Property="FlexLayout.AlignSelf" Value="Center" />
</Style>
```

@No__t_11 `Order` 설정 하면 자식 컬렉션 내의 위치에 관계 없이 중첩 된 각 `FlexLayout` 뷰에 `Image` 요소가 먼저 표시 됩니다. @No__t_1의 `AlignSelf` 속성으로 인해 `Image` `FlexLayout` 내에서 가운데 맞춤 됩니다. 이는 기본값 `Stretch`를 가진 `AlignItems` 속성의 설정을 재정의 합니다. 즉, `Label` 및 `Button` 자식은 `FlexLayout`의 전체 너비로 늘어납니다.

세 개의 `FlexLayout` 보기 모두에서 빈 `Label` `Button` 앞에 있지만 `Grow` 설정 1이 있습니다. 즉,이 빈 `Label`에 모든 추가 세로 공간이 할당 되 고,이 값은 `Button`을 아래쪽으로 효과적으로 푸시합니다.

<a name="bindable-properties" />

## <a name="the-bindable-properties-in-detail"></a>바인딩 가능한 속성 (세부 정보)

@No__t_0 몇 가지 일반적인 응용 프로그램을 살펴보았으므로 `FlexLayout` 속성을 자세히 탐색할 수 있습니다.
`FlexLayout`는 코드 또는 XAML에서 `FlexLayout` 자체에 설정 하 여 방향과 맞춤을 제어 하는 데 사용할 수 있는 6 개의 바인딩 가능한 속성을 정의 합니다. 이러한 속성 중 하나 ( [`Position`](xref:Xamarin.Forms.FlexLayout.Position))는이 문서에서 다루지 않습니다.

이 처럼 나머지 5 개의 바인딩 가능한 속성을 사용 하 여 서로 **를 볼 수** 있습니다. **[FlexLayoutDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** 이 페이지를 사용 하 여 `FlexLayout`에서 자식을 추가 하거나 제거 하 고 바인딩할 수 있는 속성 5 개를 조합 하 여 설정할 수 있습니다. @No__t_0의 모든 자식은 `Text` 속성이 `Children` 컬렉션에서의 위치에 해당 하는 숫자로 설정 된 다양 한 색 및 크기의 `Label` 뷰입니다.

프로그램이 시작 되 면 5 개의 `Picker` 보기에서 이러한 5 가지 `FlexLayout` 속성의 기본값을 표시 합니다. 화면 아래쪽에 있는 `FlexLayout`에는 다음과 같은 세 개의 자식이 있습니다.

[![실험 페이지: 기본값](flex-layout-images/ExperimentDefault.png "실험 페이지-기본값")](flex-layout-images/ExperimentDefault-Large.png#lightbox)

각 `Label` 보기에는 `FlexLayout` 내에서 해당 `Label`에 할당 된 공간을 보여 주는 회색 배경이 있습니다. @No__t_0 자체의 배경은 Alice 파란색입니다. 왼쪽 및 오른쪽의 작은 여백을 제외 하 고 페이지의 전체 아래쪽 영역을 차지 합니다.

<a name="direction" />

### <a name="the-direction-property"></a>Direction 속성

[@No__t_1](xref:Xamarin.Forms.FlexLayout.Direction) 속성은 다음과 같은 네 개의 멤버가 있는 열거형 인 [`FlexDirection`](xref:Xamarin.Forms.FlexDirection)형식입니다.

- `Column`
- `ColumnReverse` (또는 XAML의 "열 반전")
- `Row` 기본값
- `RowReverse` (또는 XAML의 "행-역방향")

XAML에서 소문자, 대문자 또는 혼합 된 case의 열거형 멤버 이름을 사용 하 여이 속성의 값을 지정 하거나, CSS 표시기와 동일한 괄호 안에 표시 된 두 개의 추가 문자열을 사용할 수 있습니다. ("열 역방향" 및 "행 역방향" 문자열은 XAML 파서에서 사용 되는 [`FlexDirectionTypeConverter`](xref:Xamarin.Forms.FlexDirectionTypeConverter) 클래스에서 정의 됩니다.)

다음은 **실험** 페이지 (왼쪽에서 오른쪽으로), `Row` 방향 `Column` 방향, `ColumnReverse` 방향입니다.

[![실험 페이지: 방향](flex-layout-images/ExperimentDirection.png "실험 페이지 방향")](flex-layout-images/ExperimentDirection-Large.png#lightbox)

@No__t_0 옵션의 경우 항목은 오른쪽 이나 아래쪽으로 시작 합니다.

<a name="wrap" />

### <a name="the-wrap-property"></a>Wrap 속성

[@No__t_1](xref:Xamarin.Forms.FlexLayout.Wrap) 속성은 다음과 같은 세 개의 멤버가 있는 열거형 인 [`FlexWrap`](xref:Xamarin.Forms.FlexWrap)형식입니다.

- `NoWrap` 기본값
- `Wrap`
- `Reverse` (또는 XAML의 "래핑 반전")

왼쪽에서 오른쪽으로 이러한 화면에는 12 개의 자식에 대 한 `NoWrap`, `Wrap` 및 `Reverse` 옵션이 표시 됩니다.

[![실험 페이지: 래핑](flex-layout-images/ExperimentWrap.png "실험 페이지-줄 바꿈")](flex-layout-images/ExperimentWrap-Large.png#lightbox)

@No__t_0 속성이 `NoWrap`로 설정 되 고 주 축이이 프로그램에서와 같이 제한 되어 있고 주 축의 크기가 작거나 모든 자식에 맞게 설정 된 경우 `FlexLayout`는 항목의 크기를 작게 설정 하려고 시도 합니다. (iOS 스크린샷에서 설명). [@No__t_1](#shrink) 연결 된 바인딩 가능한 속성을 사용 하 여 항목의 shrinkness를 제어할 수 있습니다.

<a name="justify-content" />

### <a name="the-justifycontent-property"></a>JustifyContent 속성

[@No__t_1](xref:Xamarin.Forms.FlexLayout.JustifyContent) 속성은 형식 [`FlexJustify`](xref:Xamarin.Forms.FlexJustify)이며, 다음과 같은 6 개의 멤버가 포함 된 열거형입니다.

- `Start` (또는 XAML의 "flex-시작"), 기본값
- `Center`
- `End` (또는 XAML의 "flex-end")
- `SpaceBetween` (또는 XAML의 "공간 범위")
- `SpaceAround` (또는 XAML의 "공간")
- `SpaceEvenly`

이 속성은 주 축에서 항목의 간격을 지정 하는 방법을 지정 합니다 .이 예제에서는 가로 축입니다.

[![실험 페이지: 콘텐츠 맞춤](flex-layout-images/ExperimentJustifyContent.png "실험 페이지-콘텐츠 맞춤")](flex-layout-images/ExperimentJustifyContent-Large.png#lightbox)

세 가지 스크린샷 모두에서 `Wrap` 속성은 `Wrap`로 설정 됩니다. @No__t_0 기본값은 이전 Android 스크린샷에 표시 됩니다. 여기에서 iOS 스크린샷에서는 `Center` 옵션을 보여 줍니다. 모든 항목이 가운데로 이동 합니다. 단어로 시작 하는 세 가지 다른 옵션은 항목이 차지 하지 않는 추가 공간을 할당 `Space` 합니다. `SpaceBetween`는 항목 사이에 동일한 공간을 할당 합니다.  `SpaceAround`는 각 항목 주위에 동일한 공간을 배치 하는 반면, `SpaceEvenly`는 각 항목 사이, 첫 번째 항목 앞 및 행의 마지막 항목 뒤에 동일한 공간을 배치 합니다.

<a name="align-items" />

### <a name="the-alignitems-property"></a>AlignItems 속성

[@No__t_1](xref:Xamarin.Forms.FlexLayout.AlignItems) 속성은 다음과 같은 네 개의 멤버가 있는 열거형 인 [`FlexAlignItems`](xref:Xamarin.Forms.FlexAlignItems)형식입니다.

- `Stretch` 기본값
- `Center`
- `Start` (또는 XAML의 "자유 시작")
- `End` (또는 XAML의 "flex-end")

이는 교차 축에서 자식을 정렬 하는 방법을 나타내는 두 속성 ( [`AlignContent`](#align-content)되는 다른 속성) 중 하나입니다. 다음 세 스크린샷에 표시 된 것 처럼 각 행 내에서 자식은 확장 되거나 (이전 스크린샷에서 표시 된 것 처럼) 각 항목의 시작, 가운데 또는 끝에 정렬 됩니다.

[![실험 페이지: 항목 맞춤](flex-layout-images/ExperimentAlignItems.png "실험 페이지 맞춤 항목")](flex-layout-images/ExperimentAlignItems-Large.png#lightbox)

IOS 스크린샷에서 모든 자식의 위쪽이 정렬 됩니다. Android 스크린샷에서 항목은 가장 긴 자식 항목을 기준으로 세로로 가운데 맞춤 됩니다. UWP 스크린샷에서 모든 항목의 아래쪽이 정렬 됩니다.

개별 항목에 대해 [`AlignSelf`](#align-self) 연결 된 바인딩 가능 속성을 사용 하 여 `AlignItems` 설정을 재정의할 수 있습니다.

<a name="align-content" />

### <a name="the-aligncontent-property"></a>AlignContent 속성

[@No__t_1](xref:Xamarin.Forms.FlexLayout.AlignContent) 속성은 형식 [`FlexAlignContent`](xref:Xamarin.Forms.FlexAlignContent)이며, 다음과 같은 7 개의 멤버가 포함 된 열거형입니다.

- `Stretch` 기본값
- `Center`
- `Start` (또는 XAML의 "자유 시작")
- `End` (또는 XAML의 "flex-end")
- `SpaceBetween` (또는 XAML의 "공간 범위")
- `SpaceAround` (또는 XAML의 "공간")
- `SpaceEvenly`

@No__t_0와 마찬가지로 `AlignContent` 속성도 교차 축의 자식을 정렬 하지만 전체 행 또는 열에는 영향을 줍니다.

[![실험 페이지: 콘텐츠 맞춤](flex-layout-images/ExperimentAlignContent.png "실험 페이지-콘텐츠 맞춤")](flex-layout-images/ExperimentAlignContent-Large.png#lightbox)

IOS 스크린샷에서 두 행이 맨 위에 있습니다. Android 스크린샷 가운데에 있습니다. 그리고 UWP 스크린샷 아래에 있습니다. 행은 다양 한 방식으로 간격을 지정할 수도 있습니다.

[![실험 페이지: 내용 맞춤 2](flex-layout-images/ExperimentAlignContent2.png "실험 페이지-내용 맞춤 2")](flex-layout-images/ExperimentAlignContent2-Large.png#lightbox)

행 또는 열이 하나만 있는 경우에는 `AlignContent` 적용 되지 않습니다.

<a name="attached-properties" />

## <a name="the-attached-bindable-properties-in-detail"></a>연결 된 바인딩 가능한 속성의 세부 정보

`FlexLayout`는 연결 가능한 5 개의 바인딩 가능 속성을 정의 합니다. 이러한 속성은 `FlexLayout`의 자식에 대해 설정 되며 해당 특정 자식에만 적용 됩니다.

<a name="align-self" />

### <a name="the-alignself-property"></a>AlignSelf 속성

[@No__t_1](xref:Xamarin.Forms.FlexLayout.AlignSelfProperty) 연결 된 바인딩 가능한 속성은 5 개의 멤버가 포함 된 열거형 [`FlexAlignSelf`](xref:Xamarin.Forms.FlexAlignContent)형식입니다.

- `Auto` 기본값
- `Stretch`
- `Center`
- `Start` (또는 XAML의 "자유 시작")
- `End` (또는 XAML의 "flex-end")

@No__t_0의 개별 자식에 대해이 속성 설정은 `FlexLayout` 자체에 설정 된 [`AlignItems`](#align-items) 속성을 재정의 합니다. @No__t_0의 기본 설정은 `AlignItems` 설정을 사용 하는 것입니다.

@No__t_1 (또는 예제) 라는 `Label` 요소의 경우 코드에서 다음과 같이 `AlignSelf` 속성을 설정할 수 있습니다.

```csharp
FlexLayout.SetAlignSelf(label, FlexAlignSelf.Center);
```

@No__t_1 `FlexLayout` 부모에 대 한 참조가 없습니다. XAML에서 다음과 같이 속성을 설정 합니다.

```xaml
<Label ... FlexLayout.AlignSelf="Center" ... />
```

### <a name="the-order-property"></a>Order 속성

[@No__t_1](xref:Xamarin.Forms.FlexLayout.OrderProperty) 속성은 `int` 유형입니다. 기본값은 0입니다.

@No__t_0 속성을 사용 하면 `FlexLayout`의 자식이 정렬 되는 순서를 변경할 수 있습니다. 일반적으로 `FlexLayout`의 자식은 `Children` 컬렉션에 표시 되는 순서와 동일 하 게 정렬 됩니다. 하나 이상의 자식에서 `Order` 연결 된 바인딩 가능 속성을 0이 아닌 정수 값으로 설정 하 여이 순서를 재정의할 수 있습니다. 그런 다음 `FlexLayout`는 각 자식의 `Order` 속성 설정에 따라 자식을 정렬 하지만 `Order` 설정이 같은 자식은 `Children` 컬렉션에 표시 되는 순서 대로 정렬 됩니다.

### <a name="the-basis-property"></a>기본 속성

[@No__t_1](xref:Xamarin.Forms.FlexLayout.BasisProperty) 연결 된 바인딩 가능 속성은 주 축에서 `FlexLayout`의 자식에 할당 된 공간의 크기를 나타냅니다. @No__t_0 속성으로 지정 되는 크기는 부모 `FlexLayout`의 주 축에 따라 크기가 지정 됩니다. 따라서 `Basis`는 자식이 행으로 정렬 될 때 자식의 너비를 나타냅니다. 또는 자식이 열에 정렬 될 때 높이를 나타냅니다.

@No__t_0 속성은 구조체 형식 [`FlexBasis`](xref:Xamarin.Forms.FlexBasis)입니다. 크기는 장치 독립적 단위나 `FlexLayout` 크기의 백분율로 지정할 수 있습니다. @No__t_0 속성의 기본값은 정적 속성 `FlexBasis.Auto` 이며이는 자식의 요청 된 너비 또는 높이가 사용 됨을 의미 합니다.

코드에서 `label` 라는 `Label`에 대 한 `Basis` 속성을 다음과 같이 장치 독립적 단위 40로 설정할 수 있습니다.

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40, false));
```

@No__t_0 생성자에 대 한 두 번째 인수는 `isRelative` 이름이 지정 되 고 크기가 상대적인 지 (`true`) 또는 절대 (`false`)를 나타냅니다. 인수의 기본값은 `false` 이므로 다음 코드를 사용할 수도 있습니다.

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40));
```

@No__t_0에서 `FlexBasis`로의 암시적 변환이 정의 되었으므로이를 훨씬 더 간소화할 수 있습니다.

```csharp
FlexLayout.SetBasis(label, 40);
```

다음과 같이 `FlexLayout` 부모의 25%로 크기를 설정할 수 있습니다.

```csharp
FlexLayout.SetBasis(label, new FlexBasis(0.25f, true));
```

이 소수 자릿수 값은 0에서 1 사이 여야 합니다.

XAML에서 장치 독립적 단위의 크기에는 숫자를 사용할 수 있습니다.

```xaml
<Label ... FlexLayout.Basis="40" ... />
```

또는 0%에서 100% 사이의 백분율을 지정할 수 있습니다.

```xaml
<Label ... FlexLayout.Basis="25%" ... />
```

이 샘플의 **기본 실험** 페이지 **[에서는 `Basis`](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** 속성을 사용 하 여 시험해 볼 수 있습니다. 페이지에는 배경 및 전경색이 교대로 반복 되는 5 개의 `Label` 요소를 포함 하는 래핑된 열이 표시 됩니다. 두 개의 `Slider` 요소를 사용 하 여 두 번째와 네 번째 `Label`에 `Basis` 값을 지정할 수 있습니다.

[![기준 실험 페이지](flex-layout-images/BasisExperiment.png "기준 실험 페이지")](flex-layout-images/BasisExperiment-Large.png#lightbox)

왼쪽의 iOS 스크린 샷에서는 두 개의 `Label` 요소가 장치 독립적 단위로 높이가 지정 된 것을 보여 줍니다. Android 화면에는 `FlexLayout` 총 높이의 소수로 지정 된 높이가 표시 됩니다. @No__t_0이 100%로 설정 된 경우, 자식은 `FlexLayout`의 높이가 되 고 다음 열로 래핑하고 해당 열의 전체 높이를 차지 합니다 .이는 UWP 스크린샷에서 5 개의 자식이 행에 정렬 된 것 처럼 표시 됩니다. 이지만 실제로는 5 개의 열로 정렬 됩니다.

### <a name="the-grow-property"></a>증가 속성

[@No__t_1](xref:Xamarin.Forms.FlexLayout.GrowProperty) 연결 된 바인딩 가능 속성은 `int` 형식입니다. 기본값은 0이 고 값은 0 보다 크거나 같아야 합니다.

@No__t_0 속성은 `Wrap` 속성이 `NoWrap`로 설정 되 고 자식 행의 전체 너비가 `FlexLayout`의 너비 보다 작거나 자식 열의 높이가 `FlexLayout` 보다 짧은 경우 역할을 수행 합니다. @No__t_0 속성은 자식 사이에 잔여 공간의 같아지도록 부분을 지정 하는 방법을 나타냅니다.

**실험 증가** 페이지에서 교대로 반복 되는 색의 5 `Label` 개 요소가 열에 정렬 되 고 두 개의 `Slider` 요소를 사용 하 여 두 번째 및 네 번째 `Label`의 `Grow` 속성을 조정할 수 있습니다. 왼쪽 끝에 있는 iOS 스크린샷에는 기본 `Grow` 속성 0이 표시 됩니다.

[![실험 확장 페이지](flex-layout-images/GrowExperiment.png "실험 확장 페이지")](flex-layout-images/GrowExperiment-Large.png#lightbox)

하나의 자식에 긍정 `Grow` 값이 지정 된 경우,이 자식은 Android 스크린샷에서 보여 주는 대로 나머지 공간을 모두 차지 합니다. 이 공간은 둘 이상의 자식 간에 할당 될 수도 있습니다. UWP 스크린샷에서 두 번째 `Label`의 `Grow` 속성은 0.5로 설정 되어 있지만 네 번째 `Label`의 `Grow` 속성은 1.5입니다 .이는 네 번째 `Label` 가장 많은 공간을 두 번째 `Label`으로 세 배로 제공 합니다.

자식 뷰에서 해당 공간을 사용 하는 방법은 특정 자식 유형에 따라 달라 집니다. @No__t_0의 경우 속성 `HorizontalTextAlignment` 및 `VerticalTextAlignment`를 사용 하 여 `Label`의 전체 공간 내에 텍스트를 배치할 수 있습니다.

<a name="shrink" />

### <a name="the-shrink-property"></a>Shrink 속성

[@No__t_1](xref:Xamarin.Forms.FlexLayout.ShrinkProperty) 연결 된 바인딩 가능 속성은 `int` 형식입니다. 기본값은 1이 고 값은 0 보다 크거나 같아야 합니다.

@No__t_0 속성은 `Wrap` 속성이 `NoWrap`로 설정 되 고 자식 행의 집계 너비가 `FlexLayout` 너비 보다 크거나 자식 항목의 단일 열에 대 한 집계 높이가의 높이 보다 큰 경우 역할을 수행 합니다. `FlexLayout`입니다. 일반적으로 `FlexLayout`는 크기를 constricting 하 여 이러한 자식을 표시 합니다. @No__t_0 속성은 전체 크기에 표시 되는 우선 순위가 지정 된 자식을 나타낼 수 있습니다.

[ **실험 축소** ] 페이지에서는 `FlexLayout` 너비 보다 더 많은 공간이 필요한 `Label` 자식의 단일 행이 있는 `FlexLayout`를 만듭니다. 왼쪽의 iOS 스크린샷은 기본값 1 인 모든 `Label` 요소를 보여 줍니다.

[![축소 실험 페이지](flex-layout-images/ShrinkExperiment.png "축소 실험 페이지")](flex-layout-images/ShrinkExperiment-Large.png#lightbox)

Android 스크린샷에서 두 번째 `Label`의 `Shrink` 값은 0으로 설정 되 고 해당 `Label`는 전체 너비로 표시 됩니다. 또한 네 번째 `Label`에는 1 보다 큰 `Shrink` 값이 지정 되 고 축소 됩니다. UWP 스크린 샷에서는 `Label` 요소에 `Shrink` 값이 지정 되는 것을 보여 줍니다 .이 값은 가능한 경우 전체 크기로 표시 될 수 있습니다.

집계 자식 크기가 때때로 `FlexLayout` 크기 보다 작거나 같은 경우에만 `Grow` 및 `Shrink` 값을 모두 설정할 수 있습니다.

## <a name="css-styling-with-flexlayout"></a>가는 레이아웃을 사용 하는 CSS 스타일

@No__t_1와 연결 되어 있는 Xamarin. Forms 3.0에 도입 된 [CSS 스타일](~/xamarin-forms/user-interface/styles/css/index.md) 지정 기능을 사용할 수 있습니다. ' 트 레이아웃 **[데모](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** '의 **css 카탈로그 항목** 페이지는 **카탈로그 항목** 페이지의 레이아웃을 복제 하지만 대부분의 스타일에 대 한 css 스타일 시트를 포함 합니다.

[![CSS 카탈로그 항목 페이지](flex-layout-images/CssCatalogItems.png "CSS 카탈로그 항목 페이지")](flex-layout-images/CssCatalogItems-Large.png#lightbox)

원래 **CatalogItemsPage** 파일의 `Resources` `Setter` 섹션에는 5 개의 `Style` 정의가 있습니다. **CssCatalogItemsPage** 파일에서 4 개의 `Setter` 개체가 포함 된 두 개의 `Style` 정의로 줄었습니다. 이러한 스타일은 Xamarin.ios CSS 스타일 기능이 현재 지원 하지 않는 속성에 대 한 CSS 스타일 시트를 보완 합니다.

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

CSS 스타일 시트는 `Resources` 섹션의 첫 번째 줄에서 참조 됩니다.

```xaml
<StyleSheet Source="CatalogItemsStyles.css" />
```

세 항목 각각의 두 요소에는 `StyleClass` 설정이 포함 되어 있습니다.

```xaml
<Label Text="Seated Monkey" StyleClass="header" />
···
<Label StyleClass="empty" />
```

**CatalogItemsStyles** 스타일 시트에서 선택기를 참조 합니다.

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

여러 `FlexLayout` 연결 된 바인딩 가능한 속성은 여기에서 참조 됩니다. @No__t_0 선택기에는 `Button` 위에 빈 공간을 제공 하기 위해 빈 `Label`의 스타일을 지정 하는 `flex-grow` 특성이 표시 됩니다. @No__t_0 선택기는 연결 된 바인딩 가능한 속성 `FlexLayout`에 해당 하는 `order` 특성 및 `align-self` 특성을 포함 합니다.

@No__t_0에 대 한 속성을 직접 설정 하 고 `FlexLayout`의 자식에 연결 된 바인딩 가능한 속성을 설정할 수 있습니다. 또는 기존의 XAML 기반 스타일이 나 CSS 스타일을 사용 하 여 이러한 속성을 간접적으로 설정할 수 있습니다. 이러한 속성을 알고 이해 하는 것이 중요 합니다. 이러한 속성은 `FlexLayout` 진정한 유연성을 제공 합니다.

## <a name="flexlayout-with-xamarinuniversity"></a>Xamarin.ios를 사용 하 여가는 레이아웃

> [!VIDEO https://youtube.com/embed/Ng3sel_5D_0]

**Xamarin.ios 3.0 Flex 레이아웃 비디오**

## <a name="related-links"></a>관련 링크

- [의 Layoutlayout데모](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)
