---
title: ListView 및 활동 수명 주기
ms.prod: xamarin
ms.assetid: 40840D03-6074-30A2-74DA-3664703E3367
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 6e15fb8796ae6a616c5eae44059caae3d9478aef
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="listview-and-the-activity-lifecycle"></a>ListView 및 활동 수명 주기

활동 실행 중, 일시 중지 중 및 중지 하 고 특정 상태를 통해 시작, 예: 응용 프로그램을 실행로 이동 합니다. 자세한 정보 및 상태 전환에 대 한 특정 지침에 대 한 참조는 [활동 수명 주기 자습서](~/android/app-fundamentals/activity-lifecycle/index.md)합니다.
활동 수명 주기 및 위치를 이해 하는 프로그램 `ListView` 올바른 위치에 코드입니다.

활동의에서 '설치 작업을 '를 수행할 모든이 문서의 예제 `OnCreate` 메서드 (필수) 하는 경우 및에서 '정리' 수행 `OnDestroy`합니다. 일반적으로 예제 필요 없는 데이터를 더 자주 다시 로드 하므로 변경 되지 않는 작은 데이터 집합을 사용 합니다.

그러나 데이터가 자주 변경 하거나 많은 양의 메모리를 사용 하 여는 것을 채우고 새로 고침 다른 수명 주기 메서드를 사용 하기에 적합 하면 `ListView`합니다. 예를 들어 지속적으로 변경 (또는 다른 활동에 대 한 업데이트의 영향을 받을 수) 기본 데이터에서 어댑터를 만드는 다음 `OnStart` 또는 `OnResume` 최신 데이터에는 활동 표시 될 때마다 표시 되는지 확인 합니다.

어댑터에 메모리 또는 관리 되는 커서와 같은 리소스가 사용 하는 경우 이러한 리소스에 상호 보완적인 메서드 (예: 인스턴스화된 위치를 해제 해야. 개체에서 만든 `OnStart` 수 삭제 되어야 `OnStop`).


## <a name="configuration-changes"></a>구성 변경

해당 구성이 변경 되는 것을 알아야이 &ndash; 회전 및 키보드 표시 유형을 특히 화면 &ndash; 현재 활동 개체가 제거 하 고 다시 생성을 발생할 수 있습니다 (그렇지 않은 경우 사용 하 여 지정 하지 않으면는 `ConfigurationChanges` 특성)입니다. 즉, 정상 조건에서 장치를 회전 합니다는 `ListView` 및 `Adapter` 다시 만들어야 하 고 (에 코드를 작성 한 경우가 아니면 `OnPause` 및 `OnResume`) 스크롤 위치 및 행 선택 상태는 손실 됩니다.

다음 특성에서 개체가 제거 되 고 구성 변경에 따른 다시 활동 없게:

```csharp
[Activity(ConfigurationChanges="keyboardHidden|orientation")]
```

활동 후를 재정의 해야 `OnConfigurationChanged` 적절 하 게 이러한 변경 내용에 응답할 수 있습니다. 구성 변경 내용을 처리 하는 방법에 대 한 자세한 내용은 설명서를 참조 하십시오.

