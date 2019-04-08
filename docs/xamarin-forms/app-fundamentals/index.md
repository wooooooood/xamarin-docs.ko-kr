---
title: Xamarin.Forms 애플리케이션 기본 사항
description: 필요한 모든 핵심 개념을 비롯하여 접근성 및 지역화와 같은 마무리 작업에 이르는 Xamarin.Forms 애플리케이션 개발에 대한 기본 사항을 알아봅니다.
ms.prod: xamarin
ms.assetid: 7B516BBC-F7E1-4387-9779-7754E2E69723
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/08/2018
ms.openlocfilehash: 2178c9f4115c42396635e22cb0688695b590ec26
ms.sourcegitcommit: 817d26585093cd180a36b28179eb354b0eb900b3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55292157"
---
# <a name="xamarinforms-application-fundamentals"></a>Xamarin.Forms 애플리케이션 기본 사항

## <a name="accessibilityaccessibilityindexmd"></a>[액세스 가능성](accessibility/index.md)

Xamarin.Forms로 액세스 가능한 기능(예: 화면 판독 도구 지원)을 통합하는 팁입니다.

## <a name="app-classapplication-classmd"></a>[앱 클래스](application-class.md)

`Application` 클래스는 Xamarin.Forms의 시작점입니다. 초기 페이지를 설정하려면 모든 앱에 서브클래스 `App`을 구현해야 합니다. 간단한 데이터 스토리지를 위한 `Properties` 컬렉션도 제공합니다. 이것은 C# 또는 XAML로 정의할 수 있습니다.

## <a name="app-lifecycleapp-lifecyclemd"></a>[앱 수명 주기](app-lifecycle.md)

`Application` 클래스 `OnStart`, `OnSleep` 및 `OnResume` 메서드는 물론 모달 탐색 이벤트를 사용하면 사용자 지정 코드로 애플리케이션 수명 주기 이벤트를 처리할 수 있습니다.

## <a name="application-indexing-and-deep-linkingdeep-linkingmd"></a>[애플리케이션 인덱싱 및 딥 링크 설정](deep-linking.md)

애플리케이션 인덱싱은 애플리케이션에서 검색 결과를 표시하면서 관련성 유지를 위해 몇 번 사용 후 잊어벼려도 되는 정보를 허용합니다. 딥 링크 설정은 애플리케이션이 애플리케이션 데이터를 포함하는 검색 결과에 응답할 수 있도록 하며, 일반적으로 딥 링크에서 참조하는 페이지로 이동하면 됩니다.

## <a name="behaviorsbehaviorsindexmd"></a>[동작](behaviors/index.md)

사용자 인터페이스 컨트롤은 기능을 추가하는 동작을 사용하여 서브클래스를 지정하지 않고 쉽게 확장이 가능합니다.

## <a name="custom-rendererscustom-rendererindexmd"></a>[사용자 지정 렌더러](custom-renderer/index.md)

사용자 지정 렌더러를 사용하면 개발자가 Xamarin.Forms 컨트롤의 기본 렌더링을 ‘재정의(override)’하여 각 플랫폼의 모양과 동작을 사용자 지정할 수 있습니다(필요한 경우 네이티브 SDK 사용).

## <a name="data-bindingdata-bindingindexmd"></a>[데이터 바인딩](data-binding/index.md)

데이터 바인딩은 두 개체의 속성을 연결하여 한 속성의 변경 내용이 다른 속성에 자동으로 반영되도록 합니다. 데이터 바인딩은 [MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md)(Model-View-ViewModel) 애플리케이션 아키텍처의 필수적인 부분입니다.

## <a name="dependency-servicedependency-serviceindexmd"></a>[종속성 서비스](dependency-service/index.md)

`DependencyService`는 공유 코드의 인터페이스에 코드를 작성할 수 있도록 간단한 로케이터를 제공하고 자동으로 해결되는 플랫폼별 구현을 제공하기 때문에 Xamarin.Forms의 플랫폼별 기능을 쉽게 참조할 수 있습니다.

## <a name="effectseffectsindexmd"></a>[효과](effects/index.md)

효과를 사용하면 각 플랫폼의 네이티브 컨트롤을 사용자 지정할 수 있습니다. 효과는 일반적으로 작은 스타일 지정 변경에 사용됩니다.

## <a name="filesfilesmd"></a>[파일](files.md)

Xamarin.Forms를 통한 파일 처리는 .NET Standard 라이브러리의 코드를 사용하거나 포함된 리소스를 사용하여 수행할 수 있습니다.

## <a name="gesturesgesturesindexmd"></a>[제스처](gestures/index.md)

Xamarin.Forms [`GestureRecognizer`](xref:Xamarin.Forms.GestureRecognizer) 클래스는 사용자 인터페이스 컨트롤에 탭하기, 손가락 모으기 및 이동 제스처를 지원합니다.

## <a name="localizationlocalizationindexmd"></a>[지역화](localization/index.md)

기본 제공되는 .NET 지역화 프레임워크는 Xamarin.Forms로 플랫폼 간 다국어 애플리케이션을 작성하는 데 사용할 수 있습니다.

## <a name="local-databasesdatabasesmd"></a>[로컬 데이터베이스](databases.md)

Xamarin.Forms는 SQLite 데이터베이스 엔진을 사용하여 데이터베이스 기반 애플리케이션을 지원하기 때문에 공유 코드로 개체를 읽고 저장할 수 있습니다.

## <a name="messaging-centermessaging-centermd"></a>[메시징 센터](messaging-center.md)

Xamarin.Forms `MessagingCenter`를 사용하면 뷰 모델 및 기타 구성 요소가 서로에 대해 알 필요 없이 간단한 메시지 계약으로 통신이 가능합니다.

## <a name="navigationnavigationindexmd"></a>[탐색](navigation/index.md)

Xamarin.Forms는 사용되는 `Page` 형식에 따라 다양한 페이지 탐색 환경을 제공합니다.

## <a name="shellshellmd"></a>[셸](shell.md)

Xamarin.Forms Shell은 애플리케이션의 컨테이너로, 대부분의 애플리케이션에서 필요로 하는 기본 UI 기능을 제공하므로 애플리케이션의 핵심 워크로드에 집중할 수 있습니다.

## <a name="templatestemplatesindexmd"></a>[템플릿](templates/index.md)

컨트롤 템플릿은 런타임에 애플리케이션 페이지에 쉽게 테마를 지정하고 다시 지정할 수 있는 기능을 제공하며, 데이터 템플릿은 지원되는 컨트롤의 데이터 표현을 정의할 수 있는 기능을 제공합니다.

## <a name="triggerstriggersmd"></a>[트리거](triggers.md)

XAML의 속성 변경 및 이벤트에 응답하여 컨트롤을 업데이트합니다.
