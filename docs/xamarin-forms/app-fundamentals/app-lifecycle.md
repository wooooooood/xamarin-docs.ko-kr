---
title: Xamarin.Forms 앱 수명 주기
description: 이 문서에서는 수명 주기 메서드, 페이지 탐색 이벤트 및 모달 탐색 이벤트를 포함 하는 응용 프로그램 수명 주기에 응답 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 69B416CF-B243-4790-AB29-F030B32465BE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: 5bdce8d1752b3d7273ffec233b120266909999c4
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675122"
---
# <a name="xamarinforms-app-lifecycle"></a>Xamarin.Forms 앱 수명 주기

합니다 [ `Application` ](xref:Xamarin.Forms.Application) 기본 클래스는 다음 기능을 제공 합니다.

* [수명 주기 메서드](#Lifecycle_Methods) `OnStart`하십시오 `OnSleep`, 및 `OnResume`합니다.
* [탐색 이벤트를 페이지](#page) [ `PageAppearing` ](xref:Xamarin.Forms.Application.PageAppearing)하십시오 [ `PageDisappearing` ](xref:Xamarin.Forms.Application.PageDisappearing)합니다.
* [모달 탐색 이벤트](#modal) `ModalPushing`하십시오 `ModalPushed`, `ModalPopping`, 및 `ModalPopped`합니다.

<a name="Lifecycle_Methods" />

## <a name="lifecycle-methods"></a>수명 주기 메서드

합니다 [ `Application` ](xref:Xamarin.Forms.Application) 클래스 수명 주기 메서드를 처리 하도록 재정의할 수 있는 세 가지 가상 메서드를 포함 합니다.

* **OnStart** -응용 프로그램을 시작할 때 호출 됩니다.

* **OnSleep** -응용 프로그램이 백그라운드 이동 될 때마다 호출 됩니다.

* **OnResume** -백그라운드에 전송 된 후, 응용 프로그램 다시 시작 될 때 호출 됩니다.

보면 *없습니다* 응용 프로그램 종료에 대 한 메서드.
정상적인 상황 (예: 하지 충돌) 응용 프로그램 종료에서 발생 합니다 *OnSleep* 코드에 모든 추가 알림 없이 상태입니다.

이러한 메서드를 호출 하는 경우를 살펴 보려면 구현 된 `WriteLine` (아래와 같이) 각 호출 및 각 플랫폼에서 테스트 합니다.

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

업데이트할 때 *이전* Xamarin.Forms 응용 프로그램 (예: Xamarin.Forms 1.3 또는 이전 버전)을 만들려면 Android 기본 활동이 포함 되어 있는지 확인 `ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation` 에 `[Activity()]` 특성입니다. 보게 될 것이 없는 경우는 `OnStart` 메서드는 회전 및 응용 프로그램을 처음 시작할 때 호출 됩니다. 이 특성은 현재 Xamarin.Forms 앱 템플릿에서 자동으로 포함 됩니다.

<a name="page" />

## <a name="page-navigation-events"></a>페이지 탐색 이벤트

에 두 개의 이벤트를 [ `Application` ](xref:Xamarin.Forms.Application) 표시 되었다가 사라질 페이지에 대 한 알림을 제공 하는 클래스:

- [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) -페이지 화면에 표시 되려고 할 때 발생 합니다.
- [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing) -페이지 화면에서 사라집니다 되려고 할 때 발생 합니다.

이러한 이벤트 화면에 표시 되는 것으로 페이지를 추적 하려는 시나리오에서 사용할 수 있습니다.

> [!NOTE]
> 합니다 [ `PageAppearing` ](xref:Xamarin.Forms.Application.PageAppearing) 및 [ `PageDisappearing` ](xref:Xamarin.Forms.Application.PageDisappearing) 이벤트에서 발생 하는 [ `Page` ](xref:Xamarin.Forms.Page) 기본 클래스 바로 뒤를 [ `Page.Appearing` ](xref:Xamarin.Forms.Page.Appearing) 하 고 [ `Page.Disappearing` ](xref:Xamarin.Forms.Page.Disappearing) 이벤트, 각각.

<a name="modal" />

## <a name="modal-navigation-events"></a>모달 탐색 이벤트

에 4 개의 이벤트를 [ `Application` ](xref:Xamarin.Forms.Application) 표시 되 고 해제 하는 모달 페이지에 응답할 수 있도록 하는 자체 이벤트 인수를 사용 하 여 각 클래스:

* **ModalPushing** - `ModalPushingEventArgs`
* **ModalPushed** - `ModalPushedEventArgs`
* **ModalPopping** - `ModalPoppingEventArgs` 클래스를 포함 한 `Cancel` 속성입니다. 때 `Cancel` 로 설정 된 `true` 모달 팝업을 취소 합니다.
* **ModalPopped** - `ModalPoppedEventArgs`

> [!NOTE]
> 모든 사전 모달 탐색 이벤트를 확인 하 고 응용 프로그램 수명 주기 메서드를 구현 하`Application` Xamarin.Forms 앱을 작성 하는 방법 (즉, 작성 된 응용 프로그램 버전 1.2 또는 이전 버전에서 정적을 사용 하는 `GetMainPage` 메서드)를 만드는 업데이트 된를 기본 `Application` 의 부모로 설정 됩니다 `MainPage`합니다.
>
> 이 레거시 동작을 사용 하는 Xamarin.Forms 응용 프로그램을 업데이트 해야 합니다는 `Application` 에 설명 된 대로 하위 클래스를 [응용 프로그램 클래스](~/xamarin-forms/app-fundamentals/application-class.md) 페이지입니다.
