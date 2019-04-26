---
title: Visual Studio에서 내 참조된 라이브러리 프로젝트를 빌드에 포함시키지 않는 이유는 무엇인가요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: b9009db8-e716-43aa-b40e-6f28a8eb1b82
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: d7aeac2f433e8fdf231f5887f1537f15e2bd1976
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61341310"
---
# <a name="why-doesnt-visual-studio-include-my-referenced-library-project-in-my-build"></a>Visual Studio에서 내 참조된 라이브러리 프로젝트를 빌드에 포함시키지 않는 이유는 무엇인가요?

Visual Studio에서 사용 하는 **Configuration Manager** 솔루션의 프로젝트는 자동으로 지정된 된 빌드 또는 배포 구성에 포함할지를 결정 합니다.

참조 된 라이브러리 프로젝트를 사용 하 여 생성 되는 일부 템플릿 구성;에 포함 된 참조 된 라이브러리가 이미 하지만 그렇지 않은 경우 수동으로 설정 해야 합니다.

## <a name="how-to-use-the-configuration-manager"></a>Configuration Manager를 사용 하는 방법

1. 열기 **빌드 > 구성 관리자**
2. 예를 들어 사용자 지정 구성을 선택 **디버그 | iPhone**
3. 포함 하려는 프로젝트에 대 한 확인란을 선택 합니다.

> [!NOTE]
> 회색 상자는 자동으로 처리 되며 변경이 필요 하지 않아야 합니다.

이러한 단계의 스크린 캐스트: [http://screencast.com/t/zLoQOpEn](http://screencast.com/t/zLoQOpEn)
