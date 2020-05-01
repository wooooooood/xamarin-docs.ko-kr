---
title: Xamarin.ios XAML의 제네릭
description: Xamarin.ios XAML은 제네릭 제약 조건을 형식 인수로 지정 하 여 제네릭 CLR 형식을 사용할 수 있도록 지원 합니다.
ms.prod: xamarin
ms.assetid: 97B73048-4F90-41AD-AB48-8EB804C4998B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/28/2020
ms.openlocfilehash: 9cda08a3bab0e25db2315c9795721e25d47d2429
ms.sourcegitcommit: 154a3e7aec775327565bb54eda1a610976af1d6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82624711"
---
# <a name="generics-in-xamarinforms-xaml"></a>Xamarin.ios XAML의 제네릭

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-generics/)

Xamarin.ios XAML은 제네릭 제약 조건을 형식 인수로 지정 하 여 제네릭 CLR 형식을 사용할 수 있도록 지원 합니다. 이 지원은 제네릭의 제약 형식 `x:TypeArguments` 인수를 제네릭 형식의 생성자에 전달 하는 지시문을 통해 제공 됩니다.

> [!IMPORTANT]
> `x:TypeArguments` 지시문을 사용 하 여 xamarin.ios XAML에서 제네릭 클래스를 정의 하는 것은 지원 되지 않습니다.

형식 인수는 문자열로 지정 되며 일반적으로 접두사 (예: `sys:String` 및 `sys:Int32`)로 지정 됩니다. 일반적인 형식의 CLR 제네릭 제약 조건은 기본 Xamarin.ios 네임 스페이스에 매핑되지 않은 라이브러리에서 제공 되기 때문에 접두사를 입력 해야 합니다. 그러나 `x:String` 및 `x:Int32`와 같은 xaml 2009 기본 제공 형식을 형식 인수로 지정할 수도 있습니다. 여기서 `x` 는 xaml 2009의 xaml 언어 네임 스페이스입니다. XAML 2009 기본 제공 형식에 대 한 자세한 내용은 [xaml 2009 언어 기본](/dotnet/desktop-wpf/xaml-services/types-for-primitives#xaml-2009-language-primitives)형식을 참조 하세요.

쉼표 구분 기호를 사용 하 여 여러 형식 인수를 지정할 수 있습니다. 또한 제네릭 제약 조건이 제네릭 형식을 사용 하는 경우 중첩 된 제약 조건 형식 인수는 괄호 안에 포함 되어야 합니다.

> [!NOTE]
> 태그 `x:Type` 확장은 제네릭 형식에 대 한 CLR 형식 참조를 제공 하 고 c #의 `typeof` 연산자와 유사한 함수를 제공 합니다. 자세한 내용은 [x:Type 태그 확장](~/xamarin-forms/xaml/markup-extensions/consuming.md#type)을 참조 하세요.

## <a name="single-primitive-type-argument"></a>단일 기본 형식 인수

`x:TypeArguments` 지시문을 사용 하 여 단일 기본 형식 인수를 접두사가 있는 문자열 인수로 지정할 수 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:scg="clr-namespace:System.Collections.Generic;assembly=netstandard"
             ...>
    <CollectionView>
        <CollectionView.ItemsSource>
            <scg:List x:TypeArguments="x:String">
                <x:String>Baboon</x:String>
                <x:String>Capuchin Monkey</x:String>
                <x:String>Blue Monkey</x:String>
                <x:String>Squirrel Monkey</x:String>
                <x:String>Golden Lion Tamarin</x:String>
                <x:String>Howler Monkey</x:String>
                <x:String>Japanese Macaque</x:String>
            </scg:List>
        </CollectionView.ItemsSource>
    </CollectionView>
</ContentPage>
```

이 예제에서 `System.Collections.Generic` 는 `scg` XAML 네임 스페이스로 정의 됩니다. `CollectionView.ItemsSource` 속성은 XAML 2009 기본 제공 `List<T>` `x:String` 형식을 사용 하 여 `string` 형식 인수를 사용 하 여 인스턴스화된로 설정 됩니다. 컬렉션 `List<string>` 은 여러 `string` 항목으로 초기화 됩니다.

또는 CLR `String` 형식을 사용 하 여 `List<T>` 컬렉션을 인스턴스화할 수 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:scg="clr-namespace:System.Collections.Generic;assembly=netstandard"
             xmlns:sys="clr-namespace:System;assembly=netstandard"
             ...>
    <CollectionView>
        <CollectionView.ItemsSource>
            <scg:List x:TypeArguments="sys:String">
                <sys:String>Baboon</sys:String>
                <sys:String>Capuchin Monkey</sys:String>
                <sys:String>Blue Monkey</sys:String>
                <sys:String>Squirrel Monkey</sys:String>
                <sys:String>Golden Lion Tamarin</sys:String>
                <sys:String>Howler Monkey</sys:String>
                <sys:String>Japanese Macaque</sys:String>
            </scg:List>
        </CollectionView.ItemsSource>
    </CollectionView>
</ContentPage>
```

## <a name="single-object-type-argument"></a>단일 개체 형식 인수

`x:TypeArguments` 지시문을 사용 하 여 단일 개체 형식 인수를 접두사가 있는 문자열 인수로 지정할 수 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:models="clr-namespace:GenericsDemo.Models"
             xmlns:scg="clr-namespace:System.Collections.Generic;assembly=netstandard"
             ...>
    <CollectionView>
        <CollectionView.ItemsSource>
            <scg:List x:TypeArguments="models:Monkey">
                <models:Monkey Name="Baboon"
                               Location="Africa and Asia"
                               ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg" />
                <models:Monkey Name="Capuchin Monkey"
                               Location="Central and South America"
                               ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/4/40/Capuchin_Costa_Rica.jpg/200px-Capuchin_Costa_Rica.jpg" />
                <models:Monkey Name="Blue Monkey"
                               Location="Central and East Africa"
                               ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/BlueMonkey.jpg/220px-BlueMonkey.jpg" />
            </scg:List>
        </CollectionView.ItemsSource>
        <CollectionView.ItemTemplate>
            <DataTemplate>
                <Grid Padding="10">
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="Auto" />
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto" />
                        <ColumnDefinition Width="Auto" />
                    </Grid.ColumnDefinitions>
                    <Image Grid.RowSpan="2"
                           Source="{Binding ImageUrl}"
                           Aspect="AspectFill"
                           HeightRequest="60"
                           WidthRequest="60" />
                    <Label Grid.Column="1"
                           Text="{Binding Name}"
                           FontAttributes="Bold" />
                    <Label Grid.Row="1"
                           Grid.Column="1"
                           Text="{Binding Location}"
                           FontAttributes="Italic"
                           VerticalOptions="End" />
                </Grid>
            </DataTemplate>
        </CollectionView.ItemTemplate>
    </CollectionView>
</ContentPage>
```

`GenericsDemo.Models` 이 예제에서 `models` 는 xaml 네임 스페이스로 정의 되며 `System.Collections.Generic` 는 `scg` xaml 네임 스페이스로 정의 됩니다. `CollectionView.ItemsSource` 속성은 `Monkey` 형식 인수를 사용 하 여 인스턴스화된로 `List<T>` 설정 됩니다. 컬렉션 `List<Monkey>` 은 여러 `Monkey` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 항목으로 초기화 되 고 각 `Monkey` 개체의 모양을 정의 하는는의로 `ItemTemplate` 설정 됩니다. [`CollectionView`](xref:Xamarin.Forms.CollectionView)

## <a name="multiple-type-arguments"></a>여러 형식 인수

`x:TypeArguments` 지시문을 사용 하 여 여러 형식 인수를 쉼표로 구분 하 여 접두사가 있는 문자열 인수로 지정할 수 있습니다. 제네릭 제약 조건에 제네릭 형식을 사용 하는 경우 중첩 된 제약 조건 형식 인수는 괄호로 묶여 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:models="clr-namespace:GenericsDemo.Models"
             xmlns:scg="clr-namespace:System.Collections.Generic;assembly=netstandard"
             ...>
    <CollectionView>
        <CollectionView.ItemsSource>
            <scg:List x:TypeArguments="scg:KeyValuePair(x:String,models:Monkey)">
                <scg:KeyValuePair x:TypeArguments="x:String,models:Monkey">
                    <x:Arguments>
                        <x:String>Baboon</x:String>
                        <models:Monkey Location="Africa and Asia"
                                       ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg" />
                    </x:Arguments>
                </scg:KeyValuePair>
                <scg:KeyValuePair x:TypeArguments="x:String,models:Monkey">
                    <x:Arguments>
                        <x:String>Capuchin Monkey</x:String>
                        <models:Monkey Location="Central and South America"
                                       ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/4/40/Capuchin_Costa_Rica.jpg/200px-Capuchin_Costa_Rica.jpg" />   
                    </x:Arguments>
                </scg:KeyValuePair>
                <scg:KeyValuePair x:TypeArguments="x:String,models:Monkey">
                    <x:Arguments>
                        <x:String>Blue Monkey</x:String>
                        <models:Monkey Location="Central and East Africa"
                                       ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/BlueMonkey.jpg/220px-BlueMonkey.jpg" />
                    </x:Arguments>
                </scg:KeyValuePair>
            </scg:List>
        </CollectionView.ItemsSource>
        <CollectionView.ItemTemplate>
            <DataTemplate>
                <Grid Padding="10">
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="Auto" />
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto" />
                        <ColumnDefinition Width="Auto" />
                    </Grid.ColumnDefinitions>
                    <Image Grid.RowSpan="2"
                           Source="{Binding Value.ImageUrl}"
                           Aspect="AspectFill"
                           HeightRequest="60"
                           WidthRequest="60" />
                    <Label Grid.Column="1"
                           Text="{Binding Key}"
                           FontAttributes="Bold" />
                    <Label Grid.Row="1"
                           Grid.Column="1"
                           Text="{Binding Value.Location}"
                           FontAttributes="Italic"
                           VerticalOptions="End" />
                </Grid>
            </DataTemplate>
        </CollectionView.ItemTemplate>
    </CollectionView>
</ContentPage    
```

`GenericsDemo.Models` 이 예제에서 `models` 는 xaml 네임 스페이스로 정의 되며 `System.Collections.Generic` 는 `scg` xaml 네임 스페이스로 정의 됩니다. `CollectionView.ItemsSource` 속성은 내부 제약 조건 형식 `List<T>` 인수 `string` 및 `Monkey`를 사용 하 `KeyValuePair<TKey, TValue>` 여 제약 조건을 사용 하 여 인스턴스화된로 설정 됩니다. 컬렉션 `List<KeyValuePair<string,Monkey>>` 은 기본 `KeyValuePair` `KeyValuePair` 이 아닌 생성자를 사용 하 여 여러 항목으로 초기화 되 고 각 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) `Monkey` 개체의 모양을 정의 하는는의로 `ItemTemplate` 설정 됩니다 [`CollectionView`](xref:Xamarin.Forms.CollectionView). 기본이 아닌 생성자에 인수를 전달 하는 방법에 대 한 자세한 내용은 [생성자 인수 전달](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments)을 참조 하세요.

## <a name="related-links"></a>관련 링크

- [XAML의 제네릭 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-generics/)
- [XAML 2009 언어 기본 형식](/dotnet/desktop-wpf/xaml-services/types-for-primitives#xaml-2009-language-primitives)
- [x:Type 태그 확장](~/xamarin-forms/xaml/markup-extensions/consuming.md#type)
- [생성자 인수 전달](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments)
