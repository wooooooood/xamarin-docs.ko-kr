---
title: XAML의 인수 전달
description: 이 문서에서는 기본이 아닌 생성자, 팩터리 메서드를 호출 하 고 제네릭 인수의 형식을 지정 하 여 인수를 전달할 수 있는 XAML 특성을 사용 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 8F3B267F-499E-4D79-9193-FCA99F199519
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2016
ms.openlocfilehash: 1baad2e2edfb661fff9f3ef0ccf52c9922e9f351
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61178158"
---
# <a name="passing-arguments-in-xaml"></a>XAML의 인수 전달

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/xaml/passingconstructorarguments/)

_이 문서에서는 기본이 아닌 생성자, 팩터리 메서드를 호출 하 고 제네릭 인수의 형식을 지정 하 여 인수를 전달할 수 있는 XAML 특성을 사용 하는 방법을 보여 줍니다._

## <a name="overview"></a>개요

인수를 필요로 하는 생성자 또는 정적 creation 메서드를 호출 하 여 개체를 인스턴스화하는 데 필요한 경우가 있습니다. 이 사용 하 여 XAML에서 수행할 수 있습니다 합니다 `x:Arguments` 고 `x:FactoryMethod` 특성:

- `x:Arguments` 특성 팩터리 메서드 개체 선언 또는 기본이 아닌 생성자에 대 한 생성자 인수를 지정 하는 데 사용 됩니다. 자세한 내용은 [생성자 인수 전달](#constructor_arguments)합니다.
- `x:FactoryMethod` 특성 개체를 초기화 하는 팩터리 메서드를 지정 하는 데 사용 됩니다. 자세한 내용은 [팩터리 메서드 호출](#factory_methods)합니다.

또한는 `x:TypeArguments` 제네릭 형식의 생성자에 제네릭 형식 인수를 지정할 특성을 사용할 수 있습니다. 자세한 내용은 [제네릭 형식 인수를 지정](#generic_type_arguments)합니다.

<a name="constructor_arguments" />

## <a name="passing-constructor-arguments"></a>생성자 인수 전달

인수를 사용 하 여 기본이 아닌 생성자에 전달할 수는 `x:Arguments` 특성입니다. 각 생성자 인수는 인수의 형식을 나타내는 XML 요소 내에서 구분 되어야 합니다. Xamarin.Forms 기본 형식에 대 한 다음과 같은 요소를 지원합니다.

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

다음 코드 예제에서는 합니다 `x:Arguments` 세 가지 특성과 [ `Color` ](xref:Xamarin.Forms.Color) 생성자:

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

내 요소 수를 `x:Arguments` 태그 및 이러한 요소의 형식 중 하 나와 일치 해야 합니다 [ `Color` ](xref:Xamarin.Forms.Color) 생성자. 합니다 `Color` [생성자](xref:Xamarin.Forms.Color.%23ctor(System.Double)) 단일 매개 변수를 사용 하 여 (검은색) 0에서 1 (흰색) 회색조 값이 필요 합니다. 합니다 `Color` [생성자](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double)) 세 개의 매개 변수를 사용 하 여 0에서 1 까지의 빨간색, 녹색 및 파란색 값이 필요 합니다. 합니다 `Color` [생성자](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double,System.Double)) 네 개의 매개 변수를 사용 하 여 네 번째 매개 변수로 알파 채널을 추가 합니다.

각 호출의 결과 표시 하는 다음 스크린샷과 [ `Color` ](xref:Xamarin.Forms.Color) 지정 된 인수 값을 사용 하 여 생성자:

![](passing-arguments-images/passing-arguments.png "X: 인수를 사용 하 여 지정 된 BoxView.Color")

<a name="factory_methods" />

## <a name="calling-factory-methods"></a>팩터리 메서드를 호출합니다.

메서드를 지정 하 여 XAML에서 팩터리 메서드를 호출할 수 있습니다 사용 하 여 이름을 합니다 `x:FactoryMethod` 특성 및 해당 인수를 사용 하 여는 `x:Arguments` 특성입니다. 팩터리 메서드를 `public static` 개체 또는 클래스 또는 메서드를 정의 하는 구조와 동일한 형식의 값을 반환 하는 메서드입니다.

합니다 [ `Color` ](xref:Xamarin.Forms.Color) 구조 다양 한 팩터리 메서드를 정의 하 고 다음 코드 예제에서는 이러한 세 호출 방법을 보여 줍니다.

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

내 요소 수를 `x:Arguments` 태그 및 이러한 요소 유형 중에 호출 되는 팩터리 메서드에의 인수와 일치 해야 합니다. 합니다 [ `FromRgba` ](xref:Xamarin.Forms.Color.FromRgba(System.Int32,System.Int32,System.Int32,System.Int32)) 팩터리 메서드를 사용 하려면 네 [ `Int32` ](https://docs.microsoft.com/dotnet/api/system.int32) 매개 변수를 0에서 255 까지의 각각 빨강, 녹색, 파랑 및 알파 값을 나타냅니다. 합니다 [ `FromHsla` ](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double)) 팩터리 메서드를 사용 하려면 네 [ `Double` ](https://docs.microsoft.com/dotnet/api/system.double) 색상, 채도, 광도, 및 알파 값을 0에서 1로 각각 나타내는 매개 변수입니다. 합니다 [ `FromHex` ](xref:Xamarin.Forms.Color.FromHex(System.String)) 팩터리 메서드를 사용 하려면를 [ `String` ](https://docs.microsoft.com/dotnet/api/system.string) 나타내는 16 진수 RGB 색 (A).

각 호출의 결과 표시 하는 다음 스크린샷과 [ `Color` ](xref:Xamarin.Forms.Color) 지정 된 인수 값을 사용 하 여 팩터리 메서드입니다.

![](passing-arguments-images/factory-methods.png "BoxView.Color X:factorymethod 및 x: 인수를 사용 하 여 지정 합니다.")

<a name="generic_type_arguments" />

## <a name="specifying-a-generic-type-argument"></a>제네릭 형식 인수를 지정합니다.

사용 하 여 제네릭 형식의 생성자에 대 한 제네릭 형식 인수를 지정할 수 있습니다는 `x:TypeArguments` 다음 코드 예제에 설명 된 대로 특성:

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

합니다 [ `OnPlatform` ](xref:Xamarin.Forms.OnPlatform`1) 클래스는 제네릭 클래스 및 인스턴스화해야는 `x:TypeArguments` 대상 형식과 일치 하는 특성입니다. 에 [ `On` ](xref:Xamarin.Forms.On) 클래스를 [ `Platform` ](xref:Xamarin.Forms.On.Platform) 특성에는 단일 수락할 수 있습니다 `string` 값 또는 쉼표로 구분 된 여러 `string` 값입니다. 이 예제에서는 합니다 [ `StackLayout.Margin` ](xref:Xamarin.Forms.View.Margin) 속성이 플랫폼별으로 설정 되어 [ `Thickness` ](xref:Xamarin.Forms.Thickness)합니다.

## <a name="summary"></a>요약

이 문서에서는 기본이 아닌 생성자, 팩터리 메서드를 호출 하 고 제네릭 인수의 형식을 지정 하 여 인수를 전달할 수 있는 XAML 특성을 사용 하 여 보여 줍니다.


## <a name="related-links"></a>관련 링크

- [XAML 네임스페이스](~/xamarin-forms/xaml/namespaces.md)
- [생성자 인수 전달 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/xaml/passingconstructorarguments/)
- [팩터리 메서드 호출 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/xaml/callingfactorymethods/)
