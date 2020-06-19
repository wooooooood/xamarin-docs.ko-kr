---
title: Xamarin.Forms셀
description: Xamarin.Forms셀은 Listview 및 TableViews에 추가할 수 있습니다. 이 문서에서는에 포함 된 셀을 나열 합니다 Xamarin.Forms .
ms.prod: xamarin
ms.assetid: 77DA0C89-35D6-4C09-A072-3ADE53FD56CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: fff62aea5a20a8a14271123c4664c2c0b4e26d1e
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84573328"
---
# <a name="xamarinforms-cells"></a>Xamarin.Forms셀

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

_Listview 및 TableViews에 Xamarin 형식의 셀을 추가할 수 있습니다._

*셀* 은 테이블의 항목에 사용 되는 특수 요소 이며 목록의 각 항목을 렌더링 하는 방법을 설명 합니다. [`Cell`](xref:Xamarin.Forms.Cell)클래스는 [`Element`](xref:Xamarin.Forms.Element) 도 파생 되는에서 파생 [`VisualElement`](xref:Xamarin.Forms.Element) 됩니다. 셀이 시각적 요소가 아닙니다. 대신 시각적 요소를 만들기 위한 템플릿입니다.

`Cell`는 및 컨트롤 에서만 사용 됩니다 [`ListView`](views.md#listview) [`TableView`](views.md#tableview) . 셀을 사용 및 사용자 지정 하는 방법에 대 한 자세한 내용은 [`ListView`](~/xamarin-forms/user-interface/listview/index.md) 및 설명서를 참조 [`TableView`](~/xamarin-forms/user-interface/tableview.md) 하세요.

## <a name="cells"></a>셀

Xamarin.Forms에서는 다음 셀 형식을 지원 합니다.

### <a name="textcell"></a>TextCell

|     |     |
| --- | --- |
| 는 [`TextCell`](xref:Xamarin.Forms.TextCell) 하나 또는 두 개의 텍스트 문자열을 표시 합니다. 속성을 설정 하 [`Text`](xref:Xamarin.Forms.TextCell.Text) 고 필요에 따라 [`Detail`](xref:Xamarin.Forms.TextCell.Detail) 속성을 이러한 텍스트 문자열로 설정 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.TextCell)  /  [가이드](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#textcell) | [![TextCell 예제](cells-images/TextCell.png "TextCell 예제")](cells-images/TextCell-Large.png#lightbox "TextCell 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TextCellDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TextCellDemoPage.xaml) |
|     |     |

### <a name="imagecell"></a>ImageCell

|     |     |
| --- | --- |
| 는 [`ImageCell`](xref:Xamarin.Forms.ImageCell) 와 동일한 정보를 표시 [`TextCell`](#textcell) 하지만 속성으로 설정 하는 비트맵을 포함 합니다 [`Source`](xref:Xamarin.Forms.Image.Source) .<br /><br />[API 설명서](xref:Xamarin.Forms.ImageCell)  /  [가이드](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#imagecell) | [![ImageCell 예제](cells-images/ImageCell.png "ImageCell 예제")](cells-images/ImageCell-Large.png#lightbox "ImageCell 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageCellDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageCellDemoPage.xaml) |
|     |     |

### <a name="switchcell"></a>SwitchCell

|     |     |
| --- | --- |
| 에는 속성을 포함 하는 [`SwitchCell`](xref:Xamarin.Forms.SwitchCell) 텍스트 집합과 [`Text`](xref:Xamarin.Forms.SwitchCell.Text) on/off 스위치가 처음에 부울 속성으로 설정 되어 있습니다 [`On`](xref:Xamarin.Forms.SwitchCell.On) . [`OnChanged`](xref:Xamarin.Forms.SwitchCell.OnChanged)속성이 변경 될 때 알리도록 이벤트를 처리 합니다 `On` .<br /><br />[API 설명서](xref:Xamarin.Forms.SwitchCell)  /  [가이드](~/xamarin-forms/user-interface/tableview.md#switchcell) | [![SwitchCell 예제](cells-images/SwitchCell.png "SwitchCell 예제")](cells-images/SwitchCell-Large.png#lightbox "SwitchCell 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchCellDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchCellDemoPage.xaml) |
|     |     |

### <a name="entrycell"></a>EntryCell

|     |     |
| --- | --- |
| 은 [`EntryCell`](xref:Xamarin.Forms.EntryCell) [`Label`](xref:Xamarin.Forms.EntryCell.Label) 속성에서 셀 및 편집 가능한 텍스트의 한 줄을 식별 하는 속성을 정의 [`Text`](xref:Xamarin.Forms.EntryCell.Text) 합니다. 사용자가 [`Completed`](xref:Xamarin.Forms.EntryCell.Completed) 텍스트 입력을 완료 했을 때 알리도록 이벤트를 처리 합니다.<br /><br />[API 설명서](xref:Xamarin.Forms.EntryCell)  /  [가이드](~/xamarin-forms/user-interface/tableview.md#entrycell) | [![EntryCell 예제](cells-images/EntryCell.png "EntryCell 예제")](cells-images/EntryCell-Large.png#lightbox "EntryCell 예제")<br />[이 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryCellDemoPage.cs)  /  에 대 한 c # 코드 [XAML 페이지](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryCellDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>관련 링크

- [Xamarin.Forms양식 갤러리 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.FormsAPI 설명서](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
