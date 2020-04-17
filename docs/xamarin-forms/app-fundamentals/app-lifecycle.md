---
title: Xamarin.Forms 앱 수명 주기
description: 이 문서에서는 수명 주기 메서드, 페이지 알림 이벤트 및 모달 탐색 이벤트를 포함하여 애플리케이션 수명 주기에 응답하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 69B416CF-B243-4790-AB29-F030B32465BE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: 41e8d073982bf7963b3a77a939bf28e52e86feaa
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "67675185"
---
# <a name="xamarinforms-app-lifecycle"></a>Xamarin.Forms 앱 수명 주기

[`Application`](xref:Xamarin.Forms.Application) 기본 클래스에서 제공하는 기능은 다음과 같습니다.

- [수명 주기 메서드](#Lifecycle_Methods) `OnStart`, `OnSleep` 및 `OnResume`.
- [페이지 탐색 이벤트](#page) [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing), [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing).
- [모달 탐색 이벤트](#modal) `ModalPushing`, `ModalPushed`, `ModalPopping` 및 `ModalPopped`.

<a name="Lifecycle_Methods" />

## <a name="lifecycle-methods"></a>수명 주기 메서드

[`Application`](xref:Xamarin.Forms.Application) 클래스에는 수명 주기 변경에 응답하기 위해 재정의할 수 있는 다음 세 가지 가상 메서드가 포함되어 있습니다.

- `OnStart` - 애플리케이션이 시작되면 호출됩니다.
- `OnSleep` - 애플리케이션이 백그라운드로 전환될 때마다 호출됩니다.
- `OnResume` - 애플리케이션이 백그라운드로 전환된 후 다시 시작될 때 호출됩니다.

> [!NOTE]
> 애플리케이션 종료 메서드가 *없습니다*. 정상적인 상황(즉, 충돌이 아닌 경우)에서는 코드에 추가 알림이 없이 *OnSleep* 상태에서 애플리케이션이 종료할 수 있습니다.

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

> [!IMPORTANT]
> Android에서는 기본 작업의 `[Activity()]` 특성에 `ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation`이 없는 경우 애플리케이션이 처음 시작될 때 `OnStart` 메서드가 호출됩니다.

<a name="page" />

## <a name="page-notification-events"></a>페이지 알림 이벤트

[`Application`](xref:Xamarin.Forms.Application) 클래스에는 나타나는 페이지와 사라지는 페이지에 대한 알림을 제공하는 두 개의 이벤트가 있습니다.

- [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) - 페이지가 화면에 나타나려고 할 때 발생합니다.
- [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing) - 페이지가 화면에서 사라지려고 할 때 발생합니다.

이러한 이벤트는 화면에 나타나는 페이지를 추적하려는 시나리오에서 사용할 수 있습니다.

> [!NOTE]
> [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) 및 [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing) 이벤트는 각각 [`Page.Appearing`](xref:Xamarin.Forms.Page.Appearing) 및 [`Page.Disappearing`](xref:Xamarin.Forms.Page.Disappearing) 이벤트가 발생한 직후에 [`Page`](xref:Xamarin.Forms.Page) 기본 클래스에서 발생합니다.

<a name="modal" />

## <a name="modal-navigation-events"></a>모달 탐색 이벤트

[`Application`](xref:Xamarin.Forms.Application) 클래스에는 각각 자체의 고유한 이벤트 인수가 있는 4개의 이벤트가 있으며, 이를 통해 표시 및 해제되는 모달 페이지에 응답할 수 있습니다.

- `ModalPushing` -페이지가 모달 형식으로 푸시될 때 발생합니다.
- `ModalPushed` -페이지가 모달 형식으로 푸시된 후에 발생합니다.
- `ModalPopping` -페이지가 모달 형식으로 팝될 때 발생합니다.
- `ModalPopped` -페이지가 모달 형식으로 팝된 후에 발생합니다.

> [!NOTE]
> `ModalPoppingEventArgs` 형식의 `ModalPopping` 이벤트 인수는 `Cancel` 속성을 포함합니다. `Cancel`이 `true`로 설정되면 모달 팝이 취소됩니다.
