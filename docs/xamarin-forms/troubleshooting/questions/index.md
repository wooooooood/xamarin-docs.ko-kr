---
title: Xamarin.Forms질문과 대답
ms.topic: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: edd6cfefe18ff3d5cc97ec58f3bce867f11df7c8
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84135878"
---
# <a name="xamarinforms-frequently-asked-questions"></a>Xamarin.Forms질문과 대답

## <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-packageupdate-forms-templatemd"></a>[Xamarin.Forms기본 템플릿을 최신 NuGet 패키지로 업데이트할 수 있나요?](update-forms-template.md)
이 가이드에서는 Xamarin.Forms .NET Standard 라이브러리 템플릿을 예로 사용 하지만 동일한 일반 메서드는 Xamarin.Forms 공유 프로젝트 템플릿에도 적용 됩니다.

## <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-filesforms-xaml-designermd"></a>[Visual Studio XAML 디자이너가 xaml 파일에 대해 작동 하지 않는 이유는 무엇 Xamarin.Forms 인가요?](forms-xaml-designer.md)
Xamarin.Forms는 현재 XAML 파일에 대 한 비주얼 디자이너를 지원 하지 않습니다.

## <a name="android-build-error-the-linkassemblies-task-failed-unexpectedly"></a>[Android 빌드 오류: "LinkAssemblies" 작업이 예기치 않게 실패 했습니다.](android-linkassemblies-error.md)
`The "LinkAssemblies" task failed unexpectedly`양식을 사용 하는 Xamarin Android 프로젝트를 빌드할 때 오류 메시지가 표시 될 수 있습니다. 이는 링커가 활성 상태일 때 (일반적으로 *릴리스* 빌드에서 앱 패키지의 크기를 줄이기 위해) 발생 합니다. Android 대상이 최신 프레임 워크로 업데이트 되지 않기 때문에 발생 합니다. 

## <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik--unexpected-top-level-errormaps-compiletodalvik-errormd"></a>["왜 Xamarin.Forms 합니다. 지도 Android 프로젝트가 COMPILETODALVIK로 실패 함: 예기치 않은 최상위 오류? "](maps-compiletodalvik-error.md)
이 오류는 Mac용 Visual Studio의 오류 패드 또는 Visual Studio의 빌드 출력 창에서 볼 수 있습니다. Android 프로젝트에서를 사용 Xamarin.Forms 합니다. 맵. Xamarin Android 프로젝트에 대 한 Java 힙 크기를 높여 가장 일반적으로 해결 됩니다.
