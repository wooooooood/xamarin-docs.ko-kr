---
title: ListView 및 작업 수명 주기
ms.prod: xamarin
ms.assetid: 40840D03-6074-30A2-74DA-3664703E3367
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: b2328759b3158920bc8683ec14c2aebefd7a04ae
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61187075"
---
# <a name="listview-and-the-activity-lifecycle"></a>ListView 및 작업 수명 주기

작업 실행, 일시 중지 및 중지 되 고 특정 상태를 통해 시작 하는 등 응용 프로그램은 실행으로 이동 합니다. 자세한 정보 및 상태 전환에 대 한 특정 지침에 대 한 참조를 [작업 수명 주기 자습서](~/android/app-fundamentals/activity-lifecycle/index.md)합니다.
활동 수명 주기 및 위치를 알아야 할 것에 `ListView` 올바른 위치에 코드입니다.

활동에서 '설치 작업을 '를 수행이 문서의 예제에서는 모든 `OnCreate` 메서드 (필수) 하는 경우 및 '종료' 수행 `OnDestroy`합니다. 예제에서는 필요 없는 데이터를 더 자주 다시 로드 하므로 변경 되지 않는 작은 데이터 집합을 일반적으로 사용 합니다.

그러나 데이터는 자주 변경 되는 많은 양의 메모리를 사용 하 여 경우 수 있습니다 다른 수명 주기 메서드를 채우고 새로 고치는 데 적절 한 프로그램 `ListView`합니다. 예를 들어 지속적으로 변경 됩니다 (또는 다른 활동에 대 한 업데이트에 의해 영향을 받을 수) 기본 데이터에서 어댑터를 작성 한 다음 `OnStart` 또는 `OnResume` 최신 데이터를 활동은 표시 될 때마다 표시 되도록 합니다.

여기서는 인스턴스화된 (예: 보완 메서드에서 해당 리소스를 해제 해야 하는 어댑터 메모리 또는 관리 되는 커서와 같은 리소스를 사용 하는 경우 개체에서 만든 `OnStart` 에서 삭제할 수 `OnStop`).


## <a name="configuration-changes"></a>구성 변경

해당 구성을 변경 하는 점을 기억해 야 것 &ndash; 특히 회전 및 키보드 표시 화면 &ndash; 현재 활동과 소멸 되 고 다시 생성 하면 (사용을 지정 하지 않으면는 `ConfigurationChanges` 특성)입니다. 즉, 정상 조건에서는 장치를 회전 하면 된다는 `ListView` 하 고 `Adapter` 다시 만들어야 및 (코드를 작성 하지 않으면 `OnPause` 및 `OnResume`) 스크롤 위치 및 행 선택 상태는 손실 됩니다.

다음 특성에서 제거 되 고 구성 변경으로 인해 다시 활동을 방해 합니다.

```csharp
[Activity(ConfigurationChanges="keyboardHidden|orientation")]
```

작업 재정의 해야 `OnConfigurationChanged` 적절 하 게 이러한 변화에 응답 합니다. 구성 변경 내용을 처리 하는 방법에 대 한 자세한 내용은 설명서를 참조 하세요.

