---
title: Xamarin.Forms 응용 프로그램 기본 사항
description: 접근성 및 지역화와 같은 마무리 작업을 통해 모든 필요한 핵심 개념을 비롯 하 여 Xamarin.Forms 응용 프로그램 개발의 기본 개념을 탐색 합니다.
ms.prod: xamarin
ms.assetid: 7B516BBC-F7E1-4387-9779-7754E2E69723
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: 515dbd2683619cfcfb7a6c8ecac6bc147265ef7d
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995614"
---
# <a name="xamarinforms-application-fundamentals"></a>Xamarin.Forms 응용 프로그램 기본 사항

## <a name="accessibilityaccessibilityindexmd"></a>[액세스 가능성](accessibility/index.md)

Xamarin.Forms를 사용 하 여 액세스할 수 있는 기능 (예: 화면 판독 도구 지원)를 통합 하기 위한 팁.

## <a name="app-classapplication-classmd"></a>[앱 클래스](application-class.md)

합니다 `Application` 클래스는 Xamarin.Forms에 대 한 시작점 – 하위 클래스를 구현 해야 하는 모든 앱 `App` 초기 페이지를 설정 합니다. 또한 제공 된 `Properties` 간단한 데이터 저장소에 대 한 컬렉션입니다. C# 또는 XAML에서 정의할 수 있습니다.

## <a name="app-lifecycleapp-lifecyclemd"></a>[앱 수명 주기](app-lifecycle.md)

합니다 `Application` 클래스 `OnStart`를 `OnSleep`, 및 `OnResume` 모달 탐색 이벤트 뿐만 아니라 메서드를 사용자 지정 코드를 사용 하 여 응용 프로그램 수명 주기 이벤트를 처리할 수 있습니다.

## <a name="behaviorsbehaviorsindexmd"></a>[동작](behaviors/index.md)

기능을 추가 하려면 동작을 사용 하 여 하위 클래스 지정 하지 않고 사용자 인터페이스 컨트롤을 쉽게 확장할 수 있습니다.

## <a name="custom-rendererscustom-rendererindexmd"></a>[사용자 지정 렌더러](custom-renderer/index.md)

사용자 지정 렌더링 'override' 모양과 동작 (필요한 경우 네이티브 Sdk를 통해) 각 플랫폼에서 사용자 지정 하려면 Xamarin.Forms 컨트롤의 기본 렌더링 하는 개발자를 수 있습니다.

## <a name="data-bindingdata-bindingindexmd"></a>[데이터 바인딩](data-binding/index.md)

데이터 바인딩이 자동으로 다른 속성에 적용 하는 데 하나의 속성의 변경을 허용 두 개체의 속성을 연결 합니다. 데이터 바인딩은 모델-뷰-ViewModel의 필수적인 부분입니다 ([MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md)) 응용 프로그램 아키텍처입니다.

## <a name="dependency-servicedependency-serviceindexmd"></a>[종속성 서비스](dependency-service/index.md)

`DependencyService` 공유 코드에서 인터페이스를 코딩 하 고 Xamarin.Forms에 플랫폼 특정 기능을 참조할 수 있도록 자동으로 해결 되는 플랫폼 특정 구현을 제공 수 있도록 간단한 로케이터를 제공 합니다.

## <a name="effectseffectsindexmd"></a>[효과](effects/index.md)

효과를 사용자 지정할 수는 각 플랫폼에서 네이티브 컨트롤을 허용 small 스타일 변경에 대 한 일반적으로 사용 됩니다.

## <a name="filesfilesmd"></a>[파일](files.md)

Xamarin.Forms를 사용 하 여 처리 하는 파일은.NET Standard 라이브러리에서 또는 포함 된 리소스를 사용 하 여 코드를 사용 하 여 수행할 수 있습니다.

## <a name="gesturesgesturesindexmd"></a>[제스처](gestures/index.md)

Xamarin.Forms [ `GestureRecognizer` ](xref:Xamarin.Forms.GestureRecognizer) 클래스는 사용자 인터페이스 컨트롤에서 탭, 축소, 및 팬 제스처를 지원 합니다.

## <a name="localizationlocalizationindexmd"></a>[지역화](localization/index.md)

Xamarin.Forms 사용한 플랫폼 간 다국어 응용 프로그램을 빌드하는 기본 제공.net 지역화를 사용할 수 있습니다.

## <a name="local-databasesdatabasesmd"></a>[로컬 데이터베이스](databases.md)

Xamarin.Forms는 로드 하 고 공유 코드에서 개체를 저장할 수 있도록 하는 SQLite 데이터베이스 엔진을 사용 하 여 데이터베이스 기반 응용 프로그램을 지원 합니다.

## <a name="messaging-centermessaging-centermd"></a>[메시지 센터](messaging-center.md)

Xamarin.Forms `MessagingCenter` 모델 및 기타 구성 요소는 간단한 메시지 계약 외에도 서로 대 한 지식이 필요 없이 통신할 수 있도록 확인 합니다.

## <a name="navigationnavigationindexmd"></a>[탐색](navigation/index.md)

Xamarin.Forms는 다양 한에 따라 다른 페이지 탐색 환경 제공 합니다 `Page` 형식이 사용 되는 합니다.

## <a name="templatestemplatesindexmd"></a>[템플릿](templates/index.md)

컨트롤 템플릿은 데이터 템플릿은 지원 되는 컨트롤에 데이터를 정의 하는 기능을 제공 하는 동안 런타임 시 응용 프로그램 페이지 테마 쉽게을 다시 테마를 제공 합니다.

## <a name="triggerstriggersmd"></a>[트리거](triggers.md)

속성 변경 및 XAML에서 이벤트에 응답 하 여 컨트롤을 업데이트 합니다.


## <a name="related-links"></a>관련 링크

- [Xamarin.Forms 소개](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
