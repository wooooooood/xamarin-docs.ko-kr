---
ms.topic: include
ms.openlocfilehash: b82ebeaeb28195e3ec0f5a0200c0448b6b76196a
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "61259478"
---
## <a name="sensor-speed"></a>[센서 속도](xref:Xamarin.Essentials.SensorSpeed)

- **가장 빠름** – 센서 데이터를 가장 빠르게 가져옵니다(UI 스레드에 반환이 보장되지 않음).
- **게임** – 게임에 적합한 속도(UI 스레드에 반환이 보장되지 않음).
- **기본값** - 화면 방향 변경에 적합한 기본 속도입니다.
- **UI** - 일반 사용자 인터페이스에 적합한 속도입니다.

이벤트 처리기가 UI 스레드에서 실행하도록 보장되지 않고 사용자 인터페이스 요소에 액세스해야 하는 경우 [`MainThread.BeginInvokeOnMainThread`](~/essentials/main-thread.md) 메서드를 사용하여 UI 스레드에서 해당 코드를 실행하세요.
