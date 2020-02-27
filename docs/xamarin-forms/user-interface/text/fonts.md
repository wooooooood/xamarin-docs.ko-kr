---
title: Xamarin.Forms의 글꼴
description: 이 문서에서는 Xamarin.Forms 응용 프로그램에서 텍스트를 표시 하는 컨트롤의 글꼴 정보를 지정 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/20/2020
ms.openlocfilehash: 75cf5acdb862a15d722075269b2f736264be680f
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77636170"
---
# <a name="fonts-in-xamarinforms"></a>Xamarin.Forms의 글꼴

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)

이 문서에서는 텍스트를 표시 하는 컨트롤에 Xamarin.Forms 글꼴 특성 (크기 및 무게 포함)을 지정할 수 있습니다 하는 방법을 설명 합니다. 글꼴 정보는 [코드에서 지정](#Setting_Font_in_Code) 하거나 [XAML에](#Setting_Font_in_Xaml)지정할 수 있습니다. ' [사용자 지정 글꼴](#Using_a_Custom_Font)을 사용 하 고 [글꼴 아이콘을 표시할](#display-font-icons)수도 있습니다.

<a name="Setting_Font_in_Code" />

## <a name="set-the-font-in-code"></a>코드에 글꼴을 설정 합니다.

텍스트를 표시 하는 모든 컨트롤의 3 개의 글꼴 관련 속성을 사용 합니다.

- **FontFamily** &ndash; `string` 글꼴 이름입니다.
- **FontSize** &ndash; 글꼴 크기를 `double`합니다.
- **글꼴 특성** &ndash; *기울임꼴* 및 **Bold** 와 같은 스타일 정보를 지정 하는 문자열입니다 (의 C#`FontAttributes` 열거 사용).

이 코드에는 레이블을 만들고 글꼴 크기 및 무게 표시할를 지정 하는 방법을 보여 줍니다.

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

예를 들어 `FontSize` 속성을 double 값으로 설정할 수 있습니다.

```csharp
label.FontSize = 24;
```

크기 값은 장치 독립적 단위로 측정 됩니다. 자세한 내용은 [측정 단위](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement)를 참조 하세요.

또한 xamarin.ios는 특정 글꼴 크기를 나타내는 [`NamedSize`](xref:Xamarin.Forms.NamedSize) 열거형의 필드도 정의 합니다. 명명 된 글꼴 크기에 대 한 자세한 내용은 [명명 된 글꼴 크기](#named-font-sizes)를 참조 하십시오.

<a name="FontAttributes" />

### <a name="font-attributes"></a>글꼴 특성

**Bold** 및 *이탤릭체* 와 같은 글꼴 스타일은 `FontAttributes` 속성에서 설정할 수 있습니다. 다음 값은 현재 지원 됩니다.

- **없음**
- **굵게**
- **서식을**

`FontAttribute` 열거형은 다음과 같이 사용할 수 있습니다. 단일 특성을 지정 하거나 함께 `OR` 수 있습니다.

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="set-font-info-per-platform"></a>플랫폼 당 글꼴 정보 설정

또는이 코드에서 설명한 대로 `Device.RuntimePlatform` 속성을 사용 하 여 각 플랫폼에서 다른 글꼴 이름을 설정할 수 있습니다.

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "MarkerFelt-Thin" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/ArimaMadurai-Black.ttf#Arima Madurai",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

IOS에 대 한 올바른 글꼴 정보는 [iosfonts.com](http://iosfonts.com)입니다.

<a name="Setting_Font_in_Xaml" />

## <a name="set-the-font-in-xaml"></a>XAML에서 글꼴 설정

텍스트를 표시 하는 xamarin.ios 컨트롤에는 XAML에서 설정할 수 있는 `FontSize` 속성이 모두 있습니다. XAML에서 글꼴을 설정 하는 가장 간단한 방법은 다음과 같습니다. 명명 된 크기 열거형 값을 사용 하도록이 예와 같이

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

모든 글꼴 설정이 XAML에서 문자열 값으로 표현 될 수 있도록 하는 `FontSize` 속성에 대 한 기본 제공 변환기가 있습니다. 또한 `FontAttributes` 속성을 사용 하 여 글꼴 특성을 지정할 수 있습니다.

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

XAML에서 [`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-specific-values) 속성을 사용 하 여 각 플랫폼에서 다른 글꼴을 렌더링할 수도 있습니다. 아래 예제에서는 각 플랫폼에서 다른 글꼴을 사용 합니다.

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

## <a name="named-font-sizes"></a>명명 된 글꼴 크기

Xamarin.ios는 특정 글꼴 크기를 나타내는 [`NamedSize`](xref:Xamarin.Forms.NamedSize) 열거형의 필드를 정의 합니다. 다음 표에서는 `NamedSize` 멤버와 iOS, Android 및 유니버설 Windows 플랫폼 (UWP)의 기본 크기를 보여 줍니다.

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

명명 된 글꼴 크기는 XAML과 코드를 모두 통해 설정할 수 있습니다. 또한 `Device.GetNamedSize` 메서드를 호출 하 여 명명 된 글꼴 크기를 나타내는 `double`를 반환할 수 있습니다.

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

> [!NOTE]
> IOS 및 Android에서 명명 된 글꼴 크기는 운영 체제의 내게 필요한 옵션에 따라 자동으로 크기를 조정 합니다. 이 동작은 플랫폼별로 iOS에서 사용 하지 않도록 설정할 수 있습니다. 자세한 내용은 [iOS의 명명 된 글꼴 크기에 대 한 접근성 크기 조정](~/xamarin-forms/platform/ios/named-font-size-scaling.md)을 참조 하세요.

<a name="Using_a_Custom_Font" />

## <a name="use-a-custom-font"></a>사용자 지정 글꼴 사용

사용자 지정 글꼴은 Xamarin. Forms 공유 프로젝트에 추가 하 고 추가 작업 없이 플랫폼 프로젝트에서 사용 될 수 있습니다. 이 작업을 수행하는 프로세스는 다음과 같습니다.

1. Xamarin. Forms 공유 프로젝트에 포함 리소스로 글꼴을 추가 합니다 (**빌드 작업: EmbeddedResource**).
1. `ExportFont` 특성을 사용 하 여 어셈블리에 **AssemblyInfo.cs**와 같은 파일에 글꼴 파일을 등록 합니다.

다음 예제에서는 어셈블리에 등록 되는 Lobster 글꼴을 보여 줍니다.

```csharp
using Xamarin.Forms;

[assembly: ExportFont("Lobster-Regular.ttf")]
```

> [!NOTE]
> 글꼴은 어셈블리에 글꼴을 등록할 때 폴더 이름을 지정 하지 않고도 공유 프로젝트의 모든 폴더에 있을 수 있습니다.

그런 다음 파일 확장명 없이 이름을 참조 하 여 각 플랫폼에서 글꼴을 사용할 수 있습니다.

```xaml
<Label Text="Hello Xamarin.Forms"
       FontFamily="Lobster-Regular" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
Label label = new Label
{
    Text = "Hello Xamarin.Forms!",
    FontFamily = "Lobster-Regular"
};
```

다음 스크린샷은 사용자 지정 글꼴을 보여 줍니다.

[![IOS 및 Android의 사용자 지정 글꼴](fonts-images/custom-sml.png "사용자 지정 글꼴 예")](fonts-images/custom.png#lightbox "사용자 지정 글꼴 예")

## <a name="display-font-icons"></a>글꼴 아이콘 표시

글꼴 아이콘은 `FontImageSource` 개체에서 글꼴 아이콘 데이터를 지정 하 여 Xamarin.ios 응용 프로그램에서 표시할 수 있습니다. [`ImageSource`](xref:Xamarin.Forms.ImageSource) 클래스에서 파생 되는이 클래스에는 다음과 같은 속성이 있습니다.

- `Glyph` – `string`으로 지정 된 글꼴 아이콘의 유니코드 문자 값입니다.
- `Size` – 렌더링 된 글꼴 아이콘의 크기 (장치 독립적 단위)를 나타내는 `double` 값입니다. 기본값은 30입니다. 또한이 속성은 명명 된 글꼴 크기로 설정할 수 있습니다.
- `FontFamily` – 글꼴 아이콘이 속한 글꼴 패밀리를 나타내는 `string`입니다.
- `Color` – 글꼴 아이콘을 표시할 때 사용할 선택적 [`Color`](xref:Xamarin.Forms.Color) 값입니다.

이 데이터는 `ImageSource`를 표시할 수 있는 보기에서 표시할 수 있는 PNG를 만드는 데 사용 됩니다. 이 방법을 사용 하면 [`Label`](xref:Xamarin.Forms.Label)와 같이 글꼴 아이콘을 표시 하는 단일 텍스트를 표시 하는 것과는 반대로 여러 보기에서 emojis와 같은 글꼴 아이콘을 표시할 수 있습니다.

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

이 코드는 [`Image`](xref:Xamarin.Forms.Image) 보기에서 고 아이콘 글꼴 패밀리의 XBox 아이콘을 표시 합니다. 이 아이콘의 유니코드 문자는 `\uf30c`있지만 XAML로 이스케이프 되어야 하므로 `&#xf30c;`됩니다. 해당하는 C# 코드는 다음과 같습니다.

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
