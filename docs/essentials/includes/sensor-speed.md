---
ms.topic: include
ms.openlocfilehash: e4dfd1ac12f3010939d483381a785091d71599ed
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353271"
---
## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[센서 속도](xref:Xamarin.Essentials.SensorSpeed)

- **가장 빠른** – (UI 스레드에서 반환 하는 보장 되지)는 가능한 한 빠르게 센서 데이터를 가져옵니다.
- **게임** -평가 (UI 스레드에서 반환 하는 보장 되지) 게임에 적합 합니다.
- **보통** – 기본 급여 화면 방향이 변경에 적합 합니다.
- **UI** – 일반 사용자 인터페이스에 대 한 적합 한 평가 합니다.

이벤트 처리기를 UI 스레드에서 실행 하 여 이벤트 처리기가 사용자 인터페이스 요소에 액세스 해야 하는 경우 사용 하 여 보장 되지 않습니다 합니다 [ `MainThread.BeginInvokeOnMainThread` ](~/essentials/main-thread.md) UI 스레드에서 해당 코드를 실행 하는 방법입니다.