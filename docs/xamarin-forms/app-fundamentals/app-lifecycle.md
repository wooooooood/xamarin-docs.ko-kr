---
title: 앱 수명 주기
description: 응용 프로그램 수명 주기에 응답 하는 방법
ms.prod: xamarin
ms.assetid: 69B416CF-B243-4790-AB29-F030B32465BE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: a22ad8f3f272212f5c7f088ba2112f2771ff4a7f
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34846346"
---
# <a name="app-lifecycle"></a>앱 수명 주기

[ `Application` ](xref:Xamarin.Forms.Application) 기본 클래스는 다음과 같은 기능을 제공 합니다.

* [수명 주기 메서드](#Lifecycle_Methods) `OnStart`, `OnSleep`, 및 `OnResume`합니다.
* [페이지 탐색 이벤트](#page) [ `PageAppearing` ](xref:Xamarin.Forms.Application.PageAppearing), [ `PageDisappearing` ](xref:Xamarin.Forms.Application.PageDisappearing)합니다.
* [모달 탐색 이벤트](#modal) `ModalPushing`, `ModalPushed`, `ModalPopping`, 및 `ModalPopped`합니다.

<a name="Lifecycle_Methods" />

## <a name="lifecycle-methods"></a>수명 주기 메서드

[ `Application` ](xref:Xamarin.Forms.Application) 수명 주기 메서드를 처리 하는 재정의 될 수 있는 세 개의 가상 메서드를 포함 하는 클래스:

* **OnStart** -응용 프로그램이 시작 될 때 호출 합니다.

* **OnSleep** -백그라운드 응용 프로그램이 이동 될 때마다 호출 됩니다.

* **OnResume** -백그라운드도 전송 된 후, 응용 프로그램 다시 시작 될 때 호출 합니다.

*없는* 응용 프로그램 종료에 대 한 메서드.
정상적인 상황 (ie. 충돌 하지)에서 발생 하는 응용 프로그램 종료는 *OnSleep* 코드에 추가 알림이 없이 상태입니다.

이러한 메서드를 호출 하는 경우를 살펴 보려면 구현는 `WriteLine` (아래와 같이) 각 호출 및 각 플랫폼에서 테스트 합니다.

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

업데이트할 때 *이전* Xamarin.Forms 응용 프로그램 (예:. Xamarin.Forms 1.3 이상 만들기), Android 기본 활동에 포함 되는지 확인 `ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation` 에 `[Activity()]` 특성입니다. 볼 수 없으면이 `OnStart` 메서드는 회전와 응용 프로그램이 처음 시작 될 때 호출 됩니다. 이 특성은 현재 Xamarin.Forms 응용 프로그램 템플릿에 자동으로 포함 됩니다.

<a name="page" />

## <a name="page-navigation-events"></a>페이지 탐색 이벤트

에 이벤트가 두 가지는 [ `Application` ](xref:Xamarin.Forms.Application) 나타나고 사라지면 페이지에 대 한 알림을 제공 하는 클래스:

- [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) -페이지가 화면에 표시 되려고 할 때 발생 합니다.
- [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing) -페이지가 화면에서 사라지고 되려고 할 때 발생 합니다.

이러한 이벤트를 화면에 표시 되는 때이 페이지를 추적 하려는 시나리오에서 사용할 수 있습니다.

> [!NOTE]
> [ `PageAppearing` ](xref:Xamarin.Forms.Application.PageAppearing) 및 [ `PageDisappearing` ](xref:Xamarin.Forms.Application.PageDisappearing) 에서 발생 하는 [ `Page` ](xref:Xamarin.Forms.Page) 기본 클래스 바로 뒤의 [ `Page.Appearing` ](xref:Xamarin.Forms.Page.Appearing) 및 [ `Page.Disappearing` ](xref:Xamarin.Forms.Page.Disappearing) 이벤트, 각각.

<a name="modal" />

## <a name="modal-navigation-events"></a>모달 탐색 이벤트

4 이벤트는에 [ `Application` ](xref:Xamarin.Forms.Application) 모달 페이지가 표시 되 고 해제에 응답할 수 있도록 자신의 이벤트 인수를 사용 하는 각 클래스:

* **ModalPushing** - `ModalPushingEventArgs`
* **ModalPushed** - `ModalPushedEventArgs`
* **ModalPopping** - `ModalPoppingEventArgs` 클래스를 포함 한 `Cancel` 속성입니다. 때 `Cancel` 로 설정 된 `true` 모달 pop 취소 됩니다.
* **ModalPopped** - `ModalPoppedEventArgs`

> [!NOTE]
> 응용 프로그램 수명 주기 메서드 및 모든 사전 모달 탐색 이벤트를 구현 하기 위해`Application` Xamarin.Forms 응용 프로그램을 작성 하는 방법 (즉. 정적을 사용 하는 응용 프로그램 버전 1.2 또는 이전 버전에서 작성 된 `GetMainPage` 메서드) 만들 수 있도록 업데이트 되었습니다는 기본 `Application` 의 부모로 설정 된 `MainPage`합니다.
>
> 이 레거시 동작을 사용 하는 Xamarin.Forms 응용 프로그램을 업데이트 해야는 `Application` 에 설명 된 대로 하위 클래스는 [응용 프로그램 클래스](~/xamarin-forms/app-fundamentals/application-class.md) 페이지.
