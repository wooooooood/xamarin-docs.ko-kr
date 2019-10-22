---
title: Xamarin Forms 시각적 상태 관리자
description: Visual State Manager를 사용 하 여 코드에서 설정 된 시각적 상태에 따라 XAML 요소를 변경 합니다.
ms.prod: xamarin
ms.assetid: 17296F14-640D-484B-A24C-A4E9B7013E4F
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 228501172ede71204c64e1efe1673ce92be424ea
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "68656051"
---
# <a name="the-xamarinforms-visual-state-manager"></a>Xamarin Forms 시각적 상태 관리자

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)

_Visual State Manager를 사용 하 여 코드에서 설정 된 시각적 상태에 따라 XAML 요소를 변경 합니다._

VSM (시각적 상태 관리자)은 Xamarin. Forms 3.0에 새로 있습니다. VSM은 코드에서 사용자 인터페이스에 대 한 시각적 변경을 수행 하는 구조화 된 방법을 제공 합니다. 대부분의 경우 응용 프로그램의 사용자 인터페이스는 XAML로 정의 되며이 XAML에는 시각적 상태 관리자가 사용자 인터페이스의 시각적 개체에 미치는 영향을 설명 하는 태그가 포함 됩니다.

VSM은 _시각적 상태의_개념을 소개 합니다. @No__t_0와 같은 Xamarin 폼 보기에는 기본 &mdash; 상태 (사용 안 함, 누름 또는 입력 포커스 있음)에 따라 여러 시각적 모양이 포함 될 수 있습니다. 이는 단추의 상태입니다.

시각적 상태는 _시각적 상태 그룹_에서 수집 됩니다. 시각적 상태 그룹 내의 모든 시각적 상태는 함께 사용할 수 없습니다. 시각적 상태 그룹과 시각적 상태 그룹 모두 단순 텍스트 문자열로 식별 됩니다.

Xamarin.ios 시각적 상태 관리자는 다음과 같은 세 가지 시각적 상태를 포함 하는 "CommonStates" 이라는 시각적 상태 그룹 하나를 정의 합니다.

- "Normal"
- 해제
- 편지

이 시각적 상태 그룹은 [`View`](xref:Xamarin.Forms.View) 및 [`Page`](xref:Xamarin.Forms.Page)에 대 한 기본 클래스인 [`VisualElement`](xref:Xamarin.Forms.VisualElement)에서 파생 되는 모든 클래스에 대해 지원 됩니다. 

이 문서에서 설명 하는 대로 고유한 시각적 상태 그룹 및 시각적 상태를 정의할 수도 있습니다.

> [!NOTE]
> [트리거에](~/xamarin-forms/app-fundamentals/triggers.md) 익숙한 Xamarin Forms 개발자는 보기의 속성 변경 또는 이벤트 발생을 기반으로 사용자 인터페이스의 시각적 개체에 대 한 변경을 수행할 수도 있습니다. 그러나 트리거를 사용 하 여 이러한 변경 사항의 다양 한 조합을 처리 하는 것은 매우 복잡할 수 있습니다. 지금 까지는 시각적 상태 관리자가 시각적 상태를 조합 하 여 발생 하는 혼동을 줄이기 위해 Windows XAML 기반 환경에서 도입 되었습니다. VSM을 사용할 경우 시각적 상태 그룹 내의 시각적 상태는 항상 함께 사용할 수 없습니다. 언제 든 지 각 그룹의 한 상태만 현재 상태입니다.

## <a name="the-common-states"></a>공용 상태

시각적 상태 관리자를 사용 하면 뷰가 정상적 이거나 비활성화 되었거나 입력 포커스가 있는 경우 뷰의 시각적 모양을 변경할 수 있는 섹션을 XAML 파일에 포함할 수 있습니다. 이러한 _상태를 공용 상태_라고 합니다.

예를 들어 페이지에 `Entry` 보기가 있고 다음과 같은 방법으로 `Entry`의 시각적 모양을 변경 하려는 경우를 가정해 보겠습니다.

- @No__t_1 사용 하지 않도록 설정 된 경우 `Entry`은 분홍색 배경 이어야 합니다.
- @No__t_0은 일반적으로 황록색을 포함 해야 합니다.
- 입력 포커스가 있는 경우 `Entry`은 표준 높이의 두 배까지 확장 됩니다.

VSM 태그를 개별 뷰에 연결 하거나 여러 뷰에 적용 되는 경우 스타일에서 정의할 수 있습니다. 다음 두 섹션에서는 이러한 접근 방식을 설명 합니다.

### <a name="vsm-markup-on-a-view"></a>뷰의 VSM 마크업

@No__t_0 뷰에 VSM 태그를 연결 하려면 먼저 `Entry`를 시작 태그와 끝 태그로 분리 합니다.

```xaml
<Entry FontSize="18">

</Entry>
```

상태 중 하나가 `FontSize` 속성을 사용 하 여 `Entry`의 텍스트 크기를 두 배로 지정 하므로 명시적인 글꼴 크기가 지정 됩니다.

다음으로 태그 사이에 `VisualStateManager.VisualStateGroups` 태그를 삽입 합니다.

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>

    </VisualStateManager.VisualStateGroups>
</Entry>
```

[`VisualStateGroups`](xref:Xamarin.Forms.VisualStateManager.VisualStateGroupsProperty) 은 [`VisualStateManager`](xref:Xamarin.Forms.VisualStateManager) 클래스로 정의 된 바인딩 가능한 속성입니다. 연결 된 바인딩 가능한 속성에 대 한 자세한 내용은 연결 된 [속성](~/xamarin-forms/xaml/attached-properties.md)문서를 참조 하세요. @No__t_1 속성이 `Entry` 개체에 연결 되는 방법입니다.

@No__t_0 속성은 [`VisualStateGroup`](xref:Xamarin.Forms.VisualStateGroup) 개체 컬렉션인 [`VisualStateGroupList`](xref:Xamarin.Forms.VisualStateGroupList)형식입니다. @No__t_0 태그 내에서 포함 하려는 각 시각적 상태 그룹의 `VisualStateGroup` 태그 쌍을 삽입 합니다.

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

@No__t_0 태그에는 그룹의 이름을 나타내는 `x:Name` 특성이 있습니다. @No__t_0 클래스는 대신 사용할 수 있는 `Name` 속성을 정의 합니다.

```xaml
<VisualStateGroup Name="CommonStates">
```

@No__t_0 또는 `Name`를 사용할 수 있지만 동일한 요소에서 둘 다를 사용할 수는 없습니다.

@No__t_0 클래스는 [`VisualState`](xref:Xamarin.Forms.VisualState) 개체 컬렉션인 [`States`](xref:Xamarin.Forms.VisualStateGroup.States)라는 속성을 정의 합니다. `States`은 `VisualStateGroups`의 _콘텐츠 속성_ 이므로 `VisualStateGroup` 태그 사이에 `VisualState` 태그를 직접 포함할 수 있습니다. 콘텐츠 속성은 [필수 XAML 구문](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md#content-properties)문서에 설명 되어 있습니다.

다음 단계는 해당 그룹의 모든 시각적 상태에 대 한 쌍의 태그를 포함 하는 것입니다. @No__t_0 또는 `Name`를 사용 하 여 식별할 수도 있습니다.

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

`VisualState` [`Setter`](xref:Xamarin.Forms.Setter) 개체 컬렉션인 [`Setters`](xref:Xamarin.Forms.VisualState.Setters)라는 속성을 정의 합니다. 이러한 개체는 [`Style`](xref:Xamarin.Forms.Style) 개체에서 사용 하는 것과 동일한 `Setter` 개체입니다.

`Setters`은 `VisualState`의 content 속성이 _아니므로_ `Setters` 속성에 대해 속성 요소 태그를 포함 해야 합니다.

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

이제 `Setters` 태그의 각 쌍 사이에 `Setter` 개체를 하나 이상 삽입할 수 있습니다. 다음은 앞에서 설명한 시각적 상태를 정의 하는 `Setter` 개체입니다.

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

각 `Setter` 태그는 해당 상태가 current 인 경우 특정 속성의 값을 나타냅니다. @No__t_0 개체에서 참조 하는 모든 속성은 바인딩 가능한 속성에 의해 지원 되어야 합니다.

이와 비슷한 마크업은 **[VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** 샘플 프로그램의 **보기 페이지에** 대 한 기본 설정입니다. 이 페이지에는 세 개의 `Entry` 뷰가 포함 되어 있지만 두 번째 보기에는 VSM 태그가 연결 되어 있습니다.

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

두 번째 `Entry`에는 `Trigger` 컬렉션의 일부로 `DataTrigger`도 있습니다. 이로 인해 세 번째 `Entry`에 입력 될 때까지 `Entry` 사용 하지 않도록 설정 됩니다. 시작 시 iOS, Android 및 유니버설 Windows 플랫폼 (UWP)에서 실행 되는 페이지는 다음과 같습니다.

[![보기의 VSM: 사용 안 함](vsm-images/VsmOnViewDisabled.png "뷰에서 VSM-사용 안 함")](vsm-images/VsmOnViewDisabled-Large.png#lightbox)

현재 시각적 상태는 "사용 안 함" 이므로 두 번째 `Entry`의 배경은 iOS 및 Android 화면에서 분홍색입니다. @No__t_0의 UWP 구현에서는 `Entry` 사용 하지 않도록 설정 된 경우 배경색을 설정할 수 없습니다. 

세 번째 `Entry`에 일부 텍스트를 입력 하면 두 번째 `Entry` "정상" 상태로 전환 되 고 배경은 이제 황록색입니다.

[![보기의 VSM: 보통](vsm-images/VsmOnViewNormal.png "보기-보통의 VSM")](vsm-images/VsmOnViewNormal-Large.png#lightbox)

두 번째 `Entry`을 터치 하면 입력 포커스를 가져옵니다. "집중 된" 상태로 전환 되 고 높이를 두 배로 확장 합니다.

[![뷰에서 VSM: 포커스 있음](vsm-images/VsmOnViewFocused.png "뷰 중심의 VSM")](vsm-images/VsmOnViewFocused-Large.png#lightbox)

@No__t_0는 입력 포커스를 가져오는 경우 라임 배경을 유지 하지 않습니다. 시각적 상태 관리자가 시각적 상태를 전환 하면 이전 상태로 설정 된 속성은 설정 되지 않습니다. 시각적 상태는 함께 사용할 수 없다는 점에 유의 하세요. "Normal" 상태는 `Entry` 사용 하도록 설정 된 것만을 의미 하지는 않습니다. @No__t_0 사용 하도록 설정 되어 있고 입력 포커스가 없음을 의미 합니다. 

@No__t_0에서 "포커스가 있는" 상태에 라임 배경을 표시 하려면 해당 시각적 상태에 다른 `Setter`를 추가 합니다.

```xaml
<VisualState x:Name="Focused">
    <VisualState.Setters>
        <Setter Property="FontSize" Value="36" />
        <Setter Property="BackgroundColor" Value="Lime" />
    </VisualState.Setters>
</VisualState>
```

이러한 `Setter` 개체가 제대로 작동 하려면 `VisualStateGroup` 해당 그룹의 모든 상태에 대 한 `VisualState` 개체를 포함 해야 합니다. @No__t_0 개체가 없는 시각적 상태가 표시 되는 경우 빈 태그로 포함 합니다.

```xaml
<VisualState x:Name="Normal" />
``` 

### <a name="visual-state-manager-markup-in-a-style"></a>스타일의 시각적 상태 관리자 태그

두 개 이상의 보기 간에 동일한 시각적 상태 관리자 태그를 공유 해야 하는 경우가 종종 있습니다. 이 경우 `Style` 정의에 태그를 추가 하는 것이 좋습니다.

다음은 뷰 페이지의 **VSM** 에서 `Entry` 요소에 대 한 기존 암시적 `Style`입니다.

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
</Style> 
```

@No__t_1 연결 된 바인딩 가능한 속성에 대 한 `Setter` 태그를 추가 합니다.

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">

    </Setter>
</Style> 
```

@No__t_0에 대 한 content 속성이 `Value` 되므로 `Value` 속성의 값을 해당 태그 내에서 직접 지정할 수 있습니다. 해당 속성은 `VisualStateGroupList` 형식입니다.

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

이러한 태그 내에 `VisualStateGroup` 개체를 하나 이상 포함할 수 있습니다.

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

VSM 태그의 나머지 부분은 이전과 동일 합니다.

전체 VSM 마크업을 보여 주는 **스타일 페이지의 vsm** 은 다음과 같습니다.

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

이제이 페이지의 모든 `Entry` 보기가 시각적 상태와 동일한 방식으로 응답 합니다. 또한 "포커스가 있는" 상태에는 입력 포커스가 있을 때 각 `Entry`에 대 한 라임 배경도 제공 하는 두 번째 `Setter` 포함 됩니다.

[![스타일의 VSM](vsm-images/VsmInStyle.png "스타일의 VSM")](vsm-images/VsmInStyle-Large.png#lightbox)

## <a name="defining-your-own-visual-states"></a>사용자 고유의 시각적 상태 정의

@No__t_0에서 파생 되는 모든 클래스는 "Normal", "집중 된" 및 "Disabled"의 세 가지 일반적인 상태를 지원 합니다. 내부적으로 [`VisualElement`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/VisualElement.cs) 클래스는 사용 하거나 사용 하지 않을 때, 또는 포커스가 나 포커스가 없는를 검색 하 고, 정적 [`VisualStateManager.GoToState`](xref:Xamarin.Forms.VisualStateManager.GoToState(Xamarin.Forms.VisualElement,System.String)) 메서드를 호출 합니다.

```csharp
VisualStateManager.GoToState(this, "Focused");
```

이는 `VisualElement` 클래스에서 찾을 수 있는 유일한 시각적 상태 관리자 코드입니다. @No__t_1에서 파생 되는 모든 클래스를 기반으로 하는 모든 개체에 대해 `GoToState`가 호출 되기 때문에 모든 `VisualElement` 개체에서 Visual State Manager를 사용 하 여 이러한 변경 내용에 응답할 수 있습니다.

흥미롭게도 시각적 상태 그룹 "CommonStates"의 이름은 `VisualElement`에서 명시적으로 참조 되지 않습니다. 그룹 이름은 시각적 상태 관리자에 대 한 API의 일부가 아닙니다. 지금까지 표시 된 두 샘플 프로그램 중 하나에서 그룹 이름을 "CommonStates"에서 다른 항목으로 변경할 수 있으며 프로그램은 계속 작동 합니다. 그룹 이름은 해당 그룹의 상태에 대 한 일반적인 설명 일 뿐입니다. 모든 그룹의 시각적 상태를 함께 사용할 수 없다는 것을 암시적으로 인식 합니다. 한 가지 상태 이며 언제 든 지 한 상태만 현재 상태입니다.

사용자 고유의 시각적 상태를 구현 하려면 코드에서 `VisualStateManager.GoToState`를 호출 해야 합니다. 가장 자주 사용 하는 페이지 클래스의 코드 파일에서이 호출을 수행 합니다.

**[VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** 샘플의 **VSM 유효성 검사** 페이지에서는 입력 유효성 검사와 관련 된 연결에서 Visual State Manager를 사용 하는 방법을 보여 줍니다. XAML 파일은 두 개의 `Label` 요소, `Entry` 및 `Button`으로 구성 되어 있습니다.

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

VSM 마크업은 두 번째 `Label` (`helpLabel` 명명 됨) 및 `Button` (`submitButton` 명명 됨)에 연결 됩니다. "Valid" 및 "Invalid" 라는 두 개의 상호 배타적인 상태가 있습니다. 두 "ValidationState" 그룹에는 "Valid" 및 "Invalid" 모두에 대 한 `VisualState` 태그가 포함 되어 있지만 각각의 경우에는 비어 있습니다. 

@No__t_0에 올바른 전화 번호가 포함 되어 있지 않으면 현재 상태는 "유효 하지 않음" 이므로 두 번째 `Label` 표시 되 고 `Button` 사용 하지 않도록 설정 됩니다.

[![VSM 유효성 검사: 잘못 된 상태](vsm-images/VsmValidationInvalid.png "VSM 유효성 검사-유효 하지 않음")](vsm-images/VsmValidationInvalid-Large.png#lightbox)

유효한 전화 번호를 입력 하면 현재 상태가 "유효함"이 됩니다. 두 번째 `Entry` 사라지면 이제 `Button`를 사용할 수 있습니다.

[![VSM 유효성 검사: 유효한 상태](vsm-images/VsmValidationValid.png "VSM 유효성 검사-유효")](vsm-images/VsmValidationValid-Large.png#lightbox)

코드 숨김이 담당는 `Entry`에서 `TextChanged` 이벤트를 처리 하는 데 사용할 수 있습니다. 처리기는 정규식을 사용 하 여 입력 문자열이 유효한 지 여부를 확인 합니다. @No__t_0 라는 코드 파일의 메서드는 `helpLabel` 및 `submitButton` 모두에 대해 정적 `VisualStateManager.GoToState` 메서드를 호출 합니다.

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

또한 생성자에서 `GoToState` 메서드를 호출 하 여 상태를 초기화 합니다. 항상 현재 상태가 있어야 합니다. 하지만 코드는 명확 하 게 하기 위해 XAML에서 "ValidationStates"로 참조 되지만 시각적 상태 그룹의 이름에 대 한 참조는 없습니다. 

코드 오프 파일은 이러한 시각적 상태의 영향을 받는 페이지의 모든 개체를 고려 하 고 이러한 각 개체에 대해 `VisualStateManager.GoToState`를 호출 해야 합니다. 이 예제에서는 두 개의 개체 (`Label` 및 `Button`)만 사용할 수 있지만 몇 가지 더 다를 수 있습니다.

코드 실행 파일이 이러한 시각적 상태의 영향을 받는 페이지의 모든 개체를 참조 해야 하는 경우 코드 숨김이 아닌 파일에서 개체에 직접 액세스할 수 없는 이유는 무엇입니까? 분명히 할 수 있습니다. 그러나 VSM을 사용 하면 시각적 요소가 XAML에서 완전히 다른 상태에 반응 하는 방식을 제어할 수 있습니다. 그러면 모든 UI 디자인이 한 위치에 유지 됩니다. 이렇게 하면 코드 숨김으로 시각적 요소에 직접 액세스 하 여 시각적 효과를 설정 하지 않습니다.

@No__t_0에서 클래스를 파생 하 고 외부 유효성 검사 함수로 설정할 수 있는 속성을 정의 하는 것이 좋을 수 있습니다. @No__t_0에서 파생 되는 클래스는 `VisualStateManager.GoToState` 메서드를 호출할 수 있습니다. 이 스키마는 `Entry` 다른 시각적 상태의 영향을 받는 유일한 개체인 경우에만 정상적으로 작동 합니다. 이 예제에서는 `Label` 및 `Button`에도 영향을 줍니다. @No__t_0에 연결 된 VSM 태그를 페이지의 다른 개체를 제어 하는 방법은 없으며 다른 개체에 연결 된 VSM 태그에서 다른 개체의 시각적 상태 변경을 참조할 수 있는 방법은 없습니다.

<a name="adaptive-layout" />

## <a name="using-the-visual-state-manager-for-adaptive-layout"></a>적응 레이아웃에 대 한 시각적 상태 관리자 사용

휴대폰에서 실행 되는 Xamarin Forms 응용 프로그램은 보통 세로 또는 가로 가로 세로 비율로 볼 수 있으며, 데스크톱에서 실행 되는 Xamarin Forms 프로그램의 크기를 조정 하 여 다양 한 크기와 가로 세로 비율을 가정 합니다. 잘 디자인 된 응용 프로그램은 이러한 다양 한 페이지 또는 창 폼 팩터를 위해 콘텐츠를 다르게 표시할 수 있습니다. 

이 기술을 _적응형 레이아웃_이라고 합니다. 적응 레이아웃은 프로그램의 시각적 개체와만 관련 되므로 시각적 상태 관리자의 이상적인 응용 프로그램입니다.

간단한 예제는 응용 프로그램의 콘텐츠에 영향을 주는 작은 단추 컬렉션을 표시 하는 응용 프로그램입니다. 세로 모드에서 이러한 단추는 페이지 위쪽의 가로 행에 표시 될 수 있습니다.

[![VSM 적응 레이아웃: 세로](vsm-images/VsmAdaptiveLayoutPortrait.png "VSM 적응 레이아웃-세로")](vsm-images/VsmAdaptiveLayoutPortrait-Large.png#lightbox)

가로 모드에서는 단추 배열이 한 쪽으로 이동 하 여 열에 표시 될 수 있습니다.

[![VSM 적응 레이아웃: 가로](vsm-images/VsmAdaptiveLayoutLandscape.png "VSM 적응 레이아웃-가로")](vsm-images/VsmAdaptiveLayoutLandscape-Large.png#lightbox)

위에서 아래로 프로그램은 유니버설 Windows 플랫폼, Android 및 iOS에서 실행 됩니다.

[VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos) 샘플의 **VSM 적응 레이아웃** 페이지는 이름이 "세로" 및 "가로" 인 두 개의 시각적 상태를 포함 하는 "OrientationStates" 라는 그룹을 정의 합니다. 더 복잡 한 방법은 여러 페이지 또는 창 너비를 기반으로 할 수 있습니다. 

VSM 태그는 XAML 파일의 네 위치에서 발생 합니다. @No__t_1 라는 `StackLayout`에는 `Image` 요소인 메뉴와 콘텐츠가 모두 포함 됩니다. 이 `StackLayout` 세로 모드의 세로 방향이 가로 모드의 가로 방향 이어야 합니다.

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

이름이 `menuScroll` 인 내부 `ScrollView` 및 `StackLayout` `menuStack`는 단추의 메뉴를 구현 합니다. 이러한 레이아웃의 방향은 `mainStack`의 반대입니다. 세로 모드에서는 가로 모드이 고 세로 모드에서는 세로 방향 이어야 합니다.

VSM 태그의 네 번째 섹션은 단추 자체의 암시적 스타일에 있습니다. 이 태그는 `VerticalOptions`, `HorizontalOptions` 및 `Margin` 속성을 지정 합니다.

코드 파일은 `menuStack`의 `BindingContext` 속성을 설정 하 여 `Button` 명령 구현을 구현 하 고 페이지의 `SizeChanged` 이벤트에 처리기를 연결 합니다.

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

@No__t_0 처리기는 두 `StackLayout` 및 `ScrollView` 요소에 대해 `VisualStateManager.GoToState`를 호출한 다음 `menuStack`의 자식을 반복 하 여 `VisualStateManager.GoToState` 요소에 대 한 `Button`를 호출 합니다.

XAML 파일의 요소 속성을 설정 하 여 코드 숨김이 방향 변경을 더 직접적으로 처리할 수 있는 것 처럼 보일 수 있지만 시각적 상태 관리자는 매우 구조화 된 방법입니다. 모든 시각적 개체는 XAML 파일에 저장 되므로 더 쉽게 검사 하 고 유지 관리 하 고 수정할 수 있습니다.

## <a name="visual-state-manager-with-xamarinuniversity"></a>Xamarin을 사용 하는 시각적 상태 관리자

> [!VIDEO https://youtube.com/embed/qhUHbVP5mIQ]

**Xamarin.ios 3.0 Visual State Manager 비디오**

## <a name="related-links"></a>관련 링크

- [VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)
