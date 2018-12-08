---
title: Xamarin.Forms Visual
description: 이 아티클에서 iOS 및 Android에서 동일 하 게 또는 거의 동일 하 게 뷰를 렌더링 하는 Xamarin.Forms 시각적 개체를 소개 합니다.
ms.prod: xamarin
ms.assetid: 80BF9C72-AC28-4AAF-9DDD-B60CBDD1CD59
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/12/2018
ms.openlocfilehash: df2351e35817a6468fed236752eea8e5ad11024f
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53061888"
---
# <a name="xamarinforms-visual"></a>Xamarin.Forms Visual

![미리 보기](~/media/shared/preview.png)

_이 아티클에서 iOS 및 Android에서 동일 하 게 또는 거의 동일 하 게 뷰를 렌더링 하는 Xamarin.Forms 시각적 개체를 소개 합니다._

대부분의 개발자는 iOS 및 Android에서 동일 하거나 거의 동일 보이는 Xamarin.Forms 응용 프로그램을 만들 하려고 합니다. Xamarin.Forms 4.0-pre1 통해 모양을 옵트인 하는 응용 프로그램을 사용 하 여는 시각적 모양을 구현 하는 추가 렌더러를 포함 하는 메커니즘을 포함 한 `Visual` 속성:

```xaml
<ContentPage ...
             Visual="Material">
    ...
</ContentPage>    
```

시각적 모양을 구현 하는 렌더러 기본 렌더러 대신 렌더러 보기에 사용 됩니다. 렌더러 선택 시의 `Visual` 보기의 속성을 검사 하 고 렌더러 선택 프로세스에 포함 합니다. 또한 경우는 `Visual` 속성이 런타임 시 변경, 모든 자식과 함께 지정 된 렌더러 다시 만들어집니다.

> [!IMPORTANT]
> `Visual` 속성에 정의 된 합니다 `VisualElement` 클래스를 상속 하는 뷰를 사용 하 여는 `Visual` 부모에서 속성 값입니다. 따라서 설정 합니다 `Visual` 속성을는 `ContentPage` 페이지의 지원 되는 모든 보기에서 해당 모양을 사용 되도록 보장 합니다. 또한는 `Visual` 뷰에 속성을 재정의할 수 있습니다.

Xamarin.Forms 4.0-pre1 재질 렌더러 라고도 하는 렌더러를 사용 하 여 재질 디자인 기반 하 여 실험적 visual 모양을 포함 됩니다. 재질 렌더러 iOS 및 Android에서 다음과 같은 뷰에 대 한 현재 포함 되어 있습니다.

- [`Button`](xref:Xamarin.Forms.Button)
- [`Entry`](xref:Xamarin.Forms.Entry)
- [`Frame`](xref:Xamarin.Forms.Frame)
- [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)

기능적으로 재료 렌더러는 기본 렌더러에 차이가 없습니다. 그러나 현재 실험적 이며 코드의 다음 줄을 추가 하 여 에서만 사용할 수 있습니다 하 `AppDelegate` 또는 ios의 경우 클래스에 `MainActivity` 호출 하기 전에 android에서 클래스 `Forms.Init`:

```csharp
Forms.SetFlags("Visual_Experimental");
```

또한 iOS 플랫폼 프로젝트가 있어야 합니다 [Xamarin.iOS.MaterialComponents](https://www.nuget.org/packages/Xamarin.iOS.MaterialComponents/) NuGet 패키지가 설치 되어 있습니다. Android에서 시각적 개체 API 29 에서만 작동, 플랫폼 프로젝트 v28 지원 라이브러리를 사용 하 고 자료 구성 요소 테마에서 상속 하거나 테마를 몇 가지 새 테마 특성을 추가 하는 동안는 AppCompat 테마에서 상속 하도록 계속 해당 테마를 설정 해야 합니다. 자세한 내용은 [자료 구성 요소를 사용 하 여 Android에 대 한 시작](https://github.com/material-components/material-components-android/blob/master/docs/getting-started.md)합니다.

다음 스크린샷에서 자료에 대 한 렌더러 존재 하지만 기본 렌더러를 사용 하 여 렌더링 네 가지 보기를 포함 하는 사용자 인터페이스를 보여 줍니다.

[![렌더러 기본](visual-images/default-renderers.png "기본 렌더러를 사용 하 여 뷰")](visual-images/default-renderers-large.png#lightbox)

다음 스크린샷은 재료 렌더러를 사용 하 여 렌더링 동일한 사용자 인터페이스를 보여 줍니다.

[![재질 렌더러](visual-images/material-renderers.png "재료 렌더러를 사용 하 여 뷰")](visual-images/material-renderers-large.png#lightbox)

> [!NOTE]
> 재료 렌더러를 사용 하 여 렌더링 된 컨트롤은 네이티브 컨트롤을 유지 하 고 따라서 수 있으며 사용자 인터페이스 차이점 글꼴, 그림자, 색 및 권한 상승 같은 영역에 대 한 플랫폼.

## <a name="related-links"></a>관련 링크

- [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
