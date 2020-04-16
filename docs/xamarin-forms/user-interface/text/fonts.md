---
title: 자마린의 글꼴.양식
description: 이 문서에서는 Xamarin.Forms 응용 프로그램에서 텍스트를 표시하는 컨트롤에 글꼴 정보를 지정하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2020
ms.openlocfilehash: ca4d3b242fcc73bb73e8d6ab1f817eefcc2ade4d
ms.sourcegitcommit: 89b3e383a37db5b940f0c63bbfe9cb806dc7d5d1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81388812"
---
# <a name="fonts-in-xamarinforms"></a>자마린의 글꼴.양식

[![샘플](~/media/shared/download.png) 다운로드 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)

이 문서에서는 Xamarin.Forms에서 텍스트를 표시하는 컨트롤에서 글꼴 특성(무게 및 크기 포함)을 지정하는 방법을 설명합니다. 글꼴 정보는 [코드에 지정하거나](#Setting_Font_in_Code) [XAML에 지정할](#Setting_Font_in_Xaml)수 있습니다. [사용자 지정 글꼴을](#use-a-custom-font)사용하고 [글꼴 아이콘을 표시할](#display-font-icons)수도 있습니다.

<a name="Setting_Font_in_Code" />

## <a name="set-the-font-in-code"></a>코드에서 글꼴 설정

텍스트를 표시하는 컨트롤의 세 글꼴 관련 속성을 사용합니다.

- **글꼴** &ndash; `string` 글꼴 이름을 패밀리합니다.
- **글꼴** &ndash; 글꼴 크기를 `double`의으로 크기 조정합니다.
- **FontAttributes** &ndash; *기울임꼴* 및 **굵게** 와 같은 스타일 `FontAttributes` 정보를 지정하는 문자열(C#의 열거형 사용).

이 코드는 레이블을 만들고 표시할 글꼴 크기와 가중치를 지정하는 방법을 보여 줍니다.

```csharp
var about = new Label
{
    FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
    FontAttributes = FontAttributes.Bold,
    Text = "Medium Bold Font"
};
```

<a name="FontSize" />

### <a name="font-size"></a>글꼴 크기

예를 `FontSize` 들어 속성을 이중 값으로 설정할 수 있습니다.

```csharp
label.FontSize = 24;
```

크기 값은 장치 독립적인 단위로 측정됩니다. 자세한 내용은 [측정 단위](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement)를 참조하십시오.

Xamarin.Forms는 특정 글꼴 [`NamedSize`](xref:Xamarin.Forms.NamedSize) 크기를 나타내는 열거형의 필드도 정의합니다. 명명된 글꼴 크기에 대한 자세한 내용은 [명명된 글꼴 크기를](#named-font-sizes)참조하십시오.

<a name="FontAttributes" />

### <a name="font-attributes"></a>글꼴 속성

`FontAttributes` **속성에서 굵게** 및 *기울임꼴과* 같은 글꼴 스타일을 설정할 수 있습니다. 다음 값은 현재 지원됩니다.

- **없음**
- **굵게**
- **기울임꼴**

열거형은 `FontAttribute` 다음과 같이 사용할 수 있습니다(단일 특성또는 `OR` 특성을 함께 지정할 수 있음).

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="set-font-info-per-platform"></a>플랫폼별 글꼴 정보 설정

또는 이 `Device.RuntimePlatform` 코드에서 설명한 대로 속성을 사용하여 각 플랫폼에서 다른 글꼴 이름을 설정할 수 있습니다.

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "MarkerFelt-Thin" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/ArimaMadurai-Black.ttf#Arima Madurai",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

iOS용 글꼴 정보의 좋은 소스는 [iosfonts.com.](http://iosfonts.com)

<a name="Setting_Font_in_Xaml" />

## <a name="set-the-font-in-xaml"></a>XAML에서 글꼴 설정

Xamarin.Forms 컨트롤 텍스트를 표시 `FontSize` 하는 모든 XAML에서 설정할 수 있는 속성을 가지고 있습니다. XAML에서 글꼴을 설정하는 가장 간단한 방법은 이 예제와 같이 명명된 크기 열거 값을 사용하는 것입니다.

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

모든 글꼴 설정을 XAML에서 문자열 값으로 표현할 수 있는 `FontSize` 속성에 대 한 기본 제공 변환기가 있습니다. 또한 속성을 `FontAttributes` 사용하여 글꼴 특성을 지정할 수 있습니다.

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

이 [`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-specific-values) 속성은 XAML에서 각 플랫폼에서 다른 글꼴을 렌더링하는 데 사용할 수도 있습니다. 아래 예제에서는 각 플랫폼에서 서로 다른 글꼴을 사용합니다.

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="MarkerFelt-Thin" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP" Value="Assets/Fonts/ArimaMadurai-Black.ttf#Arima Madurai" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

## <a name="named-font-sizes"></a>명명된 글꼴 크기

Xamarin.Forms는 특정 글꼴 크기를 나타내는 열거형의 [`NamedSize`](xref:Xamarin.Forms.NamedSize) 필드를 정의합니다. 다음 표에서는 `NamedSize` iOS, Android 및 유니버설 Windows 플랫폼(UWP)의 멤버 및 기본 크기를 보여 주었습니다.

| 멤버 | iOS | Android | UWP |
| --- | --- | --- | --- |
| `Default` | 16 | 14 | 14 |
| `Micro` | 11 | 10 | 15.667 |
| `Small` | 13 | 14 | 18.667 |
| `Medium` | 16 | 17 | 22.667 |
| `Large` | 20 | 22 | 32 |
| `Body` | 17 | 16 | 14 |
| `Header` | 17 | 96 | 46 |
| `Title` | 28 | 24 | 24 |
| `Subtitle` | 22 | 16 | 20 |
| `Caption` | 12 | 12 | 12 |

크기 값은 장치 독립적인 단위로 측정됩니다. 자세한 내용은 [측정 단위](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement)를 참조하십시오.

명명된 글꼴 크기는 XAML과 코드를 통해 설정할 수 있습니다. 또한 `Device.GetNamedSize` 명명된 글꼴 크기를 나타내는 `double` a를 반환하기 위해 메서드를 호출할 수 있습니다.

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

> [!NOTE]
> iOS 및 Android에서 명명된 글꼴 크기는 운영 체제 접근성 옵션에 따라 자동으로 조정됩니다. 이 동작은 플랫폼별 iOS에서 비활성화할 수 있습니다. 자세한 내용은 [iOS에서 명명된 글꼴 크기에 대한 내게 필요한 옵션 크기 조정을](~/xamarin-forms/platform/ios/named-font-size-scaling.md)참조하십시오.

## <a name="use-a-custom-font"></a>사용자 지정 글꼴 사용

사용자 지정 글꼴은 Xamarin.Forms 공유 프로젝트에 추가하고 추가 작업 없이 플랫폼 프로젝트에서 사용할 수 있습니다. 이 작업을 수행하는 프로세스는 다음과 같습니다.

1. Xamarin.Forms 공유 프로젝트에 포함된**리소스(빌드 작업: EmbeddedResource)로**글꼴을 추가합니다.
1. 특성을 사용하여 **AssemblyInfo.cs**와 같은 파일에 어셈블리에 `ExportFont` 글꼴 파일을 등록합니다. 선택적 별칭을 지정할 수도 있습니다.

> [!IMPORTANT]
> 포함된 글꼴은 Xamarin.Forms 4.5.0.530 이상의 사용을 필요로 합니다.

다음 예제에서는 별칭과 함께 어셈블리에 등록중인 랍스터 일반 글꼴을 보여 줍니다.

```csharp
using Xamarin.Forms;

[assembly: ExportFont("Lobster-Regular.ttf", Alias = "Lobster")]
```

> [!NOTE]
> 글꼴은 어셈블리에 글꼴을 등록할 때 폴더 이름을 지정하지 않고도 공유 프로젝트의 모든 폴더에 있을 수 있습니다.

그런 다음 파일 확장자 없이 이름을 참조하여 각 플랫폼에서 글꼴을 사용할 수 있습니다.

```xaml
<!-- Use font name -->
<Label Text="Hello Xamarin.Forms"
       FontFamily="Lobster-Regular" />
```

또는 별칭을 참조하여 각 플랫폼에서 사용할 수 있습니다.

```xaml
<!-- Use font alias -->
<Label Text="Hello Xamarin.Forms"
       FontFamily="Lobster" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
// Use font name
Label label1 = new Label
{
    Text = "Hello Xamarin.Forms!",
    FontFamily = "Lobster-Regular"
};

// Use font alias
Label label2 = new Label
{
    Text = "Hello Xamarin.Forms!",
    FontFamily = "Lobster"
};
```

다음 스크린샷은 사용자 지정 글꼴을 보여 줍니다.

[![iOS 및 안드로이드에서 사용자 정의 글꼴](fonts-images/custom-sml.png "사용자 지정 글꼴 예제")](fonts-images/custom.png#lightbox "사용자 지정 글꼴 예제")

> [!IMPORTANT]
> Windows에서 글꼴 파일 이름과 글꼴 이름이 다를 수 있습니다. Windows에서 글꼴 이름을 검색하려면 .ttf 파일을 마우스 오른쪽 단추로 클릭하고 **미리 보기를 선택합니다.** 그러면 미리 보기 창에서 글꼴 이름을 확인할 수 있습니다.

## <a name="display-font-icons"></a>글꼴 아이콘 표시

개체의 글꼴 아이콘 데이터를 지정하여 Xamarin.Forms 응용 프로그램에서 `FontImageSource` 글꼴 아이콘을 표시할 수 있습니다. [`ImageSource`](xref:Xamarin.Forms.ImageSource) 클래스에서 파생되는 이 클래스에는 다음과 같은 속성이 있습니다.

- `Glyph`- 글꼴 아이콘의 유니코드 문자 값으로 지정되어 있습니다. `string`
- `Size`– `double` 렌더링된 글꼴 아이콘의 장치 독립적 단위의 크기를 나타내는 값입니다. 기본값은 30입니다. 또한 이 속성을 명명된 글꼴 크기로 설정할 수 있습니다.
- `FontFamily`– `string` 글꼴 아이콘이 속한 글꼴 패밀리를 나타냅니다.
- `Color`– 글꼴 아이콘을 표시할 때 사용할 선택적 [`Color`](xref:Xamarin.Forms.Color) 값입니다.

이 데이터는 PNG를 만드는 데 사용되며, 이 PNG는 `ImageSource`을 표시할 수 있는 모든 뷰에서 표시할 수 있습니다. 이 방법을 사용하면 글꼴 아이콘 표시를 와 같은 단일 텍스트 표시 보기로 제한하는 대신 이모티콘과 같은 글꼴 [`Label`](xref:Xamarin.Forms.Label)아이콘을 여러 뷰로 표시할 수 있습니다.

> [!IMPORTANT]
> 글꼴 아이콘은 현재 유니코드 문자 표현으로만 지정할 수 있습니다.

다음 XAML 예제에는 뷰에 의해 표시되는 [`Image`](xref:Xamarin.Forms.Image) 단일 글꼴 아이콘이 있습니다.

```xaml
<Image BackgroundColor="#D1D1D1">
    <Image.Source>
        <FontImageSource Glyph="&#xf30c;"
                         FontFamily="{OnPlatform iOS=Ionicons, Android=ionicons.ttf#}"
                         Size="44" />
    </Image.Source>
</Image>
```

이 코드는 Ionicons 글꼴 패밀리의 XBox [`Image`](xref:Xamarin.Forms.Image) 아이콘을 뷰에 표시합니다. 이 아이콘의 유니코드 문자는 `\uf30c`XAML에서 이스케이프되어야 하므로. `&#xf30c;` 해당하는 C# 코드는 다음과 같습니다.

```csharp
Image image = new Image { BackgroundColor = Color.FromHex("#D1D1D1") };
image.Source = new FontImageSource
{
    Glyph = "\uf30c",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Ionicons" : "ionicons.ttf#",
    Size = 44
};
```

다음 스크린샷은 [바인딩가능한 레이아웃](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts) 샘플에서 바인딩가능한 레이아웃으로 표시되는 여러 글꼴 아이콘을 보여 줍니다.

![iOS 및 Android에서 표시되는 글꼴 아이콘의 스크린샷](fonts-images/font-image-source.png "이미지 보기에 표시되는 글꼴 아이콘")

## <a name="related-links"></a>관련 링크

- [글꼴 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)
- [텍스트(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [바인딩 가능한 레이아웃(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [바인딩 가능한 레이아웃](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
