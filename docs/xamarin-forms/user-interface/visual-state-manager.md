---
title: Xamarin.Forms Visual State Manager
description: Visual State Manager를 사용 하 여 코드에서 설정 하는 시각적 상태를 기반으로 하는 XAML 요소를 변경 해야 합니다.
ms.prod: xamarin
ms.assetid: 17296F14-640D-484B-A24C-A4E9B7013E4F
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: c2944aba05c2f0ea87f675b8830f3a1ee20b5ac1
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563877"
---
# <a name="the-xamarinforms-visual-state-manager"></a>Xamarin.Forms Visual State Manager

_Visual State Manager를 사용 하 여 코드에서 설정 하는 시각적 상태를 기반으로 하는 XAML 요소를 변경 해야 합니다._

시각적 상태 관리자 (VSM) Xamarin.Forms 3.0의 새로운 기능입니다. VSM 코드에서 시각적 항목이 변경 사용자 인터페이스에 확인 하는 구조적된 방법을 제공 합니다. 대부분의 경우에서 응용 프로그램의 사용자 인터페이스, XAML에 정의 되어이 XAML Visual State Manager 사용자 인터페이스의 시각적 개체에 미치는 영향을 설명 하는 태그를 포함 합니다.

VSM 라는 개념이 도입 되었습니다 _시각적 상태_합니다. 같은 Xamarin.Forms 뷰는 `Button` 기본 상태에 따라 여러 다른 시각적 모양이 있습니다 &mdash; 사용 하지 않으면 또는 눌렀을 때 또는 입력 포커스가 있는지 여부입니다. 이들은 단추의 상태입니다.

시각적 상태에 수집 됩니다 _시각적 상태 그룹_합니다. 시각적 상태 그룹 내에서 모든 시각적 상태는 함께 사용할 수 없습니다. 시각적 상태 및 시각적 상태 그룹은 간단한 텍스트 문자열에서 식별 됩니다.

Xamarin.Forms Visual State Manager는 세 가지 시각적 상태를 사용 하 여 "CommonStates" 라는 하나의 시각적 상태 그룹을 정의 합니다.

- "Normal"
- "사용 안 함"
- "포커스 있음"

이 시각적 상태 그룹에서 파생 되는 모든 클래스에 대해서는 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)에 대 한 기본 클래스인 [ `View` ](xref:Xamarin.Forms.View) 고 [ `Page` ](xref:Xamarin.Forms.Page)합니다. 

시각적 상태 그룹을 직접 정의할 수도 있습니다 및 시각적 상태를이 문서를 보여 줍니다.

> [!NOTE]
> 친숙 한 Xamarin.Forms 개발자 [트리거](~/xamarin-forms/app-fundamentals/triggers.md) 는 트리거 수를 변경할 수도 변경 내용 보기의 속성 또는 이벤트의 발생에 따라 사용자 인터페이스의 시각적 개체 인식 됩니다. 그러나 이러한 변경의 다양 한 조합을 사용 하 여 처리 하기 위해 트리거를 사용 하 여 매우 혼란 스 러 될 수 있습니다. 지금까지 Visual State Manager는 시각적 상태 조합에서 발생 하는 혼란을 완화 하기 위해 Windows XAML 기반 환경에서 도입 되었습니다. VSM을 통해 시각적 상태 그룹 내의 시각적 상태는 항상 함께 사용할 수 없습니다. 언제 든 지 하나의 상태 각 그룹의 현재 상태가입니다.

## <a name="the-common-states"></a>일반적인 상태

Visual State Manager를 사용 하면 뷰는 보통, 또는 사용 안 함, 또는 입력된 포커스를가지고 하는 경우 보기의 시각적 모양을 변경할 수 있는 XAML 파일의 섹션을 포함할 수 있습니다. 이러한 라고 합니다 _일반적인 상태_합니다.

예를 들어 있다고 가정를 `Entry` 페이지에서 보기의 시각적 모양을 하려는 `Entry` 다음과 같이 변경 하려면:

- 합니다 `Entry` 는 pink 있어야 때 백그라운드는 `Entry` 을 사용할 수 없습니다.
- `Entry` 라임 배경이 일반적으로 있어야 합니다.
- `Entry` 것에 입력 포커스가 있는 경우에 일반 높이 두 배가 확장 해야 합니다.

VSM 태그 개별 보기에 연결 하거나 여러 보기에 적용 되는 경우 스타일에서 정의할 수 있습니다. 다음 두 섹션에는 이러한 접근 방식을 설명합니다.

### <a name="vsm-markup-on-a-view"></a>뷰에 VSM 태그

VSM 태그를 연결 하는 `Entry` 보기에서 먼저 분리는 `Entry` 시작 및 끝 태그에:

```xaml
<Entry FontSize="18">

</Entry>
```

상태 중 하 나와 사용 되므로 명시적 글꼴 크기를 제공 합니다 `FontSize` 속성에 있는 텍스트의 크기를 두 배로 `Entry`합니다.

그런 다음 삽입 `VisualStateManager.VisualStateGroups` 해당 태그 사이의 태그:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>

    </VisualStateManager.VisualStateGroups>
</Entry>
```

[`VisualStateGroups`](xref:Xamarin.Forms.VisualStateManager.VisualStateGroupsProperty) 정의한 연결 된 바인딩 가능한 속성을 [ `VisualStateManager` ](xref:Xamarin.Forms.VisualStateManager) 클래스입니다. (연결 된 바인딩 가능한 속성에 대 한 자세한 내용은 문서 참조 [연결 된 속성](~/xamarin-forms/xaml/attached-properties.md).) 이 방법을 `VisualStateGroups` 속성에 연결 된는 `Entry` 개체입니다.

합니다 `VisualStateGroups` 형식의 속성은 [ `VisualStateGroupList` ](xref:Xamarin.Forms.VisualStateGroupList)의 컬렉션인 [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) 개체입니다. 내 합니다 `VisualStateManager.VisualStateGroups` 태그를 삽입 한 쌍의 `VisualStateGroup` 시각적 상태를 포함 하려는 각 그룹에 대 한 태그:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

에 `VisualStateGroup` 태그에는 `x:Name` 그룹의 이름을 나타내는 특성입니다. 합니다 `VisualStateGroup` 클래스 정의 `Name` 대신 사용할 수 있는 속성:

```xaml
<VisualStateGroup Name="CommonStates">
```

사용할 수 있습니다 `x:Name` 또는 `Name` 있지만 동일한 요소에서 둘 다 없습니다.

합니다 `VisualStateGroup` 라는 속성을 정의 하는 클래스 [ `States` ](xref:Xamarin.Forms.VisualStateGroup.States)의 컬렉션인 [ `VisualState` ](xref:Xamarin.Forms.VisualState) 개체입니다. `States` _콘텐츠 속성_ 의 `VisualStateGroups` 를 포함할 수 있도록 합니다 `VisualState` 사이 직접 태그를 `VisualStateGroup` 태그. (문서의 속성에 설명 콘텐츠 [필수 XAML 구문](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md#content-properties).)

다음 단계 그룹의 모든 시각적 상태에 대 한 태그 쌍을 포함 하는 것입니다. 또한 식별할 수 있습니다 사용 하 여 `x:Name` 또는 `Name`:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">

            </VisualState>

            <VisualState x:Name="Focused">

            </VisualState>

            <VisualState x:Name="Disabled">

            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

`VisualState` 라는 속성을 정의 [ `Setters` ](xref:Xamarin.Forms.VisualState.Setters)의 컬렉션인 [ `Setter` ](xref:Xamarin.Forms.Setter) 개체입니다. 동일 `Setter` 에서 사용 하는 개체를 [ `Style` ](xref:Xamarin.Forms.Style) 개체입니다.

`Setters` 됩니다 _되지_ 의 content 속성 `VisualState`에 대 한 속성 요소 태그를 포함 하는 데 필요한 이므로는 `Setters` 속성:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>

                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Focused">
                <VisualState.Setters>
    
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Disabled">
                <VisualState.Setters>

                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

이제 하나 이상의 삽입할 수 있습니다 `Setter` 개체의 각 쌍 사이 `Setters` 태그입니다. 이들은 `Setter` 앞에서 설명한 시각적 상태를 정의 하는 개체:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Lime" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Focused">
                <VisualState.Setters>
                    <Setter Property="FontSize" Value="36" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Disabled">
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Pink" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

각 `Setter` 해당 상태가 현재 태그는 특정 속성의 값을 나타냅니다. 참조 하는 모든 속성을 `Setter` 개체 바인딩 가능한 속성으로 백업 해야 합니다.

다음과 유사 하 게 하는 태그의 기반이 되는 **보기에서 VSM** 페이지에 **[VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)** 샘플 프로그램입니다. 페이지를 포함 세 `Entry` 뷰 하는데 두 번째는 연결 된 VSM 태그:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:VsmDemos"
             x:Class="VsmDemos.MainPage"
             Title="VSM Demos">

    <StackLayout>
        <StackLayout.Resources>
            <Style TargetType="Entry">
                <Setter Property="Margin" Value="20, 0" />
                <Setter Property="FontSize" Value="18" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="Margin" Value="20, 30, 20, 0" />
                <Setter Property="FontSize" Value="Large" />
            </Style>
        </StackLayout.Resources>

        <Label Text="Normal Entry:" />

        <Entry />

        <Label Text="Entry with VSM: " />

        <Entry>
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup x:Name="CommonStates">
                    
                    <VisualState x:Name="Normal">
                        <VisualState.Setters>
                            <Setter Property="BackgroundColor" Value="Lime" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState x:Name="Focused">
                        <VisualState.Setters>
                            <Setter Property="FontSize" Value="36" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState x:Name="Disabled">
                        <VisualState.Setters>
                            <Setter Property="BackgroundColor" Value="Pink" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>

            <Entry.Triggers>
                <DataTrigger TargetType="Entry"
                             Binding="{Binding Source={x:Reference entry3},
                                               Path=Text.Length}"
                             Value="0">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </Entry.Triggers>
        </Entry>

        <Label Text="Entry to enable 2nd Entry:" />

        <Entry x:Name="entry3"
               Text=""
               Placeholder="Type something to enable 2nd Entry" />
    </StackLayout>
</ContentPage>
```

두 번째 `Entry` 역시를 `DataTrigger` 의 일부로 해당 `Trigger` 컬렉션입니다. 이 인해 합니다 `Entry` 무언가 세 번째에 입력할 때까지 사용 하지 않도록 설정할 `Entry`합니다. 다음은 iOS, Android 및 유니버설 Windows 플랫폼 (UWP)에서 실행 되는 시작 페이지가입니다.

[![보기에서 VSM: 사용 안 함](vsm-images/VsmOnViewDisabled.png "보기-사용 안 함에서 VSM")](vsm-images/VsmOnViewDisabled-Large.png#lightbox)

현재 시각적 상태를 "Disabled" 하므로 두 번째 배경의 `Entry` iOS 및 Android 화면의 분홍색 이며 합니다. UWP 구현의 `Entry` 배경을 설정 하는 것을 허용 하지 않는 경우 색를 `Entry` 을 사용할 수 없습니다. 

세 번째에 일부 텍스트를 입력할 때 `Entry`, 두 번째 `Entry` "Normal" 상태를 백그라운드 스위치 라임 되었습니다.

[![보기에서 VSM: 보통](vsm-images/VsmOnViewNormal.png "VSM 보기-일반")](vsm-images/VsmOnViewNormal-Large.png#lightbox)

두 번째를 터치 할 `Entry`, 입력된 포커스를 가져옵니다. "Focused" 상태로 전환 하 고 높이 두 배가 확장 됩니다.

[![보기에서 VSM: 초점을 맞춘](vsm-images/VsmOnViewFocused.png "뷰-초점을 맞춘 VSM")](vsm-images/VsmOnViewFocused-Large.png#lightbox)

다음에 유의 합니다 `Entry` 입력된 포커스를 가져오면 라임 백그라운드 유지 되지 않습니다. Visual State Manager 시각적 상태 간의 전환으로 이전 상태에 따라 설정 된 속성에 설정 되지 않은 합니다. 시각적 상태 상호 배타적인 것을 염두에 두십시오. "Normal" 상태는 전적으로 의미 하지 않습니다는 `Entry` 사용 가능 합니다. 즉,는 `Entry` 입력된 포커스가 없고 사용 가능 합니다. 

원하는 경우는 `Entry` 라임 배경이 "Focused" 상태에 있는 추가 다른 `Setter` 해당 시각적 상태:

```xaml
<VisualState x:Name="Focused">
    <VisualState.Setters>
        <Setter Property="FontSize" Value="36" />
        <Setter Property="BackgroundColor" Value="Lime" />
    </VisualState.Setters>
</VisualState>
```

이러한 순서로 `Setter` 제대로 작동 하려면 개체를 `VisualStateGroup` 있어야 `VisualState` 해당 그룹의 모든 상태에 대 한 개체입니다. 포함 하지 않는 하는 시각적 상태 이면 `Setter` 개체 그래도 빈 태그로 포함:

```xaml
<VisualState x:Name="Normal" />
``` 

### <a name="visual-state-manager-markup-in-a-style"></a>시각적 상태 관리자 태그를 스타일

둘 이상의 뷰 간에 동일한 Visual State Manager 태그를 공유 하는 데 필요한 경우가 있습니다. 태그에 배치 하려는 경우에 `Style` 정의 합니다.

다음은 기존 암시적 `Style` 에 대 한는 `Entry` 요소에는 **보기에서 VSM** 페이지:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
</Style> 
```

추가 `Setter` 에 대 한 태그는 `VisualStateManager.VisualStateGroups` 바인딩 가능 속성을 연결 합니다.

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">

    </Setter>
</Style> 
```

콘텐츠 속성을 `Setter` 은 `Value`이므로 값은 `Value` 이러한 태그 내에서 직접 속성을 지정할 수 있습니다. 속성 형식이 한지 `VisualStateGroupList`:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>

        </VisualStateGroupList>
    </Setter>
</Style> 
```

이러한 태그 내에서 중 하나 이상 포함할 수 있습니다 `VisualStateGroup` 개체:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>
            <VisualStateGroup x:Name="CommonStates">

            </VisualStateGroup>
        </VisualStateGroupList>
    </Setter>
</Style> 
```

VSM 태그 나머지는 이전과 동일 합니다.

다음은 **스타일에서 VSM** 전체 VSM 태그를 보여 주는 페이지:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmInStylePage"
             Title="VSM in Style">
    <StackLayout>
        <StackLayout.Resources>
            <Style TargetType="Entry">
                <Setter Property="Margin" Value="20, 0" />
                <Setter Property="FontSize" Value="18" />
                <Setter Property="VisualStateManager.VisualStateGroups">
                    <VisualStateGroupList>
                        <VisualStateGroup x:Name="CommonStates">

                            <VisualState x:Name="Normal">
                                <VisualState.Setters>
                                    <Setter Property="BackgroundColor" Value="Lime" />
                                </VisualState.Setters>
                            </VisualState>

                            <VisualState x:Name="Focused">
                                <VisualState.Setters>
                                    <Setter Property="FontSize" Value="36" />
                                    <Setter Property="BackgroundColor" Value="Lime" />
                                </VisualState.Setters>
                            </VisualState>

                            <VisualState x:Name="Disabled">
                                <VisualState.Setters>
                                    <Setter Property="BackgroundColor" Value="Pink" />
                                </VisualState.Setters>
                            </VisualState>
                        </VisualStateGroup>
                    </VisualStateGroupList>
                </Setter>
            </Style>

            <Style TargetType="Label">
                <Setter Property="Margin" Value="20, 30, 20, 0" />
                <Setter Property="FontSize" Value="Large" />
            </Style>
        </StackLayout.Resources>

        <Label Text="Normal Entry:" />

        <Entry />
        
        <Label Text="Entry with VSM: " />

        <Entry>
            <Entry.Triggers>
                <DataTrigger TargetType="Entry"
                             Binding="{Binding Source={x:Reference entry3},
                                               Path=Text.Length}"
                             Value="0">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </Entry.Triggers>
        </Entry>

        <Label Text="Entry to enable 2nd Entry:" />

        <Entry x:Name="entry3"
               Text=""
               Placeholder="Type something to enable 2nd Entry" />
    </StackLayout>
</ContentPage>
```

이제 모든는 `Entry` 페이지 보기에는 해당 시각적 상태를 동일 하 게 반응 합니다. "Focused" 상태에 이제 두 번째는 또한 `Setter` 제공 하는 각 `Entry` 를 라임 경우 그에 입력 포커스가 백그라운드도:

[![스타일에서 VSM](vsm-images/VsmInStyle.png "VSM 스타일")](vsm-images/VsmInStyle-Large.png#lightbox)

## <a name="defining-your-own-visual-states"></a>사용자 고유의 시각적 상태 정의

파생 되는 모든 클래스 `VisualElement` 세 가지 일반적인 상태 "Normal", "포커스 있음" 및 "사용 안 함"을 지원 합니다. 내부적으로 [ `VisualElement` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/VisualElement.cs) 사용 또는 사용 안 함, 또는 포커스가 있거나 포커스가, 되 고 정적 호출 하는 경우를 검색 하는 클래스 [ `VisualStateManager.GoToState` ](xref:Xamarin.Forms.VisualStateManager.GoToState(Xamarin.Forms.VisualElement,System.String)) 메서드:

```csharp
VisualStateManager.GoToState(this, "Focused");
```

찾을 수 있는 코드만 Visual State Manager는이 `VisualElement` 클래스입니다. 때문에 `GoToState` 에서 파생 되는 모든 클래스를 기반으로 하는 모든 개체에 대 한 라고 `VisualElement`, Visual State Manager를 사용 하 여 사용 하 여 `VisualElement` 이러한 변경에 대응 하는 개체입니다.

흥미롭게도 "CommonStates" 시각적 상태 그룹의 이름을 명시적으로에서 참조 되지 않은 `VisualElement`합니다. 그룹 이름을 Visual State Manager에 대 한 API의 일부가 아닙니다. 앞에서 설명한 두 가지 샘플 프로그램 중 하나를 다른 값으로 "CommonStates" 그룹의 이름을 변경할 수 있습니다 및 프로그램이 계속 작동 합니다. 그룹 이름은 해당 그룹의 상태에 대해 개괄적으로 설명 하기만 합니다. 시각적 상태 그룹에는 상호 배타적인은 암시적으로 인식 됩니다: 상태 및 하나의 상태는 언제 든 지 현재 합니다.

호출 해야 사용자 고유의 시각적 상태를 구현 하려는 경우 `VisualStateManager.GoToState` 코드에서. 대부분의 page 클래스의 코드 숨김 파일에서이 호출을 해야 합니다.

합니다 **VSM 유효성 검사** 페이지에 **[VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)** 샘플에서는 입력된 유효성 검사와 관련 하 여 Visual State Manager를 사용 하는 방법을 보여 줍니다. XAML 파일을 두 이루어져 `Label` 요소는 `Entry`, 및 `Button`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmValidationPage"
             Title="VSM Validation">
    <StackLayout Padding="10, 10">
        
        <Label Text="Enter a U.S. phone number:"
               FontSize="Large" />

        <Entry Placeholder="555-555-5555"
               FontSize="Large"
               Margin="30, 0, 0, 0"
               TextChanged="OnTextChanged" />

        <Label x:Name="helpLabel"
               Text="Phone number must be of the form 555-555-5555, and not begin with a 0 or 1">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ValidityStates">
                    
                    <VisualState Name="Valid">
                        <VisualState.Setters>
                            <Setter Property="TextColor" Value="Transparent" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState Name="Invalid" />
                    
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </Label>

        <Button x:Name="submitButton"
                Text="Submit"
                FontSize="Large"
                Margin="0, 20"
                VerticalOptions="Center"
                HorizontalOptions="Center">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ValidityStates">
                    
                    <VisualState Name="Valid" />

                    <VisualState Name="Invalid">
                        <VisualState.Setters>
                            <Setter Property="IsEnabled" Value="False" />
                        </VisualState.Setters>
                    </VisualState>
                    
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </Button>
    </StackLayout>
</ContentPage>
```

두 번째 VSM 태그 연결 된 `Label` (라는 `helpLabel`) 및 `Button` (라는 `submitButton`). "Valid" 및 "잘못 됨" 이라는 두 가지 상호 배타적인 상태로 있습니다. 예를 들어 두 개의 "ValidationState" 그룹의 각 표시 `VisualState` 있지만 그 중 하나는 각각의 경우에서 빈 "Valid" 및 "잘못 됨"에 대 한 태그입니다. 

경우는 `Entry` 현재 상태는 "잘못 된" 이므로 올바른 전화 번호를 포함 하지 않습니다 두 번째 `Label` 표시 됩니다 및 `Button` 을 사용할 수 없습니다.

[![VSM 유효성 검사: 잘못 된 상태](vsm-images/VsmValidationInvalid.png "VSM 유효성 검사-잘못 되었습니다.")](vsm-images/VsmValidationInvalid-Large.png#lightbox)

올바른 전화 번호를 입력 한 다음 현재 상태가 "Valid"입니다. 두 번째 `Entry` 사라집니다 및 `Button` 활성화 되었습니다.

[![VSM 유효성 검사: 유효한 상태](vsm-images/VsmValidationValid.png "VSM 유효성 검사-유효한")](vsm-images/VsmValidationValid-Large.png#lightbox)

코드 숨김 파일은 처리 작업을 담당 합니다 `TextChanged` 에서 이벤트를 `Entry`입니다. 입력된 문자열은 올바른 경우 확인 하려면 정규식을 사용 하는 처리기. 라는 코드 숨김 파일의 메서드에 `GoToState` 정적 호출 `VisualStateManager.GoToState` 둘 다에 대 한 메서드 `helpLabel` 및 `submitButton`:

```csharp
public partial class VsmValidationPage : ContentPage
{
    public VsmValidationPage ()
    {
        InitializeComponent ();

        GoToState(false);
    }

    void OnTextChanged(object sender, TextChangedEventArgs args)
    {
        bool isValid = Regex.IsMatch(args.NewTextValue, @"^[2-9]\d{2}-\d{3}-\d{4}$");
        GoToState(isValid);
    }

    void GoToState(bool isValid)
    {
        string visualState = isValid ? "Valid" : "Invalid";
        VisualStateManager.GoToState(helpLabel, visualState);
        VisualStateManager.GoToState(submitButton, visualState);
    }
}
```

또한는 `GoToState` 상태를 초기화 하기 위해 생성자에서 호출 됩니다. 현재 상태는 항상 있을 것입니다. 이지만 곳은 코드에 있는 시각적 상태 그룹의 이름에 대 한 모든 참조 "ValidationStates" 위해 명확성으로는 XAML에서 참조 되지만 있습니다. 

코드 숨김 파일을 고려해 야 모든 개체의 영향을 받는 페이지의 이러한 시각적 상태에서 호출을 확인할 수 있습니다. `VisualStateManager.GoToState` 이러한 각 개체에 대 한 합니다. 이 예에서는 두 개의 개체 (합니다 `Label` 및 `Button`), 여러 수 있지만 자세한 합니다.

궁금할 수 있습니다: 코드 숨김 파일을 이러한 시각적 상태에 영향을 받는 페이지의 모든 개체를 참조 해야 하는 경우 이유 코드 숨김 파일을 간단히 직접 액세스할 수 없는 개체? 이 분명 할 수 없습니다. 그러나 VSM을 사용 하 여의 장점은 visual 요소를 제어할 수 있습니다 완전히 XAML 한곳에 유지 하는 모든 UI 디자인에서에서 다른 상태로 반응 합니다. 이렇게 하면 코드 숨김 파일에서 직접 시각적 요소에 액세스 하 여 설정을 시각적 모양이 없습니다.

클래스를 파생 하는 것이 좋습니다. 만들고자 할 `Entry` 및 아마도 외부 유효성 검사 함수에 설정할 수 있는 속성을 정의 합니다. 파생 된 클래스 `Entry` 호출할 수는 `VisualStateManager.GoToState` 메서드. 하지만 경우에이 체계 정상적으로 작동 합니다 `Entry` 다른 시각적 상태에 영향을 받는 유일한 개체입니다. 이 예제는 `Label` 및 `Button` 영향을 받을 수는 있습니다. VSM 태그 연결에 대 한 방법이 없기는 `Entry` 시각적 상태의 변경을 다른 개체에서 참조 하는 다른 개체에 연결 된 VSM 태그에 대 한 페이지에 없는 방식으로 다른 개체를 제어 합니다.

<a name="adaptive-layout" />

## <a name="using-the-visual-state-manager-for-adaptive-layout"></a>Visual State Manager를 사용 하 여 적응 레이아웃

여러 다양 한 크기 및 가로 세로 비율을 가정 하는 Xamarin.Forms 휴대폰에서 실행 하는 응용 프로그램을 세로 또는 가로 세로 비율을 고정 및 바탕 화면에서 실행 되는 Xamarin.Forms 프로그램에서 일반적으로 볼 수 있습니다를 조정할 수 있습니다. 잘 설계 된 응용 프로그램을 이러한 다양 한 페이지 또는 창 폼 팩터 용 다르게 해당 콘텐츠를 표시할 수 있습니다. 

이 기술은 라고도 _적응 레이아웃_합니다. 적응 레이아웃에는 전적으로 프로그램의 시각적 개체 포함 되므로 Visual State Manager 이상적인 응용 프로그램.

간단한 예제에는 응용 프로그램의 콘텐츠에 영향을 주는 단추 모음이 표시 하는 응용 프로그램입니다. 세로 모드에서는 이러한 단추는 페이지 맨 위에 있는 가로 행에 표시 될 수 있습니다.:

[![VSM 적응 레이아웃: 세로](vsm-images/VsmAdaptiveLayoutPortrait.png "VSM 적응 레이아웃-세로")](vsm-images/VsmAdaptiveLayoutPortrait-Large.png#lightbox)

가로 모드로 단추의 배열 수 한 쪽으로 이동 되며 열에 표시:

[![VSM 적응 레이아웃: 가로](vsm-images/VsmAdaptiveLayoutLandscape.png "VSM 적응 레이아웃-가로")](vsm-images/VsmAdaptiveLayoutLandscape-Large.png#lightbox)

위쪽에서 아래쪽, 프로그램은 유니버설 Windows 플랫폼, Android 및 iOS에서 실행 됩니다.

**VSM 적응 레이아웃** 페이지에 [VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/) 샘플 "Portrait" 및 "Landscape" 라는 두 시각적 상태를 사용 하 여 "OrientationStates" 라는 그룹을 정의 합니다. (다소 복잡 한 방법은 수에 따라 여러 다른 페이지 또는 창 너비입니다.) 

VSM 태그 XAML 파일에 네 곳에서 발생합니다. 합니다 `StackLayout` 라는 `mainStack` 메뉴와 인 콘텐츠를 모두 포함는 `Image` 요소입니다. 이 `StackLayout` 세로 모드에서 세로 방향 및 가로 모드에서는 가로 방향 있어야 합니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmAdaptiveLayoutPage"
             Title="VSM Adaptive Layout">

    <StackLayout x:Name="mainStack">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup Name="OrientationStates">

                <VisualState Name="Portrait">
                    <VisualState.Setters>
                        <Setter Property="Orientation" Value="Vertical" />
                    </VisualState.Setters>
                </VisualState>

                <VisualState Name="Landscape">
                    <VisualState.Setters>
                        <Setter Property="Orientation" Value="Horizontal" />
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>
        
        <ScrollView x:Name="menuScroll">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="OrientationStates">
                    
                    <VisualState Name="Portrait">
                        <VisualState.Setters>
                            <Setter Property="Orientation" Value="Horizontal" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState Name="Landscape">
                        <VisualState.Setters>
                            <Setter Property="Orientation" Value="Vertical" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
            
            <StackLayout x:Name="menuStack">
                <VisualStateManager.VisualStateGroups>
                    <VisualStateGroup Name="OrientationStates">

                        <VisualState Name="Portrait">
                            <VisualState.Setters>
                                <Setter Property="Orientation" Value="Horizontal" />
                            </VisualState.Setters>
                        </VisualState>

                        <VisualState Name="Landscape">
                            <VisualState.Setters>
                                <Setter Property="Orientation" Value="Vertical" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateManager.VisualStateGroups>

                <StackLayout.Resources>
                    <Style TargetType="Button">
                        <Setter Property="VisualStateManager.VisualStateGroups">
                            <VisualStateGroupList>
                                <VisualStateGroup Name="OrientationStates">

                                    <VisualState Name="Portrait">
                                        <VisualState.Setters>
                                            <Setter Property="HorizontalOptions" Value="CenterAndExpand" />
                                            <Setter Property="Margin" Value="10, 5" />
                                        </VisualState.Setters>
                                    </VisualState>

                                    <VisualState Name="Landscape">
                                        <VisualState.Setters>
                                            <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                                            <Setter Property="HorizontalOptions" Value="Center" />
                                            <Setter Property="Margin" Value="10" />
                                        </VisualState.Setters>
                                    </VisualState>
                                </VisualStateGroup>
                            </VisualStateGroupList>
                        </Setter>
                    </Style>
                </StackLayout.Resources>

                <Button Text="Banana"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="Banana.jpg" />
                
                <Button Text="Face Palm"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="FacePalm.jpg" />
                
                <Button Text="Monkey"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="monkey.png" />
                
                <Button Text="Seated Monkey"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="SeatedMonkey.jpg" />
            </StackLayout>
        </ScrollView>

        <Image x:Name="image"
               VerticalOptions="FillAndExpand"
               HorizontalOptions="FillAndExpand" />
    </StackLayout>
</ContentPage>
```

내부 `ScrollView` 라는 `menuScroll` 하며 `StackLayout` 라는 `menuStack` 메뉴 단추를 구현 합니다. 이러한 레이아웃의 방향을 반대쪽의 `mainStack`합니다. 메뉴를 세로 모드로 가로 및 세로 가로 모드로 있어야 합니다.

VSM 태그의 네 번째 섹션을 자체 단추에 대 한 암시적 스타일의 경우 이 태그 집합 `VerticalOptions`, `HorizontalOptions`, 및 `Margin` portait 및 가로 방향에 관련 된 속성입니다.

코드 숨김 파일 집합을 `BindingContext` 의 속성 `menuStack` 구현 하 `Button` 도 처리기를 연결 및 명령에 `SizeChanged` 페이지의 이벤트:

```csharp
public partial class VsmAdaptiveLayoutPage : ContentPage
{
    public VsmAdaptiveLayoutPage ()
    {
        InitializeComponent ();

        SizeChanged += (sender, args) =>
        {
            string visualState = Width > Height ? "Landscape" : "Portrait";
            VisualStateManager.GoToState(mainStack, visualState);
            VisualStateManager.GoToState(menuScroll, visualState);
            VisualStateManager.GoToState(menuStack, visualState);

            foreach (View child in menuStack.Children)
            {
                VisualStateManager.GoToState(child, visualState);
            }
        };

        SelectedCommand = new Command<string>((filename) =>
        {
            image.Source = ImageSource.FromResource("VsmDemos.Images." + filename);
        });

        menuStack.BindingContext = this;
    }

    public ICommand SelectedCommand { private set; get; }
}
```

`SizeChanged` 처리기 호출 `VisualStateManager.GoToState` 두 개의 `StackLayout` 및 `ScrollView` 요소 및 다음의 자식 반복 `menuStack` 호출 `VisualStateManager.GoToState` 에 대 한는 `Button` 요소입니다.

처럼 코드 숨김 파일을 XAML 파일에서 요소의 속성을 설정 하 여 직접 더 방향 변경을 처리할 수 있지만 Visual State Manager 보다 구조적인 방법이 확실 하 게 보일 수 있습니다. 모든 시각적 개체를 쉽게 검사할 수 있는, XAML 파일에 유지 되는 유지 관리 하 고 수정 합니다.

## <a name="visual-state-manager-with-xamarinuniversity"></a>Visual State Manager Xamarin.University 사용 하 여

> [!VIDEO https://youtube.com/embed/qhUHbVP5mIQ]

**Xamarin.Forms 3.0 Visual State Manager, [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>관련 링크

- [VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)

