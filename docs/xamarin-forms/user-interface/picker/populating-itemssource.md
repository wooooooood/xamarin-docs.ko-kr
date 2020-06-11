---
제목: "선택의 System.windows.controls.itemscontrol.itemssource 속성 설정" 설명: "선택 보기는 데이터 목록에서 텍스트 항목을 선택 하는 컨트롤입니다. 이 문서에서는 System.windows.controls.itemscontrol.itemssource 속성을 설정 하 고 사용자가 항목 선택 항목에 응답 하는 방법을 사용 하 여 선택기를 데이터로 채우는 방법을 설명 합니다. "
assetid: 8ECF390C-9DB2-4441-B9A3-101AE7E5AEC5: xamarin-forms author: davidbritch: dabritch:: 02/26/2019-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="setting-a-pickers-itemssource-property"></a>선택기의 ItemsSource 속성 설정

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-monkeyapppicker)

_선택 뷰는 데이터 목록에서 텍스트 항목을 선택 하는 컨트롤입니다. 이 문서에서는 System.windows.controls.itemscontrol.itemssource 속성을 설정 하 여 데이터를 사용 하 여 선택기를 채우는 방법과 사용자가 항목 선택 항목에 응답 하는 방법을 설명 합니다._

Xamarin.Forms2.3.4는 [`Picker`](xref:Xamarin.Forms.Picker) [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) 속성을 설정 하 고 속성에서 선택한 항목을 검색 하 여 데이터를 데이터로 채우는 기능을 추가 하 여 뷰를 향상 시켰습니다 [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) . 또한 속성을로 설정 하 여 선택한 항목에 대 한 텍스트 색을 변경할 수 있습니다 [`TextColor`](xref:Xamarin.Forms.Picker.TextColor) [`Color`](xref:Xamarin.Forms.Color) .

## <a name="populating-a-picker-with-data"></a>데이터를 사용 하 여 선택기 채우기

[`Picker`](xref:Xamarin.Forms.Picker)속성을 컬렉션으로 설정 하 여를 데이터로 채울 수 있습니다 [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) `IList` . 컬렉션의 각 항목은 또는 형식 이어야 합니다 `object` . 항목의 배열에서 속성을 초기화 하 여 XAML에 항목을 추가할 수 있습니다 `ItemsSource` .

```xaml
<Picker x:Name="picker"
        Title="Select a monkey"
        TitleColor="Red">
  <Picker.ItemsSource>
    <x:Array Type="{x:Type x:String}">
      <x:String>Baboon</x:String>
      <x:String>Capuchin Monkey</x:String>
      <x:String>Blue Monkey</x:String>
      <x:String>Squirrel Monkey</x:String>
      <x:String>Golden Lion Tamarin</x:String>
      <x:String>Howler Monkey</x:String>
      <x:String>Japanese Macaque</x:String>
    </x:Array>
  </Picker.ItemsSource>
</Picker>
```

> [!NOTE]
> `x:Array` 요소는 배열의 항목 유형을 나타내는 `Type` 특성이 필요합니다.

해당 c # 코드는 다음과 같습니다.

```csharp
var monkeyList = new List<string>();
monkeyList.Add("Baboon");
monkeyList.Add("Capuchin Monkey");
monkeyList.Add("Blue Monkey");
monkeyList.Add("Squirrel Monkey");
monkeyList.Add("Golden Lion Tamarin");
monkeyList.Add("Howler Monkey");
monkeyList.Add("Japanese Macaque");

var picker = new Picker { Title = "Select a monkey", TitleColor = Color.Red };
picker.ItemsSource = monkeyList;
```

## <a name="responding-to-item-selection"></a>항목 선택에 응답

는 한 [`Picker`](xref:Xamarin.Forms.Picker) 번에 하나의 항목을 선택할 수 있도록 지원 합니다. 사용자가 항목을 선택 하면 [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) 이벤트가 발생 하 [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) 고,이 속성이 목록에서 선택한 항목의 인덱스를 나타내는 정수로 업데이트 되며, [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) 속성이 선택 된 항목을 나타내는로 업데이트 됩니다 `object` . [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex)속성은 사용자가 선택한 항목을 나타내는 0부터 시작 하는 숫자입니다. 항목을 선택 하지 않은 경우 (가 [`Picker`](xref:Xamarin.Forms.Picker) 처음 만들어지고 초기화 되는 경우 `SelectedIndex` )는-1입니다.

> [!NOTE]
> 의 항목 선택 동작은 [`Picker`](xref:Xamarin.Forms.Picker) 플랫폼별로 iOS에서 사용자 지정할 수 있습니다. 자세한 내용은 [선택기 항목 선택 제어](~/xamarin-forms/platform/ios/picker-selection.md)를 참조 하세요.

다음 코드 예제에서는 [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) XAML의에서 속성 값을 검색 하는 방법을 보여 줍니다 [`Picker`](xref:Xamarin.Forms.Picker) .

```xaml
<Label Text="{Binding Source={x:Reference picker}, Path=SelectedItem}" />
```

해당 c # 코드는 다음과 같습니다.

```csharp
var monkeyNameLabel = new Label();
monkeyNameLabel.SetBinding(Label.TextProperty, new Binding("SelectedItem", source: picker));
```

또한 이벤트가 발생 하는 경우 이벤트 처리기를 실행할 수 있습니다 [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) .

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs e)
{
  var picker = (Picker)sender;
  int selectedIndex = picker.SelectedIndex;

  if (selectedIndex != -1)
  {
    monkeyNameLabel.Text = (string)picker.ItemsSource[selectedIndex];
  }
}
```

이 메서드는 [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) 속성 값을 가져오고 값을 사용 하 여 컬렉션에서 선택한 항목을 검색 합니다 [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) . 이는 속성에서 선택한 항목을 검색 하는 것과 기능적으로 동일 [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) 합니다. 컬렉션의 각 항목 `ItemsSource` 은 형식 이므로 `object` 표시를 위해로 캐스팅 해야 합니다 `string` .

> [!NOTE]
> [`Picker`](xref:Xamarin.Forms.Picker)또는 속성을 설정 하 여 특정 항목을 표시 하도록를 초기화할 수 있습니다 [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) . 그러나 컬렉션을 초기화 한 후에는 이러한 속성을 설정 해야 합니다 [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) .

## <a name="populating-a-picker-with-data-using-data-binding"></a>데이터 바인딩을 사용 하 여 선택기를 데이터로 채우기

[`Picker`](xref:Xamarin.Forms.Picker)데이터 바인딩을 사용 하 여 속성을 컬렉션에 바인딩하면 데이터를 데이터로 채울 수도 있습니다 [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) `IList` . XAML에서이 작업은 태그 확장을 사용 하 여 구현 됩니다 [`Binding`](xref:Xamarin.Forms.Xaml.BindingExtension) .

```xaml
<Picker Title="Select a monkey"
        TitleColor="Red"
        ItemsSource="{Binding Monkeys}"
        ItemDisplayBinding="{Binding Name}" />
```

해당 c # 코드는 다음과 같습니다.

```csharp
var picker = new Picker { Title = "Select a monkey", TitleColor = Color.Red };
picker.SetBinding(Picker.ItemsSourceProperty, "Monkeys");
picker.ItemDisplayBinding = new Binding("Name");
```

[`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource)속성 데이터는 컬렉션을 반환 하는 `Monkeys` 연결 된 뷰 모델의 속성에 바인딩됩니다 `IList<Monkey>` . 다음 코드 예제에서는 `Monkey` 네 개의 속성을 포함 하는 클래스를 보여 줍니다.

```csharp
public class Monkey
{
  public string Name { get; set; }
  public string Location { get; set; }
  public string Details { get; set; }
  public string ImageUrl { get; set; }
}
```

개체 목록에 바인딩하는 경우 [`Picker`](xref:Xamarin.Forms.Picker) 각 개체에서 표시할 속성을에 지시 해야 합니다. 이렇게 [`ItemDisplayBinding`](xref:Xamarin.Forms.Picker.ItemDisplayBinding) 하려면 속성을 각 개체의 필수 속성으로 설정 합니다. 위의 코드 예제에서는 `Picker` 각 속성 값을 표시 하도록 설정 됩니다 `Monkey.Name` .

### <a name="responding-to-item-selection"></a>항목 선택에 응답

데이터 바인딩을 사용 하 여 개체를 변경할 때 개체를 속성 값으로 설정할 수 있습니다 [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) .

```xaml
<Picker Title="Select a monkey"
        TitleColor="Red"
        ItemsSource="{Binding Monkeys}"
        ItemDisplayBinding="{Binding Name}"
        SelectedItem="{Binding SelectedMonkey}" />
<Label Text="{Binding SelectedMonkey.Name}" ... />
<Label Text="{Binding SelectedMonkey.Location}" ... />
<Image Source="{Binding SelectedMonkey.ImageUrl}" ... />
<Label Text="{Binding SelectedMonkey.Details}" ... />
```

해당 c # 코드는 다음과 같습니다.

```csharp
var picker = new Picker { Title = "Select a monkey", TitleColor = Color.Red };
picker.SetBinding(Picker.ItemsSourceProperty, "Monkeys");
picker.SetBinding(Picker.SelectedItemProperty, "SelectedMonkey");
picker.ItemDisplayBinding = new Binding("Name");

var nameLabel = new Label { ... };
nameLabel.SetBinding(Label.TextProperty, "SelectedMonkey.Name");

var locationLabel = new Label { ... };
locationLabel.SetBinding(Label.TextProperty, "SelectedMonkey.Location");

var image = new Image { ... };
image.SetBinding(Image.SourceProperty, "SelectedMonkey.ImageUrl");

var detailsLabel = new Label();
detailsLabel.SetBinding(Label.TextProperty, "SelectedMonkey.Details");
```

[`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem)속성 데이터는 `SelectedMonkey` 형식의 연결 된 뷰 모델의 속성에 바인딩됩니다 `Monkey` . 따라서 사용자가에서 항목을 선택 하면 [`Picker`](xref:Xamarin.Forms.Picker) `SelectedMonkey` 속성이 선택한 개체로 설정 됩니다 `Monkey` . `SelectedMonkey`개체 데이터는 및 뷰에 의해 사용자 인터페이스에 표시 [`Label`](xref:Xamarin.Forms.Label) 됩니다 [`Image`](xref:Xamarin.Forms.Image) .

![](populating-itemssource-images/monkeys.png "Picker Item Selection")

> [!NOTE]
> [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem)및 [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) 속성은 모두 기본적으로 양방향 바인딩을 지원 합니다.

## <a name="related-links"></a>관련 링크

- [선택 데모 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-pickerdemo)
- [원숭이 앱 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-monkeyapppicker)
- [바인딩 가능한 선택기 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablepicker)
- [선택 API](xref:Xamarin.Forms.Picker)
