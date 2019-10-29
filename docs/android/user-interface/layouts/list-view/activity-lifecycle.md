---
title: Xamarin Android ListView 및 작업 수명 주기
ms.prod: xamarin
ms.assetid: 40840D03-6074-30A2-74DA-3664703E3367
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: a4ebda89ad3bdcb663dc9d7cd513e45760163c34
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028944"
---
# <a name="xamarinandroid-listview-and-the-activity-lifecycle"></a>Xamarin Android ListView 및 작업 수명 주기

작업은 시작, 실행 중, 일시 중지 및 중지와 같은 응용 프로그램이 실행 될 때 특정 상태를 거칩니다. 자세한 내용 및 상태 전환 처리에 대 한 구체적인 지침은 [활동 수명 주기 자습서](~/android/app-fundamentals/activity-lifecycle/index.md)를 참조 하세요.
활동 수명 주기를 이해 하 고 `ListView` 코드를 올바른 위치에 저장 하는 것이 중요 합니다.

이 문서의 모든 예제는 활동의 `OnCreate` 메서드에서 ' 설치 작업 '을 수행 하 고 필요한 경우 `OnDestroy`에서 ' 해체 '를 수행 합니다. 이 예에서는 일반적으로 변경 되지 않는 작은 데이터 집합을 사용 하므로 데이터를 더 자주 다시 로드 하지 않아도 됩니다.

그러나 데이터가 자주 변경 되거나 많은 메모리를 사용 하는 경우에는 다른 수명 주기 방법을 사용 하 여 `ListView`를 채우고 새로 고치는 것이 적합할 수 있습니다. 예를 들어 기본 데이터가 지속적으로 변경 되는 경우 (또는 다른 작업의 업데이트에 의해 영향을 받을 수 있음) `OnStart` 또는 `OnResume`에서 어댑터를 만들면 작업이 표시 될 때마다 최신 데이터가 표시 됩니다.

어댑터에서 메모리 또는 관리 되는 커서와 같은 리소스를 사용 하는 경우 보완 메서드에서 해당 리소스를 인스턴스화한 위치로 릴리스 해야 합니다 (예: `OnStart`에서 만든 개체는 `OnStop`)에서 삭제할 수 있습니다.

## <a name="configuration-changes"></a>구성 변경

특히 `ConfigurationChanges` 특성을 사용 하 여 지정 하지 않는 한, 현재 작업을 제거 하 고 다시 만들 수 &ndash; 특히 화면 회전 및 키보드 표시 유형 &ndash; 구성 변경을 기억해 야 합니다. 즉, 정상 상태에서 장치를 회전 하면 `ListView` `Adapter` 다시 생성 되 고, `OnPause` 및 `OnResume`에서 코드를 작성 하지 않은 경우에는 스크롤 위치 및 행 선택 상태가 손실 됩니다.

다음 특성은 구성 변경의 결과로 활동이 제거 되 고 다시 생성 되지 않도록 합니다.

```csharp
[Activity(ConfigurationChanges="keyboardHidden|orientation")]
```

그러면 활동은 `OnConfigurationChanged`를 재정의 하 여 적절 하 게 변경 내용에 응답 해야 합니다. 구성 변경 내용을 처리 하는 방법에 대 한 자세한 내용은 설명서를 참조 하세요.
