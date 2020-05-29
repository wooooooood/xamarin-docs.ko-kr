---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8872c6748ba778a2622d82803d580c781bd282cd
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139635"
---
# <a name="adding-data-to-a-pickers-items-collection"></a>선택기 항목 컬렉션에 데이터 추가

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-pickerdemo)

_선택 뷰는 데이터 목록에서 텍스트 항목을 선택 하는 컨트롤입니다. 이 문서에서는 항목 컬렉션에 데이터를 추가 하 여 데이터를 선택 하 고 사용자가 항목을 선택할 때 응답 하는 방법을 설명 합니다._

## <a name="populating-a-picker-with-data"></a>데이터를 사용 하 여 선택기 채우기

2.3.4 이전에 Xamarin.Forms 는 데이터를 사용 하 여를 채우는 프로세스는 [`Picker`](xref:Xamarin.Forms.Picker) 형식의 읽기 전용 컬렉션에 표시할 데이터를 추가 하는 [`Items`](xref:Xamarin.Forms.Picker.Items) 것 이었습니다 `IList<string>` . 컬렉션의 각 항목은 형식 이어야 합니다 `string` . 항목 목록을 사용 하 여 속성을 초기화 하 여 XAML에 항목을 추가할 수 있습니다 `Items` `x:String` .

```xaml
<Picker Title="Select a monkey"
        TitleColor="Red">
  <Picker.Items>
    <x:String>Baboon</x:String>
    <x:String>Capuchin Monkey</x:String>
    <x:String>Blue Monkey</x:String>
    <x:String>Squirrel Monkey</x:String>
    <x:String>Golden Lion Tamarin</x:String>
    <x:String>Howler Monkey</x:String>
    <x:String>Japanese Macaque</x:String>
  </Picker.Items>
</Picker>
```

해당 c # 코드는 다음과 같습니다.

```csharp
var picker = new Picker { Title = "Select a monkey", TitleColor = Color.Red };
picker.Items.Add("Baboon");
picker.Items.Add("Capuchin Monkey");
picker.Items.Add("Blue Monkey");
picker.Items.Add("Squirrel Monkey");
picker.Items.Add("Golden Lion Tamarin");
picker.Items.Add("Howler Monkey");
picker.Items.Add("Japanese Macaque");
```

메서드를 사용 하 여 데이터를 추가 하는 것 외에 `Items.Add` 도 메서드를 사용 하 여 컬렉션에 데이터를 삽입할 수 있습니다 `Items.Insert` .

## <a name="responding-to-item-selection"></a>항목 선택에 응답

는 한 [`Picker`](xref:Xamarin.Forms.Picker) 번에 하나의 항목을 선택할 수 있도록 지원 합니다. 사용자가 항목을 선택 하면 [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) 이벤트가 발생 하 고 [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) 속성이 목록에서 선택한 항목의 인덱스를 나타내는 정수로 업데이트 됩니다. `SelectedIndex`속성은 사용자가 선택한 항목을 나타내는 0부터 시작 하는 숫자입니다. 항목을 선택 하지 않은 경우 (가 `Picker` 처음 만들어지고 초기화 되는 경우 `SelectedIndex` )는-1입니다.

> [!NOTE]
> 의 항목 선택 동작은 [`Picker`](xref:Xamarin.Forms.Picker) 플랫폼별로 iOS에서 사용자 지정할 수 있습니다. 자세한 내용은 [선택기 항목 선택 제어](~/xamarin-forms/platform/ios/picker-selection.md)를 참조 하세요.

다음 코드 예제에서는 `OnPickerSelectedIndexChanged` 이벤트 발생 시 실행 되는 이벤트 처리기 메서드를 보여 줍니다 [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) .

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs e)
{
  var picker = (Picker)sender;
  int selectedIndex = picker.SelectedIndex;

  if (selectedIndex != -1)
  {
    monkeyNameLabel.Text = picker.Items[selectedIndex];
  }
}
```

이 메서드는 [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) 속성 값을 가져오고 값을 사용 하 여 컬렉션에서 선택한 항목을 검색 합니다 [`Items`](xref:Xamarin.Forms.Picker.Items) . 컬렉션의 각 항목 `Items` 은 이므로 `string` [`Label`](xref:Xamarin.Forms.Label) 캐스트를 요구 하지 않고에 의해 표시 될 수 있습니다.

> [!NOTE]
> [`Picker`](xref:Xamarin.Forms.Picker)속성을 설정 하 여 특정 항목을 표시 하도록를 초기화할 수 있습니다 [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) . 그러나 컬렉션을 `SelectedIndex` 초기화 한 후에는 속성을 설정 해야 합니다 [`Items`](xref:Xamarin.Forms.Picker.Items) .

## <a name="related-links"></a>관련 링크

- [선택 데모 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-pickerdemo)
- [선택기](xref:Xamarin.Forms.Picker)
