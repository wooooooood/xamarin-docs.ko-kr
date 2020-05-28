---
제목: ' description: '의 애니메이션에는 Xamarin.Forms Xamarin.Forms 간단한 애니메이션을 만들기에는 간단 하 고 복잡 한 애니메이션을 만들 수 있을 정도로 충분 한 자체 애니메이션 인프라가 포함 되어 있습니다.
ms. prod: assetid: ms. 기술: author: ms author: ms. date: no loc:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

---

# <a name="animation-in-xamarinforms"></a>애니메이션Xamarin.Forms

_Xamarin에는 간단한 애니메이션을 만들기에는 간단 하 고 복잡 한 애니메이션을 만들 수 있는 다양 한 애니메이션 인프라가 포함 되어 있습니다._

Xamarin.Forms애니메이션 클래스는 특정 시간 동안 속성을 한 값에서 다른 값으로 변경 하는 일반적인 애니메이션으로 시각적 요소의 다른 속성을 대상으로 합니다. 애니메이션 클래스에 대 한 XAML 인터페이스는 없습니다 Xamarin.Forms . 그러나 애니메이션은 [동작](~/xamarin-forms/app-fundamentals/behaviors/index.md) 에서 캡슐화 되 고 XAML에서 참조 될 수 있습니다.

## <a name="simple-animations"></a>[간단한 애니메이션](simple.md)

[`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions)클래스는 인스턴스를 회전, 크기 조정, 변환 및 페이드 인 하는 간단한 애니메이션을 생성 하는 데 사용할 수 있는 확장 메서드를 제공 합니다 [`VisualElement`](xref:Xamarin.Forms.VisualElement) . 이 문서에서는 클래스를 사용 하 여 애니메이션을 만들고 취소 하는 방법을 보여 줍니다 `ViewExtensions` .

## <a name="easing-functions"></a>[감속/가속 함수](easing.md)

Xamarin.Forms에는 [`Easing`](xref:Xamarin.Forms.Easing) 애니메이션이 실행 되는 속도를 제어 하는 방법을 제어 하는 전송 함수를 지정 하는 데 사용할 수 있는 클래스가 포함 되어 있습니다. 이 문서에서는 미리 정의 된 감속/가속 함수를 사용 하는 방법과 사용자 지정 감속/가속 함수를 만드는 방법을 보여 줍니다.

## <a name="custom-animations"></a>[사용자 지정 애니메이션](custom.md)

[`Animation`](xref:Xamarin.Forms.Animation)클래스는 Xamarin.Forms 클래스의 확장 메서드를 사용 하 여 하나 이상의 개체를 만드는 모든 애니메이션의 빌딩 블록입니다 [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) `Animation` . 이 문서에서는 클래스를 사용 하 여 애니메이션을 만들고 취소 하 고, `Animation` 여러 애니메이션을 동기화 하 고, 기존 애니메이션 메서드에서 애니메이션을 적용 하지 않는 속성에 애니메이션을 적용 하는 사용자 지정 애니메이션을 만드는 방법을 보여 줍니다.
