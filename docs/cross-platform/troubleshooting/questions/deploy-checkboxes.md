---
title: Configuration Manager에서 사용 하지 않도록 설정 하는 확인란 배포
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: aaf675cd-d885-4dac-9754-77dbcaea3be9
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: ab825ba4d28ca8768e5c633fc3779828638a498d
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
ms.locfileid: "33919516"
---
# <a name="deploy-checkboxes-disabled-in-configuration-manager"></a>Configuration Manager에서 사용 하지 않도록 설정 하는 확인란 배포

Xamarin 3.5 이후 Xamarin.iOS 프로젝트 자동으로 배포를 누를 때마다는 **시작** 도구 모음 단추 또는 선택 된 **디버그 > 디버깅 시작** 메뉴 항목입니다. 원하는 Xamarin.iOS 앱 프로젝트를 설정 해야는 **시작 프로젝트** 전에 실행 하기 전에 그 중 하나 명령입니다.

이 인해는 **배포** 확인란 의도적으로 Xamarin.iOS 프로젝트에 대 한 Visual Studio 구성 관리자에서 사용 하지 않도록 설정 됩니다.

![](deploy-checkboxes-images/configuration.png "Xamarin 3.5에서 Xamarin.iOS 프로젝트에 사용할 수 없습니다 '배포' 확인란을 보여 주는 visual Studio 구성 관리자")

이 변경을 수행 하면 Xamarin.iOS 앱 프로젝트 배포를 설정 하지 않은 경우 이전 버전의 Xamarin (버전 3.3)에 나타날 수 있는 오류가 없습니다.

![](deploy-checkboxes-images/error.png "오류 대화 상자: 프로젝트 iPhoneApp1 시작 하기 전에 배포 해야 합니다. 프로젝트를 선택 하는 솔루션 구성 관리자에서 배포를 확인 합니다.")
