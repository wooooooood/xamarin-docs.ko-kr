---
title: Xamarin.Forms 앱 수명 주기
description: 이 문서에서는 수명 주기 메서드, 페이지 탐색 이벤트 및 모달 탐색 이벤트를 포함하여 애플리케이션 수명 주기에 응답하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 69B416CF-B243-4790-AB29-F030B32465BE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: 5bdce8d1752b3d7273ffec233b120266909999c4
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675122"
---
# <a name="xamarinforms-app-lifecycle"></a>Xamarin.Forms 앱 수명 주기

[`Application`](xref:Xamarin.Forms.Application) 기본 클래스에서 제공하는 기능은 다음과 같습니다.

* [수명 주기 메서드](#Lifecycle_Methods): `OnStart`, `OnSleep` 및 `OnResume`.
* [페이지 탐색 이벤트](#page): [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing), [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing).
* [모달 탐색 이벤트](#modal): `ModalPushing`, `ModalPushed`, `ModalPopping` 및 `ModalPopped`.

<a name="Lifecycle_Methods" />

## <a name="lifecycle-methods"></a>수명 주기 메서드

[`Application`](xref:Xamarin.Forms.Application) 클래스에는 수명 주기 메서드를 처리하기 위해 재정의할 수 있는 다음 세 가지 가상 메서드가 포함되어 있습니다.

* **OnStart** - 애플리케이션이 시작될 때 호출됩니다.

* **OnSleep** - 애플리케이션이 백그라운드로 전환될 때마다 호출됩니다.

* **OnResume** - 애플리케이션이 백그라운드로 전환 된 후 다시 시작될 때 호출됩니다.

애플리케이션 종료에 대한 메서드는 *없습니다*.
정상적인 상황(즉, 충돌이 아닌 경우)에서는 코드에 추가 알림이 없이 *OnSleep* 상태에서 애플리케이션이 종료할 수 있습니다.

이러한 메서드가 호출되는 경우를 살펴보려면 각각에 `WriteLine` 호출을 구현하고(아래 참조) 각 플랫폼에서 테스트합니다.

```csharp
protected override void OnStart()
{
    Debug.WriteLine ("OnStart");
}
protected override void OnSleep()
{
    Debug.WriteLine ("OnSleep");
}
protected override void OnResume()
{
    Debug.WriteLine ("OnResume");
}
```

*이전* Xamarin.Forms 애플리케이션(예: Xamarin.Forms 1.3 이하에서 만듦)을 업데이트할 때 Android 기본 활동에 `[Activity()]` 특성의 `ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation`이 포함되어 있는지 확인합니다. 이 항목이 없으면 애플리케이션이 처음 시작될 때뿐만 회전할 때도 `OnStart` 메서드가 호출된다는 것을 알 수 있습니다. 이 특성은 현재 Xamarin.Forms 애플리케이션 템플릿에 자동으로 포함됩니다.

<a name="page" />

## <a name="page-navigation-events"></a>페이지 탐색 이벤트

[`Application`](xref:Xamarin.Forms.Application) 클래스에는 나타나는 페이지와 사라지는 페이지에 대한 알림을 제공하는 두 개의 이벤트가 있습니다.

- [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) - 페이지가 화면에 나타나려고 할 때 발생합니다.
- [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing) - 페이지가 화면에서 사라지려고 할 때 발생합니다.

이러한 이벤트는 페이지에 나타나는 페이지를 추적하려는 시나리오에서 사용할 수 있습니다.

> [!NOTE]
> [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) 및 [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing) 이벤트는 각각 [`Page.Appearing`](xref:Xamarin.Forms.Page.Appearing) 및 [`Page.Disappearing`](xref:Xamarin.Forms.Page.Disappearing) 이벤트가 발생한 직후에 [`Page`](xref:Xamarin.Forms.Page) 기본 클래스에서 발생합니다.

<a name="modal" />

## <a name="modal-navigation-events"></a>모달 탐색 이벤트

[`Application`](xref:Xamarin.Forms.Application) 클래스에는 각각 자체의 고유한 이벤트 인수가 있는 4개의 이벤트가 있으며, 이를 통해 표시 및 해제되는 모달 페이지에 응답할 수 있습니다.

* **ModalPushing** - `ModalPushingEventArgs`
* **ModalPushed** - `ModalPushedEventArgs`
* **ModalPopping** - `ModalPoppingEventArgs` 클래스에 `Cancel` 속성이 포함되어 있습니다. `Cancel`이 `true`로 설정되면 모달 팝이 취소됩니다.
* **ModalPopped** - `ModalPoppedEventArgs`

> [!NOTE]
> 애플리케이션 수명 주기 메서드 및 모달 탐색 이벤트를 구현하기 위해 Xamarin.Forms 애플리케이션(즉, 정적 `GetMainPage` 메서드를 사용하는 버전 1.2 이하에서 작성된 애플리케이션)을 만드는 모든 사전 `Application` 메서드가 `MainPage`의 부모 항목으로 설정된 기본 `Application`을 만들도록 업데이트되었습니다.
>
> 이 레거시 동작을 사용하는 Xamarin.Forms 애플리케이션은 [Application 클래스](~/xamarin-forms/app-fundamentals/application-class.md) 페이지에서 설명한 대로 `Application` 하위 클래스로 업데이트해야 합니다.
