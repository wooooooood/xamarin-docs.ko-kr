---
ms.topic: include
ms.openlocfilehash: e4dfd1ac12f3010939d483381a785091d71599ed
ms.sourcegitcommit: 28dbb5fa2fbcc7e66cd09c1d0077496fcdefac56
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51210602"
---
## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[센서 속도](xref:Xamarin.Essentials.SensorSpeed)

- **가장 빠름** – 센서 데이터를 가장 빠르게 가져옵니다(UI 스레드에 반환이 보장되지 않음).
- **게임** – 게임에 적합한 속도(UI 스레드에 반환이 보장되지 않음).
- **일반** - 화면 방향 변경에 적합한 기본 속도입니다.
- **UI** - 일반 사용자 인터페이스에 적합한 속도입니다.

이벤트 처리기가 UI 스레드에서 실행하도록 보장되지 않고 사용자 인터페이스 요소에 액세스해야 하는 경우 [`MainThread.BeginInvokeOnMainThread`](~/essentials/main-thread.md) 메서드를 사용하여 UI 스레드에서 해당 코드를 실행하세요.