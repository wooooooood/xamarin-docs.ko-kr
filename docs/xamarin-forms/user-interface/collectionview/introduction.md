---
title: Xamarin.Forms CollectionView 소개
description: CollectionView은 다른 레이아웃 사양을 사용 하 여 데이터의 목록을 제공 하기 위한 유연 하 고 성능이 뛰어난 뷰입니다.
ms.prod: xamarin
ms.assetid: 5C08F687-B9E6-4CE4-8726-F287F6D0B6A7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 456c83808ff685a8c2199cf80c96d63b9334675e
ms.sourcegitcommit: 0fd04ea3af7d6a6d6086525306523a5296eec0df
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67512728"
---
# <a name="xamarinforms-collectionview-introduction"></a>Xamarin.Forms CollectionView 소개

![](~/media/shared/preview.png "이 API는 현재 시험판임")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) 데이터의 목록을 표시 하는 것에 대 한 뷰를 다른 레이아웃 사양을 사용 합니다. 보다 유연한 제공 하려고 하 고에 효율적인 대안 [ `ListView` ](xref:Xamarin.Forms.ListView)합니다. 예를 들어 다음 스크린샷에서 표시 된 `CollectionView` 세로 격자 눈금 두 열을 사용 하 고 있어 여러 선택:

[![IOS 및 Android에서 CollectionView 세로 격자 레이아웃의 스크린 샷](introduction-images/verticalgrid-multipleselection.png "다중 선택 영역을 사용 하 여 CollectionView 세로 격자 레이아웃")](introduction-images/verticalgrid-multipleselection-large.png#lightbox "으로 CollectionView 세로 모눈 레이아웃 다중 선택")

[`CollectionView`](xref:Xamarin.Forms.CollectionView) Xamarin.Forms 4.0에서 제공 됩니다. 그러나 현재 실험적 이므로 코드를 다음 줄을 추가 하 여 에서만 사용할 수 있습니다 하 `AppDelegate` 또는 ios의 경우 클래스에 `MainActivity` 호출 하기 전에 android에서 클래스 `Forms.Init`:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

> [!IMPORTANT]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView) iOS 및 Android에서 사용할 수 있지만 유니버설 Windows 플랫폼에서 부분적으로 제공 됩니다.

## <a name="collectionview-and-listview-differences"></a>CollectionView 및 ListView 차이

동안 합니다 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) 및 [ `ListView` ](xref:Xamarin.Forms.ListView) Api는 유사한, 몇 가지 주목할 만한 차이점이 있습니다.

- [`CollectionView`](xref:Xamarin.Forms.CollectionView) 목록 또는 표로에 세로 또는 가로로 표시 되는 데이터를 허용 하는 유연한 레이아웃 모델을 있습니다.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) 에서는 단일 및 다중 선택 합니다.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) 셀의 개념이 있습니다. 대신, 데이터 템플릿 목록에서 데이터의 각 항목의 모양을 정의할 수 사용 됩니다.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) 자동으로 기본 네이티브 컨트롤에 의해 제공 된 가상화를 활용 합니다.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) API 화면을 줄어듭니다 [ `ListView` ](xref:Xamarin.Forms.ListView)합니다. 많은 속성 및 이벤트 [ `ListView` ](xref:Xamarin.Forms.ListView) 에 존재 하지 않는 `CollectionView`합니다.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) 기본 제공 구분 기호를 포함 하지 않습니다.

## <a name="move-from-listview-to-collectionview"></a>ListView에서 CollectionView로 이동

[`ListView`](xref:Xamarin.Forms.ListView) 기존 Xamarin.Forms 구현의 구현을로 마이그레이션할 수 있습니다 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) 다음 표에서 사용 하 여 구현 합니다.

| 개념 | ListView API | CollectionView |
|---|---|---|
| 데이터 | `ItemsSource` | A [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) 설정 하 여 데이터로 채워질 때 해당 `ItemsSource` 속성입니다. 자세한 내용은 [데이터로 CollectionView 채우는](populate-data.md#populate-a-collectionview-with-data)합니다. |
| 항목 모양 | `ItemTemplate` | 각 항목의 모양을 [ `CollectionView` ](xref:Xamarin.Forms.CollectionView) 설정 하 여 정의할 수 있습니다 합니다 `ItemTemplate` 속성을을 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)합니다. 자세한 내용은 [항목의 모양을 정의](populate-data.md#define-item-appearance)합니다. |
| 셀 | `TextCell`, `ImageCell`, `ViewCell` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) 셀의 개념이 있습니다. 대신, 데이터 템플릿 목록에서 데이터의 각 항목의 모양을 정의할 수 사용 됩니다. |
| 행 구분 기호 | `SeparatorColor`, `SeparatorVisibility` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) 기본 제공 구분 기호를 포함 하지 않습니다. 이러한 항목 템플릿에 필요한 경우 제공할 수 있습니다. |
| 선택 | `SelectionMode`, `SelectedItem` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) 에서는 단일 및 다중 선택 합니다. 자세한 내용은 [Xamarin.Forms CollectionView 선택](selection.md)합니다. |
| 행 높이 | `HasUnevenRows`, `RowHeight` | 에 `CollectionView`, 하 여 각 항목의 행 높이가 결정 됩니다는 `ItemSizingStrategy` 속성입니다. 자세한 내용은 [항목 크기 조정](layout.md#item-sizing)합니다.|
| 캐싱 | `CachingStrategy` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) 자동 기본 네이티브 컨트롤에 의해 제공 된 가상화를 사용 합니다. |
| 머리글 및 바닥글 | `Header`, `HeaderElement`, `HeaderTemplate`, `Footer`, `FooterElement`, `FooterTemplate` | 머리글 및 바닥글은 현재 지원 되지 않는에서 `CollectionView`, 않지만 향후 릴리스에서 추가 됩니다.|
| 그룹화 | `GroupDisplayBinding`, `GroupHeaderTemplate`, `GroupShortNameBinding`, `IsGroupingEnabled` | 그룹화에서 지원 되지 않는 현재 `CollectionView`, 않지만 향후 릴리스에서 추가 됩니다. |
| 당겨서 새로 고침 | `IsPullToRefreshEnabled`, `IsRefreshing`, `RefreshAllowed`, `RefreshCommand`, `RefreshControlColor`, `BeginRefresh()`, `EndRefresh()` | 당겨서 새로 고침에서 지원 되지 않는 현재 `CollectionView`, 않지만 향후 릴리스에서 추가 됩니다. |
| 컨텍스트 작업 | `ContextActions` | 상황에 맞는 작업에서 현재 지원 하지 않는 `CollectionView`, 않지만 향후 릴리스에서 추가 됩니다. |
| 스크롤 | `ScrollTo()` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) 정의 `ScrollTo` 메서드를 스크롤하여 항목입니다. 자세한 내용은 [스크롤](scrolling.md)합니다. |

## <a name="related-links"></a>관련 링크

- [CollectionView (샘플)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CollectionViewDemos/)
