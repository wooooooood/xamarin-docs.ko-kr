---
title: Xamarin.Forms의 글꼴
description: 이 문서에서는 Xamarin.Forms 응용 프로그램에서 텍스트를 표시 하는 컨트롤의 글꼴 정보를 지정 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 1b90a3184b89ba9147525a87b52e048bbb59f5af
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53061155"
---
# <a name="fonts-in-xamarinforms"></a>Xamarin.Forms의 글꼴

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFonts/)

이 문서에서는 텍스트를 표시 하는 컨트롤에 Xamarin.Forms 글꼴 특성 (크기 및 무게 포함)을 지정할 수 있습니다 하는 방법을 설명 합니다. 글꼴 정보 일 수 있습니다 [코드에서 지정한](#Setting_Font_in_Code) 하거나 [XAML에 지정 된](#Setting_Font_in_Xaml)합니다.
사용할 수 이기도 한 [사용자 지정 글꼴](#Using_a_Custom_Font)합니다.

<a name="Setting_Font_in_Code" />

## <a name="setting-font-in-code"></a>코드에서 글꼴을 설정합니다.

텍스트를 표시 하는 모든 컨트롤의 3 개의 글꼴 관련 속성을 사용 합니다.

- **FontFamily** &ndash; 는 `string` 글꼴 이름입니다.
- **FontSize** &ndash; 의 글꼴 크기는 `double`합니다.
- **FontAttributes** &ndash; 와 같은 스타일 정보를 지정 하는 문자열 *기울임꼴* 및 **굵게** (사용 하 여는 `FontAttributes` C#의 열거형)입니다.

이 코드에는 레이블을 만들고 글꼴 크기 및 무게 표시할를 지정 하는 방법을 보여 줍니다.

```csharp
var about = new Label {
    FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
    FontAttributes = FontAttributes.Bold,
    Text = "Medium Bold Font"
};
```

<a name="FontSize" />

### <a name="font-size"></a>글꼴 크기

`FontSize` 속성이 double 값으로 예를 들어 설정할 수 있습니다.

```csharp
label.FontSize = 24;
```

사용할 수도 있습니다는 `NamedSize` 네 가지 기본 제공 옵션; 있는 열거형 Xamarin.Forms는 각 플랫폼에 대 한 최적의 크기를 선택합니다.

-  **Micro**
-  **작은**
-  **보통**
-  **큰**

`NamedSize` 열거형 일 수 있습니다 아무 곳에 나 사용을 `FontSize` 를 사용 하 여 지정할 수 있습니다 합니다 `Device.GetNamedSize` 값으로 변환 하는 방법을 `double`:

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

<a name="FontAttributes" />

### <a name="font-attributes"></a>글꼴 특성

글꼴 스타일과 같은 **굵게** 하 고 *기울임꼴* 에서 설정할 수 있습니다는 `FontAttributes` 속성. 다음 값은 현재 지원 됩니다.

-  **없음**
-  **굵게**
-  **기울임꼴**

합니다 `FontAttribute` 열거형을 다음과 같이 사용할 수 있습니다 (단일 특성을 지정할 수 있습니다 또는 `OR` 함께 해당):

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="setting-font-info-per-platform"></a>플랫폼 당 글꼴 정보 설정

또는 `Device.RuntimePlatform` 이 코드에서와 같이 각 플랫폼에서 다른 글꼴 이름을 설정 하려면 속성을 사용할 수 있습니다.

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/Lobster-Regular.ttf#Lobster",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

IOS에 대 한 글꼴 정보 좋은 원본은 [iosfonts.com](http://iosfonts.com)합니다.

<a name="Setting_Font_in_Xaml" />

## <a name="setting-the-font-in-xaml"></a>XAML에서 글꼴을 설정합니다.

Xamarin.Forms 컨트롤 모두 표시 텍스트를 `Font` XAML에서 설정할 수 있는 속성입니다. XAML에서 글꼴을 설정 하는 가장 간단한 방법은 다음과 같습니다. 명명 된 크기 열거형 값을 사용 하도록이 예와 같이

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

에 대 한 기본 제공 변환기는는 `Font` 속성을 사용 하는 모든 글꼴 설정을 XAML에서 문자열 값으로 표현 될 수 있습니다. 다음 예제에서는 XAML의 글꼴 특성 및 크기를 지정 하는 방법을 보여 줍니다.

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

여러 개 지정할 `Font` 하나로 필요한 설정을 결합 하는 설정을 `Font` 문자열 특성. 로 형식이 지정 된 글꼴 특성 문자열은 `"[font-face],[attributes],[size]"`합니다. 매개 변수의 순서는 중요, 모든 매개 변수는 선택 사항 및 여러 `attributes` 예를 들어 지정할 수 있습니다.

```xaml
<Label Text="Small bold text" Font="Bold, Micro" />
<Label Text="Medium custom font" Font="MarkerFelt-Thin, 42" />
<Label Text="Really big bold and italic text" Font="Bold, Italic, 72"  />
```

[`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-values) 데도 사용할 수 있습니다 XAML에서 각 플랫폼에서 다른 글꼴을 렌더링 합니다. 아래 예제에서는 사용자 지정 글꼴을 사용 하 여 iOS에서 (<span style="font-family:MarkerFelt-Thin">MarkerFelt 씬</span>) 다른 플랫폼에만 크기/특성을 지정 합니다.

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="MarkerFelt-Thin" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

사용자 지정 글꼴을 지정할 때 항상 사용 하는 것이 좋습니다 `OnPlatform`처럼 모든 플랫폼에서 사용할 수 있는 글꼴을 찾을 하기가 어렵습니다.

<a name="Using_a_Custom_Font" />

## <a name="using-a-custom-font"></a>사용자 지정 글꼴을 사용 하 여

기본 제공 서체가 아닌 글꼴을 사용 하 여 일부 플랫폼별 코딩이 필요 합니다. 사용자 지정 글꼴을 보여 주는이 스크린샷 **Lobster** 에서 [Google의 오픈 소스 글꼴](https://www.google.com/fonts) Xamarin.Forms를 사용 하 여 렌더링 합니다.

 [![IOS 및 Android에서 사용자 지정 글꼴](fonts-images/custom-sml.png "사용자 지정 글꼴 예제")](fonts-images/custom.png#lightbox "사용자 지정 글꼴 예제")

각 플랫폼에 필요한 단계는 다음과 같습니다. 응용 프로그램을 사용 하 여 사용자 지정 글꼴 파일을 포함 하는 경우에 배포에 대 한 글꼴의 라이선스를 허용 하는지 확인 해야 합니다.

### <a name="ios"></a>iOS

먼저 로드 됨을 보장 다음 Xamarin.Forms를 사용 하 여 이름으로 참조 하 여 사용자 지정 글꼴을 표시 하는 것이 불가능 `Font` 메서드.
지침을 따릅니다 [이 블로그 게시물](http://blog.xamarin.com/custom-fonts-in-ios/):

1. 사용 하 여 글꼴 파일 추가 **빌드 작업: BundleResource**, 및
2. 업데이트를 **Info.plist** 파일 (**응용 프로그램에서 제공 하는 글꼴**, 또는 `UIAppFonts`키,), 한 다음
3. Xamarin.Forms의 글꼴을 정의 하는 위치를 이름으로 참조!

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" : null // set only for iOS
}
```

### <a name="android"></a>Android

Android 용 Xamarin.Forms에는 특정 명명 표준에 따라 프로젝트에 추가 된 사용자 지정 글꼴을 참조할 수 있습니다. 먼저 글꼴 파일을 추가 합니다 **자산** 집합과 응용 프로그램 프로젝트에서 폴더 *빌드 작업: AndroidAsset*합니다. 그런 다음 전체 경로 사용 하 고 *글꼴 이름* 아래 코드 조각에서 보여 주듯이 Xamarin.Forms의 글꼴 이름으로 해시 (#)로 구분:

```csharp
new Label
{
  Text = "Hello, Forms!",
  FontFamily = Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : null // set only for Android
}
```

### <a name="windows"></a>Windows

Windows 플랫폼용 Xamarin.Forms에는 특정 명명 표준에 따라 프로젝트에 추가 된 사용자 지정 글꼴을 참조할 수 있습니다. 먼저 글꼴 파일을 추가 합니다 **/Assets 글꼴/** 집합과 응용 프로그램 프로젝트에서 폴더를 <span class="UIItem">빌드 작업: 콘텐츠</span>. 해시 (#) 뒤에 전체 경로 및 글꼴 파일을 사용 하 여 및 <span class="UIItem">글꼴 이름</span>처럼 아래 코드 조각을 보여 줍니다.

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.UWP ? "Assets/Fonts/Lobster-Regular.ttf#Lobster" : null // set only for UWP apps
}
```

> [!NOTE]
> 글꼴 이름과 글꼴 파일에 있는 참고 달라질 수 있습니다. Windows에서 글꼴 이름을 검색할.ttf 파일을 마우스 오른쪽 단추로 클릭 하 고 선택 **미리 보기**합니다. 글꼴 이름은 미리 보기 창에서 다음 확인할 수 있습니다.

응용 프로그램에 대한 공통 코드가 이제 완료되었습니다. 플랫폼 특정 전화 걸기 코드는 이제 [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)로 구현됩니다.

### <a name="xaml"></a>XAML

사용할 수도 있습니다 [ `Device.RuntimePlatform` ](~/xamarin-forms/platform/device.md#providing-platform-values) 사용자 지정 글꼴을 렌더링 하는 XAML에서:

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="Lobster-Regular" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

<a name="Summary" />

## <a name="summary"></a>요약

Xamarin.Forms 수 있도록 간단한 기본 설정을 제공 쉽게 모든 지원 되는 플랫폼에 대 한 텍스트 크기입니다. 또한 글꼴 및 크기를 지정할 수 있습니다 &ndash; 각 플랫폼에 대해 다르게도 &ndash; 보다 세부적으로 제어를 해야 하는 경우.

올바르게 서식이 지정 된 글꼴 특성을 사용 하 여 XAML에서 글꼴 정보를 지정할 수도 있습니다.

## <a name="related-links"></a>관련 링크

- [FontsSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFonts/)
- [텍스트 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/)
