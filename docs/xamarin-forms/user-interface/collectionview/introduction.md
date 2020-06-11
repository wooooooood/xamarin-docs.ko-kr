---
제목: " Xamarin.Forms CollectionView 소개" 설명: "CollectionView는 서로 다른 레이아웃 사양을 사용 하 여 데이터 목록을 표시 하기 위한 유연 하 고 성능이 뛰어난 뷰입니다."
assetid: 5C08F687-B9E6-4CE4-8726-F287F6D0B6A7 ms. 기술: xamarin forms author: davidbritch: dabritch: ms. date: 12/11/2019 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-collectionview-introduction"></a>Xamarin.FormsCollectionView 소개

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)는 다른 레이아웃 사양을 사용하여 데이터 목록을 표시하는 뷰입니다. 보다 유연 하 고 성능이 뛰어난 대안을 제공 하는 것을 목표로 [`ListView`](xref:Xamarin.Forms.ListView) 합니다. 예를 들어 다음 스크린샷은 `CollectionView` 두 열 세로 그리드를 사용 하 고 여러 항목을 선택할 수 있는을 보여 줍니다.

[![IOS 및 Android에서 CollectionView 세로 격자 레이아웃의 스크린샷](introduction-images/verticalgrid-multipleselection.png "여러 항목을 선택 하 여 세로 모눈 레이아웃 CollectionView")](introduction-images/verticalgrid-multipleselection-large.png#lightbox "여러 항목을 선택 하 여 세로 모눈 레이아웃 CollectionView")

[`CollectionView`](xref:Xamarin.Forms.CollectionView)4.3에서 사용할 수 있습니다 Xamarin.Forms .

> [!IMPORTANT]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)는 iOS 및 Android에서 사용할 수 있지만 유니버설 Windows 플랫폼 에서만 [부분적으로 사용할 수](https://gist.github.com/hartez/7d0edd4182dbc7de65cebc6c67f72e14) 있습니다.

## <a name="collectionview-and-listview-differences"></a>CollectionView 및 ListView 차이점

[`CollectionView`](xref:Xamarin.Forms.CollectionView)및 api는 [`ListView`](xref:Xamarin.Forms.ListView) 비슷하지만 몇 가지 주목할 만한 차이점이 있습니다.

- [`CollectionView`](xref:Xamarin.Forms.CollectionView)에는 데이터를 목록 또는 표에 가로 또는 세로로 표시할 수 있는 유연한 레이아웃 모델이 있습니다.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)단일 및 다중 선택을 지원 합니다.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)에는 셀의 개념이 없습니다. 대신 데이터 템플릿을 사용 하 여 목록에 있는 각 데이터 항목의 모양을 정의 합니다.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)는 기본 네이티브 컨트롤에서 제공 하는 가상화를 자동으로 활용 합니다.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)의 API 표면을 줄입니다 [`ListView`](xref:Xamarin.Forms.ListView) . 에서 많은 속성 및 이벤트가 [`ListView`](xref:Xamarin.Forms.ListView) 제공 되지 않습니다 `CollectionView` .
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)에는 기본 제공 구분 기호가 포함 되어 있지 않습니다.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)[`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)가 UI 스레드에서 업데이트 되 면에서 예외를 throw 합니다.

## <a name="move-from-listview-to-collectionview"></a>ListView에서 CollectionView로 이동

[`ListView`](xref:Xamarin.Forms.ListView)Xamarin.Forms다음 테이블의 도움말을 사용 하 여 기존 구현에서 구현을 마이그레이션할 수 있습니다 [`CollectionView`](xref:Xamarin.Forms.CollectionView) .

| 개념 | ListView API | CollectionView |
|---|---|---|
| 데이터 | `ItemsSource` | 는 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 속성을 설정 하 여 데이터로 채워집니다 `ItemsSource` . 자세한 내용은 [데이터를 사용 하 여 CollectionView 채우기](populate-data.md#populate-a-collectionview-with-data)를 참조 하세요. |
| 항목 모양 | `ItemTemplate` | 에서 각 항목의 모양은 속성을 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 로 설정 하 여 정의할 수 있습니다 `ItemTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) . 자세한 내용은 [항목 모양 정의](populate-data.md#define-item-appearance)를 참조 하세요. |
| 셀 | `TextCell`, `ImageCell`, `ViewCell` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)에는 셀 개념이 없으므로 공개 표시기의 개념이 없습니다. 대신 데이터 템플릿을 사용 하 여 목록에 있는 각 데이터 항목의 모양을 정의 합니다. |
| 행 구분 기호 | `SeparatorColor`, `SeparatorVisibility` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)에는 기본 제공 구분 기호가 포함 되어 있지 않습니다. 이러한 항목은 원하는 경우 항목 템플릿에서 제공 될 수 있습니다. |
| 선택 | `SelectionMode`, `SelectedItem` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)단일 및 다중 선택을 지원 합니다. 자세한 내용은 [ Xamarin.Forms CollectionView Selection](selection.md)을 참조 하세요. |
| 행 높이 | `HasUnevenRows`, `RowHeight` | 에서 `CollectionView` 각 항목의 행 높이는 속성에 의해 결정 됩니다 `ItemSizingStrategy` . 자세한 내용은 [항목 크기 조정](layout.md#item-sizing)을 참조 하세요.|
| 캐싱 | `CachingStrategy` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)기본 네이티브 컨트롤에서 제공 하는 가상화를 자동으로 사용 합니다. |
| 머리글 및 바닥글 | `Header`, `HeaderElement`, `HeaderTemplate`, `Footer`, `FooterElement`, `FooterTemplate` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)는 `Header` ,, `Footer` `HeaderTemplate` 및 속성을 통해 목록의 항목으로 스크롤 하는 머리글 및 바닥글을 제공할 수 있습니다 `FooterTemplate` . 자세한 내용은 [머리글 및 바닥글](layout.md#headers-and-footers)을 참조 하세요. |
| 그룹화 | `GroupDisplayBinding`, `GroupHeaderTemplate`, `GroupShortNameBinding`, `IsGroupingEnabled` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)속성을로 설정 하 여 올바르게 그룹화 된 데이터 `IsGrouped` 를 표시 `true` 합니다. 및 속성을 개체로 설정 하 여 그룹 머리글 및 그룹 바닥글을 사용자 지정할 수 있습니다 `GroupHeaderTemplate` `GroupFooterTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) . 자세한 내용은 [ Xamarin.Forms CollectionView Grouping](grouping.md)을 참조 하세요. |
| 당겨서 새로 고침 | `IsPullToRefreshEnabled`, `IsRefreshing`, `RefreshAllowed`, `RefreshCommand`, `RefreshControlColor`, `BeginRefresh()`, `EndRefresh()` | 새로 고침 기능은를의 자식으로 설정 하 여 지원 됩니다 [`CollectionView`](xref:Xamarin.Forms.CollectionView) `RefreshView` . 자세한 내용은 [Pull to refresh를](populate-data.md#pull-to-refresh)참조 하세요. |
| 컨텍스트 메뉴 항목 | `ContextActions` | 에 있는 `SwipeView` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 데이터의 각 항목의 모양을 정의 하는의 루트 뷰로를 설정 하 여 상황에 맞는 메뉴 항목을 사용할 수 있습니다 [`CollectionView`](xref:Xamarin.Forms.CollectionView) . 자세한 내용은 [상황에 맞는 메뉴](populate-data.md#context-menus)를 참조하세요. |
| 스크롤 | `ScrollTo()` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)`ScrollTo`항목을 뷰로 스크롤 하는 메서드를 정의 합니다. 자세한 내용은 [스크롤](scrolling.md)을 참조 하십시오. |

## <a name="related-links"></a>관련 링크

- [CollectionView (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
