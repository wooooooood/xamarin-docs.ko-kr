---
title: XAML의 인수 전달
description: 이 문서에서는 기본이 아닌 생성자, 팩터리 메서드를 호출 하 고 제네릭 인수의 형식을 지정 하 여 인수를 전달할 수 있는 XAML 특성을 사용 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 8F3B267F-499E-4D79-9193-FCA99F199519
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2016
ms.openlocfilehash: 80f332e45d6c46ad49543923e85cbb2eceadb378
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306562"
---
# <a name="passing-arguments-in-xaml"></a>XAML의 인수 전달

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-passingconstructorarguments)

_이 문서에서는 기본이 아닌 생성자에 인수를 전달 하 고, 팩터리 메서드를 호출 하 고, 제네릭 인수의 형식을 지정 하는 데 사용할 수 있는 XAML 특성을 사용 하는 방법을 보여 줍니다._

## <a name="overview"></a>개요

인수를 필요로 하는 생성자 또는 정적 creation 메서드를 호출 하 여 개체를 인스턴스화하는 데 필요한 경우가 있습니다. `x:Arguments` 및 `x:FactoryMethod` 특성을 사용 하 여 XAML에서이 작업을 수행할 수 있습니다.

- `x:Arguments` 특성은 기본이 아닌 생성자의 생성자 인수를 지정 하는 데 사용 되거나 팩터리 메서드 개체 선언에 사용 됩니다. 자세한 내용은 [생성자 인수 전달](#constructor_arguments)을 참조 하세요.
- `x:FactoryMethod` 특성은 개체를 초기화 하는 데 사용할 수 있는 팩터리 메서드를 지정 하는 데 사용 됩니다. 자세한 내용은 [팩터리 메서드 호출](#factory_methods)을 참조 하세요.

또한 `x:TypeArguments` 특성을 사용 하 여 제네릭 형식의 생성자에 대 한 제네릭 형식 인수를 지정할 수 있습니다. 자세한 내용은 [제네릭 형식 인수 지정](#generic_type_arguments)을 참조 하세요.

<a name="constructor_arguments" />

## <a name="passing-constructor-arguments"></a>생성자 인수 전달

`x:Arguments` 특성을 사용 하 여 기본이 아닌 생성자에 인수를 전달할 수 있습니다. 각 생성자 인수는 인수의 형식을 나타내는 XML 요소 내에서 구분 되어야 합니다. Xamarin.Forms 기본 형식에 대 한 다음과 같은 요소를 지원합니다.

- `x:Object`
- `x:Boolean`
- `x:Byte`
- `x:Int16`
- `x:Int32`
- `x:Int64`
- `x:Single`
- `x:Double`
- `x:Decimal`
- `x:Char`
- `x:String`
- `x:TimeSpan`
- `x:Array`
- `x:DateTime`

다음 코드 예제에서는 3 개의 [`Color`](xref:Xamarin.Forms.Color) 생성자를 사용 하 여 `x:Arguments` 특성을 사용 하는 방법을 보여 줍니다.

```xaml
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.9</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.25</x:Double>
        <x:Double>0.5</x:Double>
        <x:Double>0.75</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.8</x:Double>
        <x:Double>0.5</x:Double>
        <x:Double>0.2</x:Double>
        <x:Double>0.5</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
```

`x:Arguments` 태그 내의 요소 수와 이러한 요소의 형식은 [`Color`](xref:Xamarin.Forms.Color) 생성자 중 하 나와 일치 해야 합니다. 단일 매개 변수를 사용 하는 `Color` [생성자](xref:Xamarin.Forms.Color.%23ctor(System.Double)) 에는 0 (검정)에서 1 (흰색) 사이의 회색조 값이 필요 합니다. 세 개의 매개 변수가 있는 `Color` [생성자](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double)) 에는 빨강, 녹색 및 파랑 값 (0-1)이 필요 합니다. 4 개의 매개 변수가 있는 `Color` [생성자](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double,System.Double)) 는 알파 채널을 네 번째 매개 변수로 추가 합니다.

다음 스크린샷에는 지정 된 인수 값을 사용 하 여 각 [`Color`](xref:Xamarin.Forms.Color) 생성자를 호출한 결과가 나와 있습니다.

![BoxView. x:Arguments로 지정 된 색](passing-arguments-images/passing-arguments.png)

<a name="factory_methods" />

## <a name="calling-factory-methods"></a>팩터리 메서드를 호출합니다.

팩터리 메서드는 `x:FactoryMethod` 특성을 사용 하 여 메서드 이름을 지정 하 고 `x:Arguments` 특성을 사용 하 여 해당 인수를 지정 하 여 XAML에서 호출할 수 있습니다. 팩터리 메서드는 메서드를 정의 하는 클래스 또는 구조체와 동일한 형식의 개체 또는 값을 반환 하는 `public static` 메서드입니다.

[`Color`](xref:Xamarin.Forms.Color) 구조는 다양 한 팩터리 메서드를 정의 하 고, 다음 코드 예제에서는 세 가지 팩터리 메서드를 호출 하는 방법을 보여 줍니다.

```xaml
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromRgba">
      <x:Arguments>
        <x:Int32>192</x:Int32>
        <x:Int32>75</x:Int32>
        <x:Int32>150</x:Int32>                        
        <x:Int32>128</x:Int32>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromHsla">
      <x:Arguments>
        <x:Double>0.23</x:Double>
        <x:Double>0.42</x:Double>
        <x:Double>0.69</x:Double>
        <x:Double>0.7</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromHex">
      <x:Arguments>
        <x:String>#FF048B9A</x:String>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
```

`x:Arguments` 태그 내의 요소 수와 이러한 요소의 형식은 호출 되는 팩터리 메서드의 인수와 일치 해야 합니다. [`FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Int32,System.Int32,System.Int32,System.Int32)) factory 메서드에는 각각 0에서 255 사이의 빨강, 녹색, 파랑 및 알파 값을 나타내는 네 개의 [`Int32`](https://docs.microsoft.com/dotnet/api/system.int32) 매개 변수가 필요 합니다. [`FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double)) factory 메서드에는 각각 0에서 1 사이의 색상, 채도, 명도 및 알파 값을 나타내는 네 개의 [`Double`](https://docs.microsoft.com/dotnet/api/system.double) 매개 변수가 필요 합니다. [`FromHex`](xref:Xamarin.Forms.Color.FromHex(System.String)) factory 메서드에는 16 진수 (a) RGB 색을 나타내는 [`String`](https://docs.microsoft.com/dotnet/api/system.string) 필요 합니다.

다음 스크린샷에는 지정 된 인수 값을 사용 하 여 각 [`Color`](xref:Xamarin.Forms.Color) factory 메서드를 호출한 결과가 나와 있습니다.

![BoxView. x:FactoryMethod 및 x:Arguments로 지정 된 색](passing-arguments-images/factory-methods.png)

<a name="generic_type_arguments" />

## <a name="specifying-a-generic-type-argument"></a>제네릭 형식 인수를 지정합니다.

제네릭 형식의 생성자에 대 한 제네릭 형식 인수는 다음 코드 예제에서 보여 주는 것 처럼 `x:TypeArguments` 특성을 사용 하 여 지정할 수 있습니다.

```xaml
<ContentPage ...>
  <StackLayout>
    <StackLayout.Margin>
      <OnPlatform x:TypeArguments="Thickness">
        <On Platform="iOS" Value="0,20,0,0" />
        <On Platform="Android" Value="5, 10" />
        <On Platform="UWP" Value="10" />
      </OnPlatform>
    </StackLayout.Margin>
  </StackLayout>
</ContentPage>
```

[`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) 클래스는 제네릭 클래스 이며 대상 형식과 일치 하는 `x:TypeArguments` 특성을 사용 하 여 인스턴스화해야 합니다. [`On`](xref:Xamarin.Forms.On) 클래스에서 [`Platform`](xref:Xamarin.Forms.On.Platform) 특성은 단일 `string` 값 또는 쉼표로 구분 된 여러 `string` 값을 사용할 수 있습니다. 이 예제에서 [`StackLayout.Margin`](xref:Xamarin.Forms.View.Margin) 속성은 플랫폼별 [`Thickness`](xref:Xamarin.Forms.Thickness)로 설정 됩니다.

## <a name="summary"></a>요약

이 문서에서는 기본이 아닌 생성자, 팩터리 메서드를 호출 하 고 제네릭 인수의 형식을 지정 하 여 인수를 전달할 수 있는 XAML 특성을 사용 하 여 보여 줍니다.

## <a name="related-links"></a>관련 링크

- [XAML 네임스페이스](~/xamarin-forms/xaml/namespaces.md)
- [생성자 인수 전달 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-passingconstructorarguments)
- [팩터리 메서드 호출 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-callingfactorymethods)
