---
title: Xamarin.Forms 바인딩 경로
description: 이 문서에서는 Xamarin.Forms 데이터 바인딩을 사용하여 바인딩 클래스의 경로 속성에서 하위 속성 및 컬렉션 멤버에 액세스하는 방법에 대해 설명합니다.
ms.prod: xamarin
ms.assetid: 3CF721A5-E157-468B-AD3A-DA0A45E58E8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: ca27b0ba0f9e434809250a78047f3bd503f80b50
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70771653"
---
# <a name="xamarinforms-binding-path"></a>Xamarin.Forms 바인딩 경로

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)

이전의 모든 데이터 바인딩 예제에서 `Binding` 클래스의 [`Path`](xref:Xamarin.Forms.Binding.Path) 속성(또는 `Binding` 태그 확장의 [`Path`](xref:Xamarin.Forms.Xaml.BindingExtension.Path) 속성)을 단일 속성으로 설정했습니다. 실제로 `Path`를 *하위 속성*(속성의 속성) 또는 컬렉션의 멤버로 설정할 수 있습니다.

예를 들어 페이지에 `TimePicker`가 포함되었다고 가정합니다.

```xaml
<TimePicker x:Name="timePicker">
```

`TimePicker`의 `Time` 속성은 `TimeSpan`형식이지만 `TimeSpan` 값의 `TotalSeconds` 속성을 참조하는 데이터 바인딩을 만드는 것이 좋습니다. 데이터 바인딩은 다음과 같습니다.

```xaml
{Binding Source={x:Reference timePicker},
         Path=Time.TotalSeconds}
```

`Time` 속성은 `TimeSpan` 형식이며 여기에는 `TotalSeconds` 속성이 포함됩니다. `Time` 및 `TotalSeconds` 속성은 마침표를 사용하여 간단히 연결됩니다. `Path` 문자열의 항목은 항상 이러한 속성의 형식이 아닌 속성을 가리킵니다.

예제 및 여러 기타 항목은 **경로 변형** 페이지에 표시됩니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:globe="clr-namespace:System.Globalization;assembly=mscorlib"
             x:Class="DataBindingDemos.PathVariationsPage"
             Title="Path Variations"
             x:Name="page">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="HorizontalTextAlignment" Value="Center" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10, 0">
        <TimePicker x:Name="timePicker" />

        <Label Text="{Binding Source={x:Reference timePicker},
                              Path=Time.TotalSeconds,
                              StringFormat='{0} total seconds'}" />

        <Label Text="{Binding Source={x:Reference page},
                              Path=Content.Children.Count,
                              StringFormat='There are {0} children in this StackLayout'}" />

        <Label Text="{Binding Source={x:Static globe:CultureInfo.CurrentCulture},
                              Path=DateTimeFormat.DayNames[3],
                              StringFormat='The middle day of the week is {0}'}" />

        <Label>
            <Label.Text>
                <Binding Path="DateTimeFormat.DayNames[3]"
                         StringFormat="The middle day of the week in France is {0}">
                    <Binding.Source>
                        <globe:CultureInfo>
                            <x:Arguments>
                                <x:String>fr-FR</x:String>
                            </x:Arguments>
                        </globe:CultureInfo>
                    </Binding.Source>
                </Binding>
            </Label.Text>
        </Label>

        <Label Text="{Binding Source={x:Reference page},
                              Path=Content.Children[1].Text.Length,
                              StringFormat='The second Label has {0} characters'}" />
    </StackLayout>
</ContentPage>
```

두 번째 `Label`에서 바인딩 소스는 페이지 자체입니다. `Content` 속성은 `StackLayout` 형식이고, 여기에는 `IList<View>` 형식의 `Children` 속성이 포함되고, 여기에는 자식의 수를 나타내는 `Count` 속성이 포함됩니다.

## <a name="paths-with-indexers"></a>인덱서를 포함한 경로

**경로 변형** 페이지에 있는 세 번째 `Label`의 바인딩은 `System.Globalization` 네임스페이스에서 [`CultureInfo`](xref:System.Globalization.CultureInfo) 클래스를 참조합니다.

```xaml
<Label Text="{Binding Source={x:Static globe:CultureInfo.CurrentCulture},
                      Path=DateTimeFormat.DayNames[3],
                      StringFormat='The middle day of the week is {0}'}" />
```

원본은 `CultureInfo` 형식의 개체인 정적 `CultureInfo.CurrentCulture` 속성으로 설정됩니다. 클래스는 `DayNames` 컬렉션을 포함하는 [`DateTimeFormatInfo`](xref:System.Globalization.DateTimeFormatInfo) 형식의 `DateTimeFormat`이라는 속성을 정의합니다. 인덱스는 네 번째 항목을 선택합니다.

네 번째 `Label`은 이와 비슷하지만 프랑스와 관련된 문화권의 경우 사용됩니다. 바인딩의 `Source` 속성은 생성자를 포함한 `CultureInfo` 개체로 설정됩니다.

```xaml
<Label>
    <Label.Text>
        <Binding Path="DateTimeFormat.DayNames[3]"
                 StringFormat="The middle day of the week in France is {0}">
            <Binding.Source>
                <globe:CultureInfo>
                    <x:Arguments>
                        <x:String>fr-FR</x:String>
                    </x:Arguments>
                </globe:CultureInfo>
            </Binding.Source>
        </Binding>
    </Label.Text>
</Label>
```

XAML에서 생성자 인수를 지정하는 방법에 대한 자세한 내용은 [생성자 인수 전달](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments)을 참조하세요.

마지막으로, 마지막 예제는 `StackLayout` 자식 중 하나를 참조하는 점을 제외하고 두 번째 예제와 비슷합니다.

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content.Children[1].Text.Length,
                      StringFormat='The first Label has {0} characters'}" />
```

해당 자식은 `Label`이며 여기에는 `String` 형식의 `Text` 속성이 포함되고 여기에는 `Length` 속성이 포함됩니다. 첫 번째 `Label`은 `TimePicker`에 설정된 `TimeSpan`을 보고하므로 해당 텍스트가 변경되면 최종 `Label`도 변경됩니다.

실행 중인 프로그램은 다음과 같습니다.

[![경로 변형](binding-path-images/pathvariations-small.png "경로 변형")](binding-path-images/pathvariations-large.png#lightbox "경로 변형")

## <a name="debugging-complex-paths"></a>복잡한 경로 디버그

복잡한 경로 정의는 생성하기 어려울 수 있습니다. 다음 하위 속성을 제대로 추가하기 위해 각 하위 속성의 형식이나 컬렉션의 항목 형식을 알아야 하지만 형식 자체는 경로에 표시되지 않습니다. 증분적으로 경로를 빌드하고 중간 결과를 확인하는 것이 좋습니다. 마지막 예제에서는 `Path` 정의 없이 시작할 수 있습니다.

```xaml
<Label Text="{Binding Source={x:Reference page},
                      StringFormat='{0}'}" />
```

바인딩 소스의 형식 또는 `DataBindingDemos.PathVariationsPage`를 표시합니다. `PathVariationsPage`는 `ContentPage`에서 파생되므로 `Content` 속성이 포함됩니다.

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content,
                      StringFormat='{0}'}" />
```

`Content` 속성의 형식은 `Xamarin.Forms.StackLayout`이라고 표시됩니다. `Children` 속성을 `Path`에 추가하면 형식은 `Xamarin.Forms.ElementCollection'1[Xamarin.Forms.View]`입니다. 이것은 Xamarin.Forms에 대한 내부 클래스이지만 분명히 컬렉션 형식입니다. 여기에 인덱스를 추가하면 형식은 `Xamarin.Forms.Label`입니다. 이러한 방식으로 계속합니다.

Xamarin.Forms에서 바인딩 경로를 처리하면 경로의 개체에 `PropertyChanged` 처리기를 설치하여 `INotifyPropertyChanged` 인터페이스를 구현합니다. 예를 들어 `Text` 속성이 변경되기 때문에 마지막 바인딩은 첫 번째 `Label`의 변경 내용을 적용합니다.

바인딩 경로의 속성이 `INotifyPropertyChanged`를 구현하지 않는 경우 해당 속성에 대한 변경 내용이 무시됩니다. 일부 변경 내용이 바인딩 경로를 무효화할 수 있으므로 속성 및 하위 속성의 문자열이 잘못되지 않은 경우에만 이 기술을 사용해야 합니다.

## <a name="related-links"></a>관련 링크

- [데이터 바인딩 데모(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)
- [Xamarin.Forms 서적의 데이터 바인딩 장](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
