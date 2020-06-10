---
제목: " Xamarin.Forms 시각적 상태 관리자" 설명: "Visual State manager를 사용 하 여 코드에서 설정 된 시각적 상태에 따라 XAML 요소를 변경 합니다."
assetid: 17296F14-640D-484B-A24C-A4E9B7013E4F: xamarin-forms ms. custom: xamu-video author: davidbritch: dabritch: ms. date: 02/21/2020 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-visual-state-manager"></a>Xamarin.Forms 시각적 개체 상태 관리자

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)

_Visual State Manager를 사용 하 여 코드에서 설정 된 시각적 상태에 따라 XAML 요소를 변경 합니다._

VSM (시각적 상태 관리자)은 코드에서 사용자 인터페이스에 대 한 시각적 변경을 수행 하는 구조화 된 방법을 제공 합니다. 대부분의 경우 응용 프로그램의 사용자 인터페이스는 XAML로 정의 되며이 XAML에는 시각적 상태 관리자가 사용자 인터페이스의 시각적 개체에 미치는 영향을 설명 하는 태그가 포함 됩니다.

VSM은 _시각적 상태의_개념을 소개 합니다. Xamarin.Forms과 같은 뷰는 `Button` &mdash; 사용 하지 않도록 설정 되었는지, 눌러져 있는지 또는 입력 포커스를가지고 있는지에 따라 기본 상태에 따라 여러 가지 시각적 모양을 가질 수 있습니다. 이는 단추의 상태입니다.

시각적 상태는 _시각적 상태 그룹_에서 수집 됩니다. 시각적 상태 그룹 내의 모든 시각적 상태는 함께 사용할 수 없습니다. 시각적 상태 그룹과 시각적 상태 그룹 모두 단순 텍스트 문자열로 식별 됩니다.

Xamarin.Forms시각적 상태 관리자는 다음과 같은 시각적 상태를 사용 하 여 "CommonStates" 이라는 하나의 시각적 상태 그룹을 정의 합니다.

- "Normal"
- 해제
- 편지
- 선택

이 시각적 상태 그룹은 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 및에 대 한 기본 클래스인에서 파생 되는 모든 클래스에 대해 지원 됩니다 [`View`](xref:Xamarin.Forms.View) [`Page`](xref:Xamarin.Forms.Page) .

이 문서에서 설명 하는 대로 고유한 시각적 상태 그룹 및 시각적 상태를 정의할 수도 있습니다.

> [!NOTE]
> Xamarin.Forms[트리거를 사용 하](~/xamarin-forms/app-fundamentals/triggers.md) 는 개발자에 게는 뷰의 속성 변경 또는 이벤트 발생을 기반으로 사용자 인터페이스의 시각적 개체가 변경 될 수도 있음을 알 수 있습니다. 그러나 트리거를 사용 하 여 이러한 변경 사항의 다양 한 조합을 처리 하는 것은 매우 복잡할 수 있습니다. 지금 까지는 시각적 상태 관리자가 시각적 상태를 조합 하 여 발생 하는 혼동을 줄이기 위해 Windows XAML 기반 환경에서 도입 되었습니다. VSM을 사용할 경우 시각적 상태 그룹 내의 시각적 상태는 항상 함께 사용할 수 없습니다. 언제 든 지 각 그룹의 한 상태만 현재 상태입니다.

## <a name="common-states"></a>공용 상태

시각적 상태 관리자를 사용 하면 뷰가 정상적 이거나 비활성화 되었거나 입력 포커스가 있는 경우 뷰의 시각적 모양을 변경할 수 있는 태그를 XAML 파일에 포함할 수 있습니다. 이러한 _상태를 공용 상태_라고 합니다.

예를 들어 `Entry` 페이지에 보기가 있고의 시각적 모양이 다음과 같은 방식으로 변경 되는 경우를 가정해 보겠습니다 `Entry` .

- 을 `Entry` 사용 하지 않도록 설정한 경우에는가 분홍색 배경을 포함 해야 합니다 `Entry` .
- 에는 `Entry` 일반적으로 황록색이 있어야 합니다.
- `Entry`입력 포커스가 있는 경우이 표준 높이의 두 배까지 확장 됩니다.

VSM 태그를 개별 뷰에 연결 하거나 여러 뷰에 적용 되는 경우 스타일에서 정의할 수 있습니다. 다음 두 섹션에서는 이러한 접근 방식을 설명 합니다.

### <a name="vsm-markup-on-a-view"></a>뷰의 VSM 마크업

VSM 태그를 뷰에 연결 하려면 `Entry` 먼저를 `Entry` 시작 태그와 끝 태그로 분리 합니다.

```xaml
<Entry FontSize="18">

</Entry>
```

상태 중 하나에서 속성을 사용 하 여의 `FontSize` 텍스트 크기를 두 배로 지정 하므로 명시적 글꼴 크기가 지정 됩니다 `Entry` .

그런 다음 태그 사이에 태그를 삽입 합니다 `VisualStateManager.VisualStateGroups` .

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>

    </VisualStateManager.VisualStateGroups>
</Entry>
```

[`VisualStateGroups`](xref:Xamarin.Forms.VisualStateManager.VisualStateGroupsProperty)는 클래스에 의해 정의 된 바인딩 가능한 속성입니다 [`VisualStateManager`](xref:Xamarin.Forms.VisualStateManager) . 연결 된 바인딩 가능한 속성에 대 한 자세한 내용은 연결 된 [속성](~/xamarin-forms/xaml/attached-properties.md)문서를 참조 하세요. `VisualStateGroups`속성이 개체에 연결 되는 방식입니다 `Entry` .

`VisualStateGroups`속성은 [`VisualStateGroupList`](xref:Xamarin.Forms.VisualStateGroupList) 개체의 컬렉션인 형식입니다 [`VisualStateGroup`](xref:Xamarin.Forms.VisualStateGroup) . 태그 내에서 `VisualStateManager.VisualStateGroups` `VisualStateGroup` 포함 하려는 각 시각적 상태 그룹의 태그 쌍을 삽입 합니다.

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

태그에는 `VisualStateGroup` `x:Name` 그룹의 이름을 나타내는 특성이 있습니다. `VisualStateGroup`클래스는 `Name` 대신 사용할 수 있는 속성을 정의 합니다.

```xaml
<VisualStateGroup Name="CommonStates">
```

`x:Name`같은 요소에서 또는 중 하나만 사용할 수 있습니다 `Name` .

`VisualStateGroup`클래스는 [`States`](xref:Xamarin.Forms.VisualStateGroup.States) 개체의 컬렉션인 라는 속성을 정의 합니다 [`VisualState`](xref:Xamarin.Forms.VisualState) . `States`태그를 _content property_ `VisualStateGroups` 태그 사이에 직접 포함할 수 있도록의 콘텐츠 속성입니다 `VisualState` `VisualStateGroup` . 콘텐츠 속성은 [필수 XAML 구문](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md#content-properties)문서에 설명 되어 있습니다.

다음 단계는 해당 그룹의 모든 시각적 상태에 대 한 쌍의 태그를 포함 하는 것입니다. 이러한 항목은 또는을 사용 하 여 식별할 수도 있습니다 `x:Name` `Name` .

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

`VisualState`개체 컬렉션인 라는 속성을 정의 [`Setters`](xref:Xamarin.Forms.VisualState.Setters) [`Setter`](xref:Xamarin.Forms.Setter) 합니다. 이러한 개체는 개체에서 사용 하는 것과 동일한 `Setter` 개체 [`Style`](xref:Xamarin.Forms.Style) 입니다.

`Setters`는의 content 속성이 _아니므로_ `VisualState` 속성의 속성 요소 태그를 포함 해야 합니다 `Setters` .

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

이제 `Setter` 각 태그 쌍 사이에 하나 이상의 개체를 삽입할 수 있습니다 `Setters` . 다음은 `Setter` 앞에서 설명한 시각적 상태를 정의 하는 개체입니다.

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

각 `Setter` 태그는 해당 상태가 current 인 경우 특정 속성의 값을 나타냅니다. 개체에서 참조 하는 모든 속성은 `Setter` 바인딩 가능한 속성에 의해 지원 되어야 합니다.

이와 비슷한 마크업은 **[VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** 샘플 프로그램의 **보기 페이지에** 대 한 기본 설정입니다. 이 페이지에는 세 개의 보기가 포함 되어 `Entry` 있지만 두 번째 보기에는 VSM 태그가 연결 되어 있습니다.

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

두 번째는의 `Entry` 컬렉션에도 포함 되어 있습니다 `DataTrigger` `Trigger` . 이렇게 하면 `Entry` 세 번째 항목을 입력할 때까지가 비활성화 됩니다 `Entry` . 시작 시 iOS, Android 및 유니버설 Windows 플랫폼 (UWP)에서 실행 되는 페이지는 다음과 같습니다.

[![보기의 VSM: 사용 안 함](vsm-images/VsmOnViewDisabled.png "뷰에서 VSM-사용 안 함")](vsm-images/VsmOnViewDisabled-Large.png#lightbox)

현재 시각적 상태는 "사용 안 함" 이므로 두 번째의 배경은 `Entry` iOS 및 Android 화면에서 분홍색입니다. 의 UWP 구현에서는를 `Entry` 사용할 수 없을 때 배경색을 설정할 수 없습니다 `Entry` .

세 번째 텍스트를 세 번째에 입력 하면 `Entry` 두 번째는 `Entry` "정상" 상태로 전환 되 고 배경은 이제 황록색입니다.

[![보기의 VSM: 보통](vsm-images/VsmOnViewNormal.png "보기-보통의 VSM")](vsm-images/VsmOnViewNormal-Large.png#lightbox)

두 번째를 터치 하는 경우 `Entry` 입력 포커스를 가져옵니다. "집중 된" 상태로 전환 되 고 높이를 두 배로 확장 합니다.

[![뷰에서 VSM: 포커스 있음](vsm-images/VsmOnViewFocused.png "뷰 중심의 VSM")](vsm-images/VsmOnViewFocused-Large.png#lightbox)

에서 `Entry` 입력 포커스를 가져오는 경우는 라임 배경을 유지 하지 않습니다. 시각적 상태 관리자가 시각적 상태를 전환 하면 이전 상태로 설정 된 속성은 설정 되지 않습니다. 시각적 상태는 함께 사용할 수 없다는 점에 유의 하세요. "Normal" 상태는만 사용 하도록 설정 된 것을 의미 하지는 않습니다 `Entry` . 이는 `Entry` 가 사용 하도록 설정 되어 있고 입력 포커스가 없음을 의미 합니다.

가 `Entry` "포커스가 있는" 상태에서 라임 배경을 갖도록 하려면 `Setter` 해당 시각적 상태에 다른를 추가 합니다.

```xaml
<VisualState x:Name="Focused">
    <VisualState.Setters>
        <Setter Property="FontSize" Value="36" />
        <Setter Property="BackgroundColor" Value="Lime" />
    </VisualState.Setters>
</VisualState>
```

이러한 `Setter` 개체가 제대로 작동 하려면에 `VisualStateGroup` `VisualState` 해당 그룹의 모든 상태에 대 한 개체가 포함 되어 있어야 합니다. 개체를 포함 하지 않는 시각적 상태가 있는 경우 `Setter` 빈 태그로 포함 합니다.

```xaml
<VisualState x:Name="Normal" />
```

### <a name="visual-state-manager-markup-in-a-style"></a>스타일의 시각적 상태 관리자 태그

두 개 이상의 보기 간에 동일한 시각적 상태 관리자 태그를 공유 해야 하는 경우가 종종 있습니다. 이 경우 정의에 태그를 추가할 수 있습니다 `Style` .

다음은 `Style` 뷰 페이지의 VSM에서 요소에 대 한 기존 암시적입니다 `Entry` . **VSM On View**

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
</Style>
```

`Setter`연결 된 바인딩 가능한 속성에 대 한 태그를 추가 합니다 `VisualStateManager.VisualStateGroups` .

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">

    </Setter>
</Style>
```

의 content 속성은 `Setter` 이므로 `Value` 속성의 값을 `Value` 해당 태그 내에서 직접 지정할 수 있습니다. 해당 속성은 `VisualStateGroupList` 다음과 같습니다.

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

이러한 태그 내에 다음 개체 중 하나를 포함할 수 있습니다 `VisualStateGroup` .

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

이제 `Entry` 이 페이지의 모든 보기가 시각적 상태와 동일한 방식으로 응답 합니다. 또한 "포커스가 있는" 상태에는 `Setter` 입력 포커스가 있는 경우에도 각 배경을 제공 하는 두 번째가 포함 됩니다 `Entry` .

[![스타일의 VSM](vsm-images/VsmInStyle.png "스타일의 VSM")](vsm-images/VsmInStyle-Large.png#lightbox)

## <a name="visual-states-in-xamarinforms"></a>시각적 상태Xamarin.Forms

다음 표에서는에 정의 된 시각적 상태를 보여 줍니다 Xamarin.Forms .

| 클래스 | 상태 | 추가 정보 |
| ----- | ------ | ---------------- |
| `Button` | `Pressed` | [단추 시각적 상태](~/xamarin-forms/user-interface/button.md#button-visual-states) |
| `CheckBox` | `IsChecked` | [CheckBox 시각적 상태](~/xamarin-forms/user-interface/checkbox.md#checkbox-visual-states) |
| `CarouselView` | `DefaultItem`, `CurrentItem`, `PreviousItem`, `NextItem` | [CarouselView 시각적 상태](~/xamarin-forms/user-interface/carouselview/interaction.md#define-visual-states) |
| `ImageButton` | `Pressed` | [ImageButton 시각적 상태](~/xamarin-forms/user-interface/imagebutton.md#imagebutton-visual-states) |
| `RadioButton` | `IsChecked` | [RadioButton 시각적 상태](~/xamarin-forms/user-interface/radiobutton.md#radiobutton-visual-states) |
| `VisualElement` | `Normal`, `Disabled`, `Focused`, `Selected` | [공용 상태](#common-states) |

이러한 각 상태는 이라는 시각적 상태 그룹을 통해 액세스할 수 있습니다 `CommonStates` .

또한은 상태를 `CollectionView` 구현 합니다 `Selected` . 자세한 내용은 [선택한 항목 색 변경](~/xamarin-forms/user-interface/collectionview/selection.md#change-selected-item-color)을 참조 하세요.

## <a name="set-state-on-multiple-elements"></a>여러 요소에 대 한 상태 설정

이전 예제에서 시각적 상태는 단일 요소에 연결 되 고 작동 했습니다. 그러나 단일 요소에 연결 되지만 동일한 범위 내의 다른 요소에 대 한 속성을 설정 하는 시각적 상태를 만들 수도 있습니다. 이렇게 하면 상태가 작동 하는 각 요소에서 시각적 상태를 반복 하지 않아도 됩니다.

[`Setter`](xref:Xamarin.Forms.Setter)이 형식에는 `TargetName` `string` 시각적 상태의가 조작할 대상 요소를 나타내는 형식의 속성이 있습니다 `Setter` . `TargetName`속성이 정의 된 경우는에 정의 된 요소의를 `Setter` 로 설정 합니다 `Property` `TargetName` `Value` .

```xaml
<Setter TargetName="label"
        Property="Label.TextColor"
        Value="Red" />
```

이 예제에서 명명 된 `Label` 의 `label` 속성은 `TextColor` 로 설정 됩니다 `Red` . 속성을 설정할 때 `TargetName` 에는 속성의 전체 경로를 지정 해야 합니다 `Property` . 따라서에 대 한 속성을 설정 하려면 `TextColor` `Label` `Property` 는로 지정 됩니다 `Label.TextColor` .

> [!NOTE]
> 개체에서 참조 하는 모든 속성은 `Setter` 바인딩 가능한 속성에 의해 지원 되어야 합니다.

**[VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** 샘플의 **VSM with Setter TargetName** 페이지는 단일 시각적 상태 그룹에서 여러 요소에 대 한 상태를 설정 하는 방법을 보여 줍니다. XAML 파일은 요소가 포함 된, 및를 포함 하는로 구성 됩니다 `StackLayout` `Label` `Entry` `Button` .

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

VSM 태그는에 연결 됩니다 `StackLayout` . "Normal" 및 "눌린" 이라는 두 개의 상호 배타적인 상태와 태그를 포함 하는 각 상태를 포함 `VisualState` 합니다.

"Normal" 상태는를 누르지 않은 경우 활성 상태이 `Button` 고 질문에 대 한 응답을 입력할 수 있습니다.

[![VSM Setter TargetName: 표준 상태](vsm-images/VsmSetterTargetNameNormal.png "VSM setter targetname-보통")](vsm-images/VsmSetterTargetNameNormal-Large.png#lightbox)

를 누르면 "누름" 상태가 활성화 됩니다 `Button` .

[![VSM Setter TargetName: 눌린 상태](vsm-images/VsmSetterTargetNamePressed.png "VSM setter targetname-누름")](vsm-images/VsmSetterTargetNamePressed-Large.png#lightbox)

"눌러진"는를 `VisualState` `Button` 누르면 해당 `Scale` 속성이 기본값 1에서 0.8로 변경 되도록 지정 합니다. 또한 명명 된의 `Entry` `entry` `Text` 속성은 파리로 설정 됩니다. 따라서를 `Button` 누를 때 재조정 약간 더 작아가 파리를 표시 하는 결과가 발생 합니다 `Entry` . 그런 다음이 `Button` 해제 되 면 재조정의 기본값은 1이 고,은 `Entry` 이전에 입력 한 텍스트를 표시 합니다.

> [!IMPORTANT]
> 속성 경로는 현재 속성을 `Setter` 지정 하는 요소에서 지원 되지 않습니다 `TargetName` .

## <a name="define-your-own-visual-states"></a>사용자 고유의 시각적 상태 정의

에서 파생 되는 모든 클래스 `VisualElement` 는 일반적인 상태 "Normal", "집중 된" 및 "Disabled"를 지원 합니다. 또한 `CollectionView` 클래스는 "선택" 상태를 지원 합니다. 내부적으로 [`VisualElement`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/VisualElement.cs) 클래스는 사용 하거나 사용 하지 않을 때, 또는 포커스가 나 포커스가 없는를 검색 하 고, static [ `VisualStateManager.GoToState` ] (f:)을 호출 합니다 Xamarin.Forms . R. GoToState ( Xamarin.Forms . VisualElement, System.string) 메서드:

```csharp
VisualStateManager.GoToState(this, "Focused");
```

이는 클래스에서 찾을 수 있는 유일한 시각적 상태 관리자 코드입니다 `VisualElement` . `GoToState`에서 파생 되는 모든 클래스를 기반으로 하는 모든 개체에 대해를 호출 하기 때문에 `VisualElement` `VisualElement` 이러한 변경 내용에 응답 하는 모든 개체에 대해 Visual State Manager를 사용할 수 있습니다.

흥미롭게도 시각적 상태 그룹 "CommonStates"의 이름은에서 명시적으로 참조 되지 않습니다 `VisualElement` . 그룹 이름은 시각적 상태 관리자에 대 한 API의 일부가 아닙니다. 지금까지 표시 된 두 샘플 프로그램 중 하나에서 그룹 이름을 "CommonStates"에서 다른 항목으로 변경할 수 있으며 프로그램은 계속 작동 합니다. 그룹 이름은 해당 그룹의 상태에 대 한 일반적인 설명 일 뿐입니다. 모든 그룹의 시각적 상태를 함께 사용할 수 없다는 것을 암시적으로 인식 합니다. 한 가지 상태 이며 언제 든 지 한 상태만 현재 상태입니다.

사용자 고유의 시각적 상태를 구현 하려면 코드에서를 호출 해야 `VisualStateManager.GoToState` 합니다. 가장 자주 사용 하는 페이지 클래스의 코드 파일에서이 호출을 수행 합니다.

**[VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** 샘플의 **VSM 유효성 검사** 페이지에서는 입력 유효성 검사와 관련 된 연결에서 Visual State Manager를 사용 하는 방법을 보여 줍니다. XAML 파일은 `StackLayout` 및 라는 두 개의 요소가 포함 된로 구성 됩니다 `Label` `Entry` `Button` .

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

VSM 태그는 `StackLayout` (이라고 명명 됨)에 연결 됩니다 `stackLayout` . "Valid" 및 "Invalid" 라는 두 개의 상호 배타적인 상태와 태그를 포함 하는 각 상태를 사용할 수 있습니다 `VisualState` .

에 `Entry` 올바른 전화 번호가 포함 되어 있지 않으면 현재 상태가 "유효 하지 않음"이 고가 `Entry` 분홍색 배경을 가지 며, 두 번째는 `Label` 표시 되 고,은 `Button` 사용 하지 않도록 설정 됩니다.

[![VSM 유효성 검사: 잘못 된 상태](vsm-images/VsmValidationInvalid.png "VSM 유효성 검사-유효 하지 않음")](vsm-images/VsmValidationInvalid-Large.png#lightbox)

유효한 전화 번호를 입력 하면 현재 상태가 "유효함"이 됩니다. 는 `Entry` 라임 배경을 가져오고, 두 번째는 `Label` 사라지고 `Button` 이제를 사용할 수 있습니다.

[![VSM 유효성 검사: 유효한 상태](vsm-images/VsmValidationValid.png "VSM 유효성 검사-유효")](vsm-images/VsmValidationValid-Large.png#lightbox)

코드 숨김이 파일은에서 이벤트를 처리 하는 일을 담당 합니다 `TextChanged` `Entry` . 처리기는 정규식을 사용 하 여 입력 문자열이 유효한 지 여부를 확인 합니다. 이라는 코드 숨김이 파일의 메서드는 `GoToState` `VisualStateManager.GoToState` 에 대 한 정적 메서드를 호출 합니다 `stackLayout` .

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

또한 `GoToState` 생성자에서 메서드를 호출 하 여 상태를 초기화 합니다. 항상 현재 상태가 있어야 합니다. 하지만 코드는 명확 하 게 하기 위해 XAML에서 "ValidationStates"로 참조 되지만 시각적 상태 그룹의 이름에 대 한 참조는 없습니다.

코드를 표시 하는 파일은 시각적 상태를 정의 하는 페이지의 개체를 고려 하 고 `VisualStateManager.GoToState` 이 개체에 대해를 호출 해야 합니다. 이는 두 시각적 상태 모두 페이지에서 여러 개체를 대상으로 하기 때문입니다.

코드 숨김이 시각적 상태를 정의 하는 페이지에서 개체를 참조 해야 하는 경우 코드 숨김이 아닌 파일에서이 개체 및 다른 개체에 직접 액세스할 수 없는 이유는 무엇입니까? 분명히 할 수 있습니다. 그러나 VSM을 사용 하면 시각적 요소가 XAML에서 완전히 다른 상태에 반응 하는 방식을 제어할 수 있습니다. 그러면 모든 UI 디자인이 한 위치에 유지 됩니다. 이렇게 하면 코드 숨김으로 시각적 요소에 직접 액세스 하 여 시각적 효과를 설정 하지 않습니다.

## <a name="visual-state-triggers"></a>시각적 상태 트리거

시각적 상태는를 적용 해야 하는 조건을 정의 하는 특수 한 트리거 그룹인 상태 트리거를 지원 [`VisualState`](xref:Xamarin.Forms.VisualState) 합니다.

상태 트리거는 [`VisualState`](xref:Xamarin.Forms.VisualState)의 [`StateTriggers`](xref:Xamarin.Forms.VisualState.StateTriggers) 컬렉션에 추가됩니다. 이 컬렉션은 단일 상태 트리거 또는 여러 상태 트리거를 포함할 수 있습니다. 컬렉션의 상태 트리거가 활성 상태인 경우 [`VisualState`](xref:Xamarin.Forms.VisualState)가 적용됩니다.

상태 트리거를 사용하여 시각적 개체 상태를 제어하는 경우 Xamarin.Forms는 다음과 같은 우선 순위 규칙을 사용하여 활성화될 트리거(및 해당 [`VisualState`](xref:Xamarin.Forms.VisualState))를 결정합니다.

1. [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase)에서 파생되는 모든 트리거.
1. [`MinWindowWidth`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowWidth) 조건을 충족하여 활성화되는 [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger).
1. [`MinWindowHeight`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowHeight) 조건을 충족하여 활성화되는 [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger).

여러 트리거가 동시에 활성화된 경우(예: 두 개의 사용자 지정 트리거) 태그에 선언된 첫 번째 트리거가 우선적으로 적용됩니다.

상태 트리거에 대한 자세한 내용은 [상태 트리거](~/xamarin-forms/app-fundamentals/triggers.md#state-triggers)를 참조하세요.

## <a name="use-the-visual-state-manager-for-adaptive-layout"></a>적응 레이아웃에 대 한 시각적 상태 관리자 사용

Xamarin.Forms휴대폰에서 실행 되는 응용 프로그램은 일반적으로 세로 또는 가로 가로 세로 비율로 볼 수 있으며, Xamarin.Forms 데스크톱에서 실행 되는 프로그램의 크기를 조정 하 여 다양 한 크기와 가로 세로 비율을 가정 합니다. 잘 디자인 된 응용 프로그램은 이러한 다양 한 페이지 또는 창 폼 팩터를 위해 콘텐츠를 다르게 표시할 수 있습니다.

이 기술을 _적응형 레이아웃_이라고 합니다. 적응 레이아웃은 프로그램의 시각적 개체와만 관련 되므로 시각적 상태 관리자의 이상적인 응용 프로그램입니다.

간단한 예제는 응용 프로그램의 콘텐츠에 영향을 주는 작은 단추 컬렉션을 표시 하는 응용 프로그램입니다. 세로 모드에서 이러한 단추는 페이지 위쪽의 가로 행에 표시 될 수 있습니다.

[![VSM 적응 레이아웃: 세로](vsm-images/VsmAdaptiveLayoutPortrait.png "VSM 적응 레이아웃-세로")](vsm-images/VsmAdaptiveLayoutPortrait-Large.png#lightbox)

가로 모드에서는 단추 배열이 한 쪽으로 이동 하 여 열에 표시 될 수 있습니다.

[![VSM 적응 레이아웃: 가로](vsm-images/VsmAdaptiveLayoutLandscape.png "VSM 적응 레이아웃-가로")](vsm-images/VsmAdaptiveLayoutLandscape-Large.png#lightbox)

위에서 아래로 프로그램은 유니버설 Windows 플랫폼, Android 및 iOS에서 실행 됩니다.

[VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos) 샘플의 **VSM 적응 레이아웃** 페이지는 이름이 "세로" 및 "가로" 인 두 개의 시각적 상태를 포함 하는 "OrientationStates" 라는 그룹을 정의 합니다. 더 복잡 한 방법은 여러 페이지 또는 창 너비를 기반으로 할 수 있습니다.

VSM 태그는 XAML 파일의 네 위치에서 발생 합니다. `StackLayout`이라는은 `mainStack` 요소인 메뉴와 콘텐츠를 모두 포함 합니다 `Image` . `StackLayout`세로 모드의 세로 방향이 가로 모드의 가로 방향 이어야 합니다.

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

이라는 내부 `ScrollView` `menuScroll` 와 라는는 `StackLayout` `menuStack` 단추의 메뉴를 구현 합니다. 이러한 레이아웃의 방향은와 반대입니다 `mainStack` . 세로 모드에서는 가로 모드이 고 세로 모드에서는 세로 방향 이어야 합니다.

VSM 태그의 네 번째 섹션은 단추 자체의 암시적 스타일에 있습니다. 이 태그 `VerticalOptions` 는 가로 `HorizontalOptions` 및 세로 `Margin` 방향에 특정 한, 및 속성을 설정 합니다.

코드 숨김이 파일은 `BindingContext` 의 속성을 설정 `menuStack` 하 여 `Button` 명령을 구현 하 고 페이지의 이벤트에 처리기를 연결 합니다 `SizeChanged` .

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

`SizeChanged`처리기는 `VisualStateManager.GoToState` 두 개의 및 요소에 대해를 호출한 `StackLayout` `ScrollView` 다음의 자식을 반복 하 여 `menuStack` 요소를 호출 합니다 `VisualStateManager.GoToState` `Button` .

XAML 파일의 요소 속성을 설정 하 여 코드 숨김이 방향 변경을 더 직접적으로 처리할 수 있는 것 처럼 보일 수 있지만 시각적 상태 관리자는 매우 구조화 된 방법입니다. 모든 시각적 개체는 XAML 파일에 저장 되므로 더 쉽게 검사 하 고 유지 관리 하 고 수정할 수 있습니다.

## <a name="visual-state-manager-with-xamarinuniversity"></a>Xamarin을 사용 하는 시각적 상태 관리자

> [!VIDEO https://youtube.com/embed/qhUHbVP5mIQ]

**Xamarin.Forms3.0 Visual State Manager 비디오**

## <a name="related-links"></a>관련 링크

- [VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)
- [상태 트리거](~/xamarin-forms/app-fundamentals/triggers.md#state-triggers)
