---
title: Xamarin Forms CollectionView 소개
description: CollectionView는 다양 한 레이아웃 사양을 사용 하 여 데이터 목록을 표시 하기 위한 유연 하 고 성능이 뛰어난 뷰입니다.
ms.prod: xamarin
ms.assetid: 5C08F687-B9E6-4CE4-8726-F287F6D0B6A7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/24/2019
ms.openlocfilehash: 89afb0f2bfe93a5f78b0cd162f2a65e585b54b4b
ms.sourcegitcommit: 43423d4018cc0d4b0b8c98a4b3da0704495eb0cf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/14/2019
ms.locfileid: "72303226"
---
# <a name="xamarinforms-collectionview-introduction"></a>Xamarin Forms CollectionView 소개

![이 API는 현재 시험판입니다.](~/media/shared/preview.png)

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)는 다른 레이아웃 사양을 사용하여 데이터 목록을 표시하는 뷰입니다. [@No__t-1](xref:Xamarin.Forms.ListView)에 보다 유연 하 고 성능이 뛰어난 대안을 제공 하는 것을 목표로 합니다. 예를 들어 다음 스크린샷은 두 열 세로 그리드를 사용 하 고 여러 항목을 선택할 수 있는 `CollectionView`을 보여 줍니다.

[(introduction-images/verticalgrid-multipleselection.png "다중 선택을 포함 하는 iOS 및 Android CollectionView 세로 모눈 레이아웃") 의 ![CollectionView 세로 격자 레이아웃 스크린샷]](introduction-images/verticalgrid-multipleselection-large.png#lightbox "여러 항목을 선택 하 여 세로 모눈 레이아웃 CollectionView")

[`CollectionView`](xref:Xamarin.Forms.CollectionView) 은 Xamarin. Forms 4.0에서 사용할 수 있습니다. 그러나 현재는 실험적 이며, `Forms.Init`을 호출 하기 전에 iOS의 `AppDelegate` 클래스 또는 Android의 @no__t 1 클래스 또는 UWP의 `App.xaml.cs`에 다음 코드 줄을 추가 하 여 사용할 수 있습니다.

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

> [!IMPORTANT]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView) 은 IOS 및 Android에서 사용할 수 있지만 유니버설 Windows 플랫폼 [부분적 으로만 사용할 수](https://gist.github.com/hartez/7d0edd4182dbc7de65cebc6c67f72e14) 있습니다.

## <a name="collectionview-and-listview-differences"></a>CollectionView 및 ListView 차이점

[@No__t-1](xref:Xamarin.Forms.CollectionView) 및 [`ListView`](xref:Xamarin.Forms.ListView) api는 유사 하지만 몇 가지 주목할 만한 차이점이 있습니다.

- [`CollectionView`](xref:Xamarin.Forms.CollectionView) 에는 데이터를 목록 또는 표에 가로 또는 세로로 표시할 수 있는 유연한 레이아웃 모델이 있습니다.
- [`CollectionView`은](xref:Xamarin.Forms.CollectionView) 단일 및 다중 선택을 지원 합니다.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) 에는 셀 개념이 없습니다. 대신 데이터 템플릿을 사용 하 여 목록에 있는 각 데이터 항목의 모양을 정의 합니다.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) 은 기본 네이티브 컨트롤에서 제공 하는 가상화를 자동으로 활용 합니다.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`ListView`](xref:Xamarin.Forms.ListView)의 API 화면을 축소 합니다. [@No__t-1](xref:Xamarin.Forms.ListView) 의 많은 속성 및 이벤트는 `CollectionView`에 표시 되지 않습니다.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView) 은 기본 제공 구분 기호를 포함 하지 않습니다.

## <a name="move-from-listview-to-collectionview"></a>ListView에서 CollectionView로 이동

기존 Xamarin.ios의 [@no__t](xref:Xamarin.Forms.ListView) 구현: 다음 표를 참조 하 여 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 구현으로 마이그레이션할 수 있습니다.

| 개념 | ListView API | CollectionView |
|---|---|---|
| data | `ItemsSource` | [@No__t-1](xref:Xamarin.Forms.CollectionView) 은 해당 @no__t 속성을 설정 하 여 데이터로 채워집니다. 자세한 내용은 [데이터를 사용 하 여 CollectionView 채우기](populate-data.md#populate-a-collectionview-with-data)를 참조 하세요. |
| 항목 모양 | `ItemTemplate` | [@No__t-1](xref:Xamarin.Forms.CollectionView) 에서 각 항목의 모양은 `ItemTemplate` 속성을 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)로 설정 하 여 정의할 수 있습니다. 자세한 내용은 [항목 모양 정의](populate-data.md#define-item-appearance)를 참조 하세요. |
| 셀 | `TextCell`, `ImageCell`, `ViewCell` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) 에는 셀 개념이 없습니다. 대신 데이터 템플릿을 사용 하 여 목록에 있는 각 데이터 항목의 모양을 정의 합니다. |
| 행 구분 기호 | `SeparatorColor`, `SeparatorVisibility` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) 은 기본 제공 구분 기호를 포함 하지 않습니다. 이러한 항목은 원하는 경우 항목 템플릿에서 제공 될 수 있습니다. |
| 선택 | `SelectionMode`, `SelectedItem` | [`CollectionView`은](xref:Xamarin.Forms.CollectionView) 단일 및 다중 선택을 지원 합니다. 자세한 내용은 [Xamarin.ios CollectionView Selection](selection.md)을 참조 하세요. |
| 행 높이 | `HasUnevenRows`, `RowHeight` | @No__t-0에서 각 항목의 행 높이는 `ItemSizingStrategy` 속성에 의해 결정 됩니다. 자세한 내용은 [항목 크기 조정](layout.md#item-sizing)을 참조 하세요.|
| 캐싱 | `CachingStrategy` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) 은 기본 네이티브 컨트롤에서 제공 하는 가상화를 자동으로 사용 합니다. |
| 머리글 및 바닥글 | `Header`, `HeaderElement`, `HeaderTemplate`, `Footer`, `FooterElement`, `FooterTemplate` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) 은 `Header`, `Footer`, `HeaderTemplate` 및 @no__t 속성을 통해 목록의 항목으로 스크롤 하는 머리글 및 바닥글을 제공할 수 있습니다. 자세한 내용은 [머리글 및 바닥글](layout.md#headers-and-footers)을 참조 하세요. |
| 그룹화 | `GroupDisplayBinding`, `GroupHeaderTemplate`, `GroupShortNameBinding`, `IsGroupingEnabled` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) 은 @no__t 속성을 `true`으로 설정 하 여 올바르게 그룹화 된 데이터를 표시 합니다. @No__t-0 및 `GroupFooterTemplate` 속성을 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 개체로 설정 하 여 그룹 머리글 및 그룹 바닥글을 사용자 지정할 수 있습니다. 자세한 내용은 [Xamarin.ios CollectionView Grouping](grouping.md)을 참조 하세요. |
| 새로 고치려면 끌어오기 | `IsPullToRefreshEnabled`, `IsRefreshing`, `RefreshAllowed`, `RefreshCommand`, `RefreshControlColor`, `BeginRefresh()`, `EndRefresh()` | 최신 버전으로의 끌어오기는 현재 `CollectionView`에서 지원 되지 않지만 향후 릴리스에 추가 될 예정입니다. |
| 컨텍스트 작업 | `ContextActions` | 컨텍스트 동작은 현재 `CollectionView`에서 지원 되지 않지만 이후 릴리스에서 추가 될 예정입니다. |
| 스크롤 | `ScrollTo()` | [`CollectionView`](xref:Xamarin.Forms.CollectionView) 은 항목을 뷰로 스크롤 하는 @no__t 두 메서드를 정의 합니다. 자세한 내용은 [스크롤](scrolling.md)을 참조 하십시오. |

## <a name="related-links"></a>관련 링크

- [CollectionView (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
