---
title: 활동 수명 주기
description: 활동은 Android 응용 프로그램의 기본 빌딩 블록 및 서로 다른 상태의 수에 존재할 수 있습니다. 활동 수명 주기 인스턴스화로 시작 하 고으로 소멸 끝나며 사이 많은 상태가 포함 됩니다. 활동 상태 변경 되 면 올바르지 않은 상태 변경의 작업을 알리는 하므로 해당 변경 내용에 맞게 코드를 실행 하 고 적절 한 수명 주기 이벤트 메서드 호출 됩니다. 이 문서를 활동의 수명 주기를 검사 하 고 책임에 설명 활동 각 제대로 작동 하 고 신뢰할 수 있는 응용 프로그램의 일부로 이러한 상태 변경 중에 있습니다.
ms.prod: xamarin
ms.assetid: 05B34788-F2D2-4347-B66B-40AFD7B1D167
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/28/2018
ms.openlocfilehash: f35f3e59d8b669795ade3d370894e45866cea1ff
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="activity-lifecycle"></a>활동 수명 주기

_활동은 Android 응용 프로그램의 기본 빌딩 블록 및 서로 다른 상태의 수에 존재할 수 있습니다. 활동 수명 주기 인스턴스화로 시작 하 고으로 소멸 끝나며 사이 많은 상태가 포함 됩니다. 활동 상태 변경 되 면 올바르지 않은 상태 변경의 작업을 알리는 하므로 해당 변경 내용에 맞게 코드를 실행 하 고 적절 한 수명 주기 이벤트 메서드 호출 됩니다. 이 문서를 활동의 수명 주기를 검사 하 고 책임에 설명 활동 각 제대로 작동 하 고 신뢰할 수 있는 응용 프로그램의 일부로 이러한 상태 변경 중에 있습니다._

## <a name="activity-lifecycle-overview"></a>활동 수명 주기 개요

활동은 Android에는 비정상적인 프로그래밍 개념입니다. 기존 응용 프로그램 개발에서 응용 프로그램을 시작 하려면 실행 되는 정적 main 메서드를 일반적으로 합니다. 그러나 Android, 것이 서로 다릅니다. 응용 프로그램 내에서 등록 된 모든 활동을 통해 android 응용 프로그램을 실행할 수 있습니다. 실제로 대부분의 응용 프로그램의 응용 프로그램 진입점으로 지정 된 특정 활동 권한만 갖습니다. 그러나 응용 프로그램이 충돌할 경우, 종료 또는 운영 체제, 운영 체제에서 마지막 열려 활동에서 응용 프로그램 또는 이전 활동 스택 내에서 다른 위치를 다시 시작 해 볼 수 있습니다.
또한 운영 체제 활성화 있지 않는 경우 일시 중지 동작 하 고 메모리가 부족할 경우 회수할 수 있습니다. 응용 프로그램을 올바르게 활동을 다시 시작할 경우에 특히 활동 이전 작업에서 데이터에 따라 달라 지는 상태를 복원할 수 있도록 신중 하 게 고려 이루어져야 합니다.

활동 수명 주기는 활동의 수명 주기 내내 운영 체제 메서드 호출의 컬렉션으로 구현 됩니다. 이러한 메서드를 사용 하면 응용 프로그램의 상태 및 리소스 관리 요구 사항을 충족 해야 하는 기능을 구현 합니다.

것이 응용 프로그램 개발자가 각 활동의 활동 수명 주기에서 노출 하는 메서드를 구현 해야 합니다. 확인 하려면 요구 사항을 분석에 대 한 매우 중요 합니다. 이렇게 하지 않으면 응용 프로그램이 불안정, 충돌, 리소스 볼록 및 역시 낮아질 기본 운영 체제 불안정 발생할 수 있습니다.

이 장에서 활동 수명 주기를 자세히 검사 하 여 포함 하 여:

-  활동 상태
-  수명 주기 메서드
-  응용 프로그램의 상태를 유지합니다.


이 섹션에 포함 되어는 [연습](~/android/app-fundamentals/activity-lifecycle/saving-state.md) 실제 예제를 제공 하는 활동 수명 주기 동안 상태를 효율적으로 저장 하는 방법에 있습니다. 이 장의 끝 부분에서 활동 수명 주기 및 Android 응용 프로그램에서 지 원하는 방법을 이해를 해야 합니다.

## <a name="activity-lifecycle"></a>활동 수명 주기

Android 활동 수명 주기는 개발자 리소스 관리 프레임 워크를 제공 하는 활동 클래스 내에서 노출 하는 메서드 컬렉션을 구성 합니다. 이 프레임 워크에서는 개발자가 응용 프로그램 내에서 각 작업의 고유한 상태 관리 요구 사항을 충족 하 고 적절히 리소스 관리를 처리 합니다.

### <a name="activity-states"></a>활동 상태

Android OS는 활동의 상태에 따라 arbitrates 합니다. 이렇게 하면 더 이상 사용 하 여, 메모리 및 리소스를 회수 하려면 os 중인 활동을 식별 하는 Android 없습니다. 다음 다이어그램에서는 활동의 수명 동안을 통과할 수 상태를 보여 줍니다.

[![활동 상태 다이어그램](images/image1-sml.png)](images/image1.png#lightbox)

이러한 상태는 다음과 같이 4 개의 주요 그룹으로 나눌 수 있습니다.

1.  *활성 또는 실행 중인* &ndash; 활동 활성 것으로 간주 또는 활동 스택의 맨 라고도 전경에 있는 경우에 실행 됩니다. Android에서 가장 높은 우선 순위 작업으로 간주 됩니다 하 고 따라서만 종료 됩니다 극단적인 상황에서는 운영 체제에서이 등 작업 보다 더 많은 메모리를 사용 하려고 하는 경우 장치에서 사용할 수 있는 같이 됩니다 UI를 응답 하지 않게 될 수 있습니다.

1.  *일시 중지* &ndash; 활동 것으로 간주 됩니다 절전 모드로 전환 하는 장치가 작동 하거나 활동은 여전히 표시 되기는 하지만 전체 크기의 새롭거나 투명 활동에 의해 부분적으로 숨겨진를 일시 중지 합니다. 일시 중지 된 활동은 여전히 활성 즉, 모든 상태 및 멤버 정보를 유지 하며 창 관리자 연결 된 상태로 유지 합니다. 두 번째 것으로 간주 됩니다이이 작업을 중지 하면 안정적이 고 응답성이 뛰어난 액티브/실행 작업을 유지 하는 데 필요한 리소스 요구 사항을 만족 하는 경우 가장 높은 우선 순위 활동에서 Android 하 고 따라서 운영 체제에서 종료만 됩니다.

1.  *중지/Backgrounded* &ndash; 중지 되거나 백그라운드에서 다른 활동에 의해 완전히 가려진은 활동으로 간주 됩니다.
    중지 된 활동 계속 있지만 중지 된 작업 세 가지 상태 중 가장 낮은 우선 순위를 것으로 간주 됩니다 하 고 운영 체제 리소스를 충족 하려면 먼저이 상태에서는 활동 제거 된다는 것 이므로 해당 상태 및 멤버 정보를 유지 하려고 더 높은 우선 순위 활동의 요구 사항입니다.

1.  *다시 시작* &ndash; 를 Android 하 여 메모리에서 제거 하기 위해 주기에서 중지에서 아무 곳 이나가 일시 중지 되었습니다. 로 다시 이동할 작업 다시 시작 해야 합니다 이전에 저장 된 상태로 복원 하 고 사용자에 게 표시 합니다.


### <a name="activity-re-creation-in-response-to-configuration-changes"></a>구성 변경 사항에 따라 작업을 다시 만드

보다 복잡 한 문제를 위해 Android 구성 변경 내용을 호출 조합에 하나 더 많은 렌치를 throw 합니다. 구성 변경 사항을 신속 하 게 활동 소멸/다시 creation 사이클은 장치가 같은 활동의 구성을 변경 될 때 발생 하는 [회전](~/android/app-fundamentals/handling-rotation.md) (가로 또는 세로에서 다시 빌드되 해야 및 모드), 키보드 표시 될 때 (활동 자체 크기를 조정할 수 있도록 제공 됩니다), 또는 다른 규칙 으로부터 dock, 장치를 배치 하면 됩니다.

구성 변경으로 인해 발생 하는 중지 한 작업을 다시 시작 하는 동안 동일한 활동 상태 변경 내용을 계속 합니다. 그러나 응용 프로그램 응답성이 뛰어난 느낌 및 구성 변경 하는 동안에 수행 되도록, 하기 위해 것이 중요 최대한 빨리 처리할 수 있습니다. 이 인해 Android 구성 변경 하는 동안 상태를 유지 하기 위해 사용할 수 있는 특정 API에 있습니다.
이러한 작업을 나중에 설명 하겠습니다는 [the 주기 전체에서 상태 관리](~/android/app-fundamentals/activity-lifecycle/index.md#Managing_State_Throughout_the_Lifecycle) 섹션.

### <a name="activity-lifecycle-methods"></a>활동 수명 주기 메서드

Android SDK 및 확장에 의해 Xamarin.Android framework 응용 프로그램 내에서 활동의 상태를 관리 하기 위한 강력한 모델을 제공 합니다. 활동의 상태를 변경할 때는 해당 활동에 특정 메서드를 호출 하는 운영 체제에서 활동 알림이 전송 됩니다. 다음 다이어그램은 관계에는 활동 수명 주기에서 이러한 메서드를 보여줍니다.

[![활동 수명 주기 순서도](images/image2-sml.png)](images/image2.png#lightbox)

개발자는 활동 내에서 이러한 메서드를 재정의 하 여 상태 변경 내용을 처리할 수 있습니다. 단, 모든 수명 주기 메서드는 UI 스레드에서 호출 및 등 새 활동을 표시에서 현재 작업 숨기기 등의 UI 작업의 다음 부분을 수행 합니다. 운영 체제는 차단 하는 것이 유용 합니다. 따라서 이러한 메서드의 코드 간단 하 게 잘 수행 생각 될 응용 프로그램을 만들 수 있어야 합니다. 모든 장기 실행 작업은 백그라운드 스레드에서 실행 되어야 합니다.

이러한 수명 주기 메서드 및 용도의 각 살펴보겠습니다.

#### <a name="oncreate"></a>OnCreate

[OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) 활동 만들어질 때 호출 되는 첫 번째 방법입니다.
`OnCreate` 항상 재정의 같은 작업에서 요구 될 수 있는 모든 시작 초기화를 수행 하려면:

-  보기 만들기
-  변수 초기화
-  목록에 정적 데이터 바인딩


`OnCreate` 사용 된 [번들](https://developer.xamarin.com/api/type/Android.OS.Bundle/) 저장 하 고 번들 null이 아닌 경우 작업 간의 상태 정보 및 개체를 전달 하기 위한 사전 매개가 작업을 다시 시작 하 고 나타냅니다에서 해당 상태를 복원 해야는 이전 인스턴스입니다. 다음 코드에는 번들에서 값을 검색 하는 방법을 보여 줍니다.

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

한 번 `OnCreate` 는 Android 작업이 끝난 후에 호출 됩니다 `OnStart`합니다.

#### <a name="onstart"></a>OnStart

[OnStart](https://developer.xamarin.com/api/member/Android.App.Activity.OnStart/) 가 항상 후 시스템에 의해 호출 `OnCreate` 를 완료 합니다. 작업 활동 활동 내에서 보기의 현재 값을 새로 고치는 등 표시 되기 전에 특정 작업 권한을 수행 하는 경우이 메서드를 재정의 합니다. Android에서는 호출 `OnResume` 이 메서드 직후 합니다.

#### <a name="onresume"></a>OnResume

시스템 호출 [OnResume](https://developer.xamarin.com/api/member/Android.App.Activity.OnResume/) 동작은 사용자 상호 작용을 시작할 수 있습니다.
활동와 같은 작업을 수행 하려면이 메서드를 재정의 해야:

-  프레임 속도 (게임 건물에서 일반적인 작업)를 램프
-  애니메이션을 시작합니다.
-  GPS 업데이트에 대 한 수신 대기
-  관련 경고 또는 대화 상자 표시
-  외부 이벤트 처리기를 연결 합니다.


예를 들어, 다음 코드 조각에 카메라를 초기화 하는 방법을 보여 줍니다.

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

`OnResume` 이 모든 작업을 수행 하기 때문에 중요에서 수행 `OnPause` 에 되지 않은 ne 수 없습니다. `OnResume`항상 후 실행 되는 유일한 수명 주기 방법은 이므로, `OnPause` 활동 수명을으로 되돌릴 때.

#### <a name="onpause"></a>OnPause

[OnPause](https://developer.xamarin.com/api/member/Android.App.Activity.OnPause/) 시스템을 배경으로 나 부분적으로 활동을 가려지는 경우 활동을 저장 하려고 할 때 호출 됩니다. 활동 하는 경우이 메서드를 재정의 해야 합니다.

-   영구 데이터를 저장 하지 않은 변경 내용 커밋

-   Destroy 하거나 리소스를 사용 하는 다른 개체를 정리

-   프레임 속도 및 일시 중지 된 애니메이션 하였다가

-   외부 이벤트 처리기 또는 (즉, 해당 서비스에 연결 된) 알림 처리기를 등록 취소 합니다. 이 활동 메모리 누수를 방지 하기 위해 수행 되어야 합니다.

-   마찬가지로, 활동 대화 상자 또는 경고를 표시, 없으면은 정리 해야 합니다와 `.Dismiss()` 메서드.

예를 들어, 다음 코드 조각으로 작업을 만들 수 없습니다 카메라를 해제 합니다 사용 일시 중지 된 동안:

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

후 호출 되는 두 가지 가능한 수명 주기 메서드는 `OnPause`:

1.  `OnResume` 활동은 전경으로 반환할 경우 호출 됩니다.
1.  `OnStop` 백그라운드에서 작업 배치 하는 경우 호출 됩니다.


#### <a name="onstop"></a>OnStop

[OnStop](https://developer.xamarin.com/api/member/Android.App.Activity.OnStop/) 활동은 더 이상 사용자에 게 표시 될 때 호출 됩니다. 다음 중 하나가 발생할 때 발생 합니다.

-  새 활동 시작 되 고 및에 대 한이 작업을 합니다.
-  기존 활동은 전경으로 상태로 전환 되 고 됩니다.
-  활동이 제거 되 고 있습니다.


`OnStop` 항상를 호출할 수 없습니다 Android 리소스에 대 한 중단 및 활동 백그라운드 제대로 수 없는 경우 같은 메모리 부족 상태에 있습니다. 이러한 이유로 것이 좋습니다에 의존 하지 `OnStop` 소멸에 대 한 활동을 준비 하는 경우 호출 중일 합니다. 이 후에 호출 될 수 있는 다음 수명 주기 메서드 `OnDestroy` 되는 경우 활동, 또는 `OnRestart` 사용자와 상호 작용 하는 활동을 다시 가져오는 경우.

#### <a name="ondestroy"></a>OnDestroy

[OnDestroy](https://developer.xamarin.com/api/member/Android.App.Activity.OnDestroy/) 소멸 있으며 메모리에서 완전히 제거 하기 전에 활동 인스턴스에서 호출 하는 최종 메서드입니다. 극단적인 경우 연결이 Android 져 활동을 호스팅하는 응용 프로그램 프로세스를 제거할 수 있습니다 `OnDestroy` 호출 되지 않습니다. 대부분의 활동은 대부분을 정리 하 고 시스템 종료 조건이 때문에이 메서드를 구현 하지는 `OnPause` 및 `OnStop` 메서드. `OnDestroy` 메서드는 보통 재정의 장기를 정리 하는 리소스를 실행 누수 될 수 리소스입니다. 이 예제에서 시작 된 백그라운드 스레드를 수 있습니다 `OnCreate`합니다.

없는 수명 주기 메서드 활동 소멸 된 후에 호출 됩니다.

#### <a name="onrestart"></a>OnRestart

[OnRestart](https://developer.xamarin.com/api/member/Android.App.Activity.OnRestart/) 활동에 중지 된 후 다시 시작 되 고 되기 전에 호출 됩니다. 사용자가 응용 프로그램에서 작업 중에 홈 단추를 누르면이 좋은 예는 것입니다. 이 경우 `OnPause` 차례로 `OnStop` 를 메서드 호출 활동 백그라운드도 이동 하지만 제거 되지 않습니다. 사용자가 다음 작업 관리자 또는 유사한 응용 프로그램을 사용 하 여 응용 프로그램을 복원 하려면 Android 호출 됩니다는 `OnRestart` 활동 메서드.

구현 하는 논리의 종류에 대 한 일반 지침은 없습니다 `OnRestart`합니다. 때문에 이것이 `OnStart` 활동이 만들어진 여부에 관계 없이 항상 호출 되거나에서 활동에 필요한 모든 리소스를 초기화 해야 하므로 다시 시작 되 고 `OnStart`, 대신 `OnRestart`합니다.

호출 후 다음 수명 주기 메서드 `OnRestart` 됩니다 `OnStart`합니다.

### <a name="back-vs-home"></a>Vs를 백업 합니다. 홈

다양 한 Android 장치에는 두 가지 단추가: "뒤로" 단추와 "홈" 단추가 있습니다. 이러한 예: Android 4.0.3 다음 스크린 샷에 볼 수 있습니다.

[![뒤로 및 홈 단추](images/image4-sml.png)](images/image4.png#lightbox)

표시 되는 동일한 효과가 백그라운드에서 응용 프로그램을 게시 하는 경우에 두 개의 단추는 미묘한 차이가 있습니다. 사용자가 뒤로 단추를 클릭 하면 이러한 한다는 Android 작업과 완료 합니다. Android 활동을 삭제 됩니다. 반면, 사용자 홈 단추를 클릭할 때 활동 단순히에 놓입니다 백그라운드 &ndash; Android 활동을 종료 하지 것입니다.

<a name="Managing_State_Throughout_the_Lifecycle" />

## <a name="managing-state-throughout-the-lifecycle"></a>수명 주기 전체에서 상태 관리

작업은 중지 되거나 소멸 될 때 시스템 이후 리하이드레이션 대 한 작업의 상태를 저장 하는 기회를 제공 합니다.
이 상태를 저장 된 인스턴스 상태 라고 합니다. Android에는 활동 수명 주기 동안 인스턴스 상태를 저장 하기 위한 세 가지 옵션이 제공 됩니다.

1. 기본 값을 저장 한 `Dictionary` 라고는 [번들](https://developer.xamarin.com/api/type/Android.OS.Bundle/) Android 상태 저장을 사용 하는 것입니다.

1. 사용자 지정 클래스를 만드는 비트맵 같은 복잡 한 값이 포함 됩니다. Android 상태를 저장 하려면이 사용자 지정 클래스를 사용 합니다.

1. 구성 변경 수명 주기를 우회 하 고 가정할 경우 활동에서 상태 관리에 대 한 책임을 완료 합니다.


이 가이드에서는 처음 두 옵션에 설명 합니다.



### <a name="bundle-state"></a>번들 상태

인스턴스 상태를 저장 하기 위한 기본 옵션은 라고 하는 키/값 사전 개체를 사용 하는 [번들](https://developer.xamarin.com/api/type/Android.OS.Bundle/)합니다.
활동 만들어질 때 이라는 `OnCreate` 메서드 번들 매개 변수로 전달, 인스턴스 상태를 복원 하려면이 번들을 사용할 수 있습니다. 키/값 쌍 (예: 비트맵);로 쉽게 serialize 되거나 신속 하 게 되지 않습니다는 더 복잡 한 데이터에 대 한 번들을 사용 하는 것이 좋습니다 하지 대신, 문자열 처럼 간단한 값에 대 한 사용 해야 합니다.

활동 사용 하 여 저장 및 번들에 인스턴스 상태를 검색 하는 메서드를 제공 합니다.

-   [OnSaveInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnSaveInstanceState/p/Android.OS.Bundle/) &ndash; 활동 소멸 될 때 Android 여 호출 합니다. 활동 모든 키/값 상태 항목을 유지 하는 경우이 메서드를 구현할 수 있습니다.

-   [OnRestoreInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnRestoreInstanceState/p/Android.OS.Bundle/) &ndash; 이 다음에 호출 됩니다는 `OnCreate` 가 완료 되 면 메서드와 초기화가 완료 된 후 해당 상태를 복원 하는 활동에 대 한 다른 기회를 제공 합니다.

다음 다이어그램에서는 이러한 메서드의 사용 방법을 보여 줍니다.

[![번들 상태 순서도](images/image3-sml.png)](images/image3.png#lightbox)

#### <a name="onsaveinstancestate"></a>OnSaveInstanceState

[OnSaveInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnSaveInstanceState/p/Android.OS.Bundle/) 작업이 중지 되 고 호출 됩니다. 작업 상태를 저장할 수 있는 번들 매개 변수를 받지 것입니다. 장치 구성 변경, 될 때 활동 צ ְ ײ는 `Bundle` 재정의 하 여 작업 상태를 유지 하려면에 전달 되는 개체 `OnSaveInstanceState`합니다. 예를 들어, 다음 코드를 고려하세요.

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

위의 코드 증가 라는 정수 `c` 때 이라는 단추 `incrementCounter` 를 클릭 하면 결과에 표시는 `TextView` 라는 `output`합니다. 경우 구성 변경-예를 들어 때 장치를 회전-위의 코드는 값을 손실 `c` 때문에 `bundle` 것 `null`아래 그림에 표시 된 것 처럼:

[![이전 값 표시 되지 않습니다.](images/07-sml.png)](images/07.png#lightbox)

값을 보존 하려면 `c` 이 예제에서는 활동을 재정의할 수 `OnSaveInstanceState`, 아래와 같이 번들의 값을 저장 합니다.

```csharp
protected override void OnSaveInstanceState (Bundle outState)
{
  outState.PutInt ("counter", c);
  base.OnSaveInstanceState (outState);
}
```

이제 장치가 새 방향으로 회전, 정수 번들에 저장 되 고 줄 검색:

```csharp
c = bundle.GetInt ("counter", -1);
```

> [!NOTE]
> 이것은 항상에 중요 한 호출의 기본 구현 `OnSaveInstanceState` 를 뷰 계층의 상태를 저장할 수도 있습니다.



##### <a name="view-state"></a>상태 보기

재정의 `OnSaveInstanceState` 은 위 예에서 카운터 등의 방향 변경을 통해 활동에서 일시적인 데이터를 저장 하는 적절 한 메커니즘입니다. 그러나의 기본 구현은 `OnSaveInstanceState` 하므로 모든 보기에 대 한 UI에서 일시적인 데이터 저장으로 각 보기에 할당 하는 ID입니다. 예를 들어, 응용 프로그램에 `EditText` 다음과 같이 XML에 정의 된 요소:

```xml
<EditText android:id="@+id/myText"
  android:layout_width="fill_parent"
  android:layout_height="wrap_content"/>
```

이후는 `EditText` 컨트롤에는 `id` 할당, 사용자 데이터 일부를 입력 하 고가 장치를 회전할 때 데이터도 표시 됩니다, 아래와 같이:

[![가로 모드에서 데이터는 보존 됨](images/08-sml.png)](images/08.png#lightbox)

#### <a name="onrestoreinstancestate"></a>OnRestoreInstanceState

[OnRestoreInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnRestoreInstanceState/p/Android.OS.Bundle/) 후 호출할 `OnStart`합니다. 번들에는 이전 하는 동안 이전에 저장 된 모든 상태를 복원 하는 활동 제공 `OnSaveInstanceState`합니다. 이것은에 제공 되는 동일한 번들 `OnCreate`그러나 합니다.

다음 코드에서는 상태에서 복원할 수 있습니다 어떻게 `OnRestoreInstanceState`:

```csharp
protected override void OnRestoreInstanceState(Bundle savedState)
{
    base.OnRestoreSaveInstanceState(savedState);
    var myString = savedState.GetString("myString");
    var myBool = savedState.GetBoolean("myBool");
}
```

이 메서드는 유연 하 게 몇 가지 주위 상태를 복원 해야 하는 경우 존재 합니다. 경우에 따라 더 적절 한 인스턴스 상태를 복원 하기 전에 모든 초기화가 완료 될 때까지 기다려야 합니다. 또한 기존 활동의 서브 클래스 인스턴스 상태에서 특정 값을 복원 하려면 하기만 할 수도 있습니다. 대부분의 경우에서 필요 없는 재정의할 `OnRestoreInstanceState`대부분의 활동에 제공 되는 번들을 사용 하 여 상태를 복원할 수 있으므로, `OnCreate`합니다.

사용 하 여 상태 저장에 대 한 예제는 `Bundle`, 참조는 [연습-저장 하는 활동 상태](saving-state.md)합니다.


#### <a name="bundle-limitations"></a>번들 제한 사항

하지만 `OnSaveInstanceState` 를 사용 하면 일시적 데이터를 저장 하는 쉽게 몇 가지 제한이 있기 때문에:

-   모든 경우에에서는 호출 되지 않습니다. 예를 들어 키를 누르면 **홈** 또는 **다시** 활동 종료를 발생 하지 것입니다 `OnSaveInstanceState` 호출 합니다.

-   로 전달 된 번들 `OnSaveInstanceState` 이미지와 같은 큰 개체에 대 한 설계 되지 않았습니다. 개체를 저장 하는 큰 개체의 경우 [OnRetainNonConfigurationInstance](https://developer.xamarin.com/api/member/Android.App.Activity.OnRetainNonConfigurationInstance/) 아래 설명과 같이 적합 한 값이 있습니다.

-   지연을 야기할 수 있는 번들을 사용 하 여 저장 되는 데이터 직렬화 됩니다.

번들 상태는 많은 메모리를 사용 하지 않는 단순 데이터에 대 한 유용한 반면 *비구성 인스턴스 데이터* 는 더 복잡 한 데이터 또는 데이터를 검색 하는 데 비용이 유용 하 고 웹 서비스 호출 또는 복잡 한 데이터베이스 쿼리 합니다. 필요에 따라 비 구성 인스턴스 데이터 개체에 저장 됩니다. 다음 섹션에서는 소개 `OnRetainNonConfigurationInstance` 구성 변경을 통해 더 복잡 한 데이터 형식을 유지 하는 방법으로 합니다.


### <a name="persisting-complex-data"></a>복잡 한 데이터를 유지합니다.

번들의 영구 데이터 외에도 Android도 저장 기능을 지원 데이터를 재정의 하 여 [OnRetainNonConfigurationInstance](https://developer.xamarin.com/api/member/Android.App.Activity.OnRetainNonConfigurationInstance/) 의 인스턴스를 반환 하 고는 `Java.Lang.Object` 유지 하도록 데이터를 포함 하 합니다. 두 가지 주요 이점이 있습니다를 사용 하 여의 `OnRetainNonConfigurationInstance` 상태를 저장 합니다.

-   반환 된 개체 `OnRetainNonConfigurationInstance` 이 개체를 유지 하는 메모리 때문에 더 큰, 더 복잡 한 데이터 형식에도 수행 합니다.

-   `OnRetainNonConfigurationInstance` 메서드를 호출한 필요에 따라 필요한 경우에 가능 합니다. 이 수동 캐시를 사용 하 여에 비해 경제적입니다.

사용 하 여 `OnRetainNonConfigurationInstance` 여러 번 데이터를 검색 하는 데 비용이 인 시나리오에 적합 한 등에서 웹 서비스 호출 됩니다. 예를 들어, Twitter를 검색 하는 다음 코드를 살펴보세요.

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

이 코드 형식 JSON으로 지정 하는 웹에서 결과 검색 하 고, 구문 분석 하 고 다음 스크린샷에 표시 된 것 처럼 쿼리 결과 목록에 표시.

[![화면에 표시 되는 결과](images/06-sml.png)](images/06.png#lightbox)

구성 변경-예를 들어 회전할 때 발생 한 장치-코드는 프로세스를 반복 합니다. 사용할 수는 원래 검색 된 결과 다시 사용 하 고 불필요 하 게, 중복 네트워크 호출을 일으키지, `OnRetainNonconfigurationInstance` 아래와 같이 결과 저장 하려면:

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

원래 결과가 검색 되는 장치를 회전할 때 이제는 `LastNonConfiguartionInstance` 속성입니다. 이 예제에서는 결과 구성는 `string[]` 트 윗을 포함 합니다. 이후 `OnRetainNonConfigurationInstance` 있어야는 `Java.Lang.Object` 반환 될는 `string[]` 서브클래싱하는 클래스에서 래핑된 `Java.Lang.Object`다음과 같이:

```csharp
class TweetListWrapper : Java.Lang.Object
{
  public string[] Tweets { get; set; }
}
```

예를 들어 사용 하려고 한 `TextView` 에서 반환 된 개체와 `OnRetainNonConfigurationInstance` 아래 코드에 표시 된 것 처럼 활동에 누출 됩니다:

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

이 섹션에서는 이전에 배운 것 사용 하 여 간단한 상태 데이터를 유지 하는 방법의 `Bundle`와 더 복잡 한 데이터 형식을 유지 `OnRetainNonConfigurationInstance`합니다.

## <a name="summary"></a>요약

Android 활동 수명 주기는 응용 프로그램 내에서 활동을 상태 관리를 위한 강력한 기능을 제공 하지만 이해 하 고 구현 하기 어려울 수 있습니다. 이 장에서 서로 다른 상태를 활동 상태와 관련 된 수명 주기 메서드 뿐 아니라 해당 수명 동안 거칠 수 있습니다를 도입 했습니다. 그런 다음 이러한 각 방법으로 수행 되도록 논리의 종류에 대 한 지침 제공 되었습니다.


## <a name="related-links"></a>관련 링크

- [회전 처리](~/android/app-fundamentals/handling-rotation.md)
- [Android 활동](https://developer.xamarin.com/api/type/Android.App.Activity/)
