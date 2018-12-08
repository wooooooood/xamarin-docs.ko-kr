---
title: Xamarin.Forms FlexLayout
description: 스택 또는 자식 뷰의 컬렉션을 래핑 FlexLayout를 사용 합니다.
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: b37070ca627e535f9470916e9f84cdf55bb2aed3
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53056138"
---
# <a name="the-xamarinforms-flexlayout"></a>Xamarin.Forms FlexLayout

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)

_스택 또는 자식 뷰의 컬렉션을 래핑 FlexLayout를 사용 합니다._

Xamarin.Forms [ `FlexLayout` ](xref:Xamarin.Forms.FlexLayout) Xamarin.Forms 버전 3.0의에서 기능입니다. CSS 기반 [유연한 상자 레이아웃 모듈](http://www.w3.org/TR/css-flexbox-1/)일반적으로 알려진 _레이아웃 flex_ 또는 _flex 박스_, 소위 자식을 정렬 하는 여러 유연한 옵션을 포함 하기 때문에 내 레이아웃입니다.

`FlexLayout` Xamarin.Forms 비슷합니다 [ `StackLayout` ](~/xamarin-forms/user-interface/layouts/stack-layout.md) 것 수 자식을 정렬 가로 및 세로로 스택에 있습니다. 그러나는 `FlexLayout` 있는 단일 행 또는 열에 맞게 너무 많은 경우 자식 래핑 수 이기도 하며 방향, 맞춤 및 다양 한 화면 크기 조정에 대 한 많은 옵션도 있습니다.

`FlexLayout` 파생 [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1) 상속 된 [ `Children` ](xref:Xamarin.Forms.Layout`1.Children) 형식의 속성 `IList<View>`합니다.

`FlexLayout` 6 개의 바인딩 가능한 속성을 공개 하 고 크기, 방향 및 해당 자식 요소의 맞춤에 영향을 주는 5 개의 연결 된 바인딩 가능한 속성을 정의 합니다. (연결 된 바인딩 가능한 속성을 사용 하 여 잘 모르는 경우 문서를 참조  **[연결 된 속성](~/xamarin-forms/xaml/attached-properties.md)**.) 이러한 속성은 아래 섹션에서 자세히 설명 **[세부 정보에 바인딩 가능한 속성](#bindable-properties)** 하 고  **[세부정보에서연결된된바인딩가능한속성](#attached-properties)**. 이 문서에서는 일부에 대 한 섹션이 시작 하는 반면 **[일반적인 사용 시나리오](#common-scenarios)** 의 `FlexLayout` 설명 하는 이러한 속성 중 상당수 보다 쉽게 이야기 합니다. 이 문서 끝부분 결합 하는 방법을 살펴보겠습니다 `FlexLayout` 사용 하 여 [CSS 스타일 시트](~/xamarin-forms/user-interface/styles/css/index.md)합니다.

<a name="common-scenarios" />

## <a name="common-usage-scenarios"></a>일반적인 사용 시나리오

합니다 **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** 의 몇 가지 일반적인 용도 보여 주는 여러 페이지를 포함 하는 샘플 프로그램 `FlexLayout` 해당 속성을 사용 하 여 실험할 수 있습니다.

### <a name="using-flexlayout-for-a-simple-stack"></a>FlexLayout를 사용 하 여 간단한 스택

합니다 **간단한 스택** 하는 방법을 보여 줍니다 페이지 `FlexLayout` 대체할 수는 `StackLayout` 있지만 간단한 태그를 사용 하 여 합니다. 이 예제의 모든 XAML 페이지에 정의 됩니다. `FlexLayout` 자식 항목 네 개를 포함 합니다.

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

[![간단한 페이지 스택을](flex-layout-images/SimpleStack.png "간단한 스택 페이지")](flex-layout-images/SimpleStack-Large.png#lightbox)

세 가지 속성과 `FlexLayout` 에 표시 되는 **SimpleStackPage.xaml** 파일:

- 합니다 [ `Direction` ](xref:Xamarin.Forms.FlexLayout.Direction) 속성의 값으로 설정 되는 [ `FlexDirection` ](xref:Xamarin.Forms.FlexDirection) 열거형입니다. 기본값은 `Row`입니다. 속성을 설정 `Column` 자식의 하면는 `FlexLayout` 를 항목의 단일 열에 정렬 합니다.

    때 항목을 `FlexLayout` 열에 정렬 되는 `FlexLayout` 세로 했다고 _주 축_ 및 가로 _축 교차_합니다.

- 합니다 [ `AlignItems` ](xref:Xamarin.Forms.FlexLayout.AlignItems) 형식의 속성이 [ `FlexAlignItems` ](xref:Xamarin.Forms.FlexAlignItems) 항목 교차 축에 정렬 되는 방법을 지정 합니다. `Center` 옵션을 사용 하면 각 항목을 가로 방향으로 가운데에 맞춰야 합니다.

    사용한 경우는 `StackLayout` 아닌 `FlexLayout` 이 작업을 할당 하 여 모든 항목 center는 합니다 `HorizontalOptions` 각 항목의 속성 `Center`합니다. `HorizontalOptions` 속성의 자식에 대 한 작동 하지 않습니다는 `FlexLayout`, 되지만 단일 `AlignItems` 속성 같은 목표를 수행 합니다. 해야 할 경우 사용할 수는 `AlignSelf` 연결 된 바인딩 가능한 속성을 재정의 하는 `AlignItems` 개별 항목에 대 한 속성:

    ```xaml
    <Label Text="FlexLayout in Action"
           FontSize="Large"
           FlexLayout.AlignSelf="Start" />
    ```

    이 변경을 통해 `Label` 의 왼쪽된 가장자리에 배치 되는 `FlexLayout` 때 읽기 순서는 왼쪽에서 오른쪽입니다.

- 합니다 [ `JustifyContent` ](xref:Xamarin.Forms.FlexLayout.JustifyContent) 형식의 속성이 [ `FlexJustify` ](xref:Xamarin.Forms.FlexJustify), 항목 기본 축에 정렬 되는 방식을 지정 합니다. `SpaceEvenly` 모든 남은 세로 공간 모든 항목 간에 동일 하 게 하 고 첫 번째 항목을 위와 아래의 마지막 항목을 할당 하는 옵션입니다.

    사용한 경우는 `StackLayout`를 할당 해야 합니다 `VerticalOptions` 각 항목의 속성 `CenterAndExpand` 비슷한 효과 달성 하기 위해. 하지만 `CenterAndExpand` 옵션 각 항목 사이 보다 첫 번째 항목 앞과 마지막 항목 뒤 배의 공간이 할당 됩니다. 모방할 수를 `CenterAndExpand` 옵션을 `VerticalOptions` 설정 하 여는 `JustifyContent` 속성을 `FlexLayout` 에 `SpaceAround`합니다.

이러한 `FlexLayout` 속성 섹션에서 자세히 설명 됩니다 **[세부 정보에 바인딩 가능한 속성](#bindable-properties)** 아래.

### <a name="using-flexlayout-for-wrapping-items"></a>항목의 FlexLayout를 사용 하 여

**사진 래핑** 페이지를 **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** 샘플과 어떻게 `FlexLayout` 자식을 추가 행 또는 열을 래핑할 수 있습니다. XAML 파일은는 `FlexLayout` 의 두 가지 속성을 할당 합니다.

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

`Direction` 속성의 `FlexLayout` 을 설정 하지 않으면의 기본 설정 되므로 `Row`, 즉 자식 행이 정렬 되 고 기본 축이 가로입니다.

합니다 [ `Wrap` ](xref:Xamarin.Forms.FlexLayout.Wrap) 열거형 형식의 속성이 [ `FlexWrap` ](xref:Xamarin.Forms.FlexWrap)합니다. 행에 맞출 항목이 너무 많은 경우이 속성 설정 하면 다음 행으로 래핑 항목입니다.

에 `FlexLayout` 의 자식인를 `ScrollView`입니다. 페이지에 맞게 행이 너무 많은 경우 해당 `ScrollView` 기본값이 `Orientation` 속성을 `Vertical` 세로 스크롤할 수 있도록 합니다.

`JustifyContent` 속성 각 항목 주위에 같은 크기의 빈 공간을 기본 축 (가로 축)에 남아 있는 공간을 할당 합니다.

코드 숨김 파일을 액세스 하는 샘플 사진 컬렉션에 추가 합니다는 `Children` 의 컬렉션을 `FlexLayout`:

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

프로그램 실행, 점진적으로 스크롤되는 위쪽에서 아래쪽은 다음과 같습니다.

[![사진 래핑 페이지](flex-layout-images/PhotoWrapping.png "사진 래핑 페이지")](flex-layout-images/PhotoWrapping-Large.png#lightbox)

### <a name="page-layout-with-flexlayout"></a>FlexLayout 사용 하 여 페이지 레이아웃

웹 디자인 호출 중인 표준 레이아웃 합니다 [ _인프라도_ ](https://en.wikipedia.org/wiki/Holy_grail_(web_design)) 매우 효과적 이지만 자주 한가요 구현 하기가 레이아웃 형식 이므로 합니다. 레이아웃은 페이지의 맨 위에 있는 헤더 및 바닥글 페이지의 전체 너비로 확장 맨 아래에 구성 됩니다. 기본 콘텐츠를 콘텐츠 및 보조 정보를 왼쪽에 열 메뉴를 사용 하 여 않지만 종종가 차지 하는 페이지의 가운데 (라고도 _따로_ 영역) 오른쪽에 있는 합니다. [CSS 유연한 상자 레이아웃 사양의 단원 5.4.1](http://www.w3.org/TR/css-flexbox-1/#order-accessibility) flex 상자를 사용 하 여 인프라도 레이아웃을 실현할 수 있습니다 하는 방법에 대해 설명 합니다.

**인프라도 레이아웃** 페이지의 **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** 샘플 하나를 사용 하 여이 레이아웃의 간단한 구현을 보여 줍니다. `FlexLayout` 다른 중첩 합니다. 이 페이지를 세로 모드로 휴대폰에 대 한 디자인 된 수 있으므로 콘텐츠 영역의 오른쪽 및 왼쪽 영역만 50 픽셀입니다.

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

여기이 실행 됩니다.

[![인프라도 레이아웃 페이지](flex-layout-images/HolyGrailLayout.png "인프라도 레이아웃 페이지")](flex-layout-images/HolyGrailLayout-Large.png#lightbox)

탐색 및 aside 영역이 렌더링 되는 `BoxView` 왼쪽 및 오른쪽에 있습니다.

첫 번째 `FlexLayout` 는 XAML 파일에 세로 기본 축이에 열으로 정렬 하는 세 명의 자식을 포함 합니다. 이들은, 머리글, 바닥글, 페이지의 본문입니다. 중첩 된 `FlexLayout` 에 가로 주요 축 세 자식 행으로 정렬 합니다.

이 프로그램에 연결 된 세 가지 바인딩 가능한 속성을 보여 줍니다.

- 합니다 `Order` 연결 된 바인딩 가능한 속성은 첫 번째 `BoxView`입니다. 이 속성은 기본값인 0 사용 하 여 정수입니다. 레이아웃 순서를 변경 하려면이 속성을 사용할 수 있습니다. 일반적으로 개발자는 태그 탐색 항목을 이전에 표시 되도록 페이지의 콘텐츠를 선호 하 고 따로 항목. 설정 된 `Order` 첫 번째 속성 `BoxView` 값으로 다른 형제 미만으로 인해 행의 첫 번째 항목으로 표시 합니다. 마찬가지로, 확실히 설정 하 여 마지막으로 나타나는 항목의 `Order` 형제 보다 큰 값으로 속성입니다.

- 합니다 `Basis` 연결 된 바인딩 가능한 속성은 두 가지 `BoxView` 항목 50 픽셀 너비를 제공 합니다. 이 속성은 형식의 `FlexBasis`, 형식의 정적 속성을 정의 하는 구조체 `FlexBasis` 라는 `Auto`, 기본값입니다. 사용할 수 있습니다 `Basis` 공간의 양을 나타내는 백분율 또는 픽셀 크기를 지정 하는 항목 기본 축의 차지 합니다. 호출을 _별로_ 모든 후속 레이아웃의 기반이 되는 항목 크기를 지정 하기 때문에 합니다.

- `Grow` 속성에서 중첩 된 `Layout` 및를 `Label` 자식 콘텐츠를 나타내는입니다. 이 속성은 형식의 `float` 있고 기본값은 0입니다. 양수 값으로 설정 하면 주 축 따라 나머지 공간을 모두 할당의 양수 값을 사용 하 여 형제를 해당 항목에 `Grow`입니다. 별모양 사양 등과 같이 값을 공간 크기를 비례적으로 할당 되는 `Grid`합니다.

    첫 번째 `Grow` 연결 된 속성을 설정에서 중첩 `FlexLayout`나타내는이 `FlexLayout` 외부에서 사용 되지 않는 모든 세로 공간을 차지 하는 것 `FlexLayout`입니다. 두 번째 `Grow` 에 연결 된 속성을 설정 합니다 `Label` 나타내는이 콘텐츠는 내부에서 사용 되지 않는 모든 가로 공간을 차지 하는 콘텐츠를 나타내는 `FlexLayout`합니다.

    이기도 유사한 `Shrink` 연결 된 자식의 크기의 크기를 초과 하는 경우 사용할 수 있는 바인딩 가능한 속성은 `FlexLayout` 래핑 원하지 않는 있지만.

### <a name="catalog-items-with-flexlayout"></a>FlexLayout 사용 하 여 카탈로그 항목

**카탈로그 항목** 페이지에 **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** 샘플 비슷합니다 [CSSFlex레이아웃상자사양의섹션1.1의예제1](http://www.w3.org/TR/css-flexbox-1/#overview)표시 된다는 점을 제외를 가로로 스크롤할 수 있는 일련의 사진 및 세 가지 원숭이 설명은:

[![카탈로그 항목 페이지](flex-layout-images/CatalogItems.png "카탈로그 항목 페이지")](flex-layout-images/CatalogItems-Large.png#lightbox)

각 세 원숭이는 `FlexLayout` 에 포함 된를 `Frame` 는 명시적 높이 너비를 지정 하는 보다 넓은 범위의 자식 역시 및 `FlexLayout`합니다. 이 XAML 파일의 속성 중 대부분에는 `FlexLayout` 자식은 암시적 스타일은 하나만 남기고 모두 스타일에 지정 된:

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

에 대 한 암시적 스타일을 `Image` 의 두 가지 연결 된 바인딩 가능한 속성의 설정을 포함 `Flexlayout`:

```xaml
<Style TargetType="Image">
    <Setter Property="FlexLayout.Order" Value="-1" />
    <Setter Property="FlexLayout.AlignSelf" Value="Center" />
</Style>
```

합니다 `Order` 설정 &ndash;1 원인 합니다 `Image` 요소가 중첩 된 각 먼저 표시 됩니다 `FlexLayout` 자식 컬렉션 내에서 해당 위치에 관계 없이 뷰. `AlignSelf` 속성을 `Center` 하면 합니다 `Image` 내에서 가운데 맞춤 될를 `FlexLayout`합니다. 설정을 재정의 합니다 `AlignItems` 속성의 기본값은의 `Stretch`즉는 `Label` 및 `Button` 자식의 전체 너비까지 확장 됩니다는 `FlexLayout`합니다.

세 개의 각 내 `FlexLayout` 빈 뷰 `Label` 앞에 `Button`, 되었지만 `Grow` 1 설정 합니다. 즉,는 추가 세로 공간을 모두 할당할이 빈 `Label`는 효과적으로 푸시를 `Button` 아래쪽으로 합니다.

<a name="bindable-properties" />

## <a name="the-bindable-properties-in-detail"></a>세부 정보에 바인딩 가능한 속성

지금까지 살펴본 몇 가지 일반적인 응용 프로그램의 했으므로 `FlexLayout`, 속성을 `FlexLayout` 자세히 탐색할 수 있습니다.
`FlexLayout` 에 설정 하는 6 개의 바인딩 가능한 속성 정의 `FlexLayout` 코드 또는 컨트롤 방향 및 맞춤을 XAML 자체입니다. (이러한 속성 중 하나 [ `Position` ](xref:Xamarin.Forms.FlexLayout.Position),이 문서에서는 다루지 않습니다.)

다섯 가지 나머지 바인딩 가능한 속성을 사용 하 여 실험할 수 있습니다 합니다 **실험** 페이지의 **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** 샘플. 이 페이지에서는 추가 하거나에서 자식을 제거할 수 있습니다는 `FlexLayout` 및 5 바인딩 가능한 속성 조합을 설정 합니다. 모든 자식을 `FlexLayout` 는 `Label` 다양 한 색과 크기의 뷰 사용 하 여는 `Text` 숫자에 해당 하는 해당 위치를 설정 하는 속성을 `Children` 컬렉션.

시작 될 때 프로그램을 5 `Picker` 이러한 5 개 기본 값을 표시 하는 뷰 `FlexLayout` 속성입니다. `FlexLayout` 화면 아래쪽에 세 명의 자식을 포함 합니다.

[![실험 페이지: 기본값](flex-layout-images/ExperimentDefault.png "실험 페이지-기본")](flex-layout-images/ExperimentDefault-Large.png#lightbox)

각 합니다 `Label` 보기에는 할당 된 공간을 보여 주는 회색 배경이 `Label` 내는 `FlexLayout`합니다. 배경색을 `FlexLayout` 자체는 연한 청회색 합니다. 왼쪽 및 오른쪽에 작은 여백을 제외 하 고 페이지의 전체 아래쪽 영역을 차지 합니다.

<a name="direction" />

### <a name="the-direction-property"></a>Direction 속성

합니다 [ `Direction` ](xref:Xamarin.Forms.FlexLayout.Direction) 형식의 속성이 [ `FlexDirection` ](xref:Xamarin.Forms.FlexDirection), 네 가지 멤버로 구성 된 열거형:

- `Column`
- `ColumnReverse` (또는 "열 역방향" XAML에서)
- `Row`기본값
- `RowReverse` (또는 "행 역방향" XAML에서)

XAML, 소문자, 대문자, 열거형 멤버 이름을 사용 하 여이 속성의 값을 지정할 수 있습니다 또는 혼합 된 사례와 CSS 표시기와 같습니다는 괄호로 표시 된 두 개의 추가 문자열을 사용할 수 있습니다. ("열-역방향" 및 "행 반복" 문자열에 정의 된 합니다 [ `FlexDirectionTypeConverter` ](xref:Xamarin.Forms.FlexDirectionTypeConverter) XAML 파서에서 사용 되는 클래스입니다.)

같습니다.는 **실험** (왼쪽에서 오른쪽)으로 표시 하는 페이지의 `Row` 방향 `Column` 방향 및 `ColumnReverse` 방향:

[![실험 페이지: 방향을](flex-layout-images/ExperimentDirection.png "방향 실험 페이지")](flex-layout-images/ExperimentDirection-Large.png#lightbox)

대 여 `Reverse` 오른쪽 또는 아래쪽에서 옵션을 항목의 시작 합니다.

<a name="wrap" />

### <a name="the-wrap-property"></a>줄 바꿈 속성

합니다 [ `Wrap` ](xref:Xamarin.Forms.FlexLayout.Wrap) 형식의 속성이 [ `FlexWrap` ](xref:Xamarin.Forms.FlexWrap), 세 가지 멤버로 구성 된 열거형:

- `NoWrap`기본값
- `Wrap`
- `Reverse` (또는 "줄 바꿈 역방향" XAML에서)

이 화면 왼쪽에서 오른쪽으로 표시 합니다 `NoWrap`하십시오 `Wrap` 및 `Reverse` 12 자식에 대 한 옵션:

[![실험 페이지: Wrap](flex-layout-images/ExperimentWrap.png "실험 페이지-줄 바꿈")](flex-layout-images/ExperimentWrap-Large.png#lightbox)

경우는 `Wrap` 속성이 `NoWrap` 주 축 (예:이 프로그램)에 제한 되 고 기본 축이 전각 또는 모든 자식에 맞게 충분히 긴는 `FlexLayout` iOS 스크린 샷으로 더 작은 항목을 확인 하려고 합니다. 방법을 보여 줍니다. 가진 품목의 shrinkness를 제어할 수 있습니다.는 [ `Shrink` ](#shrink) 바인딩 가능 속성을 연결 합니다.

<a name="justify-content" />

### <a name="the-justifycontent-property"></a>JustifyContent 속성

합니다 [ `JustifyContent` ](xref:Xamarin.Forms.FlexLayout.JustifyContent) 형식의 속성이 [ `FlexJustify` ](xref:Xamarin.Forms.FlexJustify), 여섯 가지 멤버로 구성 된 열거형:

- `Start` (또는 "가변 start" XAML에), 기본값
- `Center`
- `End` (또는 XAML의 "가변-end")
- `SpaceBetween` (또는 "공간 간의" XAML에서)
- `SpaceAround` (또는 "공간-주위에" XAML에서)
- `SpaceEvenly`

이 속성에 지정 하는 방법 항목 정도 까우 며이 예제에서 가로 축에는 기본 축의 합니다.

[![실험 페이지: 콘텐츠를 정당화](flex-layout-images/ExperimentJustifyContent.png "실험 페이지-콘텐츠 맞춤")](flex-layout-images/ExperimentJustifyContent-Large.png#lightbox)

모든 스크린샷 세 개에 `Wrap` 속성이 `Wrap`합니다. `Start` 기본 이전 Android 스크린샷에 표시 됩니다. IOS 다음 스크린샷에서 `Center` 옵션: 모든 항목 센터로 이동 됩니다. 세 가지 다른 옵션으로 시작 `Space` 항목 차지 하지 않는 추가 공간을 할당 합니다. `SpaceBetween` 할당 된 항목 동일 하 게 사이의 공간 `SpaceAround` puts 같은 각 항목 둘레의 공간 동안 `SpaceEvenly` puts 공간 각 항목 사이 및 첫 번째 항목 전후 행에서 마지막 항목 동일 합니다.

<a name="align-items" />

### <a name="the-alignitems-property"></a>AlignItems 속성

합니다 [ `AlignItems` ](xref:Xamarin.Forms.FlexLayout.AlignItems) 형식의 속성이 [ `FlexAlignItems` ](xref:Xamarin.Forms.FlexAlignItems), 네 가지 멤버로 구성 된 열거형:

- `Stretch`기본값
- `Center`
- `Start` (또는 "가변 start" XAML에서)
- `End` (또는 XAML의 "가변-end")

이 두 속성 중 하나입니다 (다른 되 고 [ `AlignContent` ](#align-content)) 자식 교차 축에 정렬 되는 방법을 나타내는입니다. 각 행 내에서 자식 (이전 스크린샷에 표시 된)으로 확장 기능은 다음 세 가지 스크린샷에서 표시 된 것 처럼 시작, 가운데 또는 각 항목의 끝에 정렬 합니다.

[![실험 페이지: 항목을 정렬](flex-layout-images/ExperimentAlignItems.png "실험 페이지-항목 정렬")](flex-layout-images/ExperimentAlignItems-Large.png#lightbox)

IOS 스크린샷에서 모든 자식의 위쪽을 맞춥니다. Android 스크린샷에서 항목은 세로로 가운데에 가장 높은 자식 기반 합니다. UWP 스크린샷에 있는 모든 항목의 아래쪽 정렬 됩니다.

모든 개별 항목에 대 한는 `AlignItems` 설정으로 재정의할 수 있습니다 합니다 [ `AlignSelf` ](#align-self) 바인딩 가능 속성을 연결 합니다.

<a name="align-content" />

### <a name="the-aligncontent-property"></a>AlignContent 속성

합니다 [ `AlignContent` ](xref:Xamarin.Forms.FlexLayout.AlignContent) 형식의 속성이 [ `FlexAlignContent` ](xref:Xamarin.Forms.FlexAlignContent), 7 멤버로 구성 된 열거형:

- `Stretch`기본값
- `Center`
- `Start` (또는 "가변 start" XAML에서)
- `End` (또는 XAML의 "가변-end")
- `SpaceBetween` (또는 "공간 간의" XAML에서)
- `SpaceAround` (또는 "공간-주위에" XAML에서)
- `SpaceEvenly`

와 같은 `AlignItems`, `AlignContent` 속성 교차 축에 정렬 되지만 전체 행 또는 열에 영향을 줍니다.

[![실험 페이지: 콘텐츠 맞춤](flex-layout-images/ExperimentAlignContent.png "실험 페이지-콘텐츠 맞춤")](flex-layout-images/ExperimentAlignContent-Large.png#lightbox)

IOS 스크린샷에서; 맨 위에 있는 두 행은 Android 스크린 샷 있을 때에 센터; 그리고 UWP 스크린샷에서 맨 아래에서. 행은 다양 한 방법으로 간격이 수 있습니다.

[![실험 페이지: 콘텐츠 2 Align](flex-layout-images/ExperimentAlignContent2.png "실험 페이지-정렬 콘텐츠 2")](flex-layout-images/ExperimentAlignContent2-Large.png#lightbox)

`AlignContent` 행 또는 열이 하나만 있으면 효과가 없습니다.

<a name="attached-properties" />

## <a name="the-attached-bindable-properties-in-detail"></a>세부 정보에서 연결된 된 바인딩 가능한 속성

`FlexLayout` 5 개의 연결 된 바인딩 가능한 속성을 정의합니다. 이러한 속성의 자식으로 설정 됩니다는 `FlexLayout` 되 고 해당 특정 자식 요소에 적용 합니다.

<a name="align-self" />

### <a name="the-alignself-property"></a>AlignSelf 속성

합니다 [ `AlignSelf` ](xref:Xamarin.Forms.FlexLayout.AlignSelfProperty) 형식의 연결 된 바인딩 가능한 속성은 [ `FlexAlignSelf` ](xref:Xamarin.Forms.FlexAlignContent), 다섯 가지 멤버로 구성 된 열거형:

- `Auto`기본값
- `Stretch`
- `Center`
- `Start` (또는 "가변 start" XAML에서)
- `End` (또는 XAML의 "가변-end")

개별 자식에 대 한를 `FlexLayout`, 재정의 설정 하는이 속성을 [ `AlignItems` ](#align-items) 에서 속성을 설정 합니다 `FlexLayout` 자체. 기본 설정인 `Auto` 사용 하는 의미는 `AlignItems` 설정 합니다.

에 대 한는 `Label` 라는 요소가 `label` (또는 예제)를 설정할 수 있습니다는 `AlignSelf` 다음과 같은 코드의 속성:

```csharp
FlexAlign.SetAlignSelf(label, FlexAlignSelf.Center);
```

에 대 한 참조는 합니다 `FlexLayout` 의 부모는 `Label`합니다. XAML을에서 다음과 같은 속성을 설정할 수 있습니다.

```xaml
<Label ... FlexAlign.AlignSelf="Center" ... />
```

### <a name="the-order-property"></a>Order 속성

합니다 [ `Order` ](xref:Xamarin.Forms.FlexLayout.OrderProperty) 형식의 속성이 `int`합니다. 기본값은 0입니다.

합니다 `Order` 속성을 사용 하면 순서를 변경할 수 있습니다는 자식의 `FlexLayout` 정렬 됩니다. 자식의 일반적으로 `FlexLayout` 정렬에 나타나는 순서와 같습니다는 `Children` 컬렉션입니다. 설정 하 여이 순서를 재정의할 수 있습니다는 `Order` 하나 이상의 자식에서 0이 아닌 정수 값에 바인딩 가능한 속성을 연결 합니다. `FlexLayout` 의 설정에 따라 자식 정렬 한 다음 합니다 `Order` 각 자식에 있지만 동일한 자식 속성 `Order` 설정에 표시 되는 순서 대로 정렬 되는 `Children` 컬렉션입니다.

### <a name="the-basis-property"></a>기본 속성

합니다 [ `Basis` ](xref:Xamarin.Forms.FlexLayout.BasisProperty) 연결 된 바인딩 가능한 속성의 자식에 할당 된 공간의 양을 나타냅니다는 `FlexLayout` 기본 축에 합니다. 지정 된 크기는 `Basis` 속성은 부모 주 축 따라 크기 `FlexLayout`합니다. 따라서 `Basis` 열에서 자식을 정렬 하는 경우 행 높이에 자식 항목을 정렬할 때 자식의 너비를 나타냅니다.

합니다 `Basis` 형식의 속성은 [ `FlexBasis` ](xref:Xamarin.Forms.FlexBasis), 구조입니다. 두 장치 독립적 단위 또는 크기의 백분율로 크기를 지정할 수는 `FlexLayout`합니다. 기본값은 `Basis` 속성은 정적 속성 `FlexBasis.Auto`, 자식의 너비 또는 높이 데 사용 됩니다 요청 했음을 의미 하는 합니다.

코드에서 설정할 수 있습니다는 `Basis` 에 대 한 속성을 `Label` 라는 `label` 같이 40 장치 독립적 단위:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40, false));
```

두 번째 인수는 `FlexBasis` 생성자 라고 `isRelative` 상대 크기 인지 여부를 나타냅니다 (`true`) 또는 절대 (`false`). 인수에는 기본값인 `false`이므로 다음 코드를 사용할 수도 있습니다.

```csharp
FlexLayout.SetBasis(label, new FlexBasis(40));
```

암시적 변환이 `float` 에 `FlexBasis` 정의 되어 있으므로 더 간소화할 수 있습니다.

```csharp
FlexLayout.SetBasis(label, 40);
```

크기의 25%로 설정할 수 있습니다는 `FlexLayout` 같이 부모:

```csharp
FlexLayout.SetBasis(label, new FlexBasis(0.25f, true));
```

소수 자릿수 값이 0 ~ 1 범위에 있어야 합니다.

XAML, 장치 독립적 단위에서 크기에 대 한 숫자를 사용할 수 있습니다.

```xaml
<Label ... FlexLayout.Basis="40" ... />
```

또는의 0 ~ 100% 범위의 백분율을 지정할 수 있습니다.

```xaml
<Label ... FlexLayout.Basis="25%" ... />
```

**별로 실험** 페이지를 **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** 샘플을 사용 하면 실험할 수 있습니다는 `Basis` 속성입니다. 페이지 5 래핑된 열이 표시 됩니다 `Label` 배경색 및 전경색을 교대로 반복 되는 요소입니다. 두 개의 `Slider` 요소에 지정할 수 있습니다 `Basis` 두 번째 및 네 번째 값 `Label`:

[![기본 페이지를 실험](flex-layout-images/BasisExperiment.png "기반 실험 페이지")](flex-layout-images/BasisExperiment-Large.png#lightbox)

왼쪽에 있는 iOS 스크린샷은 두 `Label` 장치 독립적 단위에서 높이 지정 된 요소입니다. 전체 높이의 비율을 된 높이 지정 된 Android 화면 표시를 `FlexLayout`입니다. 경우는 `Basis` 자식의 높이 100%로 설정 됩니다는 `FlexLayout`, 고 다음 열으로 줄 바꿈되면 하 및 UWP 스크린샷에서 보여 주듯이 해당 열의 전체 높이 차지 합니다: 5 명의 자식 행에서 정렬 되는 것 처럼 표시 됩니다 하지만 실제로 5 개의 열에 정렬 됩니다.

### <a name="the-grow-property"></a>속성을 증가

합니다 [ `Grow` ](xref:Xamarin.Forms.FlexLayout.GrowProperty) 형식의 연결 된 바인딩 가능한 속성은 `int`합니다. 기본값은 0 및 0 보다 크거나 값 이어야 합니다.

`Grow` 속성 역할을 때를 `Wrap` 속성이로 설정 되어 `NoWrap` 자식 행의 너비 보다 작은 총 너비가 있고를 `FlexLayout`, 자식의 열 보다 더 짧은 높이 또는 `FlexLayout`합니다. `Grow` 속성 자식 간에 남아 있는 공간을 할당 하는 방법을 나타냅니다.

에 **증가 실험** 5 페이지 `Label` 색이 교대로 반복 되는 요소는 열 및 두 개의 정렬 됩니다 `Slider` 요소를 사용 하면를 조정할를 `Grow` 속성의 두 번째 및 네 번째 `Label`. 맨 왼쪽 iOS 스크린샷은 기본 `Grow` 0의 속성:

[![증가 실험 페이지](flex-layout-images/GrowExperiment.png "증가 실험 페이지")](flex-layout-images/GrowExperiment-Large.png#lightbox)

하나의 자식 양수를 지정 하는 경우 `Grow` 값을 해당 자식 Android 스크린샷에서 보여 주듯이 모든 나머지 공간을 차지 합니다. 또한 둘 이상의 자식 간에이 공간을 할당할 수 있습니다. UWP 스크린 샷에서 `Grow` 두 번째 속성 `Label` 0.5로 설정 됩니다 하는 동안 합니다 `Grow` 네 번째 속성 `Label` 1.5 네 번째 제공 하는 `Label` 3 배의 두번째남아있는공간`Label`.

자식 뷰를 해당 공간을 사용 하는 방법을 하는 작업은 특정 형식의 자식에 따라 다릅니다. 에 대 한는 `Label`의 총 공간 내에서 텍스트를 배치할 수는 `Label` 속성을 사용 하 여 `HorizontalTextAlignment` 및 `VerticalTextAlignment`합니다.

<a name="shrink" />

### <a name="the-shrink-property"></a>축소 속성

합니다 [ `Shrink` ](xref:Xamarin.Forms.FlexLayout.ShrinkProperty) 형식의 연결 된 바인딩 가능한 속성은 `int`합니다. 기본값은 1이 하 고 0 보다 크거나 값 이어야 합니다.

`Shrink` 속성 역할을 때를 `Wrap` 속성이 `NoWrap` 고 자식 행의 집계 너비의 너비 보다 큰 경우는 `FlexLayout`, 자식의 단일 열의 집계 높이 보다 큽니다.는 높이 `FlexLayout`입니다. 일반적으로 `FlexLayout` 크기로 constricting 하 여 이러한 하위 개체에 표시 됩니다. `Shrink` 속성이 있는 자식에는 해당 전체 크기로 표시 되 고 우선 순위가 지정 됩니다 나타낼 수 있습니다.

합니다 **실험 축소** 페이지를 만듭니다를 `FlexLayout` 5의 단일 행이 있는 `Label` 보다 더 많은 공간을 필요로 하는 자식은 `FlexLayout` 너비입니다. 왼쪽에 있는 iOS 스크린샷은 모든는 `Label` 1의 기본값을 사용 하 여 요소:

[![축소가 실험 페이지](flex-layout-images/ShrinkExperiment.png "축소가 실험 페이지")](flex-layout-images/ShrinkExperiment-Large.png#lightbox)

Android 스크린 샷에서 `Shrink` 두 번째 값 `Label` 와 0으로 설정 되어 `Label` 전체 너비에 표시 됩니다. 네 번째 뿐만 `Label` 같습니다는 `Shrink` 1 보다 큰 값 및 축소 되었습니다. UWP 스크린샷은 둘 다 `Label` 지정 된 요소를 `Shrink` 경우 전체 크기로 표시 될 수 있도록 하려면 값 0 가능 합니다.

둘 다 설정할 수 있습니다 합니다 `Grow` 및 `Shrink` 집계 자식 크기 때로는 수 있는 보다 작거나의 크기 보다 큰 경우에 따라 상황에 맞게 값을 `FlexLayout`입니다.

## <a name="css-styling-with-flexlayout"></a>FlexLayout 사용 하 여 CSS 스타일 지정

사용할 수는 [CSS 스타일이](~/xamarin-forms/user-interface/styles/css/index.md) 연결 하 여 Xamarin.Forms 3.0에 도입 된 기능 `FlexLayout`합니다. **CSS 카탈로그 항목** 페이지를 **[FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)** 샘플의 레이아웃을 복제 합니다 **카탈로그 항목** 페이지 하지만 CSS를 사용 하 여 스타일 시트 스타일:

[![카탈로그 항목 페이지의 CSS](flex-layout-images/CssCatalogItems.png "CSS 카탈로그 항목 페이지")](flex-layout-images/CssCatalogItems-Large.png#lightbox)

원래 **CatalogItemsPage.xaml** 파일에는 5 `Style` 정의에서 해당 `Resources` 15 섹션 `Setter` 개체입니다. 에 **CssCatalogItemsPage.xaml** 파일을 2로 축소 되었습니다 `Style` 방금 4를 사용 하 여 정의 `Setter` 개체입니다. 이러한 스타일 Xamarin.Forms CSS 스타일 지정 기능은 현재 지원 하지 않는 속성에 대 한 CSS 스타일 시트를 보완 합니다.

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

각 항목의 두 요소간 포함 또한 `StyleClass` 설정:

```xaml
<Label Text="Seated Monkey" StyleClass="header" />
···
<Label StyleClass="empty" />
```

이러한 참조 선택기 합니다 **CatalogItemsStyles.css** 스타일 시트.

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

여러 `FlexLayout` 연결 된 바인딩 가능한 속성은 여기 참조 됩니다. 에 `label.empty` 선택기를 표시 합니다 `flex-grow` 빈 스타일 특성을 `Label` 위의 빈 공간을 제공 하는 `Button`. `image` 선택기를 포함를 `order` 특성 및 `align-self` 특성에 해당 하는 둘 다 `FlexLayout` 바인딩 가능한 속성을 연결 합니다.

지금까지 살펴본에서 직접 속성을 설정할 수는 `FlexLayout` 의 자식에 대해 연결 된 바인딩 가능한 속성을 설정할 수 있습니다는 `FlexLayout`합니다. 또는 기존 XAML 기반 스타일 또는 CSS 스타일 직접 하지 하 여 이러한 속성을 설정할 수 있습니다. 알고 있어야 하 고 이러한 속성을 이해 하는 중요 한 것입니다. 이러한 속성은 어떻게는 `FlexLayout` 진정으로 유연 합니다.

## <a name="flexlayout-with-xamarinuniversity"></a>FlexLayout Xamarin.University 사용 하 여

> [!VIDEO https://youtube.com/embed/Ng3sel_5D_0]

**Xamarin.Forms 3.0 Flex 레이아웃에서 [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>관련 링크

- [FlexLayoutDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/)
