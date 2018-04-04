---
title: 바인딩 경로
description: 하위 속성에 액세스 및 컬렉션 멤버에 대 한 데이터 바인딩을 사용 하 여
ms.prod: xamarin
ms.assetid: 3CF721A5-E157-468B-AD3A-DA0A45E58E8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: f75cfcf4bfd5ffa71699f62b30145b732421d964
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="binding-path"></a>바인딩 경로

모든 이전 데이터 바인딩 예제에는 [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) 속성의는 `Binding` 클래스 (또는 [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.Path/) 의 속성은 `Binding` 태그 확장) 설정 된 에 단일 속성입니다. 실제로 설정 수는 `Path` 에 *하위 속성* (속성이 속성) 또는 컬렉션의 멤버입니다.

예를 들어, 페이지에는 `TimePicker`:

```xaml
<TimePicker x:Name="timePicker">
```

`Time` 속성 `TimePicker` 유형의 `TimeSpan`, 하지만 아마도 참조 하는 데이터 바인딩 만들기는 `TotalSeconds` 속성의 `TimeSpan` 값입니다. 데이터 바인딩은 다음과 같습니다.

```xaml
{Binding Source={x:Reference timePicker},
         Path=Time.TotalSeconds}
```
         
`Time` 형식의 속성은 `TimeSpan`을는 `TotalSeconds` 속성입니다. `Time` 및 `TotalSeconds` 속성은 마침표로 simply 연결 합니다. 에 있는 항목의 `Path` 이러한 속성의 형식은 아니라 속성에는 항상 문자열 참조 합니다.

예제 및 여러 다른 에서처럼는 **경로 변형** 페이지:

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
                              StringFormat='The first Label has {0} characters'}" />
    </StackLayout>
</ContentPage>
```

두 번째에서 `Label`, 바인딩 소스 자체 페이지입니다. `Content` 형식의 속성은 `StackLayout`가 `Children` 형식의 속성이 `IList<View>`가 `Count` 자식의 수를 나타내는 속성입니다.

## <a name="paths-with-indexers"></a>경로 인덱서를

세 번째에서 바인딩 `Label` 에 **경로 변형** 참조 페이지의 [ `CultureInfo` ](https://developer.xamarin.com/api/type/System.Globalization.CultureInfo/) 클래스에 `System.Globalization` 네임 스페이스:

```xaml
<Label Text="{Binding Source={x:Static globe:CultureInfo.CurrentCulture},
                      Path=DateTimeFormat.DayNames[3],
                      StringFormat='The middle day of the week is {0}'}" />
```

정적으로 설정 되어 `CultureInfo.CurrentCulture` 형식의 개체에 해당 하는 속성이 `CultureInfo`합니다. 클래스 속성을 정의 `DateTimeFormat` 형식의 [ `DateTimeFormatInfo` ](https://developer.xamarin.com/api/type/System.Globalization.DateTimeFormatInfo/) 를 포함 하는 `DayNames` 컬렉션입니다. 네 번째 항목을 선택 하는 인덱스입니다.

네 번째 `Label` 비슷한 프랑스와 관련 된 문화권에 있지만 특정 작업을 수행 합니다. `Source` 바인딩의 속성이로 설정 되어 `CultureInfo` 생성자가 있는 개체:

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

참조 [생성자 인수를 전달](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments) 에 대 한 자세한 내용은 XAML에서 생성자 인수를 지정 합니다.

마지막으로, 마지막 예제에서 두 번째 한다는 점을 제외 비슷합니다의 자식 중 하나를 참조 하는 `StackLayout`:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content.Children[1].Text.Length,
                      StringFormat='The first Label has {0} characters'}" />
```

해당 자식이 `Label`가 `Text` 형식의 속성이 `String`가 `Length` 속성입니다. 첫 번째 `Label` 보고서는 `TimeSpan` 에 설정 된 `TimePicker`하므로 해당 텍스트가 변경 될 때 최종 `Label` 도 변경 합니다.

다음은 세 플랫폼 모두에서 실행 중인 프로그램입니다.

[![경로 변형](binding-path-images/pathvariations-small.png "경로 변형")](binding-path-images/pathvariations-large.png#lightbox "경로 변형")

## <a name="debugging-complex-paths"></a>복잡 한 경로 디버깅합니다.

복합 경로 정의 생성 하기 어려운 수 있습니다: 각 하위 속성의 형식이 나 올바르게 다음 하위 속성을 추가 하는 컬렉션에 있는 항목의 형식을 알아야 하지만 형식 자체의 경로에 표시 되지 않습니다. 좋은 방법 중 하나는 증분 빌드 경로 구성 하 고 중간 결과 확인 하는 것입니다. 마지막 그 예에서 없이 시작할 수 있습니다 `Path` 전혀 정의:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      StringFormat='{0}'}" />
```

바인딩 소스의 형식을 표시 하는 또는 `DataBindingDemos.PathVariationsPage`합니다. 알고 있는 `PathVariationsPage` 에서 파생 `ContentPage`아니므로 `Content` 속성:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content,
                      StringFormat='{0}'}" />
```

종류는 `Content` 속성은 이제 번도 공개 될 `Xamarin.Forms.StackLayout`합니다. 추가 `Children` 속성을는 `Path` 이며 형식만 `Xamarin.Forms.ElementCollection'1[Xamarin.Forms.View]`, 내부 Xamarin.Forms에 있지만 분명히 컬렉션 형식에 클래스인 합니다. 에 인덱스를 추가 하 고 형식이 `Xamarin.Forms.Label`합니다. 이러한 방식으로 계속 합니다.

설치 바인딩 경로 처리 하는 Xamarin.Forms는 `PropertyChanged` 구현 하는 경로에 있는 개체에 대 한 처리기는 `INotifyPropertyChanged` 인터페이스입니다. 마지막 바인딩 첫 번째 범위에서 변경에 반응 하는 예를 들어 `Label` 때문에 `Text` 속성 변경 합니다. 

바인딩 경로에 속성 구현 하지 않는 경우 `INotifyPropertyChanged`, 해당 속성에 변경 내용을 무시 됩니다. 일부 변경 내용은 바인딩 경로 인해 무효화 완전히 있으므로 속성 및 하위 속성의 문자열 되지 유효 하지 않게 하는 경우에이 방법을 사용 해야 합니다.



## <a name="related-links"></a>관련 링크

- [데이터 바인딩 데모 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms 책에서 데이터 바인딩 장](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
