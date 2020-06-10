---
제목: " Xamarin.Forms 선택" 설명: "선택 항목은 Xamarin.Forms 사용자가 항목을 선택할 수 있는 항목의 짧은 목록을 표시 합니다. 이 문서에서는 선택 클래스를 사용 하 여 데이터 목록에서 텍스트 항목을 선택 하는 방법을 설명 합니다. "
assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86: xamarin-forms author: davidbritch: dabritch:: 02/26/2019-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="xamarinforms-picker"></a>Xamarin.Forms선택

_선택 뷰는 데이터 목록에서 텍스트 항목을 선택 하는 컨트롤입니다._

는 Xamarin.Forms [`Picker`](xref:Xamarin.Forms.Picker) 사용자가 항목을 선택할 수 있는 짧은 항목 목록을 표시 합니다. `Picker`는 다음 속성을 정의합니다.

- [`Title`](xref:Xamarin.Forms.Picker.Title)형식의 `string` 이며 기본값은 `null` 입니다.
- `TitleColor`형식의 [`Color`](xref:Xamarin.Forms.Color) 텍스트를 표시 하는 데 사용 되는 색입니다 `Title` .
- [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource)형식으로, `IList` 표시할 항목의 소스 목록 `null` 입니다. 기본값은입니다.
- [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex)유형 `int` (선택한 항목의 인덱스) 이며 기본값은-1입니다.
- [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem)`object`선택 된 항목인 형식의 이며 기본값은 `null` 입니다.
- [`TextColor`](xref:Xamarin.Forms.Picker.TextColor)형식의 [`Color`](xref:Xamarin.Forms.Color) , 텍스트를 표시 하는 데 사용 되는 색입니다. 기본값은 [`Color.Default`](xref:Xamarin.Forms.Color.Default) 입니다.
- [`FontAttributes`](xref:Xamarin.Forms.Picker.FontAttributes)형식의 [`FontAttributes`](xref:Xamarin.Forms.FontAttributes) 이며 기본값은 [`FontAtributes.None`](xref:Xamarin.Forms.FontAttributes.None) 입니다.
- [`FontFamily`](xref:Xamarin.Forms.Picker.FontFamily)형식의 `string` 이며 기본값은 `null` 입니다.
- [`FontSize`](xref:Xamarin.Forms.Picker.FontSize)형식의 `double` 이며 기본값은-1.0입니다.
- `CharacterSpacing`형식의는에서 `double` 표시 하는 항목의 문자 간 간격입니다 `Picker` .

모든 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 스타일을 지정할 수 있으며 속성은 데이터 바인딩의 대상이 될 수 있습니다. [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex)및 [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) 속성의 기본 바인딩 모드는 이며,이는 [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) [MVVM (모델-뷰-ViewModel)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) 아키텍처를 사용 하는 응용 프로그램의 데이터 바인딩 대상이 될 수 있음을 의미 합니다. 글꼴 속성을 설정 하는 방법에 대 한 자세한 내용은 [글꼴](~/xamarin-forms/user-interface/text/fonts.md)을 참조 하십시오.

는 [`Picker`](xref:Xamarin.Forms.Picker) 처음 표시 될 때 데이터를 표시 하지 않습니다. 대신, 해당 속성의 값은 [`Title`](xref:Xamarin.Forms.Picker.Title) iOS 및 Android 플랫폼에서 자리 표시자로 표시 됩니다.

[![](images/picker-initial.png "Initial Picker Display")](images/picker-initial-large.png#lightbox "Initial Picker Display")

에서 [`Picker`](xref:Xamarin.Forms.Picker) 포커스를 얻는 경우 해당 데이터가 표시 되 고 사용자가 항목을 선택할 수 있습니다.

[![](images/picker-selection.png "Picker Selecting an Item")](images/picker-selection-large.png#lightbox "Picker Selecting an Item")

는 [`Picker`](xref:Xamarin.Forms.Picker) [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) 사용자가 항목을 선택할 때 이벤트를 발생 시킵니다. 선택 후 선택한 항목은에 표시 됩니다 `Picker` .

![](images/picker-after-selection.png "Picker after Selection")

에 데이터를 채우는 방법에는 두 가지가 있습니다 [`Picker`](xref:Xamarin.Forms.Picker) .

- [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource)표시할 데이터로 속성을 설정 합니다. 이것이 권장되는 방법입니다. 자세한 내용은 [선택기의 System.windows.controls.itemscontrol.itemssource 속성 설정](populating-itemssource.md)을 참조 하세요.
- 컬렉션에 표시할 데이터를 추가 하는 중 [`Items`](xref:Xamarin.Forms.Picker.Items) 입니다. 이 기법은를 데이터로 채우는 원래 프로세스 였습니다 [`Picker`](xref:Xamarin.Forms.Picker) . 자세한 내용은 [선택기의 항목 컬렉션에 데이터 추가](populating-items.md)를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [선택기](xref:Xamarin.Forms.Picker)
