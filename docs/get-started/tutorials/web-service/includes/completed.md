---
ms.openlocfilehash: e0f7e89be5de282daf10f941d0f0b8d0175df81a
ms.sourcegitcommit: 6b833f44d5fd8dc7ab7f8546e8b7d383e5a989db
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71107305"
---
축하합니다. 자습서를 마쳤습니다. 여기서는 다음과 같은 방법을 알아보았습니다.

> [!div class="checklist"]
>
> - NuGet 패키지 관리자를 사용하여 Newtonsoft.Json을 Xamarin.Forms 프로젝트에 추가합니다.
> - 웹 서비스 클래스를 만듭니다.
> - 웹 서비스 클래스를 사용합니다.

## <a name="next-steps"></a>다음 단계

이 일련의 자습서에서는 Xamarin.Forms를 사용하여 모바일 애플리케이션을 만드는 기본 사항을 다룹니다. Xamarin.Forms 개발에 대한 자세한 내용은 다음과 같은 기능을 참고하세요.

- Xamarin.Forms 애플리케이션의 사용자 인터페이스를 만드는 데 사용되는 4개의 주 컨트롤 그룹이 있습니다. 자세한 내용은 [컨트롤 참조](~/xamarin-forms/user-interface/controls/index.md)를 참조하세요.
- 데이터 바인딩은 두 개체의 속성을 연결하여 한 속성의 변경 내용이 다른 속성에 자동으로 반영되도록 하는 기술입니다. 자세한 내용은 [데이터 바인딩](~/xamarin-forms/app-fundamentals/data-binding/index.md)을 참조하세요.
- Xamarin.Forms는 사용되는 페이지 형식에 따라 다양한 페이지 탐색 환경을 제공합니다. 자세한 내용은 [탐색](~/xamarin-forms/app-fundamentals/navigation/index.md)을 참조하세요.
- 스타일을 통해 반복 태그를 줄이고, 애플리케이션 모양을 쉽게 변경할 수 있습니다. 자세한 내용은 [Xamarin.Forms 앱 스타일 지정](~/xamarin-forms/user-interface/styles/index.md)을 참조하세요.
- XAML 태그 확장은 요소 특성을 리터럴 텍스트 문자열 이외의 원본으로 설정할 수 있도록 하여 XAML의 성능과 유연성을 확장합니다. 자세한 내용은 [XAML 태그 확장](~/xamarin-forms/xaml/markup-extensions/index.md)을 참조하세요.
- 데이터 템플릿은 지원되는 보기에서 데이터 표현을 정의하는 기능을 제공합니다. 자세한 내용은 [데이터 템플릿](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)을 참조하세요.
- 각 페이지, 레이아웃 및 보기는 차례로 네이티브 컨트롤을 만들고, 화면에 정렬하고, 공유 코드에 지정된 동작을 추가하는 `Renderer` 클래스를 사용하여 각 플랫폼에서 다르게 렌더링됩니다. 개발자는 컨트롤의 모양 및/또는 동작을 사용자 지정하기 위해 자신 만의 사용자 지정 `Renderer` 클래스를 구현할 수 있습니다. 자세한 내용은 [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)를 참조하세요.
- 각 플랫폼의 네이티브 컨트롤을 사용자 지정할 수 있는 효과가 있습니다. 효과의 경우, 플랫폼별 프로젝트에서 [`PlatformEffect`](xref:Xamarin.Forms.PlatformEffect`2) 클래스를 하위 클래스로 지정하여 만들어지고, 적절한 Xamarin.Forms 컨트롤에 연결하여 사용됩니다. 자세한 내용은 [효과](~/xamarin-forms/app-fundamentals/effects/index.md)를 참조하세요.
- 공유 코드는 [`DependencyService`](xref:Xamarin.Forms.DependencyService) 클래스를 통해 네이티브 기능에 액세스할 수 있습니다. 자세한 내용은 [DependencyService를 사용한 네이티브 기능 액세스](~/xamarin-forms/app-fundamentals/dependency-service/index.md)를 참조하세요.

또는 Charles Petzold의 책인 [_Creating Mobile Apps with Xamarin.Forms_](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)(Xamarin.Forms를 사용하여 모바일 앱 만들기)는 Xamarin.Forms에 대해 더 배울 수 있는 좋은 자료입니다. 이 책은 PDF 또는 다양한 전자책 형식으로 제공됩니다.

## <a name="related-links"></a>관련 링크

- [WebServiceTutorial(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-tutorials-webservicetutorial/)
- [RESTful Web Service 사용(가이드)](~/xamarin-forms/data-cloud/web-services/rest.md)
- [Newtonsoft.Json NuGet 패키지](https://www.nuget.org/packages/Newtonsoft.Json/)
