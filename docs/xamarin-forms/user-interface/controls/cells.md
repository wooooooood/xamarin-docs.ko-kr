---
title: Xamarin.Forms Cells
description: "Xamarin.Forms 셀 Listview 및 TableViews에 추가할 수 있습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 77DA0C89-35D6-4C09-A072-3ADE53FD56CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 0f8886546004702adbdbca7d991c67d5700e453e
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/21/2018
---
# <a name="xamarinforms-cells"></a>Xamarin.Forms Cells

_Xamarin.Forms 셀 Listview 및 TableViews에 추가할 수 있습니다._

A *셀* 는 테이블에 있는 항목에 사용 되는 특수 요소가 고 목록의 각 항목을 렌더링 해야 하는 방법에 대해 설명 합니다. [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) 클래스에서 파생 [ `Element` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/), 올 [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/) 에서도 파생 됩니다. 셀이 자체 시각적 요소입니다. 호스팅하려는 시각적 요소를 만들기 위한 템플릿입니다. 

`Cell` 제품에만 사용 되는 [ `ListView` ](views.md#listView) 및 [ `TableView` ](views.md#tableView) 컨트롤입니다. 사용 및 셀 사용자 지정 하는 방법에 자세한 참조는 [ `ListView` ](~/xamarin-forms/user-interface/listview/index.md) 및 [ `TableView` ](~/xamarin-forms/user-interface/tableview.md) 설명서입니다.

## <a name="cells"></a>셀

Xamarin.Forms에는 다음 셀 유형을 지원합니다.

<a name="textCell" />

### <a name="textcell"></a>TextCell

|     |     |
| --- | --- |
| A [ `TextCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell) 하나 또는 두 개의 텍스트 문자열을 표시 합니다. 설정의 [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Text/) 속성 및 필요에 따라는 [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Detail/) 속성 일치 하는 텍스트 문자열입니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell) / [가이드](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell) | [![TextCell 예제](cells-images/TextCell.png "TextCell 예제")](cells-images/TextCell-Large.png#lightbox "TextCell 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TextCellDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TextCellDemoPage.xaml) |
|     |     |

### <a name="imagecell"></a>ImageCell

|     |     |
| --- | --- |
| [ `ImageCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell) 와 동일한 정보를 표시 [ `TextCell` ](#textCell) 포함으로 설정 하는 비트맵의 [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Source/) 속성입니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell) / [가이드](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell) | [![ImageCell 예제](cells-images/ImageCell.png "ImageCell 예제")](cells-images/ImageCell-Large.png#lightbox "ImageCell 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageCellDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageCellDemoPage.xaml) |
|     |     |

### <a name="switchcell"></a>SwitchCell

|     |     |
| --- | --- |
| [ `SwitchCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell) 사용 하 여 설정 하는 텍스트가 포함 된는 [ `Text`'](https://developer.xamarin.com/api/property/Xamarin.Forms.SwitchCellText/) 속성 및 및 설정/해제는 부울 값을 사용 하 여 처음 설정 하는 스위치 [ `On` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SwitchCell.On/) 속성입니다. 처리는 [ `OnChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.SwitchCell.OnChanged/) 때 알림을 받을 이벤트는 `On` 속성 변경 합니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell) / [가이드](~/xamarin-forms/user-interface/tableview.md#switchcell) | [![SwitchCell 예제](cells-images/SwitchCell.png "SwitchCell 예제")](cells-images/SwitchCell-Large.png#lightbox "SwitchCell 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchCellDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchCellDemoPage.xaml) |
|     |     |

### <a name="entrycell"></a>EntryCell

|     |     |
| --- | --- |
| [ `EntryCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell) 정의 [ `Label` ](https://developer.xamarin.com/api/property/Xamarin.Forms.EntryCell.Label/) 속성은 셀과 셀의 편집 가능한 텍스트 한 줄을 식별 하는 [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.EntryCell.Text/) 속성입니다. 처리는 [ `Completed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.EntryCell.Completed/) 이벤트는 사용자가 텍스트 입력을 완료 하는 경우 알림을 받을 수 있습니다.<br /><br />[API 설명서](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell) / [가이드](~/xamarin-forms/user-interface/tableview.md#entrycell) | [![EntryCell 예제](cells-images/EntryCell.png "EntryCell 예제")](cells-images/EntryCell-Large.png#lightbox "EntryCell 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryCellDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryCellDemoPage.xaml) |
|     |     |


## <a name="related-links"></a>관련 링크

- [Xamarin.Forms 소개](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms FormsGallery 샘플](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)
- [Xamarin.Forms 샘플](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API 설명서](https://developer.xamarin.com/api/root/Xamarin.Forms/)
