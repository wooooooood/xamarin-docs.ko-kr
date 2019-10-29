---
title: 활동 수명 주기
description: 활동은 Android 응용 프로그램의 기본 구성 요소 이며 다양 한 상태에 있을 수 있습니다. 활동 수명 주기는 인스턴스화로 시작 하 여 소멸으로 끝나고, 사이에 많은 상태를 포함 합니다. 활동의 상태가 변경 되 면 적절 한 수명 주기 이벤트 메서드가 호출 되어 상태 변경 작업을 알리고 해당 변경에 맞게 코드를 실행할 수 있습니다. 이 문서에서는 활동의 수명 주기를 검토 하 고 이러한 각 상태 변경이 잘 작동 하 고 신뢰할 수 있는 응용 프로그램에 포함 되는 책임을 설명 합니다.
ms.prod: xamarin
ms.assetid: 05B34788-F2D2-4347-B66B-40AFD7B1D167
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/28/2018
ms.openlocfilehash: 6e69d21bb734f13d220c042535842538306d16c8
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016989"
---
# <a name="activity-lifecycle"></a>활동 수명 주기

_활동은 Android 응용 프로그램의 기본 구성 요소 이며 다양 한 상태에 있을 수 있습니다. 활동 수명 주기는 인스턴스화로 시작 하 여 소멸으로 끝나고, 사이에 많은 상태를 포함 합니다. 활동의 상태가 변경 되 면 적절 한 수명 주기 이벤트 메서드가 호출 되어 상태 변경 작업을 알리고 해당 변경에 맞게 코드를 실행할 수 있습니다. 이 문서에서는 활동의 수명 주기를 검토 하 고 이러한 각 상태 변경이 잘 작동 하 고 신뢰할 수 있는 응용 프로그램에 포함 되는 책임을 설명 합니다._

## <a name="activity-lifecycle-overview"></a>활동 수명 주기 개요

활동은 Android와 관련 된 비정상적인 프로그래밍 개념입니다. 일반적인 응용 프로그램 개발에는 일반적으로 응용 프로그램을 시작 하기 위해 실행 되는 정적 main 메서드가 있습니다. 그러나 Android를 사용 하는 경우 다른 점이 있습니다. 응용 프로그램 내에서 등록 된 작업을 통해 Android 응용 프로그램을 시작할 수 있습니다. 실제로 대부분의 응용 프로그램에는 응용 프로그램 진입점으로 지정 된 특정 활동만 있습니다. 그러나 응용 프로그램이 충돌 하거나 OS에서 종료 되는 경우 OS는 마지막으로 열린 작업 또는 이전 작업 스택 내의 다른 위치에서 응용 프로그램을 다시 시작 하려고 시도할 수 있습니다.
또한 OS가 활성 상태가 아닌 경우 작업을 일시 중지 하 고 메모리가 부족 한 경우 해당 작업을 회수할 수 있습니다. 작업을 다시 시작 하는 경우, 특히 작업이 이전 작업의 데이터에 종속 되는 경우 응용 프로그램에서 해당 상태를 올바르게 복원 하도록 하려면 신중 하 게 고려해 야 합니다.

활동 수명 주기는 OS가 활동의 수명 주기 동안 호출 하는 메서드의 컬렉션으로 구현 됩니다. 이러한 메서드를 통해 개발자는 응용 프로그램의 상태 및 리소스 관리 요구 사항을 충족 하는 데 필요한 기능을 구현할 수 있습니다.

응용 프로그램 개발자는 각 작업의 요구 사항을 분석 하 여 작업 수명 주기에서 노출 해야 하는 메서드를 결정 하는 것이 매우 중요 합니다. 이 작업을 수행 하지 못하면 응용 프로그램 불안정성, 충돌, 리소스 팽창 및 기본 OS 불안정 등이 발생할 수 있습니다.

이 장에서는 다음과 같은 작업 수명 주기를 자세히 살펴봅니다.

- 활동 상태
- 수명 주기 메서드
- 응용 프로그램의 상태 유지

이 단원에는 활동 수명 주기 중에 상태를 효율적으로 저장 하는 방법에 대 한 실용적인 예제를 제공 하는 [연습이](~/android/app-fundamentals/activity-lifecycle/saving-state.md) 포함 되어 있습니다. 이 챕터의 끝 부분에서는 작업 수명 주기와 Android 응용 프로그램에서이를 지 원하는 방법을 이해 해야 합니다.

## <a name="activity-lifecycle"></a>활동 수명 주기

Android 작업 수명 주기는 개발자에 게 리소스 관리 프레임 워크를 제공 하는 활동 클래스 내에 노출 되는 메서드 컬렉션으로 구성 됩니다. 개발자는이 프레임 워크를 사용 하 여 응용 프로그램 내에서 각 작업의 고유한 상태 관리 요구 사항을 충족 하 고 리소스 관리를 올바르게 처리할 수 있습니다.

### <a name="activity-states"></a>활동 상태

Android OS는 해당 상태에 따라 활동을 조정 합니다. 이렇게 하면 Android에서 더 이상 사용 하지 않는 작업을 식별 하 여 OS에서 메모리와 리소스를 회수할 수 있습니다. 다음 다이어그램에서는 활동이 수명 동안 거치는 상태를 보여 줍니다.

[![활동 상태 다이어그램](images/image1-sml.png)](images/image1.png#lightbox)

이러한 상태는 다음과 같이 4 개의 주 그룹으로 나눌 수 있습니다.

1. *활성 또는 실행 중인* &ndash; 활동은 활성 상태 이거나 활동 스택의 맨 위에 있는 포그라운드에 있는 경우 실행 중인 것으로 간주 됩니다. 이 작업은 Android에서 가장 높은 우선 순위로 간주 되며,이로 인해 장치에서 사용 가능한 것 보다 더 많은 메모리를 사용 하려고 시도 하는 경우와 같이 활동에서 UI가 응답 하지 않을 수 있는 것과 같은 극단적인 상황에서 OS에 의해서만 중단 됩니다.

1. *일시 중지* 된 &ndash; 장치가 절전 모드로 전환 되는 경우 또는 작업을 계속 해 서 볼 수 있지만 전체 크기 또는 투명 하지 않은 새 활동에 의해 부분적으로 숨겨져 있는 활동은 일시 중지 된 것으로 간주 됩니다. 일시 중지 된 활동은 여전히 활성 상태입니다. 즉, 모든 상태와 멤버 정보를 유지 관리 하 고 창 관리자에 연결 된 상태를 유지 합니다. 이 작업은 Android에서 우선 순위가 가장 높은 작업으로 간주 되며,이 작업을 중단 하면 활성/실행 중인 활동을 안정적이 고 응답성을 유지 하는 데 필요한 리소스 요구 사항을 충족 하는 경우 OS에 의해서만 중단 됩니다.

1. 다른 활동에 의해 완전히 가려진 *중지/Backgrounded* &ndash; 활동은 중지 된 것으로 간주 되거나 백그라운드에서 진행 되는 것으로 간주 됩니다.
    중지 된 활동은 가능한 한 오랫동안 상태 및 회원 정보를 유지 하려고 시도 하지만 중지 된 활동은 세 가지 상태의 가장 낮은 우선 순위로 간주 됩니다. 따라서 OS는이 상태의 활동을 먼저 중지 하 여 리소스를 충족 합니다. 우선 순위가 더 높은 활동의 요구 사항

1. 작업을 *다시 시작* 하면 Android에서 메모리에서 제거 될 때까지 일시 중지 된 상태에서 일시 중지 됨에서 중지 된 상태의 작업을 수행할 수 &ndash;. 사용자가 작업으로 다시 이동 하는 경우 다시 시작 하 고 이전에 저장 된 상태로 복원한 다음 사용자에 게 표시 해야 합니다.

### <a name="activity-re-creation-in-response-to-configuration-changes"></a>구성 변경에 대 한 응답으로 활동 다시 만들기

더 복잡 한 문제를 위해 Android는 구성 변경 이라는 혼합에서 하나 이상의 렌치를 throw 합니다. 구성 변경은 작업의 구성이 변경 될 때 발생 하는 신속한 작업 소멸/재평가 주기입니다. 예를 들어, 장치를 [회전](~/android/app-fundamentals/handling-rotation.md) 하는 경우, 즉 작업을 가로 또는 세로 모드로 다시 작성 해야 하는 경우가 여기에 해당 합니다. 키보드가 표시 됩니다 (그리고 활동은 스스로 크기를 조정할 수 있는 기회 제공 됨) 또는 장치가 도크에 배치 되는 경우에 표시 됩니다.

구성 변경으로 인해 작업을 중지 하 고 다시 시작 하는 동안 발생 하는 동일한 작업 상태 변경이 발생 합니다. 그러나 구성 변경 중에 응용 프로그램의 응답성이 향상 되 고 성능이 향상 되도록 하려면 가능한 한 빨리 처리 해야 합니다. 이로 인해 Android는 구성 변경 중 상태를 유지 하는 데 사용할 수 있는 특정 API를 포함 합니다.
이에 대 한 자세한 지침은 [수명 주기 전체에서 상태 관리](~/android/app-fundamentals/activity-lifecycle/index.md#Managing_State_Throughout_the_Lifecycle) 섹션을 진행 합니다.

### <a name="activity-lifecycle-methods"></a>활동 수명 주기 메서드

Android SDK 및 확장에서 Xamarin.ios 프레임 워크는 응용 프로그램 내의 활동 상태를 관리 하기 위한 강력한 모델을 제공 합니다. 활동의 상태가 변경 되는 경우 활동에는 해당 활동에 대 한 특정 메서드를 호출 하는 OS의 알림이 표시 됩니다. 다음 다이어그램에서는 활동 수명 주기와 관련 된 이러한 메서드를 보여 줍니다.

[![활동 수명 주기 순서도](images/image2-sml.png)](images/image2.png#lightbox)

개발자는 활동 내에서 이러한 메서드를 재정의 하 여 상태 변경을 처리할 수 있습니다. 그러나 모든 수명 주기 메서드는 UI 스레드에서 호출 되며, OS가 현재 작업을 숨기 거 나 새 작업을 표시 하는 등의 다음 UI 작업을 수행 하지 못하도록 차단 합니다. 따라서 이러한 메서드의 코드는 응용 프로그램을 제대로 수행 하기 위해 가능한 한 간단 하 게 만들어야 합니다. 백그라운드 스레드에서 장기 실행 작업을 실행 해야 합니다.

이러한 각 수명 주기 방법과 용도를 살펴보겠습니다.

#### <a name="oncreate"></a>OnCreate

[OnCreate](xref:Android.App.Activity.OnCreate*) 은 활동을 만들 때 호출 되는 첫 번째 메서드입니다.
`OnCreate`는 다음과 같은 작업에 필요할 수 있는 모든 시작 초기화를 수행 하도록 항상 재정의 됩니다.

- 뷰 만들기
- 변수 초기화
- 정적 데이터를 목록에 바인딩

번들이 null이 아닌 경우 활동 간에 상태 정보 및 개체를 저장 하 고 전달 하기 위한 사전 인 [번들](xref:Android.OS.Bundle) 매개 변수를 사용 합니다 .이는 활동이 다시 시작 되는 것을 나타내며 이전에는 해당 상태를 복원 해야 합니다. `OnCreate` 인스턴스. 다음 코드는 번들에서 값을 검색 하는 방법을 보여 줍니다.

```csharp
protected override void OnCreate(Bundle bundle)
{
   base.OnCreate(bundle);

   string intentString;
   bool intentBool;

   if (bundle != null)
   {
      intentString = bundle.GetString("myString");
      intentBool = bundle.GetBoolean("myBool");
   }

   // Set our view from the "main" layout resource
   SetContentView(Resource.Layout.Main);
}
```

`OnCreate` 완료 되 면 Android에서 `OnStart`를 호출 합니다.

#### <a name="onstart"></a>OnStart

`OnCreate` 완료 되 면 항상 [OnStart](xref:Android.App.Activity.OnStart) 가 시스템에 의해 호출 됩니다. 활동 내에서 뷰의 현재 값을 새로 고치는 것과 같이 활동은 활동을 표시 하기 바로 전에 특정 작업을 수행 해야 하는 경우이 메서드를 재정의할 수 있습니다. Android는이 메서드 바로 다음에 `OnResume`를 호출 합니다.

#### <a name="onresume"></a>OnResume

활동에서 사용자와의 상호 작용을 시작할 준비가 되 면 시스템에서 [Onresume](xref:Android.App.Activity.OnResume) 을 호출 합니다.
작업은 다음과 같은 작업을 수행 하기 위해이 메서드를 재정의 해야 합니다.

- 프레임 속도 증가 (게임 개발의 일반 작업)
- 애니메이션 시작
- GPS 업데이트 수신 대기
- 관련 경고 또는 대화 상자 표시
- 외부 이벤트 처리기 연결

예를 들어 다음 코드 조각에서는 카메라를 초기화 하는 방법을 보여 줍니다.

```csharp
public void OnResume()
{
    base.OnResume(); // Always call the superclass first.

    if (_camera==null)
    {
        // Do camera initializations here
    }
}
```

`OnPause`에서 수행 되는 모든 작업은 활동을 다시 생명으로 가져올 때 `OnPause` 후에 실행 되도록 보장 되는 유일한 수명 주기 방법 이기 때문에 `OnResume`에서 수행 되지 않아야 하므로 `OnResume`는 중요 합니다.

#### <a name="onpause"></a>OnPause

[Onpause](xref:Android.App.Activity.OnPause) 는 시스템이 작업을 백그라운드에 배치 하려고 하거나 활동이 부분적으로 가려져 있을 때 호출 됩니다. 작업은 다음을 수행 해야 하는 경우이 메서드를 재정의 해야 합니다.

- 영구 데이터에 저장 하지 않은 변경 내용 커밋

- 리소스를 소비 하는 다른 개체 제거 또는 정리

- 프레임 속도 아래로 램프 및 애니메이션 일시 중지

- 외부 이벤트 처리기 또는 알림 처리기를 등록 취소 합니다 (즉, 서비스에 연결 된 처리기). 작업 메모리 누수가 발생 하지 않도록 하려면이 작업을 수행 해야 합니다.

- 마찬가지로, 활동이 대화 상자 또는 경고를 표시 한 경우에는 `.Dismiss()` 메서드를 사용 하 여 정리 해야 합니다.

예를 들어 다음 코드 조각은 활동을 일시 중지 하는 동안 사용할 수 없으므로 카메라를 해제 합니다.

```csharp
public void OnPause()
{
    base.OnPause(); // Always call the superclass first

    // Release the camera as other activities might need it
    if (_camera != null)
    {
        _camera.Release();
        _camera = null;
    }
}
```

`OnPause`후에 호출 될 수 있는 두 가지 수명 주기 방법이 있습니다.

1. 작업이 포그라운드로 반환 될 경우 `OnResume`가 호출 됩니다.
1. 작업이 백그라운드에 배치 되는 경우 `OnStop`가 호출 됩니다.

#### <a name="onstop"></a>OnStop

작업이 더 이상 사용자에 게 표시 되지 않을 때 [OnStop](xref:Android.App.Activity.OnStop) 가 호출 됩니다. 이는 다음 중 하나가 발생할 때 발생 합니다.

- 새 활동이 시작 되 고 있으며이 활동을 처리 하 고 있습니다.
- 기존 작업을 포그라운드로 가져오는 중인 경우
- 활동을 제거 하 고 있습니다.

`OnStop`는 Android가 리소스에 대해 않다거나 하 고 작업을 올바르게 백그라운드에서 사용할 수 없는 경우와 같이 메모리 부족 상황에서 항상 호출 될 수 있습니다. 따라서 소멸을 위해 활동을 준비할 때 호출 `OnStop`를 사용 하지 않는 것이 좋습니다. 이 작업을 진행 하는 경우에 호출 될 수 있는 다음 수명 주기 메서드 또는 작업이 사용자와 상호 작용 하는 데 다시 제공 되는 경우 `OnRestart` `OnDestroy` 됩니다.

#### <a name="ondestroy"></a>OnDestroy

[Ondestroy](xref:Android.App.Activity.OnDestroy) 은 작업 인스턴스에서 제거 되 고 메모리에서 완전히 제거 되기 전에 호출 되는 최종 메서드입니다. 극단적인 상황에서 Android는 활동을 호스트 하는 응용 프로그램 프로세스를 중단할 수 있습니다. 그러면 `OnDestroy` 호출 되지 않습니다. 대부분의 정리 및 종료가 `OnPause` 및 `OnStop` 메서드에서 수행 되었으므로 대부분의 작업은이 메서드를 구현 하지 않습니다. 일반적으로 `OnDestroy` 메서드는 리소스 누수가 발생할 수 있는 장기 실행 리소스를 정리 하기 위해 재정의 됩니다. 이에 대 한 예는 `OnCreate`에서 시작 된 백그라운드 스레드 일 수 있습니다.

활동이 제거 된 후에는 수명 주기 메서드가 호출 되지 않습니다.

#### <a name="onrestart"></a>OnRestart

[Onrestart](xref:Android.App.Activity.OnRestart) 는 작업이 중지 된 후 다시 시작 되기 전에 호출 됩니다. 이에 대 한 좋은 예는 사용자가 응용 프로그램의 활동에서 홈 단추를 누를 때입니다. 이 경우 `OnPause` 된 다음 `OnStop` 메서드가 호출 되 고 작업은 배경으로 이동 하지만 제거 되지 않습니다. 사용자가 작업 관리자 또는 유사한 응용 프로그램을 사용 하 여 응용 프로그램을 복원 하는 경우 Android는 활동의 `OnRestart` 메서드를 호출 합니다.

`OnRestart`에서 구현 해야 하는 논리 종류에 대 한 일반적인 지침은 없습니다. 이는 작업이 생성 되 고 있는지 여부에 관계 없이 `OnStart` 항상 호출 되기 때문입니다. 따라서 활동에 필요한 모든 리소스를 `OnRestart`이 아니라 `OnStart`에서 초기화 해야 합니다.

`OnRestart` 후에 호출 되는 다음 수명 주기 메서드는 `OnStart`됩니다.

### <a name="back-vs-home"></a>Back vs Home

많은 Android 장치에는 "뒤로" 단추와 "홈" 단추 라는 두 개의 고유 단추가 있습니다. 이에 대 한 예는 Android 4.0.3의 다음 스크린샷에서 볼 수 있습니다.

[![뒤로 및 홈 단추](images/image4-sml.png)](images/image4.png#lightbox)

응용 프로그램을 백그라운드에 배치 하는 것과 동일한 결과를 표시 하더라도 두 단추 사이에는 약간의 차이가 있습니다. 사용자가 뒤로 단추를 클릭 하면 사용자가 작업을 수행 하는 것을 Android에 지시 하 게 됩니다. Android에서 활동을 제거 합니다. 반면에 사용자가 홈 단추를 클릭 하면 활동이 백그라운드에만 배치 &ndash; Android는 활동을 종료 하지 않습니다.

<a name="Managing_State_Throughout_the_Lifecycle" />

## <a name="managing-state-throughout-the-lifecycle"></a>수명 주기 전체에서 상태 관리

활동을 중지 하거나 소멸 하면 이후 리하이드레이션 활동의 상태를 저장할 수 있는 기회가 제공 됩니다.
이 저장 된 상태를 인스턴스 상태 라고 합니다. Android는 활동 수명 주기 동안 인스턴스 상태를 저장 하기 위한 세 가지 옵션을 제공 합니다.

1. Android에서 상태를 저장 하는 데 사용 하는 [번들](xref:Android.OS.Bundle) 로 알려진 `Dictionary`에 기본 값을 저장 합니다.

1. 비트맵과 같은 복합 값을 저장할 사용자 지정 클래스를 만듭니다. Android는이 사용자 지정 클래스를 사용 하 여 상태를 저장 합니다.

1. 구성 변경 수명 주기를 우회 하 고 활동의 상태를 유지 관리 하는 데 필요한 책임을 가정 합니다.

이 가이드에서는 처음 두 가지 옵션을 설명 합니다.

### <a name="bundle-state"></a>번들 상태

인스턴스 상태를 저장 하는 기본 옵션은 [번들](xref:Android.OS.Bundle)로 알려진 키/값 사전 개체를 사용 하는 것입니다.
작업을 만들 때 `OnCreate` 메서드가 번들을 매개 변수로 전달 하는 경우이 번들을 사용 하 여 인스턴스 상태를 복원할 수 있습니다. 키/값 쌍 (예: 비트맵)으로 빠르고 쉽게 직렬화 하지 않는 보다 복잡 한 데이터에는 번들을 사용 하지 않는 것이 좋습니다. 대신 문자열 같은 간단한 값에 사용 해야 합니다.

활동은 번들에서 인스턴스 상태를 저장 하 고 검색 하는 데 도움이 되는 메서드를 제공 합니다.

- [OnSaveInstanceState](xref:Android.App.Activity.OnSaveInstanceState*) &ndash; 작업이 제거 될 때 Android에서 호출 됩니다. 작업에서는 키/값 상태 항목을 유지 해야 하는 경우이 메서드를 구현할 수 있습니다.

- [OnRestoreInstanceState](xref:Android.App.Activity.OnRestoreInstanceState*) &ndash; `OnCreate` 메서드가 완료 된 후에 호출 되며, 초기화가 완료 된 후 활동에서 해당 상태를 복원할 수 있는 또 다른 기회를 제공 합니다.

다음 다이어그램에서는 이러한 메서드를 사용 하는 방법을 보여 줍니다.

[![번들 상태 순서도](images/image3-sml.png)](images/image3.png#lightbox)

#### <a name="onsaveinstancestate"></a>OnSaveInstanceState

활동이 중지 될 때 [OnSaveInstanceState](xref:Android.App.Activity.OnSaveInstanceState*) 가 호출 됩니다. 활동에서 해당 상태를 저장할 수 있는 번들 매개 변수를 받게 됩니다. 장치에서 구성 변경이 발생 하는 경우 작업은 `OnSaveInstanceState`를 재정의 하 여 활동 상태를 유지 하기 위해 전달 된 `Bundle` 개체를 사용할 수 있습니다. 예를 들어, 다음 코드를 고려하세요.

```csharp
int c;

protected override void OnCreate (Bundle bundle)
{
  base.OnCreate (bundle);

  this.SetContentView (Resource.Layout.SimpleStateView);

  var output = this.FindViewById<TextView> (Resource.Id.outputText);

  if (bundle != null) {
    c = bundle.GetInt ("counter", -1);
  } else {
    c = -1;
  }

  output.Text = c.ToString ();

  var incrementCounter = this.FindViewById<Button> (Resource.Id.incrementCounter);

  incrementCounter.Click += (s,e) => {
    output.Text = (++c).ToString();
  };
}
```

위의 코드는 `incrementCounter` 이라는 단추를 클릭할 때 `c` 이라는 정수를 증가 시켜 이름이 `output`인 `TextView` 결과를 표시 합니다. 구성 변경이 발생 하는 경우, 예를 들어 장치가 회전 될 때 위의 코드는 아래 그림에 표시 된 것 처럼 `bundle` `null`되므로 `c`의 값을 잃게 됩니다.

[![표시에 이전 값이 표시 되지 않습니다.](images/07-sml.png)](images/07.png#lightbox)

이 예제에서 `c` 값을 유지 하기 위해 활동은 아래와 같이 번들에 값을 저장 하 여 `OnSaveInstanceState`를 재정의할 수 있습니다.

```csharp
protected override void OnSaveInstanceState (Bundle outState)
{
  outState.PutInt ("counter", c);
  base.OnSaveInstanceState (outState);
}
```

이제 장치가 새 방향으로 회전 될 때 해당 정수는 번들에 저장 되 고 다음 줄로 검색 됩니다.

```csharp
c = bundle.GetInt ("counter", -1);
```

> [!NOTE]
> 뷰 계층의 상태를 저장할 수도 있도록 항상 `OnSaveInstanceState`의 기본 구현을 호출 하는 것이 중요 합니다.

##### <a name="view-state"></a>상태 보기

`OnSaveInstanceState`를 재정의 하는 것은 위의 예에 나와 있는 카운터와 같이 방향 변경 내용에 따라 활동에 임시 데이터를 저장 하는 데 적합 한 메커니즘입니다. 그러나 `OnSaveInstanceState`의 기본 구현에서는 각 보기에 할당 된 ID가 있는 한 모든 보기에 대 한 UI에 임시 데이터를 저장 하는 데 사용 됩니다. 예를 들어, 응용 프로그램에 다음과 같이 XML로 정의 된 `EditText` 요소가 있다고 가정 합니다.

```xml
<EditText android:id="@+id/myText"
  android:layout_width="fill_parent"
  android:layout_height="wrap_content"/>
```

`EditText` 컨트롤에는 할당 된 `id` 있고, 사용자가 일부 데이터를 입력 하 고 장치를 회전 하는 경우 아래와 같이 데이터가 계속 표시 됩니다.

[![데이터는 가로 모드에서 유지 됩니다.](images/08-sml.png)](images/08.png#lightbox)

#### <a name="onrestoreinstancestate"></a>OnRestoreInstanceState

[OnRestoreInstanceState](xref:Android.App.Activity.OnRestoreInstanceState*) 는 `OnStart`후에 호출 됩니다. 이전 `OnSaveInstanceState`중에 번들에 저장 된 모든 상태를 복원할 수 있는 작업을 제공 합니다. 그러나 `OnCreate`에 제공 되는 것과 동일한 번들입니다.

다음 코드는 `OnRestoreInstanceState`에서 상태를 복원 하는 방법을 보여 줍니다.

```csharp
protected override void OnRestoreInstanceState(Bundle savedState)
{
    base.OnRestoreSaveInstanceState(savedState);
    var myString = savedState.GetString("myString");
    var myBool = savedState.GetBoolean("myBool");
}
```

이 메서드는 상태를 복원 해야 하는 경우에 대 한 몇 가지 유연성을 제공 하기 위해 존재 합니다. 경우에 따라 인스턴스 상태를 복원 하기 전에 모든 초기화가 완료 될 때까지 대기 하는 것이 더 적절할 수 있습니다. 또한 기존 작업의 서브 클래스는 인스턴스 상태의 특정 값만 복원 하면 됩니다. 대부분의 작업은 `OnCreate`에 제공 된 번들을 사용 하 여 상태를 복원할 수 있으므로 대부분의 경우 `OnRestoreInstanceState`를 재정의할 필요가 없습니다.

`Bundle`를 사용 하 여 상태를 저장 하는 예제는 [연습-작업 상태 저장](saving-state.md)을 참조 하세요.

#### <a name="bundle-limitations"></a>번들 제한 사항

`OnSaveInstanceState`를 사용 하면 임시 데이터를 쉽게 저장할 수 있지만 몇 가지 제한 사항이 있습니다.

- 모든 경우에 호출 되지 않습니다. 예를 들어 **Home** 또는 **Back** 을 눌러 활동을 종료 하면 `OnSaveInstanceState` 호출 되지 않습니다.

- `OnSaveInstanceState`에 전달 된 번들은 이미지와 같은 커다란 개체를 위해 디자인 되지 않았습니다. 개체가 클 경우 아래에 설명 된 대로 [OnRetainNonConfigurationInstance](xref:Android.App.Activity.OnRetainNonConfigurationInstance) 에서 개체를 저장 하는 것이 좋습니다.

- 번들을 사용 하 여 저장 된 데이터는 serialize 되며이로 인해 지연이 발생할 수 있습니다.

번들 상태는 많은 메모리를 사용 하지 않는 단순 데이터에 유용 하지만 *구성 되지 않은 인스턴스 데이터* 는 보다 복잡 한 데이터 또는 검색 비용이 많이 드는 데이터 (예: 웹 서비스 호출 또는 복잡 한 데이터베이스 쿼리)에 유용 합니다. 구성 되지 않은 인스턴스 데이터는 필요에 따라 개체에 저장 됩니다. 다음 섹션에서는 구성 변경을 통해 보다 복잡 한 데이터 형식을 유지 하는 방법으로 `OnRetainNonConfigurationInstance`를 소개 합니다.

### <a name="persisting-complex-data"></a>복합 데이터 유지

Android는 번들에 데이터를 유지 하는 것 외에도 [OnRetainNonConfigurationInstance](xref:Android.App.Activity.OnRetainNonConfigurationInstance) 를 재정의 하 고 유지할 데이터가 포함 된 `Java.Lang.Object`의 인스턴스를 반환 하 여 데이터를 저장 하도록 지원 합니다. `OnRetainNonConfigurationInstance`를 사용 하 여 상태를 저장 하는 두 가지 주요 이점이 있습니다.

- `OnRetainNonConfigurationInstance`에서 반환 되는 개체는 메모리가이 개체를 유지 하기 때문에 더 크고 더 복잡 한 데이터 형식에서 잘 수행 됩니다.

- `OnRetainNonConfigurationInstance` 메서드는 요청 시 및 필요한 경우에만 호출 됩니다. 이 방법은 수동 캐시를 사용 하는 것 보다 경제적입니다.

`OnRetainNonConfigurationInstance` 사용은 웹 서비스 호출 등에서 데이터를 여러 번 검색 하는 데 비용이 많이 드는 시나리오에 적합 합니다. 예를 들어 Twitter를 검색 하는 다음 코드를 살펴보겠습니다.

```csharp
public class NonConfigInstanceActivity : ListActivity
{
  protected override void OnCreate (Bundle bundle)
  {
    base.OnCreate (bundle);
    SearchTwitter ("xamarin");
  }

  public void SearchTwitter (string text)
  {
    string searchUrl = String.Format("http://search.twitter.com/search.json?" + "q={0}&rpp=10&include_entities=false&" + "result_type=mixed", text);

    var httpReq = (HttpWebRequest)HttpWebRequest.Create (new Uri (searchUrl));
    httpReq.BeginGetResponse (new AsyncCallback (ResponseCallback), httpReq);
  }

  void ResponseCallback (IAsyncResult ar)
  {
    var httpReq = (HttpWebRequest)ar.AsyncState;

    using (var httpRes = (HttpWebResponse)httpReq.EndGetResponse (ar)) {
      ParseResults (httpRes);
    }
  }

  void ParseResults (HttpWebResponse httpRes)
  {
    var s = httpRes.GetResponseStream ();
    var j = (JsonObject)JsonObject.Load (s);

    var results = (from result in (JsonArray)j ["results"] let jResult = result as JsonObject select jResult ["text"].ToString ()).ToArray ();

    RunOnUiThread (() => {
      PopulateTweetList (results);
    });
  }

  void PopulateTweetList (string[] results)
  {
    ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.ItemView, results);
  }
}
```

다음 스크린샷에 표시 된 것 처럼이 코드는 웹에서 JSON으로 형식이 지정 된 결과를 검색 하 고 구문 분석 한 다음 결과를 목록에 표시 합니다.

[화면에 표시 되는![결과](images/06-sml.png)](images/06.png#lightbox)

구성 변경이 발생 하는 경우, 예를 들어 장치가 회전 되 면 코드가 프로세스를 반복 합니다. 원래 검색 된 결과를 다시 사용 하 고 불필요 한 중복 네트워크 호출이 발생 하지 않도록 하려면 아래와 같이 `OnRetainNonconfigurationInstance`를 사용 하 여 결과를 저장할 수 있습니다.

```csharp
public class NonConfigInstanceActivity : ListActivity
{
  TweetListWrapper _savedInstance;

  protected override void OnCreate (Bundle bundle)
  {
    base.OnCreate (bundle);

    var tweetsWrapper = LastNonConfigurationInstance as TweetListWrapper;

    if (tweetsWrapper != null) {
      PopulateTweetList (tweetsWrapper.Tweets);
    } else {
      SearchTwitter ("xamarin");
    }

    public override Java.Lang.Object OnRetainNonConfigurationInstance ()
    {
      base.OnRetainNonConfigurationInstance ();
      return _savedInstance;
    }

    ...

    void PopulateTweetList (string[] results)
    {
      ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.ItemView, results);
      _savedInstance = new TweetListWrapper{Tweets=results};
    }
}
```

이제 장치를 회전할 때 원래 결과가 `LastNonConfiguartionInstance` 속성에서 검색 됩니다. 이 예제에서 결과는 트 윗을 포함 하는 `string[]` 구성 됩니다. `OnRetainNonConfigurationInstance`에서 `Java.Lang.Object` 반환 해야 하므로 아래와 같이 `string[]` 서브 클래스가 `Java.Lang.Object`클래스에 래핑됩니다.

```csharp
class TweetListWrapper : Java.Lang.Object
{
  public string[] Tweets { get; set; }
}
```

예를 들어 `OnRetainNonConfigurationInstance`에서 반환 된 개체로 `TextView`를 사용 하려고 하면 아래 코드에 나와 있는 것 처럼 활동이 누출 됩니다.

```csharp
TextView _textView;

protected override void OnCreate (Bundle bundle)
{
  base.OnCreate (bundle);

  var tv = LastNonConfigurationInstance as TextViewWrapper;

  if(tv != null) {
    _textView = tv;
    var parent = _textView.Parent as FrameLayout;
    parent.RemoveView(_textView);
  } else {
    _textView = new TextView (this);
    _textView.Text = "This will leak.";
  }

  SetContentView (_textView);
}

public override Java.Lang.Object OnRetainNonConfigurationInstance ()
{
  base.OnRetainNonConfigurationInstance ();
  return _textView;
}
```

이 섹션에서는 `Bundle`를 사용 하 여 단순 상태 데이터를 유지 하는 방법과 `OnRetainNonConfigurationInstance`를 사용 하 여 더 복잡 한 데이터 형식을 유지 하는 방법을 배웠습니다.

## <a name="summary"></a>요약

Android 작업 수명 주기는 응용 프로그램 내에서 활동의 상태 관리를 위한 강력한 프레임 워크를 제공 하지만 이해 하 고 구현 하기는 어려울 수 있습니다. 이 장에서는 활동이 수명 중에 발생할 수 있는 다양 한 상태와 이러한 상태와 연결 된 수명 주기 방법을 소개 했습니다. 다음으로 이러한 각 방법에서 수행 해야 하는 논리 종류에 대 한 지침을 제공 했습니다.

## <a name="related-links"></a>관련 링크

- [회전 처리](~/android/app-fundamentals/handling-rotation.md)
- [Android 작업](xref:Android.App.Activity)
