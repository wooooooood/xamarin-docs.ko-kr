---
제목: " Xamarin.Forms progressbar" 설명: " Xamarin.Forms progressbar는 유동 속성을 기반으로 채워지는 가로 막대로 진행률을 시각적으로 나타내는 컨트롤입니다."
assetId: C2F85FED-797C-466B-A0FD-E73CFB79B267: xamarin-forms author: profexorgeek: jusjohns:: 07/09/2019-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="xamarinforms-progressbar"></a>Xamarin.FormsProgressBar
[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/)

Xamarin.Forms [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) 컨트롤은 값으로 표시 되는 백분율로 채워지는 가로 막대로 진행률을 시각적으로 나타냅니다 `float` . `ProgressBar`클래스는에서 상속 [`View`](xref:Xamarin.Forms.View) 됩니다.

다음 스크린샷은 iOS 및 Android의 `ProgressBar`를 보여줍니다.

![IOS 및 Android의 ProgressBar 스크린샷](progressbar-images/progressbars-default.png "IOS 및 Android의 ProgressBar")

`ProgressBar`컨트롤은 다음 두 가지 속성을 정의 합니다.

* [`Progress`](xref:Xamarin.Forms.ProgressBar.Progress)`float`현재 진행률을 0에서 1 사이의 값으로 나타내는 값입니다. `Progress`0 보다 작은 값은 0으로 고정, 1 보다 큰 값은 1로 고정 됩니다.
* [`ProgressColor`](xref:Xamarin.Forms.ProgressBar.ProgressColor)는 `Color` 현재 진행률을 나타내는 내부 막대 색에 영향을 주는입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉,의 스타일을 지정 `ProgressBar` 하 고 데이터 바인딩의 대상이 될 수 있습니다.

`ProgressBar`또한 컨트롤은 `ProgressTo` 현재 값에서 지정 된 값으로 막대에 애니메이션 효과를 주는 메서드를 정의 합니다. 자세한 내용은 [ProgressBar에 애니메이션 효과 주기](#animate-a-progressbar)를 참조 하세요.

> [!NOTE]
> 는 `ProgressBar` Tab 키를 사용 하 여 컨트롤을 선택할 때 건너뛰도록 사용자 조작을 허용 하지 않습니다.

## <a name="create-a-progressbar"></a>ProgressBar 만들기

는 `ProgressBar` XAML에서 인스턴스화될 수 있습니다. 해당 `Progress` 속성은 색이 지정 된 안쪽 막대의 채우기 비율을 결정 합니다. 기본 `Progress` 속성 값은 0입니다. 다음 예제에서는 `ProgressBar` 선택적 속성 집합을 사용 하 여 XAML에서를 인스턴스화하는 방법을 보여 줍니다 `Progress` .

```xaml
<ProgressBar Progress="0.5" />
```

`ProgressBar`코드에서를 만들 수도 있습니다.

```csharp
ProgressBar progressBar = new ProgressBar { Progress = 0.5f };
```

> [!WARNING]
> ,, 등의 제한 없는 가로 레이아웃 옵션 `Center` 을 `Start` `End` 와 함께 사용 하지 마십시오 `ProgressBar` . UWP에서는 `ProgressBar` 너비가 0 인 막대로 축소 됩니다. 을 레이아웃에 넣을 때의 기본값을 유지 `HorizontalOptions` `Fill` 하 고 너비를 사용 하지 않습니다 `Auto` `ProgressBar` `Grid` .

## <a name="progressbar-appearance-properties"></a>ProgressBar 모양 속성

속성은 `ProgressColor` `Progress` 속성이 0 보다 클 때 내부 막대 색을 정의 합니다. 다음 예제에서는 속성 집합을 사용 하 여 XAML에서를 인스턴스화하는 방법을 보여 줍니다 `ProgressBar` `ProgressColor` .

```xaml
<ProgressBar ProgressColor="Orange" />
```

`ProgressColor`코드에서을 만들 때도 속성을 설정할 수 있습니다 `ProgressBar` .

```csharp
ProgressBar progressBar = new ProgressBar { ProgressColor = Color.Orange };
```

다음 스크린샷에는 `ProgressBar` `ProgressColor` IOS 및 Android에서 속성이로 설정 된가 나와 `Color.Orange` 있습니다.

![IOS 및 Android에서 스타일이 지정 된 ProgressBar의 스크린샷](progressbar-images/progressbars-styled.png "IOS 및 Android의 스타일이 지정 된 ProgressBar")

## <a name="animate-a-progressbar"></a>ProgressBar에 애니메이션 효과 주기

`ProgressTo`메서드는 `ProgressBar` 현재 `Progress` 값에서 시간 경과에 따라 제공 된 값으로에 애니메이션 효과를 적용 합니다. 메서드는 `float` 진행률 값, `uint` 기간 (밀리초), `Easing` 열거형 값을 허용 하 고을 반환 `Task<bool>` 합니다. 다음 코드에서는에 애니메이션 효과를 주는 방법을 보여 줍니다 `ProgressBar` .

```csharp
// animate to 75% progress over 500 milliseconds with linear easing
await progressBar.ProgressTo(0.75, 500, Easing.Linear);
```

열거형에 대 한 자세한 내용은 `Easing` [의 Xamarin.Forms 감속/가속 함수 ](~/xamarin-forms/user-interface/animation/easing.md)를 참조 하세요.

## <a name="related-links"></a>관련 링크

* [ProgressBar 데모](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/)
