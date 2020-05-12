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
ms.openlocfilehash: 507f78bf887d8d11e93a5a6a1f7d074c55e69360
ms.sourcegitcommit: 83cf2a4d99546751c6394510a463a2b2a8bf75b8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83149963"
---
# <a name="the-xamarinforms-flexlayout"></a>Xamarin.ios의 레이아웃

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)

_자식 보기의 컬렉션을 누적 하거나 래핑하는 데는는 안 됩니다._

Xamarin.ios는 [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) xamarin.ios 버전 3.0에 새로 있습니다. 이는 레이아웃 내에서 자식을 정렬 하는 여러 유연한 _옵션을 포함_하기 때문에 _일반적으로 사용_ 되는 CSS [유연한 상자 레이아웃 모듈](https://www.w3.org/TR/css-flexbox-1/)을 기반으로 합니다.

`FlexLayout`는 [`StackLayout`](~/xamarin-forms/user-interface/layouts/stacklayout.md) 해당 자식을 스택에서 가로 및 세로로 정렬할 수 있다는 점에서 xamarin.ios와 비슷합니다. 그러나는 `FlexLayout` 단일 행 이나 열에 맞지 않는 경우에도 자식을 래핑할 수 있으며, 다양 한 화면 크기에 대 한 방향, 맞춤 및 조정에 대 한 다양 한 옵션을 포함 하 고 있습니다.

`FlexLayout`에서 파생 되 [`Layout<View>`](xref:Xamarin.Forms.Layout`1) 고 [`Children`](xref:Xamarin.Forms.Layout`1.Children) 형식의 속성을 상속 `IList<View>` 합니다.

`FlexLayout`자식 요소의 크기, 방향 및 맞춤에 영향을 주는 6 개의 공용 바인딩 가능 속성 및 연결 된 바인딩 가능 속성 5 개를 정의 합니다. 연결 된 바인딩 가능한 속성에 대해 잘 모르는 경우 **[연결 된 속성](~/xamarin-forms/xaml/attached-properties.md)** 문서를 참조 하세요. 이러한 속성은 **[바인딩 가능한 속성](#bindable-properties)** 에 대 한 자세한 내용 및 **[연결 된 바인딩 가능한 속성](#attached-properties)** 에 자세히 설명 되어 있습니다. 그러나이 문서에서는 이러한 많은 속성을 보다 비공식적으로 설명 하는의 **[일반적인 사용 시나리오](#common-scenarios)** 에 대 한 섹션으로 시작 합니다 `FlexLayout` . 이 문서의 끝 부분을 향해 `FlexLayout` [CSS 스타일 시트](~/xamarin-forms/user-interface/styles/css/index.md)와 결합 하는 방법을 확인할 수 있습니다.

<a name="common-scenarios" />

## <a name="common-usage-scenarios"></a>일반 시나리오

이 **[샘플 프로그램에는](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** 의 몇 가지 일반적인 사용을 보여 주고 `FlexLayout` 해당 속성을 시험해 볼 수 있는 여러 페이지가 포함 되어 있습니다.

### <a name="using-flexlayout-for-a-simple-stack"></a>단순 스택에 대해가는 레이아웃 사용

**단순 스택** 페이지는 `FlexLayout` 가 `StackLayout` 보다 간단한 태그를 사용 하 여 대체 하는 방법을 보여 줍니다. 이 샘플의 모든 항목은 XAML 페이지에서 정의 됩니다. 에는 `FlexLayout` 다음과 같은 네 개의 자식이 있습니다.

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

의 세 가지 속성 `FlexLayout` 은 **Simplestackpage** 파일에 표시 됩니다.

- [`Direction`](xref:Xamarin.Forms.FlexLayout.Direction)속성은 열거형의 값으로 설정 됩니다 [`FlexDirection`](xref:Xamarin.Forms.FlexDirection) . 기본값은 `Row`입니다. 속성을로 설정 하면 `Column` 의 자식이 `FlexLayout` 항목의 단일 열에 정렬 됩니다.

    의 항목이 열에 정렬 되는 경우에는 `FlexLayout` `FlexLayout` 세로 _주 축_ 과 가로 _교차 축_이 포함 됩니다.

- [`AlignItems`](xref:Xamarin.Forms.FlexLayout.AlignItems)속성은 형식이 [`FlexAlignItems`](xref:Xamarin.Forms.FlexAlignItems) 며, 교차 축에 항목이 정렬 되는 방식을 지정 합니다. `Center`옵션은 각 항목의 가로 가운데 맞춤을 설정 합니다.

    이 작업에 대해가 아닌를 사용 하는 경우 `StackLayout` `FlexLayout` `HorizontalOptions` 각 항목의 속성을에 할당 하 여 모든 항목을 가운데에 맞춥니다 `Center` . `HorizontalOptions`속성은의 자식에 대해서는 작동 하지 `FlexLayout` 않지만 단일 속성은 `AlignItems` 동일한 목표를 달성 합니다. 필요한 경우 연결 된 바인딩 가능한 속성을 사용 하 여 `AlignSelf` `AlignItems` 개별 항목에 대 한 속성을 재정의할 수 있습니다.

    ```xaml
    <Label Text="FlexLayout in Action"
           FontSize="Large"
           FlexLayout.AlignSelf="Start" />
    ```

    이렇게 변경 하면 `Label` `FlexLayout` 읽기 순서가 왼쪽에서 오른쪽 인 경우이 항목은의 왼쪽 가장자리에 배치 됩니다.

- [`JustifyContent`](xref:Xamarin.Forms.FlexLayout.JustifyContent)속성은 형식이 며 [`FlexJustify`](xref:Xamarin.Forms.FlexJustify) 주 축에 항목을 정렬 하는 방법을 지정 합니다. `SpaceEvenly`옵션은 모든 항목, 첫 번째 항목 위 및 마지막 항목 아래에 있는 모든 잔여 세로 공간을 동일 하 게 할당 합니다.

    을 사용 하는 경우 `StackLayout` `VerticalOptions` 비슷한 효과를 얻으려면 각 항목의 속성을에 할당 해야 `CenterAndExpand` 합니다. 하지만 `CenterAndExpand` 옵션은 첫 번째 항목 앞과 마지막 항목 다음에 오는 각 항목 사이에 공백을 두 번 할당 합니다. `CenterAndExpand` `VerticalOptions` 의 속성을로 설정 하 여의 옵션을 모방할 수 있습니다 `JustifyContent` `FlexLayout` `SpaceAround` .

이러한 `FlexLayout` 속성은 아래에서 **[자세히 설명 하는 바인딩 가능한 속성](#bindable-properties)** 섹션에 자세히 설명 되어 있습니다.

### <a name="using-flexlayout-for-wrapping-items"></a>항목 래핑에 대해가는 레이아웃 사용

이 샘플의 **사진 줄 바꿈** 페이지 **[는에서](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** `FlexLayout` 자식 항목을 추가 행 또는 열로 래핑할 수 있는 방법을 보여 줍니다. XAML 파일은를 인스턴스화하고 `FlexLayout` 두 개의 속성을 할당 합니다.

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

`Direction`이의 속성은 `FlexLayout` 설정 되지 않으므로 기본 설정이 있습니다 `Row` . 즉, 자식이 행으로 정렬 되 고 주 축은 가로입니다.

[`Wrap`](xref:Xamarin.Forms.FlexLayout.Wrap)속성은 열거형 형식입니다 [`FlexWrap`](xref:Xamarin.Forms.FlexWrap) . 행에 포함할 항목이 너무 많은 경우이 속성 설정으로 인해 항목이 다음 행으로 래핑됩니다.

는 `FlexLayout` 의 자식입니다 `ScrollView` . 페이지에 포함할 행이 너무 많은 경우에는 `ScrollView` `Orientation` 의 기본 속성이 `Vertical` 있으며 세로 스크롤이 가능 합니다.

`JustifyContent`속성은 주 축 (가로 축)에 남아 있는 공간을 할당 하 여 각 항목에 동일한 양의 빈 공간을 둘러쌉니다.

코드 숨김이 파일은 샘플 사진의 컬렉션에 액세스 하 여의 컬렉션에 추가 합니다 `Children` `FlexLayout` .

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

웹 디자인의 표준 레이아웃은 매우 바람직한 레이아웃 형식이 기는 하지만 한가요를 사용 하는 것이 [_grail_](https://en.wikipedia.org/wiki/Holy_grail_(web_design)) . 레이아웃은 페이지의 위쪽에 있는 머리글과 아래쪽의 바닥글로 구성 되며 모두 페이지의 전체 너비로 확장 됩니다. 페이지의 중심을 차지 하는 것은 주요 내용 이지만 콘텐츠 왼쪽에 컬럼 형식의 메뉴와 오른쪽의 보조 정보 (또는 _옆_ 에 있는 영역 이라고 함)가 있습니다. [CSS 유연한 상자 레이아웃 사양 5.4.1](https://www.w3.org/TR/css-flexbox-1/#order-accessibility) 에는 flex 상자를 사용 하 여 성 grail 레이아웃을 실현 하는 방법이 설명 되어 있습니다.

Grail **[Layout데모가](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** 샘플의 **성 레이아웃** 페이지는 다른 중첩 된 항목을 사용 하 여이 레이아웃의 간단한 구현을 보여 줍니다 `FlexLayout` . 이 페이지는 세로 모드의 휴대폰으로 설계 되었기 때문에 콘텐츠 영역의 왼쪽과 오른쪽에 있는 영역은 50 픽셀 너비입니다.

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

탐색과 옆의 영역은 `BoxView` 왼쪽 및 오른쪽에으로 렌더링 됩니다.

XAML 파일의 첫 번째에는 `FlexLayout` 세로 주 축이 있고 열에 정렬 된 세 개의 자식이 포함 되어 있습니다. 이는 머리글, 페이지의 본문 및 바닥글입니다. 중첩 된에는 `FlexLayout` 세 개의 자식이 하나의 행에 정렬 된 가로 주 축이 있습니다.

이 프로그램에는 세 개의 연결 된 바인딩 가능 속성이 나와 있습니다.

- `Order`연결 된 바인딩 가능 속성은 첫 번째에 설정 됩니다 `BoxView` . 이 속성은 정수 이며 기본값은 0입니다. 이 속성을 사용 하 여 레이아웃 순서를 변경할 수 있습니다. 일반적으로 개발자는 탐색 항목 이전에 페이지 콘텐츠를 태그에 표시 하는 것을 선호 합니다. 첫 번째 `Order` 의 속성 `BoxView` 을 다른 형제 보다 작은 값으로 설정 하면 해당 값이 행의 첫 번째 항목으로 표시 됩니다. 마찬가지로 `Order` 속성을 형제 보다 큰 값으로 설정 하 여 항목이 마지막에 표시 되도록 할 수 있습니다.

- `Basis`연결 된 바인딩 가능 속성은 두 항목에 대해 설정 되어 `BoxView` 너비 50 픽셀을 제공 합니다. 이 속성은 형식이 며 `FlexBasis` 기본값 인 이라는 형식의 정적 속성을 정의 하는 구조체입니다 `FlexBasis` `Auto` . `Basis`를 사용 하 여 기본 축에서 항목이 차지 하는 공간의 크기를 나타내는 백분율 또는 픽셀 크기를 지정할 수 있습니다. 모든 후속 레이아웃의 기반이 되는 항목 크기를 지정 하기 때문에이를 _기준_ 이라고 합니다.

- `Grow`속성은 `Layout` `Label` 콘텐츠를 나타내는 자식의 중첩 된 및에 대해 설정 됩니다. 이 속성은 형식이 `float` 며 기본값은 0입니다. 양수 값으로 설정 된 경우 주 축을 따라 나머지 모든 공간이 해당 항목에 할당 되 고 값이 양수 인 형제 항목에 할당 됩니다 `Grow` . 공백은의 별모양 사양과 약간 유사 하 게 값에 비례 하 게 할당 됩니다 `Grid` .

    첫 번째 `Grow` 연결 된 속성은 중첩 된에 설정 되며 `FlexLayout` 이는 `FlexLayout` 외부에서 사용 되지 않는 모든 세로 공간을 차지 함을 나타냅니다 `FlexLayout` . 두 번째 `Grow` 연결 된 속성은 콘텐츠를 나타내는에서 설정 되며 `Label` ,이 내용이 내부 내에서 사용 되지 않는 모든 가로 공간을 차지 함을 나타냅니다 `FlexLayout` .

    또한 `Shrink` 자식의 크기가의 크기를 초과 하는 경우에는 `FlexLayout` 줄 바꿈이 필요 하지 않을 때 사용할 수 있는 유사한 연결 된 바인딩 가능 속성도 있습니다.

### <a name="catalog-items-with-flexlayout"></a>가는 레이아웃을 사용 하는 카탈로그 항목

Monkeys **[layoutsample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** 샘플의 **카탈로그 항목** 페이지는 다음 세 개의에 대해 가로로 스크롤할 수 있는 일련의 그림 및 설명을 표시 한다는 점을 제외 하 고는 [CSS Flex 레이아웃 상자 사양의 섹션 1.1에 있는 예제 1](https://www.w3.org//TR/css-flexbox-1/#overview) 과 비슷합니다.

[![카탈로그 항목 페이지](flex-layout-images/CatalogItems.png "카탈로그 항목 페이지")](flex-layout-images/CatalogItems-Large.png#lightbox)

세 개의 monkeys 각각은 `FlexLayout` `Frame` 명시적 높이와 너비를 제공 하는에 포함 되어 있으며, 더 큰의 자식 이기도 합니다 `FlexLayout` . 이 XAML 파일에서 자식의 속성 대부분 `FlexLayout` 은 스타일로 지정 됩니다 .이 중 하나는 모두 암시적 스타일입니다.

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

의 암시적 스타일에는 `Image` 의 연결 된 바인딩 가능한 두 속성에 대 한 설정이 포함 됩니다 `Flexlayout` .

```xaml
<Style TargetType="Image">
    <Setter Property="FlexLayout.Order" Value="-1" />
    <Setter Property="FlexLayout.AlignSelf" Value="Center" />
</Style>
```

`Order`1을 설정 하면 &ndash; `Image` `FlexLayout` 자식 컬렉션 내의 위치에 관계 없이 각 중첩 된 뷰에서 요소가 먼저 표시 됩니다. `AlignSelf`의 속성은가 `Center` 내에서 `Image` 가운데에 오도록 합니다 `FlexLayout` . 이는 기본값을 가진 속성의 설정을 재정의 합니다 `AlignItems` `Stretch` . 즉, `Label` 및 자식은의 `Button` 전체 너비로 늘어납니다 `FlexLayout` .

세 개의 각 뷰 내에서 `FlexLayout` 공백 앞에 `Label` 는가 `Button` 있지만 1은 설정 되어 있습니다 `Grow` . 즉, 모든 추가 세로 공간이이 빈에 할당 되며,이는를 `Label` 아래쪽에 효과적으로 푸시합니다 `Button` .

<a name="bindable-properties" />

## <a name="the-bindable-properties-in-detail"></a>바인딩 가능한 속성 (세부 정보)

이제의 몇 가지 일반적인 응용 프로그램을 살펴보았으므로 `FlexLayout` 의 속성을 `FlexLayout` 자세히 탐색할 수 있습니다.
`FlexLayout`코드 또는 XAML에서 직접 설정 하 여 방향과 맞춤을 제어 하는 데 사용할 수 있는 6 개의 바인딩 가능한 속성을 정의 `FlexLayout` 합니다. 이러한 속성 중 하나 [`Position`](xref:Xamarin.Forms.FlexLayout.Position) 는이 문서에서 다루지 않습니다.

이 처럼 나머지 5 개의 바인딩 가능한 속성을 사용 하 여 서로 **를 볼 수** 있습니다. **[FlexLayoutDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** 이 페이지에서는에서 자식을 추가 하거나 제거 하 `FlexLayout` 고 바인딩 가능한 속성 5 개 속성의 조합을 설정할 수 있습니다. 의 모든 자식은 다양 한 `FlexLayout` `Label` 색 및 크기의 뷰입니다 `Text` . 속성은 컬렉션의 해당 위치에 해당 하는 숫자로 설정 됩니다 `Children` .

프로그램이 시작 되 면 5 개 `Picker` 보기에 이러한 다섯 가지 속성의 기본값이 표시 `FlexLayout` 됩니다. `FlexLayout`화면 아래쪽에는 세 개의 자식이 있습니다.

[![실험 페이지: 기본값](flex-layout-images/ExperimentDefault.png "실험 페이지-기본값")](flex-layout-images/ExperimentDefault-Large.png#lightbox)

각 보기에는 `Label` 내에 할당 된 공간을 표시 하는 회색 배경이 있습니다 `Label` `FlexLayout` . 자체의 배경은 `FlexLayout` Alice 파란색입니다. 왼쪽 및 오른쪽의 작은 여백을 제외 하 고 페이지의 전체 아래쪽 영역을 차지 합니다.

<a name="direction" />

### <a name="the-direction-property"></a>Direction 속성

[`Direction`](xref:Xamarin.Forms.FlexLayout.Direction)속성은 형식입니다 [`FlexDirection`](xref:Xamarin.Forms.FlexDirection) . 네 개의 멤버가 포함 된 열거형입니다.

- `Column`
- `ColumnReverse`(또는 XAML의 "열 반전")
- `Row`, 기본값
- `RowReverse`(또는 XAML의 "행 역방향")

XAML에서 소문자, 대문자 또는 혼합 된 case의 열거형 멤버 이름을 사용 하 여이 속성의 값을 지정 하거나, CSS 표시기와 동일한 괄호 안에 표시 된 두 개의 추가 문자열을 사용할 수 있습니다. ("열 역방향" 및 "행 역방향" 문자열은 [`FlexDirectionTypeConverter`](xref:Xamarin.Forms.FlexDirectionTypeConverter) XAML 파서에서 사용 되는 클래스에 정의 됩니다.)

다음은 **실험** 페이지에서 (왼쪽에서 오른쪽으로), `Row` 방향, `Column` 방향 및 방향을 표시 하는 것입니다 `ColumnReverse` .

[![실험 페이지: 방향](flex-layout-images/ExperimentDirection.png "실험 페이지 방향")](flex-layout-images/ExperimentDirection-Large.png#lightbox)

옵션의 경우 `Reverse` 항목이 오른쪽 이나 아래쪽에서 시작 됩니다.

<a name="wrap" />

### <a name="the-wrap-property"></a>Wrap 속성

[`Wrap`](xref:Xamarin.Forms.FlexLayout.Wrap)속성은 [`FlexWrap`](xref:Xamarin.Forms.FlexWrap) 다음과 같은 세 개의 멤버를 포함 하는 열거형입니다.

- `NoWrap`, 기본값
- `Wrap`
- `Reverse`(또는 XAML의 "래핑 반전")

왼쪽에서 오른쪽으로 이러한 화면에는 `NoWrap` 12 개의 `Wrap` `Reverse` 자식에 대 한, 및 옵션이 표시 됩니다.

[![실험 페이지: 래핑](flex-layout-images/ExperimentWrap.png "실험 페이지-줄 바꿈")](flex-layout-images/ExperimentWrap-Large.png#lightbox)

`Wrap`속성이로 설정 되 `NoWrap` 고 주 축이 제한 되는 경우 (이 프로그램에서와 같이) 주 축이 모든 자식 항목에 맞게 크기를 조정 하는 데 충분 하지 않은 경우에는 `FlexLayout` iOS 스크린샷에서 보여 주는 대로 항목을 더 작게 만들려고 시도 합니다. 연결 된 바인딩 가능한 속성을 사용 하 여 항목의 shrinkness를 제어할 수 있습니다 [`Shrink`](#shrink) .

<a name="justify-content" />

### <a name="the-justifycontent-property"></a>JustifyContent 속성

[`JustifyContent`](xref:Xamarin.Forms.FlexLayout.JustifyContent)속성은 형식이 며 [`FlexJustify`](xref:Xamarin.Forms.FlexJustify) , 열거형은 6 개의 멤버가 포함 된 열거형입니다.

- `Start`(또는 XAML의 "flex-시작"), 기본값
- `Center`
- `End`(또는 XAML의 "flex-end")
- `SpaceBetween`(또는 XAML의 "공간 범위")
- `SpaceAround`(또는 XAML의 "공간")
- `SpaceEvenly`

이 속성은 주 축에서 항목의 간격을 지정 하는 방법을 지정 합니다 .이 예제에서는 가로 축입니다.

[![실험 페이지: 콘텐츠 맞춤](flex-layout-images/ExperimentJustifyContent.png "실험 페이지-콘텐츠 맞춤")](flex-layout-images/ExperimentJustifyContent-Large.png#lightbox)

세 가지 스크린샷 모두에서 `Wrap` 속성은로 설정 됩니다 `Wrap` . `Start`기본값은 이전 Android 스크린샷에 표시 됩니다. IOS 스크린샷은 `Center` 모든 항목이 가운데로 이동 하는 옵션을 보여 줍니다. 단어로 시작 하는 세 가지 다른 옵션은 `Space` 항목에서 차지 하지 않는 추가 공간을 할당 합니다. `SpaceBetween`항목 사이에 동일한 공간을 할당 합니다. `SpaceAround`는 각 항목 주위에 동일한 공간을 배치 하 `SpaceEvenly` 고는 각 항목 사이, 첫 번째 항목 앞 및 행의 마지막 항목 뒤에 동일한 공백을 배치 합니다.

<a name="align-items" />

### <a name="the-alignitems-property"></a>AlignItems 속성

[`AlignItems`](xref:Xamarin.Forms.FlexLayout.AlignItems)속성은 형식입니다 [`FlexAlignItems`](xref:Xamarin.Forms.FlexAlignItems) . 네 개의 멤버가 포함 된 열거형입니다.

- `Stretch`, 기본값
- `Center`
- `Start`(또는 XAML의 "자유 시작")
- `End`(또는 XAML의 "flex-end")

이는 [`AlignContent`](#align-content) 교차 축에서 자식을 맞추는 방법을 나타내는 두 속성 중 하나입니다. 다음 세 스크린샷에 표시 된 것 처럼 각 행 내에서 자식은 확장 되거나 (이전 스크린샷에서 표시 된 것 처럼) 각 항목의 시작, 가운데 또는 끝에 정렬 됩니다.

[![실험 페이지: 항목 맞춤](flex-layout-images/ExperimentAlignItems.png "실험 페이지 맞춤 항목")](flex-layout-images/ExperimentAlignItems-Large.png#lightbox)

IOS 스크린샷에서 모든 자식의 위쪽이 정렬 됩니다. Android 스크린샷에서 항목은 가장 긴 자식 항목을 기준으로 세로로 가운데 맞춤 됩니다. UWP 스크린샷에서 모든 항목의 아래쪽이 정렬 됩니다.

모든 개별 항목에 대해 `AlignItems` 연결 된 바인딩 가능한 속성을 사용 하 여 설정을 재정의할 수 있습니다 [`AlignSelf`](#align-self) .

<a name="align-content" />

### <a name="the-aligncontent-property"></a>AlignContent 속성

[`AlignContent`](xref:Xamarin.Forms.FlexLayout.AlignContent)속성은 형식이 며 [`FlexAlignContent`](xref:Xamarin.Forms.FlexAlignContent) , 열거형은 일곱 개의 멤버가 있습니다.

- `Stretch`, 기본값
- `Center`
- `Start`(또는 XAML의 "자유 시작")
- `End`(또는 XAML의 "flex-end")
- `SpaceBetween`(또는 XAML의 "공간 범위")
- `SpaceAround`(또는 XAML의 "공간")
- `SpaceEvenly`

마찬가지로 `AlignItems` 속성은 `AlignContent` 교차 축에 자식을 정렬 하지만 전체 행 또는 열에는 영향을 줍니다.

[![실험 페이지: 콘텐츠 맞춤](flex-layout-images/ExperimentAlignContent.png "실험 페이지-콘텐츠 맞춤")](flex-layout-images/ExperimentAlignContent-Large.png#lightbox)

IOS 스크린샷에서 두 행이 맨 위에 있습니다. Android 스크린샷 가운데에 있습니다. 그리고 UWP 스크린샷 아래에 있습니다. 행은 다양 한 방식으로 간격을 지정할 수도 있습니다.

[![실험 페이지: 내용 맞춤 2](flex-layout-images/ExperimentAlignContent2.png "실험 페이지-내용 맞춤 2")](flex-layout-images/ExperimentAlignContent2-Large.png#lightbox)

`AlignContent`행 또는 열이 하나만 있는 경우는 영향을 주지 않습니다.

<a name="attached-properties" />

## <a name="the-attached-bindable-properties-in-detail"></a>연결 된 바인딩 가능한 속성의 세부 정보

`FlexLayout`5 개의 연결 된 바인딩 가능한 속성을 정의 합니다. 이러한 속성은의 자식에 대해 설정 되며 `FlexLayout` 해당 특정 자식에만 관련 됩니다.

<a name="align-self" />

### <a name="the-alignself-property"></a>AlignSelf 속성

[`AlignSelf`](xref:Xamarin.Forms.FlexLayout.AlignSelfProperty)연결 된 바인딩 가능한 속성은 형식이 며 [`FlexAlignSelf`](xref:Xamarin.Forms.FlexAlignContent) , 열거형은 5 개의 멤버가 있습니다.

- `Auto`, 기본값
- `Stretch`
- `Center`
- `Start`(또는 XAML의 "자유 시작")
- `End`(또는 XAML의 "flex-end")

의 개별 자식에 대해 `FlexLayout` 이 속성 설정은 [`AlignItems`](#align-items) 자체에 설정 된 속성을 재정의 합니다 `FlexLayout` . 의 기본 설정은 `Auto` 설정을 사용 하는 것입니다 `AlignItems` .

`Label`또는 예제 라는 요소의 경우 `label` `AlignSelf` 다음과 같이 코드에서 속성을 설정할 수 있습니다.

```csharp
FlexLayout.SetAlignSelf(label, FlexAlignSelf.Center);
```

의 부모에 대 한 참조가 없습니다 `FlexLayout` `Label` . XAML에서 다음과 같이 속성을 설정 합니다.

```xaml
<Label ... FlexLayout.AlignSelf="Center" ... />
```

### <a name="the-order-property"></a>Order 속성

[`Order`](xref:Xamarin.Forms.FlexLayout.OrderProperty)속성은 형식입니다 `int` . 기본값은 0입니다.

`Order`속성을 사용 하면의 자식 항목이 정렬 되는 순서를 변경할 수 있습니다 `FlexLayout` . 일반적으로의 자식 항목은 `FlexLayout` 컬렉션에 표시 되는 순서와 동일 합니다 `Children` . `Order`하나 이상의 자식에서 연결 된 바인딩 가능 속성을 0이 아닌 정수 값으로 설정 하 여이 순서를 재정의할 수 있습니다. `FlexLayout`그런 다음는 각 자식의 속성 설정에 따라 자식을 정렬 `Order` 하지만 동일한 설정을 사용 하는 자식은 `Order` 컬렉션에 표시 되는 순서 대로 정렬 됩니다 `Children` .

### <a name="the-basis-property"></a>기본 속성

[`Basis`](xref:Xamarin.Forms.FlexLayout.BasisProperty)연결 된 바인딩 가능 속성은 `FlexLayout` 주 축에서의 자식에 할당 된 공간의 크기를 나타냅니다. 속성으로 지정 되는 크기는 `Basis` 부모의 주 축에 따른 크기입니다 `FlexLayout` . 따라서은 자식이 `Basis` 행으로 정렬 될 때 자식의 너비를 나타냅니다. 또는 자식이 열에 정렬 된 경우에는 높이를 나타냅니다.

`Basis`속성은 구조체 형식입니다 [`FlexBasis`](xref:Xamarin.Forms.FlexBasis) . 크기는 장치 독립적 단위나 크기의 백분율로 지정할 수 있습니다 `FlexLayout` . 속성의 기본값은 `Basis` 정적 속성 `FlexBasis.Auto` 으로, 자식의 요청 된 너비 또는 높이가 사용 됨을 의미 합니다.

코드에서 `Basis` `Label` `label` 다음과 같이 40 명명 된 장치 독립적 단위에 대 한 속성을 설정할 수 있습니다.

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40, false));
```

생성자에 대 한 두 번째 인수 `FlexBasis` 이름은 `isRelative` 이며 크기가 상대 () 인지 아니면 `true` 절대값 () 인지를 나타냅니다 `false` . 인수의 기본값은 `false` 이므로 다음 코드를 사용할 수도 있습니다.

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40));
```

에서로의 암시적 `float` 변환이 `FlexBasis` 정의 되었으므로이를 훨씬 더 간소화할 수 있습니다.

```csharp
FlexLayout.SetBasis(label, 40);
```

다음과 같이 부모의 25%로 크기를 설정할 수 있습니다 `FlexLayout` .

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

이 샘플의 **기본 실험** 페이지 **[에서는](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** 속성을 사용 하 여 시험해 볼 수 있습니다 `Basis` . 이 페이지에는 `Label` 배경 및 전경색이 교대로 반복 되는 5 개 요소로 된 래핑된 열이 표시 됩니다. 두 개의 `Slider` 요소를 사용 하 `Basis` 여 두 번째 및 네 번째에 대 한 값을 지정할 수 있습니다 `Label` .

[![기준 실험 페이지](flex-layout-images/BasisExperiment.png "기준 실험 페이지")](flex-layout-images/BasisExperiment-Large.png#lightbox)

왼쪽의 iOS 스크린 샷에서는 `Label` 장치 독립적 단위의 높이가 지정 되는 두 요소가 표시 됩니다. Android 화면에는의 전체 높이 중 분수로 지정 된 높이가 표시 됩니다 `FlexLayout` . `Basis`이 100%로 설정 된 경우에는 자식이의 높이가 되 `FlexLayout` 고 다음 열로 래핑하고 해당 열의 전체 높이를 차지 합니다. 즉,이는 5 개의 자식이 행에 정렬 되어 있는 것 처럼 보이지만 실제로는 5 개의 열로 정렬 됩니다.

### <a name="the-grow-property"></a>증가 속성

[`Grow`](xref:Xamarin.Forms.FlexLayout.GrowProperty)연결 된 바인딩 가능한 속성은 형식입니다 `int` . 기본값은 0이 고 값은 0 보다 크거나 같아야 합니다.

속성이 `Grow` `Wrap` 로 설정 되 `NoWrap` 고 자식 행의 너비가의 너비 보다 작거나 `FlexLayout` , 자식의 열 높이가 보다 짧으면 속성은 역할을 수행 합니다 `FlexLayout` . `Grow`속성은 자식 사이에 남아 있는 공간의 같아지도록 부분을 지정 하는 방법을 나타냅니다.

**실험 증가** 페이지에서 `Label` 교대로 반복 되는 색의 5 개 요소가 열에 정렬 되 고 두 개의 요소를 사용 하 여 두 `Slider` `Grow` 번째와 네 번째의 속성을 조정할 수 있습니다 `Label` . 왼쪽 끝에 있는 iOS 스크린샷에는 기본 속성 0이 표시 됩니다 `Grow` .

[![실험 확장 페이지](flex-layout-images/GrowExperiment.png "실험 확장 페이지")](flex-layout-images/GrowExperiment-Large.png#lightbox)

하나에 양수 값이 지정 된 경우 `Grow` Android 스크린샷에서 보여 주는 것 처럼 자식은 나머지 공간을 모두 사용 합니다. 이 공간은 둘 이상의 자식 간에 할당 될 수도 있습니다. UWP 스크린샷에서 두 번째의 속성은 0.5로 설정 되 고, 네 번째의 속성은 1.5로 설정 됩니다 .이는 네 번째를 `Grow` `Label` 두 번째의 가장 `Grow` `Label` `Label` 큰 공간으로 세 번 제공 합니다 `Label` .

자식 뷰에서 해당 공간을 사용 하는 방법은 특정 자식 유형에 따라 달라 집니다. 의 경우 `Label` 및 속성을 사용 하 여의 전체 공간 내에 텍스트를 배치할 수 있습니다 `Label` `HorizontalTextAlignment` `VerticalTextAlignment` .

<a name="shrink" />

### <a name="the-shrink-property"></a>Shrink 속성

[`Shrink`](xref:Xamarin.Forms.FlexLayout.ShrinkProperty)연결 된 바인딩 가능한 속성은 형식입니다 `int` . 기본값은 1이 고 값은 0 보다 크거나 같아야 합니다.

속성이 `Shrink` `Wrap` 로 설정 되 `NoWrap` 고 자식 행의 집계 너비가의 너비 보다 크거나 `FlexLayout` 자식 항목의 단일 열에 대 한 집계 높이가의 높이 보다 큰 경우 속성은 역할을 수행 합니다 `FlexLayout` . 일반적으로는 `FlexLayout` 크기를 constricting 하 여 이러한 자식을 표시 합니다. `Shrink`속성은 우선 순위가 지정 된 자식이 전체 크기에 표시 되는 것을 나타낼 수 있습니다.

[ **실험 축소** ] 페이지에서는 `FlexLayout` `Label` 너비 보다 더 많은 공간이 필요한 5 개 자식의 단일 행이 있는을 만듭니다 `FlexLayout` . 왼쪽의 iOS 스크린샷에는 `Label` 기본값 1 인 모든 요소가 표시 됩니다.

[![축소 실험 페이지](flex-layout-images/ShrinkExperiment.png "축소 실험 페이지")](flex-layout-images/ShrinkExperiment-Large.png#lightbox)

Android 스크린샷에서 `Shrink` 두 번째의 값은 `Label` 0으로 설정 되 고 `Label` 전체 너비로 표시 됩니다. 또한 네 번째에는 `Label` `Shrink` 1 보다 큰 값이 지정 되 고 축소 됩니다. UWP 스크린 샷에서는 `Label` `Shrink` 가능한 경우 전체 크기에 표시 될 수 있도록 두 요소에 값 0이 지정 되어 있음을 보여 줍니다.

`Grow` `Shrink` 집계 자식 크기가의 크기 보다 작거나 때로는 크기 보다 클 수 있는 상황을 수용 하기 위해 및 값을 모두 설정할 수 있습니다 `FlexLayout` .

## <a name="css-styling-with-flexlayout"></a>가는 레이아웃을 사용 하는 CSS 스타일

와의 연결에서 Xamarin. Forms 3.0에 도입 된 [CSS 스타일](~/xamarin-forms/user-interface/styles/css/index.md) 지정 기능을 사용할 수 있습니다 `FlexLayout` . ' 트 레이아웃 **[데모](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)** '의 **css 카탈로그 항목** 페이지는 **카탈로그 항목** 페이지의 레이아웃을 복제 하지만 대부분의 스타일에 대 한 css 스타일 시트를 포함 합니다.

[![CSS 카탈로그 항목 페이지](flex-layout-images/CssCatalogItems.png "CSS 카탈로그 항목 페이지")](flex-layout-images/CssCatalogItems-Large.png#lightbox)

원래 **CatalogItemsPage** 파일의 `Style` 섹션에는 15 개의 개체가 있는 5 개의 정의가 있습니다 `Resources` `Setter` . **CssCatalogItemsPage** 파일에서 4 개의 개체만 포함 하는 두 개의 정의로 줄었습니다 `Style` `Setter` . 이러한 스타일은 Xamarin.ios CSS 스타일 기능이 현재 지원 하지 않는 속성에 대 한 CSS 스타일 시트를 보완 합니다.

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

CSS 스타일 시트는 섹션의 첫 번째 줄에서 참조 됩니다 `Resources` .

```xaml
<StyleSheet Source="CatalogItemsStyles.css" />
```

세 항목 각각의 두 요소에는 설정이 포함 됩니다 `StyleClass` .

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

여기에서 여러 `FlexLayout` 연결 된 바인딩 가능한 속성을 참조 합니다. 선택기에 `label.empty` `flex-grow` `Label` 는 위에 빈 공간을 제공 하기 위해 비어 있는의 스타일을 지정 하는 특성이 표시 됩니다 `Button` . 선택기에는 `image` `order` 특성 및 특성이 포함 되어 있으며 `align-self` 둘 다 `FlexLayout` 연결 된 바인딩 가능 속성에 해당 합니다.

에서 직접 속성을 설정 하 `FlexLayout` 고의 자식에 대해 연결 된 바인딩 가능 속성을 설정할 수 있습니다 `FlexLayout` . 또는 기존의 XAML 기반 스타일이 나 CSS 스타일을 사용 하 여 이러한 속성을 간접적으로 설정할 수 있습니다. 이러한 속성을 알고 이해 하는 것이 중요 합니다. 이러한 속성은 정말 유연 하 게 만드는 것 `FlexLayout` 입니다.

## <a name="flexlayout-with-xamarinuniversity"></a>Xamarin.ios를 사용 하 여가는 레이아웃

> [!VIDEO https://youtube.com/embed/Ng3sel_5D_0]

**Xamarin.ios 3.0 Flex 레이아웃 비디오**

## <a name="related-links"></a>관련 링크

- [의 Layoutlayout데모](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos)
