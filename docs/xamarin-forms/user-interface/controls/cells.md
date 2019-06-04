---
title: Xamarin.Forms 셀
description: Xamarin.Forms 셀 TableViews Listview를 추가할 수 있습니다. 이 문서에서는 Xamarin.Forms에 포함 된 셀을 나열 합니다.
ms.prod: xamarin
ms.assetid: 77DA0C89-35D6-4C09-A072-3ADE53FD56CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: c4d73f131b8b20f17c5a3df13a3c4590f4ca926c
ms.sourcegitcommit: 4f8dc5298a95d591a59e97cdd347fd82858a1019
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/03/2019
ms.locfileid: "66469501"
---
# <a name="xamarinforms-cells"></a>Xamarin.Forms 셀

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)

_Xamarin.Forms 셀 TableViews Listview를 추가할 수 있습니다._

A *셀* 테이블의 항목에 사용 되는 특수 한 요소 이며 목록의 각 항목을 렌더링 해야 하는 방법에 대해 설명 합니다. 합니다 [ `Cell` ](xref:Xamarin.Forms.Cell) 클래스에서 파생 됩니다 [ `Element` ](xref:Xamarin.Forms.Element), 올 [ `VisualElement` ](xref:Xamarin.Forms.Element) 에서도 파생 됩니다. 셀이 있다는 시각적 요소입니다. 이 대신 시각적 요소를 만들기 위한 템플릿입니다.

`Cell` 단독으로 사용 됩니다 [ `ListView` ](views.md#listView) 하 고 [ `TableView` ](views.md#tableView) 컨트롤입니다. 를 사용 하 여 셀 사용자 지정 하는 방법을 알아보려면 참조를 [ `ListView` ](~/xamarin-forms/user-interface/listview/index.md) 하 고 [ `TableView` ](~/xamarin-forms/user-interface/tableview.md) 설명서.

## <a name="cells"></a>셀

Xamarin.Forms 셀 형식을 지원 합니다.

<a name="textCell" />

### <a name="textcell"></a>TextCell

|     |     |
| --- | --- |
| A [ `TextCell` ](xref:Xamarin.Forms.TextCell) 하나 또는 두 개의 텍스트 문자열을 표시 합니다. 설정 합니다 [ `Text` ](xref:Xamarin.Forms.TextCell.Text) 속성 및 필요에 따라 합니다 [ `Detail` ](xref:Xamarin.Forms.TextCell.Detail) 속성 이러한 텍스트 문자열을 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.TextCell) / [가이드](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell) | [![TextCell 예제](cells-images/TextCell.png "TextCell 예제")](cells-images/TextCell-Large.png#lightbox "TextCell 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TextCellDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TextCellDemoPage.xaml) |
|     |     |

### <a name="imagecell"></a>ImageCell

|     |     |
| --- | --- |
| 합니다 [ `ImageCell` ](xref:Xamarin.Forms.ImageCell) 와 동일한 정보를 표시 [ `TextCell` ](#textCell) 사용 하 여 설정 하는 비트맵을 포함 하지만 합니다 [ `Source` ](xref:Xamarin.Forms.Image.Source) 속성입니다.<br /><br />[API 설명서](xref:Xamarin.Forms.ImageCell) / [가이드](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell) | [![ImageCell 예제](cells-images/ImageCell.png "ImageCell 예제")](cells-images/ImageCell-Large.png#lightbox "ImageCell 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageCellDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageCellDemoPage.xaml) |
|     |     |

### <a name="switchcell"></a>SwitchCell

|     |     |
| --- | --- |
| [ `SwitchCell` ](xref:Xamarin.Forms.SwitchCell) 로 설정 하는 텍스트를 포함 합니다 [ `Text` ](xref:Xamarin.Forms.SwitchCell.Text) 속성 및 설정/해제 스위치를 처음에 부울 값을 사용 하 여 설정 [ `On` ](xref:Xamarin.Forms.SwitchCell.On) 속성입니다. 처리를 [ `OnChanged` ](xref:Xamarin.Forms.SwitchCell.OnChanged) 때 알림을 받도록 이벤트는 `On` 속성 변경 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.SwitchCell) / [가이드](~/xamarin-forms/user-interface/tableview.md#switchcell) | [![SwitchCell 예제](cells-images/SwitchCell.png "SwitchCell 예제")](cells-images/SwitchCell-Large.png#lightbox "SwitchCell 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchCellDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchCellDemoPage.xaml) |
|     |     |

### <a name="entrycell"></a>EntryCell

|     |     |
| --- | --- |
| 합니다 [ `EntryCell` ](xref:Xamarin.Forms.EntryCell) 정의 [ `Label` ](xref:Xamarin.Forms.EntryCell.Label) 셀과 편집 가능한 텍스트 한 줄을 식별 하는 속성을 [ `Text` ](xref:Xamarin.Forms.EntryCell.Text) 속성입니다. 처리를 [ `Completed` ](xref:Xamarin.Forms.EntryCell.Completed) 이벤트는 사용자가 텍스트 입력을 완료 하는 경우 알림을 받을 수 있습니다.<br /><br />[API 설명서](xref:Xamarin.Forms.EntryCell) / [가이드](~/xamarin-forms/user-interface/tableview.md#entrycell) | [![EntryCell 예제](cells-images/EntryCell.png "EntryCell 예제")](cells-images/EntryCell-Large.png#lightbox "EntryCell 예제")<br />[이 페이지에 대 한 C# 코드](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryCellDemoPage.cs) / [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryCellDemoPage.xaml) |
|     |     |


## <a name="related-links"></a>관련 링크

- [Xamarin.Forms FormsGallery 샘플](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)
- [Xamarin.Forms 샘플](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API 설명서](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
