---
title: Xamarin.Forms에 대 한 질문과 대답
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 89364175-53BA-4A09-B3E2-44AC67DD971C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 24e2ae456e478585f30aa704917f66bb0bf11da9
ms.sourcegitcommit: 650458de1d362cd7de174cacef7838f0e74426f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/15/2019
ms.locfileid: "57981642"
---
# <a name="xamarinforms-frequently-asked-questions"></a>Xamarin.Forms에 대 한 질문과 대답

## <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-packageupdate-forms-templatemd"></a>[Xamarin.Forms 기본 템플릿을 최신 NuGet 패키지로 업데이트할 수 있나요?](update-forms-template.md)
이 가이드에서는 예를 들어, Xamarin.Forms.NET Standard 라이브러리 템플릿을 사용 하지만 동일한 일반 메서드는 Xamarin.Forms 공유 프로젝트 템플릿에 대해 에서도 작동 합니다.

## <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-filesforms-xaml-designermd"></a>[Visual Studio XAML 디자이너가 Xamarin.Forms XAML 파일에 대해 작동하지 않는 이유는 무엇인가요?](forms-xaml-designer.md)
Xamarin.Forms는 XAML 파일에 대 한 현재 비주얼 디자이너를 지원 하지 않습니다.

## <a name="android-build-error-the-linkassemblies-task-failed-unexpectedlyandroid-linkassemblies-errormd"></a>[Android 빌드 오류: "LinkAssemblies" 작업이 예기치 않게 실패했습니다.](android-linkassemblies-error.md)
오류 메시지가 표시 될 수 있습니다 `The "LinkAssemblies" task failed unexpectedly` Forms를 사용 하는 Xamarin.Android 프로젝트를 빌드할 경우. 링커 활성화 되어 있을 때 이런 (에서 일반적으로 *릴리스* 앱 패키지의 크기를 줄이기 위해 빌드); Android 대상을 최신 프레임 워크에 업데이트 되지 않은 때문에 발생 하 고 합니다. 

## <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik--unexpected-top-level-errormaps-compiletodalvik-errormd"></a>["이유는 Xamarin.Forms.Maps Android 프로젝트가 실패 COMPILETODALVIK: 예기치 않은 최상위 오류 인가요? "](maps-compiletodalvik-error.md)
Mac 또는 Visual Studio의 빌드 출력 창의 오류 패드의 Visual Studio에서이 오류가 표시 될 수 있습니다. Xamarin.Forms.Maps를 사용 하 여 Android 프로젝트입니다. Xamarin.Android 프로젝트에 대 한 Java 힙 크기를 늘려 가장 일반적으로 확인 됩니다.
