---
title: Xamarin.Forms CollectionView
description: CollectionView은 다른 레이아웃 사양을 사용 하 여 데이터의 목록을 제공 하기 위한 유연 하 고 성능이 뛰어난 뷰입니다.
ms.prod: xamarin
ms.assetid: 2BC9B223-2D5C-4B09-849C-B9D578954557
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/15/2019
ms.openlocfilehash: b22449659d2d4b7791328d53ed2d2b29d405ffc8
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61019998"
---
# <a name="xamarinforms-collectionview"></a>Xamarin.Forms CollectionView

![미리 보기](~/media/shared/preview.png)

> [!IMPORTANT]
> `CollectionView` 는 현재 미리 보기, 및 일부 계획된 기능 부족 합니다. 또한 API 구현을 완료 되 면 변경할 수 있습니다.

`CollectionView` 데이터의 목록을 표시 하는 것에 대 한 뷰를 다른 레이아웃 사양을 사용 합니다. 보다 유연한 제공 하려고 하 고에 효율적인 대안 [ `ListView` ](xref:Xamarin.Forms.ListView)합니다. 동안 합니다 `CollectionView` 및 `ListView` Api는 유사한, 몇 가지 주목할 만한 차이점이 있습니다.

- `CollectionView` 목록 또는 표로에 세로 또는 가로로 표시 되는 데이터를 허용 하는 유연한 레이아웃 모델을 있습니다.
- `CollectionView` 셀의 개념이 있습니다. 대신, 데이터 템플릿 목록에서 데이터의 각 항목의 모양을 정의할 수 사용 됩니다.
- `CollectionView` 자동으로 기본 네이티브 컨트롤에 의해 제공 된 가상화를 활용 합니다.
- `CollectionView` API 화면을 줄어듭니다 [ `ListView` ](xref:Xamarin.Forms.ListView)합니다. 많은 속성 및 이벤트 `ListView` 에 없는 `CollectionView`합니다.
- `CollectionView` 기본 제공 구분 기호를 포함 하지 않습니다.

`CollectionView` Xamarin.Forms 4.0 시험판 버전에서 제공 됩니다. 그러나 현재 실험적 이므로 코드를 다음 줄을 추가 하 여 에서만 사용할 수 있습니다 하 `AppDelegate` 또는 ios의 경우 클래스에 `MainActivity` 호출 하기 전에 android에서 클래스 `Forms.Init`:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

> [!NOTE]
> `CollectionView` iOS 및 Android에서 사용할 수만 있습니다.

## <a name="populate-collectionview-with-datapopulate-datamd"></a>[CollectionView 데이터로 채우기](populate-data.md)

A `CollectionView` 설정 하 여 데이터를 채운 해당 `ItemsSource` 속성을 구현 하는 컬렉션 `IEnumerable`합니다. 목록의 각 항목의 모양을 설정 하 여 정의할 수 있습니다 합니다 `ItemTemplate` 속성을 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)합니다.

## <a name="specify-collectionview-layoutlayoutmd"></a>[CollectionView 레이아웃 지정](layout.md)

기본적으로 `CollectionView` 세로 목록에 항목을 표시 합니다. 그러나 가로 및 세로 목록 및 표를 지정할 수 있습니다.

## <a name="set-collectionview-selection-modeselectionmd"></a>[CollectionView 선택 모드를 설정 합니다.](selection.md)

기본적으로 `CollectionView` 선택할 수 없게 됩니다. 그러나 단일 및 다중 선택을 사용할 수 있습니다.

## <a name="display-an-emptyview-when-data-is-unavailableemptyviewmd"></a>[데이터를 사용할 수 없는 경우는 EmptyView 표시](emptyview.md)

`CollectionView`를 표시할 수 있는 데이터가 없는 경우 사용자에 게 피드백을 제공 하는 빈 뷰를 지정할 수 있습니다. 빈 뷰는 문자열로, 뷰 또는 여러 뷰 수 있습니다.

## <a name="scroll-an-item-into-viewscrollingmd"></a>[항목을 스크롤하여](scrolling.md)

스크롤을 시작 하는 사용자 천공 기와 때 항목이 완전히 표시 되도록 스크롤의 끝 위치를 제어할 수 있습니다. 또한 `CollectionView` 두 개의 정의 `ScrollTo` 메서드를 프로그래밍 방식으로 스크롤하여 항목입니다. 오버 로드 중 하나는 동안 지정된 된 항목을 뷰로 스크롤합니다 다른 보기에 지정된 된 인덱스에 항목을 스크롤합니다.
