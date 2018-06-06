---
title: Visual Studio은 이유 내 빌드에 참조 라이브러리 프로젝트 포함 하지 않는?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: b9009db8-e716-43aa-b40e-6f28a8eb1b82
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: d7aeac2f433e8fdf231f5887f1537f15e2bd1976
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782224"
---
# <a name="why-doesnt-visual-studio-include-my-referenced-library-project-in-my-build"></a>Visual Studio은 이유 내 빌드에 참조 라이브러리 프로젝트 포함 하지 않는?

Visual Studio를 사용 하는 **Configuration Manager** 어떤 프로젝트 솔루션에서 자동으로 지정된 된 빌드 또는 배포 구성에 포함할지를 결정 합니다.

참조 된 라이브러리 프로젝트와 함께 생성 되는 일부 템플릿은; 구성에 포함 된 참조 라이브러리에 이미 있을 것합니다 하지만 그렇지 않은 경우 수동으로 설정 해야 합니다.

## <a name="how-to-use-the-configuration-manager"></a>구성 관리자를 사용 하는 방법

1. 열기 **빌드 > 구성 관리자**
2. 예를 들어 사용자 지정 구성을 선택 **디버그 | iPhone**
3. 포함 하려는 프로젝트에 대 한 확인란을 선택 합니다.

> [!NOTE]
> 회색 상자 자동으로 처리 되 고 변경 내용이 필요는 없습니다.

다음이 단계 중 동영상 가이드: [http://screencast.com/t/zLoQOpEn](http://screencast.com/t/zLoQOpEn)
