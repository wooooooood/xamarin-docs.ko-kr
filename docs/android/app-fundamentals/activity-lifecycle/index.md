---
title: 활동 수명 주기
description: 활동은 Android 응용 프로그램의 기본 빌딩 블록 및 여러 다른 상태에에서 존재할 수 있습니다. 작업 수명 주기 인스턴스화를 사용 하 여 시작 및 소멸을 사용 하 여 종료 사이 많은 상태가 포함 되어 있습니다. 활동 상태 변경 되 면 임박한 상태 변경의 활동에 알리고 해당 변경에 맞게 코드를 실행할 수 있도록 적절 한 수명 주기 이벤트 메서드가 호출 됩니다. 활동의 수명 주기를 검사 하 고 책임에 설명 하는이 문서는 올바르게 동작 하 고 안정적인 응용 프로그램의 일부로 이러한 상태 변화 하는 동안 활동에 합니다.
ms.prod: xamarin
ms.assetid: 05B34788-F2D2-4347-B66B-40AFD7B1D167
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/28/2018
ms.openlocfilehash: 48ff30397b2592dd2c4dbd445987392d78ced6f3
ms.sourcegitcommit: d3f48bfe72bfe03aca247d47bc64bfbfad1d8071
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66740772"
---
# <a name="activity-lifecycle"></a>활동 수명 주기

_활동은 Android 응용 프로그램의 기본 빌딩 블록 및 여러 다른 상태에에서 존재할 수 있습니다. 작업 수명 주기 인스턴스화를 사용 하 여 시작 및 소멸을 사용 하 여 종료 사이 많은 상태가 포함 되어 있습니다. 활동 상태 변경 되 면 임박한 상태 변경의 활동에 알리고 해당 변경에 맞게 코드를 실행할 수 있도록 적절 한 수명 주기 이벤트 메서드가 호출 됩니다. 활동의 수명 주기를 검사 하 고 책임에 설명 하는이 문서는 올바르게 동작 하 고 안정적인 응용 프로그램의 일부로 이러한 상태 변화 하는 동안 활동에 합니다._

## <a name="activity-lifecycle-overview"></a>활동 수명 주기 개요

활동은 Android에 대 한 특정는 비정상적인 프로그래밍 개념입니다. 기존 응용 프로그램 개발의 경우 일반적으로 응용 프로그램을 시작 하기 위해 실행 되는 정적 main 메서드를 그러나 Android를 사용 하 여는 이야기가 다릅니다. 응용 프로그램 내에서 등록 된 모든 활동을 통해 android 응용 프로그램을 시작할 수 있습니다. 실제로 대부분의 응용 프로그램의 응용 프로그램 진입점으로 지정 된 특정 활동만 해야 합니다. 그러나 응용 프로그램이 충돌할 경우 종료 또는 OS에서 OS 마지막 열려 활동에서 응용 프로그램 또는 이전 활동 스택 내에서 다른 위치를 다시 시작을 시도할 수 있습니다.
또한 OS 활성 있지 않은 경우 작업을 일시 중지 하 고 메모리가 부족할 경우을 회수할 수 있습니다. 신중 하 게 고려 올바르게 해당 상태를 복원 작업 다시 시작 되 면 경우에 특히 작업 이전 작업에서 데이터에 의존 하는 응용 프로그램이 수행 되어야 합니다.

작업 수명 주기는 활동의 수명 주기 동안 OS 메서드 호출의 컬렉션으로 구현 됩니다. 이러한 메서드는 응용 프로그램의 상태 및 리소스 관리 요구 사항을 충족 하는 데 필요한 기능을 구현 하는 개발자를 사용 합니다.

것이 확인 작업 수명 주기에서 노출 하는 메서드를 구현 해야 합니다. 각 작업의 요구 사항 분석을 응용 프로그램 개발자를 위한 매우 중요 합니다. 이렇게 하지 않으면 응용 프로그램이 불안정, 충돌, 리소스 팽창 및 더 기본 OS 불안정 발생할 수 있습니다.

작업 수명 주기를 자세히 검사 하는이 장의 포함:

-  활동 상태
-  수명 주기 메서드
-  응용 프로그램의 상태를 유지합니다.


이 섹션에 포함 되어는 [연습](~/android/app-fundamentals/activity-lifecycle/saving-state.md) 효율적으로 작업 수명 주기 동안 상태를 저장 하는 방법에 실질적인 예제를 제공 하는 합니다. 이 장의 끝 부분에서 작업 수명 주기 및 Android 응용 프로그램에서 지 원하는 방법을 이해를 해야 합니다.

## <a name="activity-lifecycle"></a>활동 수명 주기

Android 작업 수명 주기 개발자 리소스 관리 프레임 워크를 제공 하는 활동 클래스 내에서 노출 하는 메서드 컬렉션을 구성 합니다. 이 프레임 워크에는 개발자를가 응용 프로그램 내의 각 작업의 고유한 상태 관리 요구 사항을 충족 하 고 리소스 관리를 제대로 처리할 수 있습니다.

### <a name="activity-states"></a>활동 상태

Android OS 상태에 따라 작업을 조정 합니다. 이렇게 하면 더 이상 사용, 메모리 및 리소스를 회수 하려면 os에 있는 활동을 식별 하는 Android 수 없습니다. 다음 다이어그램에서는 활동의 수명 동안 진행할 수는 상태를 보여 줍니다.

[![활동 상태 다이어그램](images/image1-sml.png)](images/image1.png#lightbox)

이러한 상태는 다음과 같은 4 개의 기본 그룹으로 분류할 수 있습니다.

1.  *활성 또는 실행 중인* &ndash; 활동을 활성으로 간주 하거나 활동 스택의 최상위를 전경으로 있는 경우 실행 라고도 합니다. 이 android에서 가장 높은 우선 순위 작업으로 간주 되며 같이 종료 됩니다 극단적인 상황에서 os, 같은 활동 보다 더 많은 메모리를 사용 하려고 하는 경우에 따라은 장치에서 사용할 수 있는 UI가 응답 하지 발생할 수 있습니다.

1.  *일시 중지* &ndash; 활동 비율은 장치가 절전 모드로 전환 되거나 활동은 여전히 표시 되지만 전체 크기의 새롭거나 투명 하 게 활동에 의해 부분적으로 숨겨진 경우 일시 중지 합니다. 일시 중지 된 작업은 여전히 활성 즉, 모든 상태 및 멤버 정보를 유지 하 고 창 관리자는 연결 된 상태로 유지 합니다. 두 번째 것으로 간주 됩니다이 Android 및 따라서 가장 높은 우선 순위 작업 안정적이 고 응답성이 뛰어난 Active/실행 중인 작업을 유지 하는 데 필요한 리소스 요구 사항을 충족는이 작업을 종료 하는 경우 OS가 종료만 됩니다.

1.  *중지/Backgrounded* &ndash; 중지 되거나 백그라운드에서 다른 작업으로 완전히 가려진 되는 작업으로 간주 됩니다.
    중지 된 작업으로 가능 하지만 중지 된 작업으로 간주 됩니다 세 가지 상태 중 가장 낮은 우선 순위 및 OS를 리소스를 충족 하려면 먼저이 상태의 작업을 종료 하는 이와 같이 해당 상태 및 멤버 정보를 유지 하려고 시도할 수 더 높은 우선 순위 작업의 요구 사항입니다.

1.  *다시 시작* &ndash; 가능한 활동에서 아무 곳 이나은 Android에서 메모리에서 제거할 수명 주기에서 중지를 일시 중지 됨입니다. 로 다시 이동할 경우 다시 시작 해야, 이전에 저장 된 상태로 복원할 활동과 다음 사용자에 게 표시 합니다.


### <a name="activity-re-creation-in-response-to-configuration-changes"></a>구성 변경에 대 한 응답으로 작업 다시 생성

더욱 복잡을 Android 구성 변경 내용을 호출 목록에서 하나의 자세한 렌치를 throw 합니다. 구성 변경 사항은 신속 하 게 활동 소멸/다시 creation 주기 장치가 같은 활동의 구성을 변경 될 때 발생 하는 [회전](~/android/app-fundamentals/handling-rotation.md) (가로 또는 세로 모드에서 다시 빌드되지 해야 하는 작업 및 모드) 키보드 표시 됩니다 (및 활동 자체 크기를 조정할 수 있도록 표시 됩니다), dock, 특히 장치를 배치 하면 또는 합니다.

구성 변경을 중지 하 고 작업을 다시 시작 하는 동안 발생 하는 동일한 작업 상태 변경 내용을 유발 합니다. 그러나를 응용 프로그램 응답성이 뛰어난 느낌 하 고 구성 변경 하는 동안에 수행 하려면 반드시 최대한 신속 하 게 처리 되도록 합니다. 이 인해 Android 구성 변경 하는 동안 상태를 유지 하기 위해 사용할 수 있는 특정 API에 있습니다.
이 나중에 다룰 것은 [the 주기 전체에서 상태 관리](~/android/app-fundamentals/activity-lifecycle/index.md#Managing_State_Throughout_the_Lifecycle) 섹션입니다.

### <a name="activity-lifecycle-methods"></a>활동 수명 주기 메서드

Android SDK 및 확장을 통해 Xamarin.Android 프레임 워크는 응용 프로그램 내에서 활동의 상태를 관리 하기 위한 강력한 모델을 제공 합니다. 활동의 상태를 변경할 때 해당 작업에서 특정 메서드를 호출 하는 OS로 하 여 활동에 알립니다. 다음 다이어그램은 작업 수명 주기를 기준으로 이러한 메서드를 보여 줍니다.

[![활동 수명 주기 순서도](images/image2-sml.png)](images/image2.png#lightbox)

개발자는 활동 내에서 이러한 메서드를 재정의 하 여 상태 변경 내용을 처리할 수 있습니다. 단, 모든 수명 주기 메서드는 UI 스레드에서 호출 됩니다 하 고 등 새 활동을 표시 하 여 OS의 다음 부분 현재 활동에 숨기기 등의 UI 작업을 수행을 차단 하는 두는 것이 반드시 합니다. 따라서 이러한 메서드의 코드는 잘 수행 생각 하는 응용 프로그램을 가능한 한 간결 이어야 합니다. 모든 장기 실행 작업을 백그라운드 스레드에서 실행 되어야 합니다.

각 이러한 수명 주기 메서드 및 해당 사용을 살펴보겠습니다.

#### <a name="oncreate"></a>OnCreate

[OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) 활동이 만들어질 때 호출 될 첫 번째 메서드입니다.
`OnCreate` 와 같은 작업에서 필요할 수 있는 모든 시작 초기화를 수행 하기 위해 항상 재정의 됩니다.

-  뷰 만들기
-  변수 초기화
-  목록에 정적 데이터 바인딩


`OnCreate` 사용 된 [번들](https://developer.xamarin.com/api/type/Android.OS.Bundle/) 매개 변수를 저장 하 고 번들 null이 아닌 경우 작업 간의 상태 정보 및 개체를 전달 하기 위한 사전 인이 작업을 다시 시작 하 고 나타냅니다에서 해당 상태를 복원 해야 합니다 이전 인스턴스입니다. 다음 코드에 번들에서 값을 검색 하는 방법을 보여 줍니다.

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

한 번 `OnCreate` 가 완료 되 면 Android 호출 `OnStart`합니다.

#### <a name="onstart"></a>OnStart

[OnStart](https://developer.xamarin.com/api/member/Android.App.Activity.OnStart/) 후 시스템에서 항상 라고 `OnCreate` 완료 됩니다. 작업 활동 보기의 현재 값을 새로 고치는 등 표시 되기 전에 특정 작업 권리를 수행 해야 할 경우 활동에서이 메서드를 재정의할 수 있습니다. Android 호출 `OnResume` 이 메서드 직후입니다.

#### <a name="onresume"></a>OnResume

시스템 호출 [OnResume](https://developer.xamarin.com/api/member/Android.App.Activity.OnResume/) 작업이 사용자 상호 작용을 시작할 준비가 됩니다.
활동와 같은 작업을 수행 하려면이 메서드를 재정의 해야 합니다.

-  프레임 속도 (게임 개발의 일반적인 작업)를 늘려
-  시작 애니메이션
-  GPS 업데이트에 대 한 수신 대기
-  관련 경고 또는 대화 상자를 표시 합니다.
-  외부 이벤트 처리기를 연결 합니다.


예를 들어 다음 코드 조각에는 카메라를 초기화 하는 방법을 보여 줍니다.

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

`OnResume` 작업 이기 때문에 반드시 수행 `OnPause` 에 되지 않은 새 수 없습니다. `OnResume`후에 실행할 보장 되는 유일한 수명 주기 방법 이므로, `OnPause` 수명으로 활동을 제공 하는 경우.

#### <a name="onpause"></a>OnPause

[OnPause](https://developer.xamarin.com/api/member/Android.App.Activity.OnPause/) 시스템 백그라운드 또는 작업은 부분적으로 불명확 하는 경우 활동을 배치 하려고 할 때 호출 됩니다. 작업 하는 경우이 메서드를 재정의 해야 합니다.

-   영구 데이터를 저장 하지 않은 변경 내용 커밋

-   삭제 하거나 리소스를 소비 하는 다른 개체를 정리

-   프레임 속도 및 일시 중지 애니메이션 감속

-   외부 이벤트 처리기 또는 (즉, 해당 서비스에 연결 된) 알림 처리기의 등록을 취소 합니다. 이 작업 메모리 누수를 방지 하기 위해 수행 해야 합니다.

-   마찬가지로, 활동을 표시 하는 대화 상자 또는 경고 하는 경우 이러한 정리 해야 합니다와 `.Dismiss()` 메서드.

예를 들어 다음 코드 조각으로 작업을 만들 수 없습니다 카메라를 해제 합니다 일시 중지 하는 동안 사용 합니다.

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

두 가지가 있습니다 가능한 수명 주기 후 호출 되는 `OnPause`:

1.  `OnResume` 작업은 전경으로 반환할 경우 호출 됩니다.
1.  `OnStop` 백그라운드에서 작업 배치 하는 경우 호출 됩니다.


#### <a name="onstop"></a>OnStop

[OnStop](https://developer.xamarin.com/api/member/Android.App.Activity.OnStop/) 활동을 더 이상 사용자에 게 표시 하는 경우 호출 됩니다. 다음 중 하나가 발생 하면이 상황이 발생 합니다.

-  새 활동 시작 되 고 및는이 작업을 다룹니다.
-  기존 활동은 전경으로 상태가 됩니다.
-  활동 소멸 되 고 있습니다.


`OnStop` 항상 호출할 수 없습니다 Android 리소스를 겪게 됩니다 하 고 작업을 백그라운드 제대로 없습니다 하는 경우와 같이 메모리 부족 상황에서. 이러한 이유로 유용 하지 의존 `OnStop` 소멸에 대 한 작업을 준비 하는 경우 호출 시작 합니다. 이 개 후 호출할 수 있는 다음 수명 주기 메서드 `OnDestroy` 활동이 제거 될 경우 또는 `OnRestart` 경우 활동 사용자 상호 작용에 다시 제공 됩니다.

#### <a name="ondestroy"></a>OnDestroy

[OnDestroy](https://developer.xamarin.com/api/member/Android.App.Activity.OnDestroy/) 소멸 있고 메모리에서 완전히 제거 하려면 먼저 활동 인스턴스에서 호출 되는 최종 메서드입니다. 극단적인 경우 Android 됩니다 하는 활동을 호스팅하는 응용 프로그램 프로세스를 제거할 수 있습니다 `OnDestroy` 호출 되지 않습니다. 대부분의 작업은 대부분 정리 및 종료 저희 때문에이 메서드를 구현 하지는 `OnPause` 고 `OnStop` 메서드. `OnDestroy` 메서드를 재정의 일반적으로 장기를 정리 하는 리소스를 실행할 누수 될 수 리소스입니다. 이 예제에서 시작 된 백그라운드 스레드 수 `OnCreate`입니다.

수명 주기 메서드가 활동 소멸 된 후에 호출 됩니다.

#### <a name="onrestart"></a>OnRestart

[OnRestart](https://developer.xamarin.com/api/member/Android.App.Activity.OnRestart/) 작업 중지 된 후에 다시 시작 되 고 전에 호출 됩니다. 사용자가 응용 프로그램의 활동에 있는 동안 홈 단추를 누르면이 좋은 예가 것입니다. 이 경우 `OnPause` 차례로 `OnStop` 메서드가 호출 되 고 작업이 백그라운드로 이동 되지만 제거 되지 않습니다. 사용자가 Android 작업 관리자 또는 유사한 응용 프로그램을 사용 하 여 응용 프로그램을 복원 하려면 호출 하는 경우는 `OnRestart` 활동의 메서드.

어떤 종류의 논리를 구현 해야 하는 것에 대 한 일반 지침은 없습니다 `OnRestart`합니다. 왜냐하면 `OnStart` 는 활동이 만들어지고 있는지 여부에 관계 없이 항상 호출 또는 작업에 필요한 모든 리소스에서 초기화 해야 하므로 다시 시작 되 `OnStart`를 대신 `OnRestart`합니다.

호출 후 다음 수명 주기 메서드 `OnRestart` 됩니다 `OnStart`합니다.

### <a name="back-vs-home"></a>Vs를 백업 합니다. 홈

많은 Android 장치에는 두 개의 고유 단추: "뒤로" 단추 및 "홈" 단추가 있습니다. 이 예제는 Android 4.0.3의 다음 스크린 샷에서 확인할 수 있습니다.

[![뒤로 및 홈 단추](images/image4-sml.png)](images/image4.png#lightbox)

백그라운드에서 응용 프로그램 설정의 효과 동일 하 게 표시 되는 경우에 두 단추는 미묘한 차이가 있습니다. 사용자가 뒤로 단추를 클릭 하면 이러한 지시 Android 활동을 사용 하 여 완료 합니다. Android 활동을 삭제 됩니다. 반면, 사용자 홈 단추를 클릭할 때 작업 단순히 배치 된 백그라운드 &ndash; Android 활동을 종료 하지 것입니다.

<a name="Managing_State_Throughout_the_Lifecycle" />

## <a name="managing-state-throughout-the-lifecycle"></a>수명 주기 전체에서 상태 관리

작업 중지 되거나 삭제 하는 경우 시스템 이상 리하이드레이션 작업의 상태를 저장 하는 기회를 제공 합니다.
이 상태를 저장 된 인스턴스 상태 라고 합니다. Android 활동 수명 주기 동안 인스턴스 상태를 저장 하기 위한 세 가지 옵션을 제공 합니다.

1. 기본 값을 저장을 `Dictionary` 라고는 [번들](https://developer.xamarin.com/api/type/Android.OS.Bundle/) 는 Android를 사용 하 여 상태를 저장 합니다.

1. 사용자 지정 클래스를 만드는 비트맵 같은 복잡 한 값이 포함 됩니다. Android는 상태를 저장 하려면이 사용자 지정 클래스를 사용 합니다.

1. 구성 변경의 수명 주기를 우회 하 고 작업의 상태를 유지 하는 것에 대 한 전체 책임입니다.


이 가이드에서는 처음 두 옵션을 설명합니다.



### <a name="bundle-state"></a>번들 상태

인스턴스 상태를 저장 하는 것에 대 한 기본 옵션으로 알려진 키/값 사전 개체를 사용 하는 것을 [번들](https://developer.xamarin.com/api/type/Android.OS.Bundle/)합니다.
활동이 만들어질 때 회수 하는 `OnCreate` 메서드 번들 매개 변수로 전달, 인스턴스 상태를 복원 하려면이 번들을 사용할 수 있습니다. 신속 하 게 않습니다. 또는 키/값 쌍 (예: 비트맵);으로 쉽게 직렬화는 복잡 한 데이터에 대 한 번들을 사용 하는 것은 권장 되지 않습니다. 대신, 문자열 같은 간단한 값에 대 한 사용 해야 합니다.

작업 저장 하 고 번들의 인스턴스 상태를 검색 하는 데는 메서드를 제공 합니다.

-   [OnSaveInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnSaveInstanceState/p/Android.OS.Bundle/) &ndash; 활동 소멸 될 때 Android에서 호출 됩니다. 모든 키/값 상태 항목을 유지 하는 경우 활동에서이 메서드를 구현할 수 있습니다.

-   [OnRestoreInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnRestoreInstanceState/p/Android.OS.Bundle/) &ndash; 후 이것을 `OnCreate` 메서드 완료 되 고 초기화가 완료 된 후 해당 상태를 복원 하기 위한 작업에 대 한 또 다른 기회를 제공 합니다.

다음 다이어그램에서는 이러한 메서드를 사용 하는 방법을 보여 줍니다.

[![번들 상태 순서도](images/image3-sml.png)](images/image3.png#lightbox)

#### <a name="onsaveinstancestate"></a>OnSaveInstanceState

[OnSaveInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnSaveInstanceState/p/Android.OS.Bundle/) 작업을 중지 하는 대로 호출 됩니다. 이 활동에서 상태를 저장할 수 있는 번들 매개 변수를 받습니다. 활동 장치 환경을 구성 변경, 하는 경우 사용할 수는 `Bundle` 재정의 하 여 작업 상태를 유지 하기 위해 전달 되는 개체 `OnSaveInstanceState`합니다. 예를 들어, 다음 코드를 고려하세요.

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

위의 코드 증가 라는 정수 `c` 때 단추가 `incrementCounter` 를 클릭 하면 결과에 표시를 `TextView` 라는 `output`합니다. 경우 구성 변경-예를 들어 경우 장치를 회전-위의 코드의 값을 잃을 `c` 때문에 `bundle` 것 `null`아래 그림에 나와 있는 것 처럼:

[![이전 값 표시 되지 않습니다.](images/07-sml.png)](images/07.png#lightbox)

값을 보존 하려면 `c` 이 예제에서는 활동을 재정의할 수 `OnSaveInstanceState`, 아래와 같이 번들의 값을 저장 합니다.

```csharp
protected override void OnSaveInstanceState (Bundle outState)
{
  outState.PutInt ("counter", c);
  base.OnSaveInstanceState (outState);
}
```

이제 장치는 새 방향으로 회전 하는 경우 정수 번들에 저장 되 고 줄을 사용 하 여 검색 됩니다.

```csharp
c = bundle.GetInt ("counter", -1);
```

> [!NOTE]
> 것이 항상 중요 한 호출의 기본 구현을 `OnSaveInstanceState` 뷰 계층의 상태를 저장할 수도 있습니다 있도록 합니다.



##### <a name="view-state"></a>뷰 상태

재정의 `OnSaveInstanceState` 위의 예제에서 카운터 등의 방향 변경 간에 작업에서 임시 데이터를 저장 하기 위한 적절 한 메커니즘입니다. 그러나 기본 구현은 `OnSaveInstanceState` 이 알아서 모든 보기에 대 한 UI에서 임시 데이터를 저장 하는 각 보기에 할당 하는 ID 하기만 합니다. 예를 들어, 응용 프로그램에는 `EditText` 다음과 같이 XML에 정의 된 요소:

```xml
<EditText android:id="@+id/myText"
  android:layout_width="fill_parent"
  android:layout_height="wrap_content"/>
```

이후 합니다 `EditText` 컨트롤에는 `id` 할당, 사용자는 일부 데이터를 입력 하 고가 장치를 회전할 때 데이터가 여전히 표시 됩니다, 아래와 같이:

[![데이터를 가로 모드로 유지 됩니다.](images/08-sml.png)](images/08.png#lightbox)

#### <a name="onrestoreinstancestate"></a>OnRestoreInstanceState

[OnRestoreInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnRestoreInstanceState/p/Android.OS.Bundle/) 후 호출할 `OnStart`합니다. 활동 번들을 이전 하는 동안 이전에 저장 된 모든 상태를 복원할 수 있는 기회를 제공 `OnSaveInstanceState`합니다. 에 제공 되는 동일한 번들 이것이 `OnCreate`하지만 합니다.

다음 코드에서는 어떻게에서 상태를 복원할 수 있습니다 `OnRestoreInstanceState`:

```csharp
protected override void OnRestoreInstanceState(Bundle savedState)
{
    base.OnRestoreSaveInstanceState(savedState);
    var myString = savedState.GetString("myString");
    var myBool = savedState.GetBoolean("myBool");
}
```

이 메서드는 유연 하 게 몇 가지 관련 상태를 복원할 때 존재 합니다. 경우에 따라 인스턴스 상태를 복원 하기 전에 모든 초기화가 완료 될 때까지 대기 하는 것이 적합 합니다. 또한 기존 활동의 서브 클래스 인스턴스 상태를 특정 값 복원할 수만 지정할 수 있습니다. 대부분의 경우에 필요 없는 재정의할 `OnRestoreInstanceState`대부분의 작업을 제공 하는 번들을 사용 하 여 상태를 복원할 수 있으므로, `OnCreate`합니다.

사용 하 여 상태를 저장 하는 예는 `Bundle`를 참조 합니다 [연습-저장 하는 활동 상태](saving-state.md).


#### <a name="bundle-limitations"></a>번들 제한 사항

하지만 `OnSaveInstanceState` 를 사용 하면 몇 가지 제한 사항이 임시 데이터를 저장 하는 쉬우며:

-   모든 경우에는 호출 되지 않습니다. 예를 들어, 키를 눌러 **홈** 또는 **다시** 활동을 종료 되지 것입니다 `OnSaveInstanceState` 호출 합니다.

-   에 전달 된 번들 `OnSaveInstanceState` 이미지와 같은 큰 개체에 적합 하지 않습니다. 개체를 저장 하는 큰 개체의 경우 [OnRetainNonConfigurationInstance](https://developer.xamarin.com/api/member/Android.App.Activity.OnRetainNonConfigurationInstance/) 은 아래 설명과 같이 것이 좋습니다.

-   번들을 사용 하 여 저장 된 데이터 serialize 되 고 지연이 발생할 수 있습니다.

번들 상태가 많은 메모리를 사용 하지 않는 단순 데이터에 대 한 유용한 반면 *비구성 인스턴스 데이터* 더 복잡 한 데이터 또는 데이터를 검색 하는 데 유용 하 고 웹 서비스 호출 또는 복잡 한가 데이터베이스 쿼리 합니다. 필요에 따라 구성 되지 않은 인스턴스 데이터 개체에 저장 됩니다. 다음 섹션에서는 `OnRetainNonConfigurationInstance` 구성 변경을 통해 보다 복잡 한 데이터 형식을 유지 하는 방법으로 합니다.


### <a name="persisting-complex-data"></a>복잡 한 데이터를 유지합니다.

번들의 데이터를 유지 하는 것 외에도 Android에서는 데이터 저장 재정의 하 여 [OnRetainNonConfigurationInstance](https://developer.xamarin.com/api/member/Android.App.Activity.OnRetainNonConfigurationInstance/) 의 인스턴스를 반환 하 고는 `Java.Lang.Object` 데이터 유지를 포함 하는 합니다. 두 가지 주요 이점이 있습니다 사용 하 여 `OnRetainNonConfigurationInstance` 상태를 저장 합니다.

-   반환 되는 개체 `OnRetainNonConfigurationInstance` 메모리가이 개체를 유지 하므로 큰 규모의 더 복잡 한 데이터 형식과 잘 수행 합니다.

-   `OnRetainNonConfigurationInstance` 메서드는 주문형으로 호출 하 고 필요한 경우에 합니다. 이 수동 캐시를 사용 하 여 보다 더 경제적입니다.

사용 하 여 `OnRetainNonConfigurationInstance` 같이 되는 시나리오는 여러 번 데이터를 검색 하는 데에 적합 한 웹 서비스 호출 합니다. 예를 들어 Twitter를 검색 하는 다음 코드를 고려 합니다.

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

이 코드에서 JSON으로 서식이 지정 되는 웹 결과 검색 하 고,를 구문 분석 하 고 다음 스크린샷에 표시 된 대로 다음 결과 목록에 표시:

[![화면에 표시 되는 결과](images/06-sml.png)](images/06.png#lightbox)

구성 변경이 발생할 때-예를 들어 장치-회전할 때 코드는 프로세스를 반복 합니다. 원래 검색된 된 결과 다시 사용 하 고 불필요 한 중복 네트워크 호출을 발생 하지를 사용할 수 있습니다. `OnRetainNonconfigurationInstance` 아래와 같이 결과 저장 합니다.

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

원래 결과에서 검색 된 장치를 회전할 때 이제는 `LastNonConfiguartionInstance` 속성입니다. 이 예제에서는 결과 구성는 `string[]` 트 윗을 포함 합니다. 이후 `OnRetainNonConfigurationInstance` 는 `Java.Lang.Object` 반환 될를 `string[]` 래핑됩니다 클래스에서 서브클래싱하는 `Java.Lang.Object`아래 표시 된 것 처럼:

```csharp
class TweetListWrapper : Java.Lang.Object
{
  public string[] Tweets { get; set; }
}
```

예를 들어 사용 하려고를 `TextView` 에서 반환 된 개체와 `OnRetainNonConfigurationInstance` 아래 코드에 나온 것 처럼 활동 누수 됩니다.

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

이 섹션에서는 사용 하 여 간단한 상태 데이터를 유지 하는 방법을 배웠습니다 합니다 `Bundle`를 사용 하 여 보다 복잡 한 데이터 형식을 유지 하 고 `OnRetainNonConfigurationInstance`입니다.

## <a name="summary"></a>요약

Android 작업 수명 주기는 응용 프로그램 내에서 작업의 상태 관리를 위한 강력한 프레임 워크를 제공 하지만 이해 하 고 구현 하기 어려울 수 있습니다. 이 장에서 해당 상태와 연관 된 수명 주기 메서드 뿐 아니라 해당 수명 동안 활동이 거칠 수 있습니다 하는 다양 한 상태를 도입 되었습니다. 다음으로, 이러한 각 방법에서 어떤 종류의 논리를 수행 해야 하는 것에 대 한 지침 제공 되었습니다.


## <a name="related-links"></a>관련 링크

- [회전 처리](~/android/app-fundamentals/handling-rotation.md)
- [Android 활동](https://developer.xamarin.com/api/type/Android.App.Activity/)
