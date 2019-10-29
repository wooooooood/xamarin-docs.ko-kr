---
title: Visual Studio 프로세스의 현재 호출 스택을 수집하려면 어떻게 할까요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 64c24b09-2c4a-43ad-b94d-6cd05a1aee44
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: 9ed79b2273758b8051a96169d4c9b53870de1fb1
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73013032"
---
# <a name="how-do-i-collect-the-current-call-stacks-of-the-visual-studio-process"></a>Visual Studio 프로세스의 현재 호출 스택을 수집하려면 어떻게 할까요?

Visual Studio에서 GUI가 잠김 (중지, 중지) 되 면 수집할 진단 정보의 중요 한 부분은 Visual Studio 프로세스의 모든 스레드에서 호출 스택 집합입니다. Visual Studio의 중지 된 인스턴스에 대 한이 정보를 저장 하기 위해 Visual Studio의 두 번째 인스턴스를 사용할 수 있습니다.

1. Visual Studio의 두 번째 인스턴스 (새 창)를 시작 합니다.

2. Visual Studio의 새 인스턴스에서 열려 있는 솔루션을 모두 닫습니다.

3. **디버그 > 프로세스에 연결**을 선택합니다.

   ![](vs-callstack-images/image1.png "Select Debug > Attach to Process")

4. **사용 가능한 프로세스**목록에서 `devenv.exe`의 원래 정지 된 인스턴스를 선택 합니다.

5. **디버그 > 모두 중단**을 선택 합니다.

   ![](vs-callstack-images/image2.png "Select Debug > Break All")

6. **디버그 > 덤프를 다른 이름으로 저장을**선택 합니다.

   ![](vs-callstack-images/image3.png "Select Debug > Save Dump As")

7. **다른 이름으로 저장** 을 **미니 덤프 (\*.dmp)** 로 변경 합니다. 이렇게 하면 **힙을 사용 하는 미니 덤프**보다 훨씬 더 작은 파일이 생성 되 고, 일반적으로 힙은 고정 진단 작업과 관련이 없습니다.

   ![](vs-callstack-images/image4.png "This will produce a much smaller file than Minidump with Heap, and the heap is usually not relevant for diagnosing freezes")

8. 덤프 파일을 저장 합니다. 온라인으로 파일을 전송 하는 경우 파일을 압축 하 여 크기를 줄일 수 있습니다.
