---
title: Xamarin.Forms 바인딩 경로
description: 이 문서에서는 데이터 바인딩을 Xamarin.Forms를 사용 하 여 하위 속성 및 바인딩 클래스의 Path 속성을 사용 하 여 컬렉션 멤버를 액세스 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 3CF721A5-E157-468B-AD3A-DA0A45E58E8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 5ffc167b1e5695663dff6005f3d7e0ba0ea958db
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52172108"
---
# <a name="xamarinforms-binding-path"></a>Xamarin.Forms 바인딩 경로

모든 이전 데이터 바인딩 예제에서를 [ `Path` ](xref:Xamarin.Forms.Binding.Path) 의 속성을 `Binding` 클래스 (또는 [ `Path` ](xref:Xamarin.Forms.Xaml.BindingExtension.Path) 속성은 `Binding` 태그 확장) 설정한 단일 속성입니다. 실제로 설정 수 있기 `Path` 에 *하위 속성* (속성을 속성) 또는 컬렉션의 멤버입니다.

예를 들어, 페이지에는 `TimePicker`:

```xaml
<TimePicker x:Name="timePicker">
```

`Time` 의 속성 `TimePicker` 형식입니다 `TimeSpan`를 참조 하는 데이터 바인딩 만들기 하려는 아마도 `TotalSeconds` 속성의 `TimeSpan` 값. 데이터 바인딩에 다음과 같습니다.

```xaml
{Binding Source={x:Reference timePicker},
         Path=Time.TotalSeconds}
```

`Time` 형식의 속성은 `TimeSpan`에 있는 `TotalSeconds` 속성. 합니다 `Time` 고 `TotalSeconds` 속성은 연결 됩니다 마침표를 사용 하 여 합니다. 항목을 `Path` 문자열에는 항상 이러한 속성의 형식은 아닌 속성에 참조 합니다.

예제 및 여러 다른 에서처럼 합니다 **경로 변형** 페이지:

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

두 번째에서 `Label`, 바인딩 소스 자체 페이지입니다. 합니다 `Content` 형식의 속성은 `StackLayout`, 있는 `Children` 형식의 속성 `IList<View>`, 있는 `Count` 자식의 수를 나타내는 속성입니다.

## <a name="paths-with-indexers"></a>인덱서를 사용 하 여 경로

세 번째에서 바인딩을 `Label` 에 **경로 변형** 참조 페이지를 [ `CultureInfo` ](xref:System.Globalization.CultureInfo) 클래스를 `System.Globalization` 네임 스페이스:

```xaml
<Label Text="{Binding Source={x:Static globe:CultureInfo.CurrentCulture},
                      Path=DateTimeFormat.DayNames[3],
                      StringFormat='The middle day of the week is {0}'}" />
```

소스는 정적으로 설정 되어 `CultureInfo.CurrentCulture` 형식의 개체 속성 `CultureInfo`합니다. 클래스 라는 속성을 정의 하 `DateTimeFormat` 형식의 [ `DateTimeFormatInfo` ](xref:System.Globalization.DateTimeFormatInfo) 포함 하는 `DayNames` 컬렉션입니다. 네 번째 항목을 선택 하는 인덱스입니다.

네 번째 `Label` 이와 비슷하게 프랑스 연관 되지만 문화권입니다. 합니다 `Source` 바인딩의 속성 `CultureInfo` 생성자를 사용 하 여 개체:

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

참조 [생성자 인수 전달](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments) 자세한 XAML에서 생성자 인수를 지정 합니다.

마지막으로, 마지막 예제는 자식 중 하나를 참조 하는 점을 제외 하 고 두 번째 비슷합니다는 `StackLayout`:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content.Children[1].Text.Length,
                      StringFormat='The first Label has {0} characters'}" />
```

해당 자식이 `Label`, 있는 `Text` 형식의 속성 `String`에 있는 `Length` 속성. 첫 번째 `Label` 보고서를 `TimeSpan` 에 설정 합니다 `TimePicker`이므로 해당 텍스트가 변경 되 면 최종 `Label` 도 변경 합니다.

실행 중인 프로그램이 다음과 같습니다.

[![경로 변형을](binding-path-images/pathvariations-small.png "경로 변형")](binding-path-images/pathvariations-large.png#lightbox "경로 변형")

## <a name="debugging-complex-paths"></a>복잡 한 경로 디버깅

복잡 한 경로 정의 생성 하기 어려울 수 있습니다: 각 하위 속성의 형식이 나 다음 하위 속성을 제대로 추가할 컬렉션의 항목 형식을 알아야 하지만 형식 자체는 경로에 표시 되지 않습니다. 좋은 방법 중 하나는 증분 빌드 경로 구성 하 고 중간 결과 확인 하는 것입니다. 예: 해당 마지막 없이 시작할 수 있습니다 `Path` 전혀 정의:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      StringFormat='{0}'}" />
```

바인딩 소스의 형식을 표시 하는 또는 `DataBindingDemos.PathVariationsPage`합니다. 알고 `PathVariationsPage` 에서 파생 되 `ContentPage`것이 아니므로 `Content` 속성:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content,
                      StringFormat='{0}'}" />
```

형식의 합니다 `Content` 되도록 이제 작아 속성 `Xamarin.Forms.StackLayout`합니다. 추가 합니다 `Children` 속성을를 `Path` 형식이 고 `Xamarin.Forms.ElementCollection'1[Xamarin.Forms.View]`, Xamarin.Forms 있지만 확실히 컬렉션 형식의 내부 클래스인. 인덱스를 추가 하 고 형식이 `Xamarin.Forms.Label`합니다. 이러한 방식으로 계속 합니다.

Xamarin.Forms 바인딩 경로 처리 하는 대로 설치를 `PropertyChanged` 처리기를 구현 하는 경로에 모든 개체에는 `INotifyPropertyChanged` 인터페이스입니다. 마지막 바인딩 첫 번째 변경에 반응 하는 예를 들어 `Label` 때문에 `Text` 속성 변경 합니다.

바인딩 경로에서 속성을 구현 하지 않는 경우 `INotifyPropertyChanged`, 해당 속성에 변경 내용을 무시 됩니다. 완전히 일부 변경 내용이 있으므로 속성 및 하위 속성의 문자열 되지 무효화 하는 경우에이 기술을 사용 해야 바인딩 경로 무효화할 수 있습니다.



## <a name="related-links"></a>관련 링크

- [데이터 바인딩 데모 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms 책에서 데이터 바인딩 장](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
