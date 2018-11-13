---
title: Configuration Manager에서 비활성화된 확인란 배포
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: aaf675cd-d885-4dac-9754-77dbcaea3be9
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: 35efb00a721062ad3217300f7e3a5430b1bd1560
ms.sourcegitcommit: 849bf6d1c67df943482ebf3c80c456a48eda1e21
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51528145"
---
# <a name="deploy-checkboxes-disabled-in-configuration-manager"></a>Configuration Manager에서 비활성화된 확인란 배포

Xamarin 3.5부터 Xamarin.iOS 프로젝트 자동으로 배포 키를 누르면 합니다 **시작** 도구 모음 단추 또는 선택 합니다 **디버그 > 디버깅 시작** 메뉴 항목입니다. 으로 원하는 Xamarin.iOS 앱 프로젝트를 설정 해야 합니다 **시작 프로젝트** 이러한 명령 중 하나를 실행 하기 전에 합니다.

이 인해 합니다 **배포** 확인란 의도적으로 Xamarin.iOS 프로젝트에 대 한 Visual Studio 구성 관리자에서 사용 하지 않도록 설정 됩니다.

![](deploy-checkboxes-images/configuration.png "Xamarin 3.5에서 Xamarin.iOS 프로젝트에 사용할 수 없습니다 '배포' 확인란을 보여 주는 visual Studio 구성 관리자")

이 변경은 Xamarin.iOS 앱 프로젝트를 사용 하 여 배포를 설정 하지 않은 경우 이전 버전의 Xamarin (버전 3.3 및 이전 버전)에 나타날 수 있는 오류를 제거 합니다.

![](deploy-checkboxes-images/error.png "오류 대화 상자: 프로젝트 iPhoneApp1 시작 하기 전에 배포 해야 합니다. 솔루션 구성 관리자에서 배포할 프로젝트를 선택 하는 것을 확인 합니다.")
