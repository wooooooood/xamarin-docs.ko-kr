---
title: XAML의 인수 전달
description: 이 문서는 팩터리 메서드를 호출 하 고 제네릭 인수의 형식을 지정 하려면 기본이 아닌 생성자에 인수를 전달 하는 데 사용할 수 있는 XAML 특성 사용 하 여 보여줍니다.
ms.prod: xamarin
ms.assetid: 8F3B267F-499E-4D79-9193-FCA99F199519
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2016
ms.openlocfilehash: c6ba3de9e50fd2ac452d9eeac169e4c1afd52ae0
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848436"
---
# <a name="passing-arguments-in-xaml"></a>XAML의 인수 전달

_이 문서는 팩터리 메서드를 호출 하 고 제네릭 인수의 형식을 지정 하려면 기본이 아닌 생성자에 인수를 전달 하는 데 사용할 수 있는 XAML 특성 사용 하 여 보여줍니다._

## <a name="overview"></a>개요

인수를 필요로 하는 생성자 또는 정적 생성 메서드를 호출 하 여 개체를 인스턴스화할 필요는 경우가 많습니다. 사용 하 여 XAML에서 수행할 수 있습니다는 `x:Arguments` 및 `x:FactoryMethod` 특성:

- `x:Arguments` 특성 팩터리 메서드 개체 선언 또는 기본이 아닌 생성자에 대 한 생성자 인수를 지정 하는 데 사용 됩니다. 자세한 내용은 참조 [생성자 인수를 전달](#constructor_arguments)합니다.
- `x:FactoryMethod` 특성 개체를 초기화 하는 데 사용할 수 있는 팩터리 메서드를 지정 하는 데 사용 됩니다. 자세한 내용은 참조 [팩터리 메서드를 호출](#factory_methods)합니다.

또한는 `x:TypeArguments` 제네릭 형식의 생성자에 제네릭 형식 인수를 지정할 특성을 사용할 수 있습니다. 자세한 내용은 참조 [제네릭 형식 인수를 지정](#generic_type_arguments)합니다.

<a name="constructor_arguments" />

## <a name="passing-constructor-arguments"></a>생성자 인수 전달

사용 하 여 기본이 아닌 생성자에 전달할 수 있는 인수는 `x:Arguments` 특성입니다. 각 생성자 인수는 인수 형식을 나타내는 XML 요소 내에서 구분 되어야 합니다. Xamarin.Forms는 기본 형식에 대 한 다음과 같은 요소를 지원합니다.

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

다음 코드 예제는 `x:Arguments` 3 개 특성 [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) 생성자:

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

에 포함 된 요소의 수는 `x:Arguments` 태그 및 이러한 요소 유형 중 하 나와 일치 해야 합니다는 [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) 생성자입니다. `Color` [생성자](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/) 단일 매개 변수를 1 (흰색)로 0 (검정) 회색조 값이 필요 합니다. `Color` [생성자](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/) 세 개의 매개 변수가 있는 0에서 1 사이의 빨강, 녹색 및 파랑 값을 지정 해야 합니다. `Color` [생성자](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/System.Double/) 4 개의 매개 변수가 있는 네 번째 매개 변수로 알파 채널을 추가 합니다.

각 호출의 결과 표시 하는 다음 스크린샷을 [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) 지정 된 인수 값이 포함 된 생성자:

![](passing-arguments-images/passing-arguments.png "지정 된 X:arguments BoxView.Color")

<a name="factory_methods" />

## <a name="calling-factory-methods"></a>팩터리 메서드를 호출합니다.

메서드를 지정 하 여 XAML의 팩터리 메서드를 호출할 수 있습니다를 사용 하 여 이름을 `x:FactoryMethod` 특성 및 해당 인수를 사용 하 여는 `x:Arguments` 특성입니다. 팩터리 메서드는 `public static` 개체 또는 클래스 또는 메서드를 정의 하는 구조와 동일한 형식의 값을 반환 하는 메서드입니다.

[ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) 구조는 다양 한 팩터리 메서드를 정의 하 고 다음 코드 예제에서는 호출 이러한 세 가지 방법을 보여 줍니다.

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

에 포함 된 요소의 수는 `x:Arguments` 태그 및 이러한 요소 유형의 호출 되는 팩터리 메서드에 인수 일치 해야 합니다. [ `FromRgba` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgba/p/System.Int32/System.Int32/System.Int32/System.Int32/) 팩터리 메서드를 사용 하려면 4 개 [ `Int32` ](https://docs.microsoft.com/dotnet/api/system.int32) 매개 변수를 0에서 255 까지의 각각 빨강, 녹색, 파랑 및 알파 값을 나타냅니다. [ `FromHsla` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHsla/p/System.Double/System.Double/System.Double/System.Double/) 팩터리 메서드를 사용 하려면 4 개 [ `Double` ](https://docs.microsoft.com/dotnet/api/system.double) 색상, 채도, 명도, 및 각각을 1로 0에서 까지의 알파 값을 나타내는 매개 변수입니다. [ `FromHex` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHex/p/System.String/) 팩터리 메서드를 사용 하려면 한 [ `String` ](https://docs.microsoft.com/dotnet/api/system.string) 16 진수 나타내는 RGB 색 (A).

다음 스크린샷에서 각 호출의 결과 표시 [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) 팩터리 메서드에 지정 된 인수 값을 사용 합니다.

![](passing-arguments-images/factory-methods.png "BoxView.Color X:factorymethod X:arguments로 지정")

<a name="generic_type_arguments" />

## <a name="specifying-a-generic-type-argument"></a>제네릭 형식 인수를 지정합니다.

사용 하 여 제네릭 형식의 생성자에 대 한 제네릭 형식 인수를 지정할 수 있습니다는 `x:TypeArguments` 다음 코드 예제에서와 같이 특성:

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

[ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) 클래스는 제네릭 클래스 이며으로 인스턴스화해야는 `x:TypeArguments` 대상 형식과 일치 하는 특성입니다. 에 [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) 클래스는 [ `Platform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Platform/) 특성은 단일을 받아들일 수 `string` 값 또는 쉼표로 구분 된 여러 `string` 값입니다. 이 예제는 [ `StackLayout.Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) 플랫폼 관련 속성은 [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/)합니다.

## <a name="summary"></a>요약

이 문서에 기본이 아닌 생성자, 팩터리 메서드를 호출 하 고 제네릭 인수의 형식을 지정 하 여 인수를 전달할 사용할 수 있는 XAML 특성 사용 명시 합니다.


## <a name="related-links"></a>관련 링크

- [XAML 네임스페이스](~/xamarin-forms/xaml/namespaces.md)
- [생성자 인수 전달 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/xaml/passingconstructorarguments/)
- [팩터리 메서드 호출 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/xaml/callingfactorymethods/)
