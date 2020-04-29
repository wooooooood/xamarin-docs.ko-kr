---
title: Xamarin.ios의 글꼴
description: 이 문서에서는 Xamarin.ios 응용 프로그램에 텍스트를 표시 하는 컨트롤에 대 한 글꼴 정보를 지정 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2020
ms.openlocfilehash: 160cbbfa99114d74fa5fdaa5f92b8397ea7d3367
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82516486"
---
# <a name="fonts-in-xamarinforms"></a>Xamarin.ios의 글꼴

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)

이 문서에서는 Xamarin.ios를 사용 하 여 텍스트를 표시 하는 컨트롤의 글꼴 특성 (두께 및 크기 포함)을 지정 하는 방법을 설명 합니다. 글꼴 정보는 [코드에서 지정](#Setting_Font_in_Code) 하거나 [XAML에](#Setting_Font_in_Xaml)지정할 수 있습니다. ' [사용자 지정 글꼴](#use-a-custom-font)을 사용 하 고 [글꼴 아이콘을 표시할](#display-font-icons)수도 있습니다.

<a name="Setting_Font_in_Code" />

## <a name="set-the-font-in-code"></a>코드에 글꼴을 설정 합니다.

텍스트를 표시 하는 컨트롤의 세 가지 글꼴 관련 속성을 사용 합니다.

- `string` 글꼴 이름을 **FontFamily** &ndash; 합니다.
- 글꼴 크기를로 **FontSize** &ndash; 합니다 `double`.
- **글꼴 특성** &ndash; *기울임꼴* 및 **Bold** 와 같은 스타일 정보를 지정 하는 문자열 `FontAttributes` 입니다 (c #의 열거형 사용).

이 코드에서는 레이블을 만들고 표시할 글꼴 크기와 두께를 지정 하는 방법을 보여 줍니다.

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

예 `FontSize` 를 들어 속성을 double 값으로 설정할 수 있습니다.

```csharp
label.FontSize = 24;
```

크기 값은 장치 독립적 단위로 측정 됩니다. 자세한 내용은 [측정 단위](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement)를 참조 하세요.

또한 xamarin.ios는 특정 글꼴 크기를 나타내는 [`NamedSize`](xref:Xamarin.Forms.NamedSize) 열거형의 필드도 정의 합니다. 명명 된 글꼴 크기에 대 한 자세한 내용은 [명명 된 글꼴 크기](#named-font-sizes)를 참조 하십시오.

<a name="FontAttributes" />

### <a name="font-attributes"></a>글꼴 특성

속성에서 bold 및 *이탤릭체* 와 같은 글꼴 스타일을 설정할 수 있습니다. **bold** `FontAttributes` 현재 지원 되는 값은 다음과 같습니다.

- **없음**
- **글꼴로**
- **서식을**

열거형 `FontAttribute` 은 다음과 같이 사용할 수 있습니다. 단일 특성을 지정 하거나 `OR` 함께 지정할 수 있습니다.

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="set-font-info-per-platform"></a>플랫폼 당 글꼴 정보 설정

또는이 코드 `Device.RuntimePlatform` 에서 설명한 대로 속성을 사용 하 여 각 플랫폼에서 다른 글꼴 이름을 설정할 수 있습니다.

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "MarkerFelt-Thin" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/ArimaMadurai-Black.ttf#Arima Madurai",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

IOS에 대 한 올바른 글꼴 정보는 [iosfonts.com](http://iosfonts.com)입니다.

<a name="Setting_Font_in_Xaml" />

## <a name="set-the-font-in-xaml"></a>XAML에서 글꼴 설정

텍스트를 표시 하는 xamarin.ios 컨트롤에는 XAML `FontSize` 에서 설정할 수 있는 속성이 있습니다. XAML에서 글꼴을 설정 하는 가장 간단한 방법은 다음 예제에 표시 된 것 처럼 명명 된 크기 열거형 값을 사용 하는 것입니다.

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

모든 글꼴 설정이 XAML에서 문자열 값으로 표현 `FontSize` 될 수 있도록 하는 속성에 대 한 기본 제공 변환기가 있습니다. 또한 `FontAttributes` 속성을 사용 하 여 글꼴 특성을 지정할 수 있습니다.

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

또한 [`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#provide-platform-specific-values) 속성은 각 플랫폼에서 다른 글꼴을 렌더링 하기 위해 XAML에서 사용할 수 있습니다. 아래 예제에서는 각 플랫폼에서 다른 글꼴을 사용 합니다.

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

Xamarin.ios는 특정 글꼴 크기를 나타내는 [`NamedSize`](xref:Xamarin.Forms.NamedSize) 열거형의 필드를 정의 합니다. 다음 표에서는 `NamedSize` 구성원 및 IOS, Android 및 유니버설 WINDOWS 플랫폼 (UWP)의 기본 크기를 보여 줍니다.

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

크기 값은 장치 독립적 단위로 측정 됩니다. 자세한 내용은 [측정 단위](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement)를 참조 하세요.

명명 된 글꼴 크기는 XAML과 코드를 모두 통해 설정할 수 있습니다. 또한 `Device.GetNamedSize` 메서드를 호출 하 여 명명 된 글꼴 크기를 `double` 나타내는를 반환할 수 있습니다.

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

> [!NOTE]
> IOS 및 Android에서 명명 된 글꼴 크기는 운영 체제의 내게 필요한 옵션에 따라 자동으로 크기를 조정 합니다. 이 동작은 플랫폼별로 iOS에서 사용 하지 않도록 설정할 수 있습니다. 자세한 내용은 [iOS의 명명 된 글꼴 크기에 대 한 접근성 크기 조정](~/xamarin-forms/platform/ios/named-font-size-scaling.md)을 참조 하세요.

## <a name="use-a-custom-font"></a>사용자 지정 글꼴 사용

사용자 지정 글꼴은 Xamarin. Forms 공유 프로젝트에 추가 하 고 추가 작업 없이 플랫폼 프로젝트에서 사용 될 수 있습니다. 이 작업을 수행하는 프로세스는 다음과 같습니다.

1. Xamarin. Forms 공유 프로젝트에 포함 리소스로 글꼴을 추가 합니다 (**빌드 작업: EmbeddedResource**).
1. 특성을 사용 하 여 어셈블리에 AssemblyInfo.cs와 같은 파일에 글꼴 파일을 등록 합니다. **AssemblyInfo.cs** `ExportFont` 선택적 별칭을 지정할 수도 있습니다.

> [!IMPORTANT]
> 포함 된 글꼴을 사용 하려면 Xamarin.ios 4.5.0.530 이상을 사용 해야 합니다.

다음 예제에서는 별칭과 함께 어셈블리에 등록 되는 Lobster 글꼴을 보여 줍니다.

```csharp
using Xamarin.Forms;

[assembly: ExportFont("Lobster-Regular.ttf", Alias = "Lobster")]
```

> [!NOTE]
> 글꼴은 어셈블리에 글꼴을 등록할 때 폴더 이름을 지정 하지 않고도 공유 프로젝트의 모든 폴더에 있을 수 있습니다.

그런 다음 파일 확장명 없이 이름을 참조 하 여 각 플랫폼에서 글꼴을 사용할 수 있습니다.

```xaml
<!-- Use font name -->
<Label Text="Hello Xamarin.Forms"
       FontFamily="Lobster-Regular" />
```

또는 해당 별칭을 참조 하 여 각 플랫폼에서 사용 될 수 있습니다.

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

[![IOS 및 Android의 사용자 지정 글꼴](fonts-images/custom-sml.png "사용자 지정 글꼴 예")](fonts-images/custom.png#lightbox "사용자 지정 글꼴 예")

> [!IMPORTANT]
> Windows에서는 글꼴 파일 이름과 글꼴 이름이 다를 수 있습니다. Windows에서 글꼴 이름을 검색 하려면 .ttf 파일을 마우스 오른쪽 단추로 클릭 하 고 **미리 보기**를 선택 합니다. 그런 다음 미리 보기 창에서 글꼴 이름을 확인할 수 있습니다.

## <a name="display-font-icons"></a>글꼴 아이콘 표시

글꼴 아이콘 데이터를 `FontImageSource` 개체에 지정 하 여 xamarin.ios 응용 프로그램에서 글꼴 아이콘을 표시할 수 있습니다. [`ImageSource`](xref:Xamarin.Forms.ImageSource) 클래스에서 파생 되는이 클래스에는 다음과 같은 속성이 있습니다.

- `Glyph`–로 지정 된 글꼴 아이콘의 유니코드 문자 값입니다 `string`.
- `Size`– 렌더링 `double` 된 글꼴 아이콘의 크기 (장치 독립적 단위)를 나타내는 값입니다. 기본값은 30입니다. 또한이 속성은 명명 된 글꼴 크기로 설정할 수 있습니다.
- `FontFamily`– 글꼴 `string` 아이콘이 속한 글꼴 패밀리를 나타내는입니다.
- `Color`– 글꼴 아이콘 [`Color`](xref:Xamarin.Forms.Color) 을 표시할 때 사용 되는 선택적 값입니다.

이 데이터는를 표시할 수 있는 보기에서 표시할 수 있는 PNG를 만드는 데 사용 됩니다 `ImageSource`. 이 접근 방식을 사용 하면 [`Label`](xref:Xamarin.Forms.Label)글꼴 아이콘 표시를 같은 단일 텍스트 (예:)로 제한 하는 것과 달리 여러 뷰에 표시 되는 emojis와 같은 글꼴 아이콘을 사용할 수 있습니다.

> [!IMPORTANT]
> 글꼴 아이콘은 현재 유니코드 문자 표시로만 지정할 수 있습니다.

다음 XAML 예제에는 [`Image`](xref:Xamarin.Forms.Image) 뷰에서 표시 되는 단일 글꼴 아이콘이 있습니다.

```xaml
<Image BackgroundColor="#D1D1D1">
    <Image.Source>
        <FontImageSource Glyph="&#xf30c;"
                         FontFamily="{OnPlatform iOS=Ionicons, Android=ionicons.ttf#}"
                         Size="44" />
    </Image.Source>
</Image>
```

이 코드는 아이콘 글꼴 패밀리의 [`Image`](xref:Xamarin.Forms.Image) 보기에서 XBox 아이콘을 표시 합니다. 이 아이콘 `\uf30c`의 유니코드 문자는 이지만 XAML에서 이스케이프 되어야 하므로가 됩니다 `&#xf30c;`. 해당하는 C# 코드는 다음과 같습니다.

```csharp
Image image = new Image { BackgroundColor = Color.FromHex("#D1D1D1") };
image.Source = new FontImageSource
{
    Glyph = "\uf30c",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Ionicons" : "ionicons.ttf#",
    Size = 44
};
```

[바인딩](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts) 가능한 레이아웃 샘플에서 다음 스크린샷에는 바인딩 가능한 레이아웃에 의해 표시 되는 몇 가지 글꼴 아이콘이 표시 됩니다.

![IOS 및 Android에서 표시 되는 글꼴 아이콘의 스크린샷](fonts-images/font-image-source.png "이미지 뷰에 표시 되는 글꼴 아이콘")

## <a name="related-links"></a>관련 링크

- [FontsSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)
- [텍스트 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [바인딩 가능한 레이아웃 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [바인딩 가능한 레이아웃](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
