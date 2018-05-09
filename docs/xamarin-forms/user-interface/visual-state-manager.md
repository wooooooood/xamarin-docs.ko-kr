---
title: Xamarin.Forms Visual State Manager
description: Visual State Manager를 사용 하 여 코드에서 설정 하는 시각적 상태를 기반으로 하는 XAML 요소에서 변경할 수 있습니다.
ms.prod: xamarin
ms.assetid: 17296F14-640D-484B-A24C-A4E9B7013E4F
ms.technology: xamarin-forms
ms.custom: xamu-video
author: charlespetzold
ms.author: chape
ms.date: 05/07/2018
ms.openlocfilehash: f511f5c33b947704a42df850d2772c0b26511173
ms.sourcegitcommit: daa089d41cfe1ed0456d6de2f8134cf96ae072b1
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2018
---
# <a name="the-xamarinforms-visual-state-manager"></a>Xamarin.Forms Visual State Manager

_Visual State Manager를 사용 하 여 코드에서 설정 하는 시각적 상태를 기반으로 하는 XAML 요소에서 변경할 수 있습니다._

시각적 상태 관리자 (VSM) Xamarin.Forms 3.0의 새로운 기능입니다. VSM 코드에서 시각적 항목이 변경 된 사용자 인터페이스에 수행 하는 구조적된 방법을 제공 합니다. XAML에서 정의 된 대부분의 경우 응용 프로그램의 사용자 인터페이스와이 XAML Visual State Manager 사용자 인터페이스의 시각적 개체에 미치는 영향을 설명 하는 태그를 포함 합니다.

개념을 소개 하는 VSM _시각적 상태_합니다. 와 같은 Xamarin.Forms 보기는 `Button` 기본 상태에 따라 여러 시각적 모양이 여러 가지 있을 수 있습니다 &mdash; 하지 않는 경우 또는 누른 또는 입력 포커스가 여부. 이들은 단추의 상태입니다.

시각적 상태에 수집 됩니다. _시각적 상태 그룹_합니다. 시각적 상태 그룹 내의 모든 시각적 상태는 함께 사용할 수 없습니다. 시각적 상태와 표시 상태 그룹 모두에 게 간단한 텍스트 문자열으로 식별 됩니다.

초기 릴리스 Xamarin.Florms Visual State Manager 세 시각적 상태 "CommonStates" 이라는 하나의 시각적 상태 그룹을 정의 합니다.

- "Normal"
- "비활성화"
- "포커스 있음"

이 시각적 상태 그룹에서 파생 되는 모든 클래스에 대해서는 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement),이 대 한 기본 클래스 [ `View` ](xref:Xamarin.Forms.View) 및 [ `Page` ](xref:Xamarin.Forms.Page)합니다. 

사용자 고유의 시각적 상태 그룹을 정의할 수 있습니다 및 시각적 상태도이 문서에서는 보여 줍니다.

> [!NOTE]
> Xamarin.Forms에 잘 알고 있는 개발자가 [트리거](~/xamarin-forms/app-fundamentals/triggers.md) 는 트리거 수를 변경할 수도 보기의 속성 또는 이벤트 발생의 변화에 따라 사용자 인터페이스의 시각적 개체를 인식 합니다. 그러나 이러한 변경의 다양 한 조합으로 처리 하도록 트리거를 사용 하 여 매우 혼동 될 수 있습니다. 지금까지 시각적 상태 조합 으로부터 결과 혼동을 줄이기 위해서는 Windows XAML 기반 환경에서 Visual State Manager 도입 되었습니다. Vsm, 시각적 상태 그룹 내의 시각적 상태는 항상 상호 배타적입니다. 언제 든 지 각 그룹에서 하나의 상태는 현재 상태입니다.

## <a name="the-common-states"></a>일반적인 상태

초기 릴리스에서 Visual State Manager를 사용 하면 보기 보통 또는 사용 안 함 되었거나에 입력된 포커스가 있는 경우 보기의 시각적 모양을 변경할 수 있는 XAML 파일의 섹션을 포함 하는 수 있습니다. 이 라고는 _일반적인 상태_합니다.

예를 들어 있다고 가정는 `Entry` 페이지 보기. 여기의 시각적 모양을 원하는 상태가 `Entry` 변경 하려면:

- `Entry` 는 pink 있어야 때 백그라운드에서 `Entry` 을 사용할 수 없습니다.
- `Entry` 정상적으로 라임 배경 있어야 합니다.
- `Entry` 에 포커스를 입력 하는 경우 두 번 기본 높이를 확장 해야 합니다.

여러 보기에 적용 되는 경우 스타일에서 정의할 수 있습니다 또는 개별 보기를 VSM 태그를 연결할 수 있습니다. 다음 두 섹션에서는 이러한 방법에 설명 합니다.

### <a name="vsm-markup-on-a-view"></a>뷰에 VSM 태그

VSM 태그를 연결 하는 `Entry` 보기에서 먼저 분리는 `Entry` 시작 및 끝 태그에:

```xaml
<Entry FontSize="18">

</Entry>
```

명시적 글꼴 크기는 상태 중 하나 사용 되기 때문에 제공 된 `FontSize` 속성에 있는 텍스트의 크기를 두 배로을 `Entry`합니다.

다음으로 삽입 `VisualStateManager.VisualStateGroups` 해당 태그 사이의 태그가:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>

    </VisualStateManager.VisualStateGroups>
</Entry>
```

조금 이상 해 보일 수 있습니다. 이러한 종류의 두 태그 사이 나타나는 유일한 태그 내용 또는 속성 요소에 대 한 일반적으로 및 `VisualStateManager.VisualStateGroups` 태그는 모두 합니다.

법적 XAML 구문 때문에 이것이 [ `VisualStateGroups` ](xref:Xamarin.Forms.VisualStateManager.VisualStateGroupsProperty) 으로 정의 하는 연결 된 바인딩 가능한 속성의 [ `VisualStateManager` ](xref:Xamarin.Forms.VisualStateManager) 클래스입니다. (연결 된 바인딩 가능한 속성에 대 한 자세한 내용은 문서 참조 [연결 된 속성](~/xamarin-forms/xaml/attached-properties.md).) 이것은 방법을 `VisualStateGroups` 속성이에 연결 된는 `Entry` 개체입니다.

`VisualStateGroups` 형식의 속성은 [ `VisualStateGroupList` ](xref:Xamarin.Forms.VisualStateGroupList)의 컬렉션인 [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) 개체입니다. 내에서 `VisualStateManager.VisualStateGroups` 태그를 삽입 한 쌍의 `VisualStateGroup` 각 그룹에 포함 하려는 시각적 상태에 대 한 태그:

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

에 `VisualStateGroup` 태그에는 `x:Name` 그룹의 이름을 나타내는 특성입니다. `VisualStateGroup` 클래스 정의 `Name` 대신 사용할 수 있는 속성:

```xaml
<VisualStateGroup Name="CommonStates">
```

`VisualStateGroup` 라는 속성을 정의 하는 클래스 [ `States` ](xref:Xamarin.Forms.VisualStateGroup.States)의 컬렉션인 [ `VisualState` ](xref:Xamarin.Forms.VisualState) 개체입니다. `States` 콘텐츠 속성은 `VisualStateGroups` 에 사용할 수 있도록는 `VisualState` 사이 직접 태그는 `VisualStateGroup` 태그입니다.

다음 단계에서는 해당 그룹의 한 쌍의 모든 시각적 상태에 대 한 태그를 포함 하는 것입니다. 또한 식별할 수 있습니다를 사용 하 여 `x:Name` 또는 `Name`:

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

`VisualState` 라는 속성을 정의 [ `Setters` ](xref:Xamarin.Forms.VisualState.Setters)의 컬렉션인 [ `Setter` ](xref:Xamarin.Forms.Setter) 개체입니다. 동일한 이들은 `Setter` 에서 사용 하는 개체는 [ `Style` ](xref:Xamarin.Forms.Style) 개체입니다.

`Setters` _하지_ 의 content 속성 `VisualState`에 대 한 속성 요소 태그를 포함 하는 데 필요한 큽니다는 `Setters` 속성:

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

하나 이상의 이제 삽입할 수 `Setter` 의 각 쌍 사이 개체 `Setters` 태그입니다. 이들은 `Setter` 앞에서 설명한 시각적 상태를 정의 하는 개체:

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

각 `Setter` 해당 상태가 현재이 태그는 특정 속성의 값을 나타냅니다. 참조 하는 모든 속성을 `Setter` 개체 바인딩 가능한 속성으로 지원 되어야 합니다.

이와 유사한 태그의 기준이 되는 **보기 VSM** 페이지에 **[VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)** 샘플 프로그램. 페이지에 세 개의 포함 `Entry` 뷰를 지원 하지만 두 번째 식만에 첨부 된 VSM 태그:

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

두 번째 `Entry` 역시는 `DataTrigger` 의 일부로 해당 `Trigger` 컬렉션입니다. 이 인해는 `Entry` 무언가 세 번째에 입력할 때까지 사용 하지 않도록 설정할 `Entry`합니다. 다음은 iOS, Android 및 유니버설 Windows 플랫폼 (UWP)에서 실행 되는 시작 시의 페이지가입니다.

[![보기에는 VSM: 사용 하지 않도록 설정](vsm-images/VsmOnViewDisabled.png "VSM 보기-사용 하지 않도록 설정")](vsm-images/VsmOnViewDisabled-Large.png#lightbox)

현재 visual 상태가 "Disabled" 하므로 두 번째의 배경을 `Entry` 은 iOS 및 Android 화면의 분홍색 합니다. UWP 구현의 `Entry` 배경을 설정 하지 못하도록 때 색는 `Entry` 을 사용할 수 없습니다. 

세 번째 요구 사항에 입력할 사용자 지정 하는 것 `Entry`, 두 번째 `Entry` "Normal" 상태 및 백그라운드를 스위치 라임 포함 되었습니다.

[![보기에는 VSM: 보통](vsm-images/VsmOnViewNormal.png "VSM 보기-일반")](vsm-images/VsmOnViewNormal-Large.png#lightbox)

두 번째를 터치 할 `Entry`, 입력된 포커스를 가져옵니다. "Focused" 상태로 전환 하 고 두 번 높이를 확장 합니다.

[![보기에는 VSM: 초점을 맞춘](vsm-images/VsmOnViewFocused.png "VSM에 포커스가 있는 뷰-")](vsm-images/VsmOnViewFocused-Large.png#lightbox)

에 `Entry` 입력 포커스를 가져오면 라임 백그라운드 유지 되지 않습니다. Visual State Manager 시각적 상태 간의 전환 하는 대로 이전 상태로에서 설정한 속성 설정 하지 않습니다. 시각적 상태 상호 배타적인 것을 명심 하십시오. "Normal" 상태만 있음을 의미 하지 않으며는 `Entry` 를 사용할 수 있습니다. 즉,는 `Entry` 켜져 있고 입력된 포커스가 없습니다. 

원하는 경우는 `Entry` 라임 배경 "Focused" 상태에서 할 다른 항목 추가 `Setter` 해당 시각적 상태:

```xaml
<VisualState x:Name="Focused">
    <VisualState.Setters>
        <Setter Property="FontSize" Value="36" />
        <Setter Property="BackgroundColor" Value="Lime" />
    </VisualState.Setters>
</VisualState>
```

이러한 순서로 `Setter` 올바르게 작동 하기 위해서는 개체는 `VisualStateGroup` 포함 해야 `VisualState` 해당 그룹의 모든 상태에 대 한 개체입니다. 포함 하지 않는 시각적 상태 이면 `Setter` 개체 그래도 빈 태그를 포함 합니다.

```xaml
<VisualState x:Name="Normal" />
``` 

### <a name="vsm-markup-in-a-style"></a>VSM 태그를 스타일

필요 동일한 Visual State Manager 태그를 두 개 이상의 보기 사이에서 공유 하는 경우가 많습니다. 태그에 저장 해야 하는 경우에 `Style` 정의 합니다.

다음은 기존 암시적 `Style` 에 대 한는 `Entry` 의 요소는 **VSM에 보기** 페이지:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
</Style> 
```

추가 `Setter` 에 대 한 태그는 `VisualStateManager.VisualStateGroups` 연결 된 바인딩 가능한 속성:

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">

    </Setter>
</Style> 
```

에 대 한 콘텐츠 속성 `Setter` 은 `Value`하므로의 값은 `Value` 해당 태그 내에서 직접 속성을 지정할 수 있습니다. 속성 형식이 한지 `VisualStateGroupList`:

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

해당 태그 내에서 하나 이상의 포함할 수 있습니다 `VisualStateGroup` 개체:

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

VSM 태그의 나머지 부분에서는 이전과 동일 합니다.

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

이제 모든는 `Entry` 이 페이지에서 보기의 시각적 상태에 같은 방법으로 응답 합니다. "Focused" 상태는 두 번째를 이제는 있습니다 `Setter` 에서는 각 `Entry` 때 것에 입력 포커스가 라임도 백그라운드:

[![스타일의 VSM](vsm-images/VsmInStyle.png "VSM 스타일")](vsm-images/VsmInStyle-Large.png#lightbox)

## <a name="defining-your-own-visual-states"></a>사용자 고유의 시각적 상태 정의

파생 되는 모든 클래스는 `VisualElement` 는 세 가지 일반적인 상태 "Normal", "포커스 있음" 및 "비활성화"를 지원 합니다. 내부적으로 [ `VisualElement` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/VisualElement.cs) 범위가 사용 또는 사용 안 함 또는 포커스가 있거나 포커스가 없을, 점점 하 고 정적을 호출 하는 경우를 검색 하는 클래스 [ `VisualStateManager.GoToState` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualStateManager.GoToState/p/Xamarin.Forms.VisualElement/System.String/) 다음과 같이 메서드:

```csharp
VisualStateManager.GoToState(this, "Focused");
```

이 중요 한 방법 이며 Visual State Manager 코드만에서 확인할 수는 `VisualElement` 클래스입니다. 때문에 `GoToState` 에서 파생 되는 모든 클래스 동시에 기반 하 여 모든 개체에 대해 호출 됩니다 `VisualElement`, Visual State Manager를 사용 하 여 함께 `VisualElement` 이러한 변경에 응답 하는 개체입니다.

흥미롭게도 "CommonStates" 시각적 상태 그룹의 이름을 명시적으로에서 참조 되지 않은 `VisualElement`합니다. 그룹 이름은 Visual State Manager에 대 한 API의 일부가 아닙니다. 앞에서 설명한 두 개의 샘플 프로그램 중 어느 하나에 다른 모든 항목에 대 한 "CommonStates" 그룹의 이름을 변경할 수 있습니다 및 프로그램이 계속 작동 합니다. 그룹 이름은 해당 그룹의 상태에 대해 개괄적으로 설명 하기만 합니다. 시각적 상태 그룹에서 상호 배타적인은 암시적으로 인식 됩니다: 한 상태 및 하나만 상태 언제 든 지 최신 상태입니다.

호출 해야 사용자 고유의 시각적 상태를 구현 하려는 경우 `VisualStateManager.GoToState` 코드에서. 가장 자주 페이지 클래스의 코드 숨김 파일에서이 호출이 지정 합니다.

**VSM 유효성 검사** 페이지에 **[VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)** 샘플 입력된 유효성 검사와 관련 하 여 Visual State Manager를 사용 하는 방법을 보여 줍니다. XAML 파일은 두 개의 `Label` 요소는 `Entry`, 및 `Button`:

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

VSM 태그 두 번째에 연결 되어 `Label` (라는 `helpLabel`) 및 `Button` (라는 `submitButton`). "Valid" 및 "잘못 됨" 이라는 두 개의 상호 배타적인 상태 있습니다. (곧 이러한 상태를 설정 하는 코드 숨김 파일이 표시 됩니다.) 예를 들어 각각 두 개의 "ValidationState" 그룹의 표시 `VisualState` 각각의 경우에서 그 중 하나가 비어 있지만 "Valid" 및 "잘못 된" 모두에 대 한 태그입니다. 

경우는 `Entry` 현재 상태는 "잘못 된"는 올바른 전화 번호가 없습니다. 두 번째 `Label` 표시 되 고 `Button` 을 사용할 수 없습니다.

[![VSM 유효성 검사: 잘못 된 상태](vsm-images/VsmValidationInvalid.png "VSM 유효성 검사-잘못 되었습니다.")](vsm-images/VsmValidationInvalid-Large.png#lightbox)

올바른 전화 번호를 입력 한 다음 현재 상태는 "Valid"입니다. 두 번째 `Entry` 사라집니다 및 `Button` 활성화 되었습니다.

[![VSM 유효성 검사: 유효한 상태](vsm-images/VsmValidationValid.png "VSM 유효성 검사-유효한")](vsm-images/VsmValidationValid-Large.png#lightbox)

코드 숨김 파일은 처리를 위한 reponsible는 `TextChanged` 에서 이벤트는 `Entry`합니다. 입력된 문자열이 유효한 인지 인지 확인 하려면 정규식을 사용 하는 처리기. 명명 된 코드 숨김 파일의 메서드에 `GoToState` 정적 호출 `VisualStateManager.GoToState` 메서드 모두에 대 한 `helpLabel` 및 `submitButton`:

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

또한는 `GoToState` 상태를 초기화 하는 생성자에서 메서드를 호출 합니다. 현재 상태는 항상 있을 것입니다. 인데 곳은 코드에 있는 시각적 상태 그룹의 이름에 대 한 참조 "ValidationStates" 위해 명확성으로 XAML에서 참조 되는 있지만. 

호출 하 고 이러한 시각적 상태에서 코드 숨김 파일이 영향을 받는 페이지에서 모든 개체의 계정을 수행 해야 확인 `VisualStateManager.GoToState` 이러한 각 개체에 대 한 합니다. 이 예에서 두 개체는 (의 `Label` 및 `Button`)를 여러 개 있을 수 있지만 더 많은 합니다.

궁금할: 코드 숨김 파일에서 이러한 시각적 상태에 영향을 받는 페이지에서 모든 개체를 참조 해야 하는 경우 이유 코드 숨김 파일 단순히 직접 액세스할 수 없는 개체? 이 분명 할 수 없습니다. 그러나 Visual State Manager를 사용 하 여 이러한 개체를 한 곳에서 모든 사용자 인터페이스 디자인 되므로 XAML에서 다른 시각적 상태에 대응 방법을 제어할 수 있습니다.

클래스를 파생 하는 것이 좋습니다. 상자로 `Entry` 아마도 외부 유효성 검사 함수에 설정할 수 있는 속성을 정의 합니다. 파생 된 클래스 `Entry` 호출할 수는 `VisualStateManager.GoToState` 메서드. 경우에 제대로 작동할 것이 체계는 `Entry` 다른 시각적 상태에 영향을 받는 유일한 개체 되었습니다. 이 예제는 `Label` 및 `Button` 영향을 받을 수는 있습니다. 에 연결 된 VSM 태그에 대 한 방법이 있으면는 `Entry` VSM 태그 시각적 상태 변경을 다른 개체에서 참조 하는 다른 개체에 해당 연결에 대 한 페이지, 및 방법은 다른 개체를 제어할 수 있습니다.

<a name="adaptive-layout" />

## <a name="using-the-vsm-for-adaptive-layout"></a>레이아웃의 적응는 VSM을 사용 하 여

휴대폰에서 실행 되는 Xamarin.Forms 프로그램 일반적으로 세로 또는 가로 세로 비율로 볼 수 있습니다 및 많은 다양 한 크기 및 가로 세로 비율을 가정 하는 데스크톱에서 실행 되는 Xamarin.Forms 프로그램 크기를 조정할 수 있습니다. 잘 디자인 된 응용 프로그램 다르게 이러한 다양 한 페이지 또는 창 폼 요소 모두에 해당 콘텐츠를 표시할 수 있습니다. 

이 기술은 라고도 _적응 레이아웃_합니다. 적응 레이아웃만을 포함 되는 프로그램의 시각적 표시 되므로 Visual State Manager의 이상적인 응용 프로그램.

간단한 예는 응용 프로그램의 내용에 영향을 주는 단추의 작은 컬렉션을 표시 하는 프로그램입니다. 세로 모드의 이러한 단추 페이지 위쪽의 가로 행으로 표시 될 수 있습니다.

[![VSM 적응 레이아웃: 세로](vsm-images/VsmAdaptiveLayoutPortrait.png "VSM 적응 레이아웃-세로")](vsm-images/VsmAdaptiveLayoutPortrait-Large.png#lightbox)

가로 모드로 단추의 배열은 한 쪽으로 이동 및 열에 표시 될 수 있습니다.

[![VSM 적응 레이아웃: 가로로](vsm-images/VsmAdaptiveLayoutLandscape.png "VSM 적응 레이아웃-가로")](vsm-images/VsmAdaptiveLayoutLandscape-Large.png#lightbox)

위쪽에서 아래쪽, 프로그램 유니버설 Windows 플랫폼, Android 및 iOS에서 실행 됩니다.

이것이 Visual State Manager에 대 한 작업입니다. **VSM 적응 레이아웃** 페이지에 [VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/) 샘플 "가로"와 "세로" 라는 두 개의 시각적 상태 "OrientationStates" 라는 그룹을 정의 합니다. (여러 다른 페이지 또는 창 너비 보다 복잡 한 접근을 기준이 될 수 있습니다.) 

XAML 파일의 4 개 위치에서 VSM 태그 표시 됩니다. `StackLayout` 라는 `mainStack` 메뉴와 콘텐츠를 모두 포함 한 `Image` 요소입니다. 이 `StackLayout` 세로 모드의 세로 방향 및 가로 모드에서 가로 방향이 있어야 합니다.

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

내부 `ScrollView` 라는 `menuScroll` 및 `StackLayout` 라는 `menuStack` 메뉴의 단추를 구현 합니다. 이러한 레이아웃 방향은 반대의 `mainStack`: 메뉴 세로 모드의 가로 및 세로에서 가로 모드 여야 합니다.

네 번째 청크 VSM 태그 자체 단추에 대 한 암시적 스타일입니다. 이 태그 설정 `VerticalOptions`, `HorizontalOptions`, 및 `Margin` portait 가로 및 세로 orienations 관련 속성입니다.

코드 숨김 파일 세트는 `BindingContext` 속성 `menuStack` 구현 하려면 `Button` , 명령 및 처리기를 연결 합니다는 `SizeChanged` 페이지의 이벤트:

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

`SizeChanged` 처리기 호출 `VisualStateManager.GoToState` 두 `StackLayout` 및 `ScrollView` 요소 및 다음 루프의 하위 항목 `menuStack` 호출할 `VisualStateManager.GoToState` 에 대 한는 `Button` 요소.

처음에 코드 숨김 파일 XAML 파일에서 요소의 속성을 설정 하 여 보다 직접적 방향 변경을 처리할 수 있지만 Visual State Manager 보다 훨씬 구조화 된 방법을 분명 이벤트 처럼 보입니다. 모든 시각적 개체를 검사 하는 보다 쉽게 수 있는, XAML 파일에 보관 되는 유지 관리 하 고 수정 합니다.

## <a name="visual-state-manager-with-xamarinuniversity"></a>Xamarin.University와 표시 상태 관리자

> [!VIDEO https://youtube.com/embed/qhUHbVP5mIQ]

**Xamarin.Forms 3.0 Visual State Manager를 위한 것 이며 [Xamarin 대학](https://university.xamarin.com/)**

## <a name="related-links"></a>관련된 링크

- [VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)

