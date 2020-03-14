---
title: Xamarin.ios 시각적 상태 관리자
description: Visual State Manager를 사용 하 여 코드에서 설정 하는 시각적 상태를 기반으로 하는 XAML 요소를 변경 해야 합니다.
ms.prod: xamarin
ms.assetid: 17296F14-640D-484B-A24C-A4E9B7013E4F
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 02/21/2020
ms.openlocfilehash: 0149806f3ab3772bc206cea9540a989d997c817b
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306544"
---
# <a name="xamarinforms-visual-state-manager"></a>Xamarin.ios 시각적 상태 관리자

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)

_Visual State Manager를 사용 하 여 코드에서 설정 된 시각적 상태에 따라 XAML 요소를 변경 합니다._

VSM (시각적 상태 관리자)은 코드에서 사용자 인터페이스에 대 한 시각적 변경을 수행 하는 구조화 된 방법을 제공 합니다. 대부분의 경우에서 응용 프로그램의 사용자 인터페이스, XAML에 정의 되어이 XAML Visual State Manager 사용자 인터페이스의 시각적 개체에 미치는 영향을 설명 하는 태그를 포함 합니다.

VSM은 _시각적 상태의_개념을 소개 합니다. `Button`와 같은 Xamarin 폼 보기에는 기본 &mdash; 상태 (사용 안 함, 누름 또는 입력 포커스 있음)에 따라 여러 시각적 모양이 포함 될 수 있습니다. 이들은 단추의 상태입니다.

시각적 상태는 _시각적 상태 그룹_에서 수집 됩니다. 시각적 상태 그룹 내에서 모든 시각적 상태는 함께 사용할 수 없습니다. 시각적 상태 및 시각적 상태 그룹은 간단한 텍스트 문자열에서 식별 됩니다.

Xamarin.ios 시각적 상태 관리자는 다음과 같은 시각적 상태를 사용 하 여 "CommonStates" 이라는 하나의 시각적 상태 그룹을 정의 합니다.

- "Normal"
- "사용 안 함"
- "포커스 있음"
- 선택

이 시각적 상태 그룹은 [`View`](xref:Xamarin.Forms.View) 및 [`Page`](xref:Xamarin.Forms.Page)에 대 한 기본 클래스인 [`VisualElement`](xref:Xamarin.Forms.VisualElement)에서 파생 되는 모든 클래스에 대해 지원 됩니다.

시각적 상태 그룹을 직접 정의할 수도 있습니다 및 시각적 상태를이 문서를 보여 줍니다.

> [!NOTE]
> [트리거에](~/xamarin-forms/app-fundamentals/triggers.md) 익숙한 Xamarin Forms 개발자는 보기의 속성 변경 또는 이벤트 발생을 기반으로 사용자 인터페이스의 시각적 개체에 대 한 변경을 수행할 수도 있습니다. 그러나 이러한 변경의 다양 한 조합을 사용 하 여 처리 하기 위해 트리거를 사용 하 여 매우 혼란 스 러 될 수 있습니다. 지금까지 Visual State Manager는 시각적 상태 조합에서 발생 하는 혼란을 완화 하기 위해 Windows XAML 기반 환경에서 도입 되었습니다. VSM을 통해 시각적 상태 그룹 내의 시각적 상태는 항상 함께 사용할 수 없습니다. 언제 든 지 하나의 상태 각 그룹의 현재 상태가입니다.

## <a name="common-states"></a>공용 상태

시각적 상태 관리자를 사용 하면 뷰가 정상적 이거나 비활성화 되었거나 입력 포커스가 있는 경우 뷰의 시각적 모양을 변경할 수 있는 태그를 XAML 파일에 포함할 수 있습니다. 이러한 _상태를 공용 상태_라고 합니다.

예를 들어 페이지에 `Entry` 보기가 있고 다음과 같은 방법으로 `Entry`의 시각적 모양을 변경 하려는 경우를 가정해 보겠습니다.

- `Entry` 사용 하지 않도록 설정 된 경우 `Entry`은 분홍색 배경 이어야 합니다.
- `Entry`은 일반적으로 황록색을 포함 해야 합니다.
- 입력 포커스가 있는 경우 `Entry`은 표준 높이의 두 배까지 확장 됩니다.

VSM 태그 개별 보기에 연결 하거나 여러 보기에 적용 되는 경우 스타일에서 정의할 수 있습니다. 다음 두 섹션에는 이러한 접근 방식을 설명합니다.

### <a name="vsm-markup-on-a-view"></a>뷰에 VSM 태그

`Entry` 뷰에 VSM 태그를 연결 하려면 먼저 `Entry`를 시작 태그와 끝 태그로 분리 합니다.

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

[`VisualStateGroups`](xref:Xamarin.Forms.VisualStateManager.VisualStateGroupsProperty) 은 [`VisualStateManager`](xref:Xamarin.Forms.VisualStateManager) 클래스로 정의 된 바인딩 가능한 속성입니다. 연결 된 바인딩 가능한 속성에 대 한 자세한 내용은 연결 된 [속성](~/xamarin-forms/xaml/attached-properties.md)문서를 참조 하세요. `VisualStateGroups` 속성이 `Entry` 개체에 연결 되는 방법입니다.

`VisualStateGroups` 속성은 [`VisualStateGroup`](xref:Xamarin.Forms.VisualStateGroup) 개체 컬렉션인 [`VisualStateGroupList`](xref:Xamarin.Forms.VisualStateGroupList)형식입니다. `VisualStateManager.VisualStateGroups` 태그 내에서 포함 하려는 각 시각적 상태 그룹의 `VisualStateGroup` 태그 쌍을 삽입 합니다.

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

`VisualStateGroup` 태그에는 그룹의 이름을 나타내는 `x:Name` 특성이 있습니다. `VisualStateGroup` 클래스는 대신 사용할 수 있는 `Name` 속성을 정의 합니다.

```xaml
<VisualStateGroup Name="CommonStates">
```

`x:Name` 또는 `Name`를 사용할 수 있지만 동일한 요소에서 둘 다를 사용할 수는 없습니다.

`VisualStateGroup` 클래스는 [`VisualState`](xref:Xamarin.Forms.VisualState) 개체 컬렉션인 [`States`](xref:Xamarin.Forms.VisualStateGroup.States)라는 속성을 정의 합니다. `States`은 `VisualStateGroups`의 _콘텐츠 속성_ 이므로 `VisualStateGroup` 태그 사이에 `VisualState` 태그를 직접 포함할 수 있습니다. 콘텐츠 속성은 [필수 XAML 구문](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md#content-properties)문서에 설명 되어 있습니다.

다음 단계 그룹의 모든 시각적 상태에 대 한 태그 쌍을 포함 하는 것입니다. `x:Name` 또는 `Name`를 사용 하 여 식별할 수도 있습니다.

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

각 `Setter` 태그는 해당 상태가 current 인 경우 특정 속성의 값을 나타냅니다. `Setter` 개체에서 참조 하는 모든 속성은 바인딩 가능한 속성에 의해 지원 되어야 합니다.

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

두 번째 `Entry`에는 `Trigger` 컬렉션의 일부로 `DataTrigger`도 있습니다. 이로 인해 세 번째 `Entry`에 입력 될 때까지 `Entry` 사용 하지 않도록 설정 됩니다. 다음은 iOS, Android 및 유니버설 Windows 플랫폼 (UWP)에서 실행 되는 시작 페이지가입니다.

[![보기의 VSM: 사용 안 함](vsm-images/VsmOnViewDisabled.png "뷰에서 VSM-사용 안 함")](vsm-images/VsmOnViewDisabled-Large.png#lightbox)

현재 시각적 상태는 "사용 안 함" 이므로 두 번째 `Entry`의 배경은 iOS 및 Android 화면에서 분홍색입니다. `Entry`의 UWP 구현에서는 `Entry` 사용 하지 않도록 설정 된 경우 배경색을 설정할 수 없습니다.

세 번째 `Entry`에 일부 텍스트를 입력 하면 두 번째 `Entry` "정상" 상태로 전환 되 고 배경은 이제 황록색입니다.

[![보기의 VSM: 보통](vsm-images/VsmOnViewNormal.png "보기-보통의 VSM")](vsm-images/VsmOnViewNormal-Large.png#lightbox)

두 번째 `Entry`을 터치 하면 입력 포커스를 가져옵니다. "Focused" 상태로 전환 하 고 높이 두 배가 확장 됩니다.

[![뷰에서 VSM: 포커스 있음](vsm-images/VsmOnViewFocused.png "뷰 중심의 VSM")](vsm-images/VsmOnViewFocused-Large.png#lightbox)

`Entry`는 입력 포커스를 가져오는 경우 라임 배경을 유지 하지 않습니다. Visual State Manager 시각적 상태 간의 전환으로 이전 상태에 따라 설정 된 속성에 설정 되지 않은 합니다. 시각적 상태 상호 배타적인 것을 염두에 두십시오. "Normal" 상태는 `Entry` 사용 하도록 설정 된 것만을 의미 하지는 않습니다. `Entry` 사용 하도록 설정 되어 있고 입력 포커스가 없음을 의미 합니다.

`Entry`에서 "포커스가 있는" 상태에 라임 배경을 표시 하려면 해당 시각적 상태에 다른 `Setter`를 추가 합니다.

```xaml
<VisualState x:Name="Focused">
    <VisualState.Setters>
        <Setter Property="FontSize" Value="36" />
        <Setter Property="BackgroundColor" Value="Lime" />
    </VisualState.Setters>
</VisualState>
```

이러한 `Setter` 개체가 제대로 작동 하려면 `VisualStateGroup` 해당 그룹의 모든 상태에 대 한 `VisualState` 개체를 포함 해야 합니다. `Setter` 개체가 없는 시각적 상태가 표시 되는 경우 빈 태그로 포함 합니다.

```xaml
<VisualState x:Name="Normal" />
```

### <a name="visual-state-manager-markup-in-a-style"></a>시각적 상태 관리자 태그를 스타일

둘 이상의 뷰 간에 동일한 Visual State Manager 태그를 공유 하는 데 필요한 경우가 있습니다. 이 경우 `Style` 정의에 태그를 추가 하는 것이 좋습니다.

다음은 뷰 페이지의 **VSM** 에서 `Entry` 요소에 대 한 기존 암시적 `Style`입니다.

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
</Style>
```

`VisualStateManager.VisualStateGroups` 연결 된 바인딩 가능한 속성에 대 한 `Setter` 태그를 추가 합니다.

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">

    </Setter>
</Style>
```

`Setter`에 대 한 content 속성이 `Value`되므로 `Value` 속성의 값을 해당 태그 내에서 직접 지정할 수 있습니다. 해당 속성은 `VisualStateGroupList`형식입니다.

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

VSM 태그 나머지는 이전과 동일 합니다.

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

## <a name="visual-states-in-xamarinforms"></a>Xamarin.ios의 시각적 상태

다음 표에서는 Xamarin.ios에 정의 된 시각적 상태를 보여 줍니다.

| 클래스 | 상태 | 추가 정보 |
| ----- | ------ | ---------------- |
| `Button` | `Pressed` | [단추 시각적 상태](~/xamarin-forms/user-interface/button.md#button-visual-states) |
| `CarouselView` | `DefaultItem`, `CurrentItem`, `PreviousItem`, `NextItem` | [CarouselView 시각적 상태](~/xamarin-forms/user-interface/carouselview/interaction.md#define-visual-states) |
| `ImageButton` | `Pressed` | [ImageButton 시각적 상태](~/xamarin-forms/user-interface/imagebutton.md#imagebutton-visual-states) |
| `VisualElement` | `Normal`, `Disabled`, `Focused`, `Selected` | [공용 상태](#common-states) |

이러한 각 상태는 `CommonStates`이라는 시각적 상태 그룹을 통해 액세스할 수 있습니다.

또한 `CollectionView`는 `Selected` 상태를 구현 합니다. 자세한 내용은 [선택한 항목 색 변경](~/xamarin-forms/user-interface/collectionview/selection.md#change-selected-item-color)을 참조 하세요.

## <a name="set-state-on-multiple-elements"></a>여러 요소에 대 한 상태 설정

이전 예제에서 시각적 상태는 단일 요소에 연결 되 고 작동 했습니다. 그러나 단일 요소에 연결 되지만 동일한 범위 내의 다른 요소에 대 한 속성을 설정 하는 시각적 상태를 만들 수도 있습니다. 이렇게 하면 상태가 작동 하는 각 요소에서 시각적 상태를 반복 하지 않아도 됩니다.

[`Setter`](xref:Xamarin.Forms.Setter) 형식에는 시각적 상태의 `Setter`가 조작할 대상 요소를 나타내는 `string`형식의 `TargetName` 속성이 있습니다. `TargetName` 속성이 정의 된 경우 `Setter`는 `TargetName`에 정의 된 요소의 `Property`을 `Value`로 설정 합니다.

```xaml
<Setter TargetName="label"
        Property="Label.TextColor"
        Value="Red" />
```

이 예에서는 `label` 라는 `Label` `TextColor` 속성이 `Red`로 설정 됩니다. `TargetName` 속성을 설정 하는 경우 `Property`에서 속성의 전체 경로를 지정 해야 합니다. 따라서 `Label`에서 `TextColor` 속성을 설정 하기 위해 `Property` `Label.TextColor`로 지정 됩니다.

> [!NOTE]
> `Setter` 개체에서 참조 하는 모든 속성은 바인딩 가능한 속성에 의해 지원 되어야 합니다.

**[VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** 샘플의 **VSM with Setter TargetName** 페이지는 단일 시각적 상태 그룹에서 여러 요소에 대 한 상태를 설정 하는 방법을 보여 줍니다. XAML 파일은 `Label` 요소, `Entry`및 `Button`를 포함 하는 `StackLayout` 구성 됩니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmSetterTargetNamePage"
             Title="VSM with Setter TargetName">
    <StackLayout Margin="10">
        <Label Text="What is the capital of France?" />
        <Entry x:Name="entry"
               Placeholder="Enter answer" />
        <Button Text="Reveal answer">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup x:Name="CommonStates">
                    <VisualState x:Name="Normal" />
                    <VisualState x:Name="Pressed">
                        <VisualState.Setters>
                            <Setter Property="Scale"
                                    Value="0.8" />
                            <Setter TargetName="entry"
                                    Property="Entry.Text"
                                    Value="Paris" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </Button>
    </StackLayout>
</ContentPage>
```

VSM 태그가 `StackLayout`에 연결 되어 있습니다. "Normal" 및 "눌린" 이라는 두 개의 상호 배타적인 상태와 `VisualState` 태그가 포함 된 각 상태를 포함 합니다.

`Button`를 누르지 않으면 "Normal" 상태가 활성 이며 질문에 대 한 응답을 입력할 수 있습니다.

[![VSM Setter TargetName: 표준 상태](vsm-images/VsmSetterTargetNameNormal.png "VSM setter targetname-보통")](vsm-images/VsmSetterTargetNameNormal-Large.png#lightbox)

`Button` 누르면 "누름" 상태가 활성화 됩니다.

[![VSM Setter TargetName: 눌린 상태](vsm-images/VsmSetterTargetNamePressed.png "VSM setter targetname-누름")](vsm-images/VsmSetterTargetNamePressed-Large.png#lightbox)

"눌러진" `VisualState` `Button`를 누르면 `Scale` 속성이 기본값 1에서 0.8로 변경 됩니다. 또한 `entry` 라는 `Entry` `Text` 속성이 파리로 설정 됩니다. 따라서 `Button`을 누를 때 약간 더 작은 재조정, `Entry` 파리를 표시 하는 결과가 발생 합니다. 그런 다음 `Button` 릴리스되는 경우 기본값은 1로 재조정 `Entry`는 이전에 입력 한 텍스트를 표시 합니다.

> [!IMPORTANT]
> 속성 경로는 현재 `TargetName` 속성을 지정 하는 `Setter` 요소에서 지원 되지 않습니다.

## <a name="define-your-own-visual-states"></a>사용자 고유의 시각적 상태 정의

`VisualElement`에서 파생 되는 모든 클래스는 일반적인 상태 "Normal", "집중 된" 및 "Disabled"를 지원 합니다. 또한 `CollectionView` 클래스는 "선택" 상태를 지원 합니다. 내부적으로 [`VisualElement`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/VisualElement.cs) 클래스는 사용 하거나 사용 하지 않을 때, 또는 포커스가 나 포커스가 없는를 검색 하 고, 정적 [`VisualStateManager.GoToState`](xref:Xamarin.Forms.VisualStateManager.GoToState(Xamarin.Forms.VisualElement,System.String)) 메서드를 호출 합니다.

```csharp
VisualStateManager.GoToState(this, "Focused");
```

이는 `VisualElement` 클래스에서 찾을 수 있는 유일한 시각적 상태 관리자 코드입니다. `VisualElement`에서 파생 되는 모든 클래스를 기반으로 하는 모든 개체에 대해 `GoToState`가 호출 되기 때문에 모든 `VisualElement` 개체에서 Visual State Manager를 사용 하 여 이러한 변경 내용에 응답할 수 있습니다.

흥미롭게도 시각적 상태 그룹 "CommonStates"의 이름은 `VisualElement`에서 명시적으로 참조 되지 않습니다. 그룹 이름을 Visual State Manager에 대 한 API의 일부가 아닙니다. 앞에서 설명한 두 가지 샘플 프로그램 중 하나를 다른 값으로 "CommonStates" 그룹의 이름을 변경할 수 있습니다 및 프로그램이 계속 작동 합니다. 그룹 이름은 해당 그룹의 상태에 대해 개괄적으로 설명 하기만 합니다. 시각적 상태 그룹에는 상호 배타적인은 암시적으로 인식 됩니다: 상태 및 하나의 상태는 언제 든 지 현재 합니다.

사용자 고유의 시각적 상태를 구현 하려면 코드에서 `VisualStateManager.GoToState`를 호출 해야 합니다. 대부분의 page 클래스의 코드 숨김 파일에서이 호출을 해야 합니다.

**[VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** 샘플의 **VSM 유효성 검사** 페이지에서는 입력 유효성 검사와 관련 된 연결에서 Visual State Manager를 사용 하는 방법을 보여 줍니다. XAML 파일은 두 개의 `Label` 요소, `Entry`및 `Button`를 포함 하는 `StackLayout` 구성 됩니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmValidationPage"
             Title="VSM Validation">
    <StackLayout x:Name="stackLayout"
                 Padding="10, 10">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ValidityStates">
                    <VisualState Name="Valid">
                        <VisualState.Setters>
                            <Setter TargetName="helpLabel"
                                    Property="Label.TextColor"
                                    Value="Transparent" />
                            <Setter TargetName="entry"
                                    Property="Entry.BackgroundColor"
                                    Value="Lime" />
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState Name="Invalid">
                        <VisualState.Setters>
                            <Setter TargetName="entry"
                                    Property="Entry.BackgroundColor"
                                    Value="Pink" />
                            <Setter TargetName="submitButton"
                                    Property="Button.IsEnabled"
                                    Value="False" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        <Label Text="Enter a U.S. phone number:"
               FontSize="Large" />
        <Entry x:Name="entry"
               Placeholder="555-555-5555"
               FontSize="Large"
               Margin="30, 0, 0, 0"
               TextChanged="OnTextChanged" />
        <Label x:Name="helpLabel"
               Text="Phone number must be of the form 555-555-5555, and not begin with a 0 or 1" />
        <Button x:Name="submitButton"
                Text="Submit"
                FontSize="Large"
                Margin="0, 20"
                VerticalOptions="Center"
                HorizontalOptions="Center" />
    </StackLayout>
</ContentPage>
```

VSM 태그는 `StackLayout` (`stackLayout`명명 됨)에 연결 됩니다. "Valid" 및 "Invalid" 라는 두 개의 상호 배타적인 상태는 `VisualState` 태그가 포함 된 각 상태에 있습니다.

`Entry`에 올바른 전화 번호가 포함 되어 있지 않으면 현재 상태는 "유효 하지 않음" 이므로 `Entry` 분홍색 배경, 두 번째 `Label` 표시 되 고 `Button` 사용 하지 않도록 설정 됩니다.

[![VSM 유효성 검사: 잘못 된 상태](vsm-images/VsmValidationInvalid.png "VSM 유효성 검사-유효 하지 않음")](vsm-images/VsmValidationInvalid-Large.png#lightbox)

올바른 전화 번호를 입력 한 다음 현재 상태가 "Valid"입니다. `Entry`는 라임 배경을 가져오고, 두 번째 `Label` 사라지면 `Button` 사용 하도록 설정 됩니다.

[![VSM 유효성 검사: 유효한 상태](vsm-images/VsmValidationValid.png "VSM 유효성 검사-유효")](vsm-images/VsmValidationValid-Large.png#lightbox)

코드 숨김이 `Entry`에서 `TextChanged` 이벤트를 처리 하는 것을 담당 합니다. 입력된 문자열은 올바른 경우 확인 하려면 정규식을 사용 하는 처리기. `GoToState` 라는 코드 파일의 메서드는 `stackLayout`에 대 한 정적 `VisualStateManager.GoToState` 메서드를 호출 합니다.

```csharp
public partial class VsmValidationPage : ContentPage
{
    public VsmValidationPage()
    {
        InitializeComponent();

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
        VisualStateManager.GoToState(stackLayout, visualState);
    }
}
```

또한 생성자에서 `GoToState` 메서드를 호출 하 여 상태를 초기화 합니다. 현재 상태는 항상 있을 것입니다. 이지만 곳은 코드에 있는 시각적 상태 그룹의 이름에 대 한 모든 참조 "ValidationStates" 위해 명확성으로는 XAML에서 참조 되지만 있습니다.

코드를 표시 하는 파일은 시각적 상태를 정의 하는 페이지에서 개체를 사용 하 고이 개체에 대 한 `VisualStateManager.GoToState`를 호출 하기만 하면 됩니다. 이는 두 시각적 상태 모두 페이지에서 여러 개체를 대상으로 하기 때문입니다.

코드 숨김이 시각적 상태를 정의 하는 페이지에서 개체를 참조 해야 하는 경우 코드 숨김이 아닌 파일에서이 개체 및 다른 개체에 직접 액세스할 수 없는 이유는 무엇입니까? 이 분명 할 수 없습니다. 그러나 VSM을 사용 하 여의 장점은 visual 요소를 제어할 수 있습니다 완전히 XAML 한곳에 유지 하는 모든 UI 디자인에서에서 다른 상태로 반응 합니다. 이렇게 하면 코드 숨김 파일에서 직접 시각적 요소에 액세스 하 여 설정을 시각적 모양이 없습니다.

## <a name="visual-state-triggers"></a>시각적 상태 트리거

시각적 상태는 [`VisualState`](xref:Xamarin.Forms.VisualState) 적용 해야 하는 조건을 정의 하는 특수 한 트리거 그룹인 상태 트리거를 지원 합니다.

상태 트리거는 [`VisualState`](xref:Xamarin.Forms.VisualState)의 [`StateTriggers`](xref:Xamarin.Forms.VisualState.StateTriggers) 컬렉션에 추가 됩니다. 이 컬렉션은 단일 상태 트리거 또는 여러 상태 트리거를 포함할 수 있습니다. 컬렉션의 상태 트리거가 활성 상태인 경우 [`VisualState`](xref:Xamarin.Forms.VisualState) 적용 됩니다.

상태 트리거를 사용 하 여 시각적 상태를 제어 하는 경우 Xamarin.ios는 다음과 같은 우선 순위 규칙을 사용 하 여 활성화 되는 트리거 (및 해당 [`VisualState`](xref:Xamarin.Forms.VisualState))를 결정 합니다.

1. [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase)에서 파생 되는 모든 트리거입니다.
1. [`MinWindowWidth`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowWidth) 조건이 충족 되어 [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger) 활성화 되었습니다.
1. [`MinWindowHeight`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowHeight) 조건이 충족 되어 [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger) 활성화 되었습니다.

여러 트리거가 동시에 활성화 된 경우 (예: 두 개의 사용자 지정 트리거) 태그에 선언 된 첫 번째 트리거가 우선적으로 적용 됩니다.

상태 트리거에 대 한 자세한 내용은 [상태 트리거](~/xamarin-forms/app-fundamentals/triggers.md#state-triggers)를 참조 하세요.

<a name="adaptive-layout" />

## <a name="use-the-visual-state-manager-for-adaptive-layout"></a>적응 레이아웃에 대 한 시각적 상태 관리자 사용

여러 다양 한 크기 및 가로 세로 비율을 가정 하는 Xamarin.Forms 휴대폰에서 실행 하는 응용 프로그램을 세로 또는 가로 세로 비율을 고정 및 바탕 화면에서 실행 되는 Xamarin.Forms 프로그램에서 일반적으로 볼 수 있습니다를 조정할 수 있습니다. 잘 설계 된 응용 프로그램을 이러한 다양 한 페이지 또는 창 폼 팩터 용 다르게 해당 콘텐츠를 표시할 수 있습니다.

이 기술을 _적응형 레이아웃_이라고 합니다. 적응 레이아웃에는 전적으로 프로그램의 시각적 개체 포함 되므로 Visual State Manager 이상적인 응용 프로그램.

간단한 예제에는 응용 프로그램의 콘텐츠에 영향을 주는 단추 모음이 표시 하는 응용 프로그램입니다. 세로 모드에서는 이러한 단추는 페이지 맨 위에 있는 가로 행에 표시 될 수 있습니다.:

[![VSM 적응 레이아웃: 세로](vsm-images/VsmAdaptiveLayoutPortrait.png "VSM 적응 레이아웃-세로")](vsm-images/VsmAdaptiveLayoutPortrait-Large.png#lightbox)

가로 모드로 단추의 배열 수 한 쪽으로 이동 되며 열에 표시:

[![VSM 적응 레이아웃: 가로](vsm-images/VsmAdaptiveLayoutLandscape.png "VSM 적응 레이아웃-가로")](vsm-images/VsmAdaptiveLayoutLandscape-Large.png#lightbox)

위쪽에서 아래쪽, 프로그램은 유니버설 Windows 플랫폼, Android 및 iOS에서 실행 됩니다.

[VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos) 샘플의 **VSM 적응 레이아웃** 페이지는 이름이 "세로" 및 "가로" 인 두 개의 시각적 상태를 포함 하는 "OrientationStates" 라는 그룹을 정의 합니다. (다소 복잡 한 방법은 수에 따라 여러 다른 페이지 또는 창 너비입니다.)

VSM 태그 XAML 파일에 네 곳에서 발생합니다. `mainStack` 라는 `StackLayout`에는 `Image` 요소인 메뉴와 콘텐츠가 모두 포함 됩니다. 이 `StackLayout` 세로 모드의 세로 방향이 가로 모드의 가로 방향 이어야 합니다.

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

이름이 `menuScroll` 인 내부 `ScrollView` 및 `StackLayout` `menuStack`는 단추의 메뉴를 구현 합니다. 이러한 레이아웃의 방향은 `mainStack`의 반대입니다. 메뉴를 세로 모드로 가로 및 세로 가로 모드로 있어야 합니다.

VSM 태그의 네 번째 섹션을 자체 단추에 대 한 암시적 스타일의 경우 이 태그는 가로 및 세로 방향과 관련 된 `VerticalOptions`, `HorizontalOptions`및 `Margin` 속성을 설정 합니다.

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

`SizeChanged` 처리기는 두 `StackLayout` 및 `ScrollView` 요소에 대해 `VisualStateManager.GoToState`를 호출한 다음 `menuStack`의 자식을 반복 하 여 `VisualStateManager.GoToState` 요소에 대 한 `Button`를 호출 합니다.

처럼 코드 숨김 파일을 XAML 파일에서 요소의 속성을 설정 하 여 직접 더 방향 변경을 처리할 수 있지만 Visual State Manager 보다 구조적인 방법이 확실 하 게 보일 수 있습니다. 모든 시각적 개체를 쉽게 검사할 수 있는, XAML 파일에 유지 되는 유지 관리 하 고 수정 합니다.

## <a name="visual-state-manager-with-xamarinuniversity"></a>Visual State Manager Xamarin.University 사용 하 여

> [!VIDEO https://youtube.com/embed/qhUHbVP5mIQ]

**Xamarin.ios 3.0 Visual State Manager 비디오**

## <a name="related-links"></a>관련 링크

- [VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)
- [상태 트리거](~/xamarin-forms/app-fundamentals/triggers.md#state-triggers)
