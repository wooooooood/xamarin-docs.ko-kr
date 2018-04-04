---
title: 선택 항목 컬렉션에 대 한 추가 데이터
description: 선택기 보기에는 데이터의 목록에서 텍스트 항목을 선택 하기 위한 컨트롤입니다. 이 문서에는 Items 컬렉션에 추가 하 여 선택기를 데이터로 채우는 방법 및 사용자가 항목 선택에 응답 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 3C840F64-A430-457D-A4B2-3D7AF46F9DBE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
ms.openlocfilehash: 63a72861895f79d2d0154297f88610ddb8bb8beb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="adding-data-to-a-pickers-items-collection"></a>선택 항목 컬렉션에 대 한 추가 데이터

_선택기 보기에는 데이터의 목록에서 텍스트 항목을 선택 하기 위한 컨트롤입니다. 이 문서에는 Items 컬렉션에 추가 하 여 선택기를 데이터로 채우는 방법 및 사용자가 항목 선택에 응답 하는 방법을 설명 합니다._

## <a name="populating-a-picker-with-data"></a>선택기를 데이터로 채우기

Xamarin.Forms 2.3.4 채우기 위한 프로세스 전에 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) 데이터로 읽기 전용으로 표시 된 데이터를 추가 했습니다. [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) 형식인 컬렉션`IList<string>`. 컬렉션의 각 항목 유형 이어야 `string`합니다. 초기화 하 여 XAML에서 항목을 추가할 수는 `Items` 속성 목록이 `x:String` 항목:

```xaml
<Picker Title="Select a monkey">
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

해당 하는 C# 코드는 다음과 같습니다.

```csharp
var picker = new Picker { Title = "Select a monkey" };
picker.Items.Add("Baboon");
picker.Items.Add("Capuchin Monkey");
picker.Items.Add("Blue Monkey");
picker.Items.Add("Squirrel Monkey");
picker.Items.Add("Golden Lion Tamarin");
picker.Items.Add("Howler Monkey");
picker.Items.Add("Japanese Macaque");
```

사용 하 여 데이터를 추가할 뿐만 아니라는 `Items.Add` 메서드, 데이터도 컬렉션에 사용 하 여 삽입할 수는 `Items.Insert` 메서드.

## <a name="responding-to-item-selection"></a>항목 선택에 응답

A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) 한 번에 하나의 항목을 선택할 수 있습니다. 사용자가 항목을 선택 하는 경우는 [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) 이벤트 발생 및 [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) 목록에서 선택한 항목의 인덱스를 나타내는 정수로 속성이 업데이트 됩니다. `SelectedIndex` 속성은 사용자가 선택한 항목을 나타내는 숫자 0부터 시작 합니다. 선택 된 항목이 있는 되는 경우 때는 `Picker` 를 처음 생성 및 초기화 `SelectedIndex` -1이 됩니다.

> [!NOTE]
> 항목의 선택 동작에는 [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) 는 플랫폼 특정 iOS에서 사용자 지정할 수 있습니다. 자세한 내용은 참조 [선택 항목 선택 제어](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode)합니다.

다음 코드 예제는 `OnPickerSelectedIndexChanged` 은 이벤트 처리기 메서드를 될 때 실행 되는 [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) 이벤트 발생:

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

이 메서드를 가져옵니다는 [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) 속성 값, 값을 사용 하 여에서 선택된 된 항목을 검색 하 고는 [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) 컬렉션입니다. 각 항목에 때문에 `Items` 컬렉션은 한 `string`,으로 표시 될 수는 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 캐스팅 하지 않고도 합니다.

> [!NOTE]
> A [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) 설정 하 여 특정 항목을 표시 하도록 초기화 된 [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) 속성입니다. 그러나는 `SelectedIndex` 초기화 한 다음 속성을 설정 해야는 [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) 컬렉션입니다.

## <a name="summary"></a>요약

[ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) 뷰는 데이터의 목록에서 텍스트 항목을 선택 하기 위한 컨트롤입니다. 이 문서에서는를 채우는 방법을 `Picker` 추가 하 여 데이터와는 [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) 컬렉션 및 사용자가 항목 선택에 응답 하는 방법입니다. 이 사용 하기 위한 프로세스는 `Picker` Xamarin.Forms 2.3.4 이전 합니다.


## <a name="related-links"></a>관련 링크

- [선택기 데모 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/)
- [선택기](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)
