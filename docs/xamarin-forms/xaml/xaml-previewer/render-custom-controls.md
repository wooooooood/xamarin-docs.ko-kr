---
title: XAML 미리 보기에서 사용자 지정 컨트롤 렌더링
description: 이 문서에서는 XAML 미리 보기에서 사용자 지정 컨트롤을 표시 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 4D795372-CB8F-48F4-B63D-845E44B261F7
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 03/27/2019
ms.openlocfilehash: 977c29312e0be8b92f216c224414f9bd03f8562d
ms.sourcegitcommit: c4be32ef914465e808d89767c4d5ee72afe93cc6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58858973"
---
# <a name="render-custom-controls-in-the-xaml-previewer"></a>XAML 미리 보기에서 사용자 지정 컨트롤 렌더링

_경우에 따라 사용자 지정 컨트롤 XAML 미리 보기에서 예상 대로 작동 하지 않습니다. 사용자 지정 컨트롤을 미리 보기의 제한 사항을 이해 하려면이 문서의 지침을 사용 합니다._

## <a name="basic-preview-mode"></a>기본 미리 보기 모드

프로젝트를 빌드한 하지 않은 경우에 XAML 미리 보기 페이지를 렌더링 합니다. 를 빌드할 때까지 기본 Xamarin.Forms 형식과 코드 숨김에 의존 하는 모든 컨트롤에 표시 됩니다. 프로젝트를 빌드하면 XAML 미리 보기를 사용 하도록 설정 하는 디자인 타임 렌더링을 사용 하 여 사용자 지정 컨트롤을 표시 하려고 합니다. 렌더링에 실패 하면 기본 Xamarin.Forms 형식이 표시 됩니다.

## <a name="enable-design-time-rendering-for-custom-controls"></a>사용자 지정 컨트롤에 대 한 디자인 타임 렌더링을 사용 하도록 설정

사용자 고유의 사용자 지정 컨트롤을 확인 하거나 타사 라이브러리에서 컨트롤을 사용 하는 경우 미리 보기 수 제대로 표시 되지 합니다. 사용자 지정 컨트롤은 디자인 시간 렌더링 컨트롤을 작성 하거나 라이브러리에서 가져온 미리 보기에 나타나는 데 옵트인 해야 합니다. 만든 컨트롤을 추가 합니다 [ `[DesignTimeVisible(true)]` ](xref:System.ComponentModel.DesignTimeVisibleAttribute) 미리 보기에서 표시할 컨트롤의 클래스:

```csharp
namespace MyProject
{
  [DesignTimeVisible(true)]
  public class MyControl : BaseControl
  {
    // Your control's code here
  }

}
```

사용 하 여 [James Montemagno의 ImageCirclePlugin의 기본 클래스](https://github.com/jamesmontemagno/ImageCirclePlugin/blob/master/src/ImageCircle/CircleImage.shared.cs) 예입니다.


## <a name="skiasharp-controls"></a>SkiaSharp 컨트롤

현재 SkiaSharp 컨트롤은 iOS에서 미리 볼 때에 지원 됩니다. Android 미리 보기에서 렌더링 되지 않습니다.

## <a name="troubleshooting"></a>문제 해결

### <a name="check-your-xamarinforms-version"></a>Xamarin.Forms 버전 확인
이상 있는지 확인 Xamarin.Forms 3.6을 설치 합니다. NuGet에서 Xamarin.Forms 버전을 업데이트할 수 있습니다.

### <a name="even-with-designtimevisibletrue-my-custom-control-isnt-rendering-properly"></a>도 `[DesignTimeVisible(true)]`, 내 사용자 지정 컨트롤이 제대로 렌더링 되지 않습니다.
코드 숨김 또는 백 엔드 데이터에 크게 의존 하는 사용자 지정 컨트롤 XAML 미리 보기에서 항상 작동 하지 않습니다. 시도할 수 있습니다.
* 컨트롤을 이동 하는 경우 초기화 하지 [디자인 모드가 설정 된](index.md#detect-design-mode)
* 설정 [디자인 타임 데이터](design-time-data.md) 백 엔드에서 모조 데이터를 표시 하려면

### <a name="the-xaml-previewer-shows-the-error-custom-controls-arent-rendering-properly"></a>"사용자 지정 컨트롤 제대로 렌더링 되지 않음" XAML 미리 보기가 오류를 보여 줍니다.
시도 정리 하 고 프로젝트를 다시 작성 또는 닫았다가 XAML 파일입니다.
