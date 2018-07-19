---
title: Visual Studio 프로세스의 현재 호출 스택을 수집 방법
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 64c24b09-2c4a-43ad-b94d-6cd05a1aee44
author: asb3993
ms.author: amburns
ms.date: 03/30/2017
ms.openlocfilehash: e81c28f0610a0df2e4fe06349685ef5e0744071a
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
ms.locfileid: "33919763"
---
# <a name="how-do-i-collect-the-current-call-stacks-of-the-visual-studio-process"></a>Visual Studio 프로세스의 현재 호출 스택을 수집 방법

언제 GUI (일시 중단, 고정) Visual Studio에서 작동 중지, 진단 정보를 수집 하는 중요 한 부분을은 Visual Studio 프로세스의 모든 스레드에서 호출 스택 집합. Visual Studio의 응답 하지 않는 인스턴스에 대 한이 정보를 저장 하려면 Visual Studio의 두 번째 인스턴스를 사용할 수 있습니다.

1. Visual Studio의 두 번째 인스턴스 (새 창)을 시작 합니다.

2. 열려 있는 솔루션에 Visual Studio의 새 인스턴스를 닫습니다.

3. **디버그 > 프로세스에 연결**을 선택합니다.

  ![](vs-callstack-images/image1.png "디버그 선택 > 프로세스에 연결")

4. 응답이 없는 원래 인스턴스를 선택 `devenv.exe` 목록에서 **사용 가능한 프로세스**합니다.

5. 선택 **디버그 > 모두 중단**합니다.

  ![](vs-callstack-images/image2.png "디버그 선택 > 모두 중단")

6. 선택 **디버그 > 다른 이름으로 덤프 저장**합니다.

  ![](vs-callstack-images/image3.png "디버그 선택 > 다른 이름으로 덤프 저장")

7. 변경 **파일 형식** 를 **미니 덤프 (\*.dmp)** 합니다. 이러한 보다 훨씬 작은 파일을 생성 됩니다 **힙 사용 미니 덤프**, 및 일반적으로 힙은 고정 진단와 관련이 없습니다.

  ![](vs-callstack-images/image4.png "일반적으로 힙은 고정 진단와 관련이 없습니다 및 힙 사용 미니 덤프 보다 훨씬 작은 파일 생성 됩니다.")

8. 덤프 파일을 저장 합니다. 온라인 파일을 전송 하는 경우를 압축 하 크기를 줄일 수 있습니다.
