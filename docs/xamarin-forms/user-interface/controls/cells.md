---
title: Xamarin.Forms Cells
description: "Xamarin.Forms 셀 Listview 및 TableViews에 추가할 수 있습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: F2A02DEE-7137-42F4-9C0A-4E1CF75EA08F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 509ecc509754bba544115c140e619f634bd64eae
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-cells"></a>Xamarin.Forms Cells

_Xamarin.Forms 셀 Listview 및 TableViews에 추가할 수 있습니다._

<style>.tableimg {최대 너비: 없음! 중요;을 (를)</style>

## <a name="cells"></a>셀

셀이 테이블의 항목에 대 한 사용 되는 특수 한 요소를 목록의 각 항목을 그리는 해야 하는 방법에 대해 설명 합니다. 셀에서 파생 [ `Element` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/)에서 있는 VisualElement 에서도 파생 됩니다. 셀이 시각적 요소를 없는 되지만 시각적 요소를 만들기 위한 템플릿이 설명 합니다. [`Cell`](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) 모든 Xamarin.Forms 셀에 대 한 기본 클래스 및 기능을 제공합니다. 셀은 요소에 추가 하도록 설계 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 또는 [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) 컨트롤입니다.

사용 및 셀 사용자 지정 하는 방법에 자세한 참조는 [ListView](~/xamarin-forms/user-interface/listview/index.md) 및 [TableView](~/xamarin-forms/user-interface/tableview.md) 설명서입니다.

<table align="center" border="1" cellpadding="1" cellspacing="1">
<thead>
    <th>
      <strong>Type</strong>
    </th>
    <th>
      <strong>설명</strong>
    </th>
    <th style="min-width:400px">
      <strong>스크린 샷</strong>
    </th>
  </thead>
  <tbody>
    <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/">EntryCell</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">Xamarin.Forms.Cell</a> 레이블과 한 줄 텍스트 입력 필드를 사용 합니다.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/EntryDemoPage.cs"><img src="cells-images/EntryCell.png" title="EntryCell 예제" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/">SwitchCell</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">Xamarin.Forms.Cell</a> 레이블과 켜기/끄기 스위치를 사용 합니다.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SwitchCellDemoPage.cs"><img src="cells-images/SwitchCell.png" title="SwitchCell 예제" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/">TextCell</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">Xamarin.Forms.Cell</a> 기본 및 보조 텍스트를 포함 합니다.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TextCellDemoPage.cs"><img src="cells-images/TextCell.png" title="TextCell 예제" class="tableimg">
    </a></td>
  </tr>
      <tr>
    <td>
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/">ImageCell</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/">텍스트 셀</a> 이미지로 포함 된 합니다.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ImageCellDemoPage.cs"><img src="cells-images/ImageCell.png" title="ImageCell 예제" class="tableimg">
    </a></td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>관련 링크

- [Xamarin.Forms 소개](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms 갤러리 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)
- [Xamarin.Forms 샘플](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API 설명서](https://developer.xamarin.com/api/namespace/Xamarin.Forms/)
